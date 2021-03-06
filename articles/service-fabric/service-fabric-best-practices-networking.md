---
title: Рекомендации по Service Fabric сети Azure
description: Правила и вопросы проектирования для управления сетевыми подключениями с помощью Azure Service Fabric.
author: chrpap
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: chrpap
ms.openlocfilehash: caba864e77822ccab649f694df7e63e0ee5d6e51
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "101732570"
---
# <a name="networking"></a>Сеть

При создании кластеров Azure Service Fabric и управлении ими вы обеспечиваете сетевое подключение для узлов и приложений. Сетевые ресурсы включают в себя диапазоны IP-адресов, виртуальные сети, подсистемы балансировки нагрузки и группы безопасности сети. В этой статье вы узнаете рекомендации по работе с этими ресурсами.

Изучите [шаблоны сетей Azure Service Fabric](./service-fabric-patterns-networking.md) , чтобы узнать, как создавать кластеры, использующие следующие функции: существующую виртуальную сеть или подсеть, статический общедоступный IP-адрес, подсистему балансировки нагрузки только для внутреннего использования или внутреннюю и внешнюю подсистемы балансировки нагрузки.

## <a name="infrastructure-networking"></a>Сеть инфраструктуры
Чтобы максимально увеличить производительность виртуальной машины с помощью ускоренной сети, объявите свойство *enableAcceleratedNetworking* в шаблоне диспетчер ресурсов, следующий фрагмент кода представляет собой NetworkInterfaceConfigurations набор виртуальных машин, который обеспечивает ускоренную работу в сети.

```json
"networkInterfaceConfigurations": [
  {
    "name": "[concat(variables('nicName'), '-0')]",
    "properties": {
      "enableAcceleratedNetworking": true,
      "ipConfigurations": [
        {
        <snip>
        }
      ],
      "primary": true
    }
  }
]
```
Кластер Service Fabric можно подготовить с ускоренной работой в сети в [Linux](../virtual-network/create-vm-accelerated-networking-cli.md) и [Windows](../virtual-network/create-vm-accelerated-networking-powershell.md).

Ускоренная сеть поддерживается для номеров SKU серии виртуальных машин Azure: D/DSv2, D/DSv3, E/ESv3, F/FS, серия fsv2 и MS/MMS. Ускоренная работа в сети успешно протестирована с использованием номера SKU Standard_DS8_v3 на 01/23/2019 для кластера Service Fabric Windows и использования Standard_DS12_v2 на 01/29/2019 для кластера Service Fabric Linux. Обратите внимание, что для ускорения работы в сети требуется по крайней мере 4 виртуальных ЦП. 

Чтобы включить ускоренную работу в сети в имеющемся кластере Service Fabric, следует [масштабировать кластер Service Fabric путем добавления масштабируемого набора виртуальных машин](./virtual-machine-scale-set-scale-node-type-scale-out.md). Затем вы сможете выполнить следующие действия:
1. Подготовить NodeType с поддержкой ускоренной работы в сети.
2. Перенести службы и их состояния в подготовленный NodeType с поддержкой ускоренной работы в сети.

Масштабирование инфраструктуры требуется для включения ускоренной работы в сети в имеющемся кластере. Включение ускоренной работы в сети приведет к простою, так как все виртуальные машины в группе доступности следует [остановить и освободить, прежде чем включить ускорение на любой имеющейся сетевой карте](../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms).

## <a name="cluster-networking"></a>Сеть кластера

* Кластеры Service Fabric можно развернуть в имеющейся виртуальной сети, выполнив действия, описанные в статье [Схемы сетевых подключений Service Fabric](./service-fabric-patterns-networking.md).

* Для типов узлов рекомендуется использовать группы безопасности сети (группы безопасности сети), чтобы ограничить входящий и исходящий трафик к своему кластеру. Убедитесь, что в NSG открыты необходимые порты. 

* Основной тип узла, который содержит системные службы Service Fabric, не должен предоставляться через внешний балансировщик нагрузки и может предоставляться с помощью [внутреннего](./service-fabric-patterns-networking.md#internal-only-load-balancer).

* Используйте [статический общедоступный IP-адрес](./service-fabric-patterns-networking.md#static-public-ip-address-1) для кластера.

## <a name="network-security-rules"></a>Правила сетевой безопасности

Основные правила являются минимальными для безопасности управляемого кластера Azure Service Fabric. Сбой при открытии следующих портов или утверждение IP/URL-адреса помешает правильной работе кластера и может не поддерживаться. Если это правило задано, необходимо использовать [Автоматическое обновление образа ОС](../virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade.md), иначе потребуется открыть дополнительные порты.

### <a name="inbound"></a>Входящий трафик 
|Приоритет   |Имя               |Порт        |Протокол  |Источник             |Назначение       |Действие   
|---        |---                |---         |---       |---                |---               |---
|3900       |Azure              |19080;       |TCP       |Интернет           |Виртуальная сеть    |Allow
|3910       |Клиент             |19000;       |TCP       |Интернет           |Виртуальная сеть    |Allow
|3920       |Кластер            |1025-1027   |TCP       |Виртуальная сеть     |Виртуальная сеть    |Allow
|3930       |Временные          |49152-65534 |TCP       |Виртуальная сеть     |Виртуальная сеть    |Allow
|3940       |Приложение        |20000-30000 |TCP       |Виртуальная сеть     |Виртуальная сеть    |Allow
|3950       |SMB                |445         |TCP       |Виртуальная сеть     |Виртуальная сеть    |Allow
|3960       |RDP                |3389-3488   |TCP       |Интернет           |Виртуальная сеть    |Запрет
|3970       |SSH                |22          |TCP       |Интернет           |Виртуальная сеть    |Запрет
|3980       |Пользовательская конечная точка    |80          |TCP       |Интернет           |Виртуальная сеть    |Allow
|4100       |Блокировать входящие      |443         |Любой       |Любой                |Любой               |Allow

Дополнительные сведения о правилах безопасности для входящего трафика:

* **Azure**. Этот порт используется Service Fabric Explorer для просмотра кластера и управления им. Он также используется поставщиком ресурсов Service Fabric для запроса сведений о кластере для просмотра в портал управления Azure. Если этот порт недоступен из поставщика Service Fabric ресурсов, вы увидите сообщение, например "узлы не найдены" или "Упградесервиценотреачабле" в портал Azure, и ваш узел и список приложений будут отображаться пустыми. Это означает, что если вы хотите, чтобы ваш кластер был видимым в портал управления Azure, подсистема балансировки нагрузки должна предоставить общедоступный IP-адрес, а NSG должен разрешить входящий трафик 19080.  

* **Клиент**. Конечная точка подключения клиента для интерфейсов API, например, для функций RESTFUL, PowerShell и CLI. 

* **Кластер**. Используется для обмена данными между узлами; никогда не следует блокировать.

* **Эфемерная**. Service Fabric использует часть из них в качестве портов приложений, а остальные доступны для операционной системы. Он также сопоставляет этот диапазон с существующим диапазоном в операционной системе, поэтому для всех целей можно использовать диапазоны, приведенные в этом примере. Убедитесь в том, что разница между номерами начального и конечного портов составляет не менее 255. Если это различие слишком мало, могут возникнуть конфликты, потому что этот диапазон также используется для ОС. Чтобы просмотреть настроенный динамический диапазон портов, выполните *команду netsh int IPv4 показать динамический порт TCP*. Эти порты не нужны для кластеров Linux.

* **Приложение**. Диапазон портов приложения должен быть достаточным для обеспечения потребностей приложений в конечных точках. Этот диапазон должен отличаться от диапазона динамических портов на компьютере, т. е. диапазона ephemeralPorts, указанного в конфигурации. Service Fabric использует эти порты каждый раз, когда требуются новые порты, и потребует открытия брандмауэра для этих портов на узлах.

* **SMB**. Протокол SMB используется службой ImageStore для двух сценариев. Этот порт необходим для загрузки пакетов из ImageStore узлами, а также для их репликации между репликами. 

* **Протокол удаленного рабочего стола**. Необязательно, если требуется протокол RDP из Интернета или VirtualNetwork для сценариев Jumpbox. 

* **SSH**. Необязательно, если требуется SSH из Интернета или VirtualNetwork для сценариев Jumpbox.

* **Пользовательская конечная точка**. Пример приложения для включения конечной точки, доступной в Интернете.

### <a name="outbound"></a>Исходящий трафик

|Приоритет   |Имя               |Порт        |Протокол  |Источник             |Назначение       |Действие   
|---        |---                |---         |---       |---                |---               |---
|3900       |Сеть            |Любой         |TCP       |Виртуальная сеть     |Виртуальная сеть    |Allow
|3910       |Поставщик ресурсов  |443         |TCP       |Виртуальная сеть     |Service Fabric     |Allow
|3920       |Обновление            |443         |TCP       |Виртуальная сеть     |Интернет          |Allow
|3950       |Блокировать исходящие     |Любой         |Любой       |Любой                |Любой               |Запрет

Дополнительные сведения о правилах безопасности для исходящего трафика:

* **Сеть**. Коммуникационный канал для подсетей и других виртуальных сетей.

* **Поставщик ресурсов**. Подключение с помощью службы обновления для выполнения всех развертываний ARM поставщиком ресурсов Service Fabric.

* **Обновление с более ранней версии**. Служба обновления, использующая адрес download.microsoft.com для получения битов, необходима для установки, повторного создания образа и обновления среды выполнения. Служба работает с динамическими IP-адресами. В сценарии с подсистемой балансировки нагрузки "внутренний только" в шаблон необходимо добавить дополнительный внешний балансировщик нагрузки с правилом, разрешающим исходящий трафик для порта 443. При необходимости этот порт может быть заблокирован после успешной установки, но в этом случае пакет обновления должен быть распространен на узлы, или порт необходимо открыть в течение короткого периода времени, после чего потребуется выполнить обновление вручную.

Используйте брандмауэр Azure с [журналом потоков NSG](../network-watcher/network-watcher-nsg-flow-logging-overview.md) и [аналитикой трафика](../network-watcher/traffic-analytics.md) для мониторинга проблем с блокировкой безопасности. Хорошим примером является шаблон ARM [Service Fabric с NSG](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-1-NodeTypes-Secure-NSG) . 


## <a name="application-networking"></a>Сеть для приложений

* Для запуска рабочих нагрузок контейнера Windows используйте [режим открытой сети](./service-fabric-networking-modes.md#set-up-open-networking-mode), чтобы упростить взаимодействие между службами.

* Используйте обратный прокси-сервер, например [Traefik](https://docs.traefik.io/v1.6/configuration/backends/servicefabric/) или [обратный прокси-сервер Service Fabric](./service-fabric-reverseproxy.md), чтобы предоставить распространенные порты для приложений, например 80 или 443.

* Для контейнеров Windows, размещенных на компьютерах Air гаппед, которые не могут получать базовые уровни из облачного хранилища Azure, переопределяйте поведение внешнего слоя с помощью флага [--allow-нераспространяемый-артефакты](/virtualization/windowscontainers/about/faq#how-do-i-make-my-container-images-available-on-air-gapped-machines) в управляющей программе DOCKER.

## <a name="next-steps"></a>Дальнейшие действия

* Создание кластера на основе виртуальных машин или компьютеров под управлением Windows Server: [Создание кластера Azure Service Fabric в локальной или облачной средах](service-fabric-cluster-creation-for-windows-server.md)
* Создание кластера на основе виртуальных машин или компьютеров под управлением Linux: [Создание кластера Linux](service-fabric-cluster-creation-via-portal.md).
* Дополнительные сведения о [вариантах поддержки Service Fabric](service-fabric-support.md)

[NSGSetup]: ./media/service-fabric-best-practices/service-fabric-nsg-rules.png

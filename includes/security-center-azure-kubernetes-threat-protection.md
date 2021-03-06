---
author: memildin
ms.author: memildin
manager: rkarlin
ms.date: 06/30/2020
ms.topic: include
ms.openlocfilehash: 979bc3ea6a01d31e64aa503216b8fd69164eadc6
ms.sourcegitcommit: 56b0c7923d67f96da21653b4bb37d943c36a81d6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450436"
---
Центр безопасности обеспечивает защиту контейнерных сред от угроз в реальном времени и создает оповещения о подозрительных действиях. Эти сведения можно использовать для быстрого устранения проблем безопасности и повышения безопасности контейнеров.

Центр безопасности обеспечивает защиту от угроз на разных уровнях. 

* **Уровень узла (предоставляется Azure Defender для серверов).** Тот же агент Log Analytics, который используется Центром безопасности на других виртуальных машинах, позволяет Azure Defender отслеживать подозрительные действия на узлах AKS в Linux, например обнаруживать веб-оболочки и подключения с известных подозрительных IP-адресов. Этот агент отслеживает такие события, как создание привилегированных контейнеров, подозрительный доступ к серверам API и запуск серверов Secure Shell (SSH) в контейнере Docker.

    Если вы решили не устанавливать агенты на узлах, то получите только часть преимуществ защиты от угроз и лишь некоторые из оповещений системы безопасности. Вы будете по-прежнему получать оповещения, связанные с анализом сети и обменом данными с вредоносными серверами.

    >[!IMPORTANT]
    > Сейчас установка агента Log Analytics в кластерах Службы Azure Kubernetes, работающих в масштабируемых наборах виртуальных машин, не поддерживается.

    Список оповещений на уровне узла AKS см. в [справочной таблице оповещений](../articles/security-center/alerts-reference.md#alerts-containerhost).


* **Уровень кластера AKS (предоставляется Azure Defender для Kubernetes).** На уровне кластера защита от угроз реализуется на основе анализа журналов аудита Kubernetes. Чтобы включить **мониторинг без агента**, включите Azure Defender. Для создания предупреждений на этом уровне Центр безопасности отслеживает службы, которыми управляет служба AKS, с помощью полученных ею журналов. Примерами событий на этом уровне могут служить представление панелей мониторинга Kubernetes, создание ролей с высоким уровнем привилегий и создание конфиденциальных подключений.

    >[!NOTE]
    > Центр безопасности создает оповещения системы безопасности для действий и развертываний Службы Azure Kubernetes, происходящих после включения параметра Kubernetes в параметрах подписки. 

    Список оповещений на уровне кластера AKS см. в [справочной таблице оповещений](../articles/security-center/alerts-reference.md#alerts-akscluster).

Следует также отметить, что наша международная команда специалистов по безопасности постоянно отслеживает ландшафт угроз. Они добавляют оповещение и уязвимости для конкретных контейнеров по мере их обнаружения.

> [!TIP]
> Вы можете имитировать оповещения для контейнеров, выполнив инструкции в [этой записи блога](https://techcommunity.microsoft.com/t5/azure-security-center/how-to-demonstrate-the-new-containers-features-in-azure-security/ba-p/1011270).
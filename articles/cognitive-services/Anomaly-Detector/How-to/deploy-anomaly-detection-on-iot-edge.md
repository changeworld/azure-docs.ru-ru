---
title: Запустить детектор аномалий на IoT Edge
titleSuffix: Azure Cognitive Services
description: Разверните модуль детектор аномалий для IoT Edge.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 12/03/2020
ms.author: mbullwin
ms.openlocfilehash: b4153b07b153a9ee0b16dc032ab5e7810e236d7d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "98936267"
---
# <a name="deploy-an-anomaly-detector-module-to-iot-edge"></a>Развертывание модуля детектора аномалий для IoT Edge

Узнайте, как развернуть модуль [обнаружения аномалий](../anomaly-detector-container-howto.md) Cognitive Services на устройстве IOT Edge. После развертывания в IoT Edge модуль выполняется в IoT Edge вместе с другими модулями в качестве экземпляров контейнера. Он предоставляет те же интерфейсы API, что и экземпляр контейнера детекторов аномалий, выполняющийся в стандартной среде контейнера DOCKER. 

## <a name="prerequisites"></a>Предварительные условия

* Используйте подписку Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись](https://azure.microsoft.com/free), прежде чем начинать работу.
* Установка [Azure CLI](/cli/azure/install-azure-cli).
* [Центр Интернета вещей](../../../iot-hub/iot-hub-create-through-portal.md) и устройство [IOT Edge](../../../iot-edge/quickstart-linux.md) .

[!INCLUDE [Create a Cognitive Services Anomaly Detector resource](../includes/create-anomaly-detector-resource.md)]

## <a name="deploy-the-anomaly-detection-module-to-the-edge"></a>Развертывание модуля обнаружения аномалий на границе

1. В портал Azure введите **детектор аномалий на IOT Edge** в поиск и откройте результат Azure Marketplace.
2. Откроется [страница портал Azure целевые устройства для IOT Edge модуля](https://portal.azure.com/#create/azure-cognitive-service.edge-anomaly-detector). Введите следующие необходимые сведения.

    1. Выберите свою подписку.

    1. Выберите центр Интернета вещей.

    1. Выберите **найти устройство** и найдите устройство IOT Edge.

3. Нажмите кнопку **Создать**.

4. Выберите модуль **аномалидетекторониотедже** .

    :::image type="content" source="../media/deploy-anomaly-detection-on-iot-edge/iot-edge-modules.png" alt-text="Изображение пользовательского интерфейса модулей IoT Edge с выделенной ссылкой Аномалидетекторониотедже с красной рамкой, указывающей, что это элемент для выбора.":::

5. Перейдите в раздел **Переменные среды** и укажите следующие сведения.

    1.  Оставьте значение Принять для запроса **Условия лицензионного соглашения**.

    1. Укажите сведения в разделе **Выставление счетов** для конечной точки вашей службы.

    1. В качестве значения параметра **ApiKey** укажите ключ API службы Cognitive Services.

    :::image type="content" source="../media/deploy-anomaly-detection-on-iot-edge/environment-variables.png" alt-text="Переменные среды с красными рамками вокруг областей, для которых требуется заполнить значения для конечной точки и ключа API":::

6. Щелкните **Обновить**.

7. Выберите **Далее: Маршруты** , чтобы определить маршрут. Укажите, что все сообщения от всех модулей направляются в Центр Интернета вещей Azure.

8. Нажмите **Далее: Просмотр и создание**. Можно просмотреть файл JSON, определяющий все модули, которые развернуты на устройстве IoT Edge.
    
9. Нажмите кнопку **Создать**, чтобы начать развертывание модуля.

10. После завершения развертывания модулей вернитесь на страницу IoT Edge Центра Интернета вещей. Чтобы просмотреть сведения об устройстве, выберите его в списке устройств IoT Edge.

11. Прокрутите вниз и просмотрите список модулей. Убедитесь, что состояние выполнения для нового модуля — "работает". 

Сведения об устранении неполадок состояния среды выполнения устройства IoT Edge см. в разделе [руководство по устранению неполадок](../../../iot-edge/troubleshoot.md) .

## <a name="test-anomaly-detector-on-an-iot-edge-device"></a>Проверка детектора аномалий на устройстве IoT Edge

Вы выполните HTTP-вызов к устройству Azure IoT Edge, в котором выполняется контейнер Azure Cognitive Services. Контейнер предоставляет интерфейсы API конечной точки на основе интерфейса RESTFUL. `http://<your-edge-device-ipaddress>:5000`Для API модуля используйте узел,.

Если на пограничном устройстве не разрешается входящая связь через порт 5000, необходимо создать новое **правило для входящего порта**. 

Для виртуальной машины Azure это можно задать в разделе **Параметры виртуальной машины**  >    >    >  **правило входящего** трафика  >  **Добавление правила для входящих портов**.

Существует несколько способов проверить, выполняется ли модуль. Перейдите к *внешнему IP-* адресу и предоставленному порту пограничной устройства и откройте свой любимый веб-браузер. Используйте различные URL-адреса запросов ниже, чтобы проверить, выполняется контейнер. Примеры URL-адресов запросов приведены ниже `http://<your-edge-device-ipaddress:5000` , но конкретный контейнер может отличаться. Помните, что необходимо использовать *внешний IP-* адрес пограничной устройства.

| URL-адрес запроса | Назначение |
|:-------------|:---------|
| `http://<your-edge-device-ipaddress>:5000/` | Контейнер предоставляет домашнюю страницу. |
| `http://<your-edge-device-ipaddress>:5000/status` | Кроме того, запрос с помощью GET проверяет, является ли ключ API, используемый для запуска контейнера, допустимым, не вызывая запрос конечной точки. Этот запрос может использоваться для [проб активности и готовности](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) Kubernetes. |
| `http://<your-edge-device-ipaddress>:5000/swagger` | Контейнер предоставляет полный набор документации по конечным точкам и функции **Попробовать**. Эта функция позволяет ввести параметры в веб-форму HTML и создать запрос без необходимости писать код. После возвращения результатов запроса предоставляется пример команды CURL с примером требуемого формата HTTP-заголовков и текста. |

![Домашняя страница контейнера](../../../../includes/media/cognitive-services-containers-api-documentation/container-webpage.png)

## <a name="next-steps"></a>Дальнейшие действия

* Проверьте раздел [Установка и запуск контейнеров](../anomaly-detector-container-configuration.md) , чтобы получить образ контейнера и запустить контейнер.
* Ознакомьтесь со статьей о [конфигурации контейнеров](../anomaly-detector-container-configuration.md).
* [Дополнительные сведения о службе API детектора аномалий](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)

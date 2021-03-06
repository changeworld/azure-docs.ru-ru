---
title: включить файл
description: включить файл
services: iot-hub
author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 04/28/2020
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 7d6718fb25b3743a50c52f11c8e19d80839b485c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "95561559"
---
<!-- operation names for the diag logs for IoT Hub -->

|Имя операции|Level|Описание|
|------------- |-----|-----------|
|ундефинедраутивалуатион|Сведения|Невозможно вычислить сообщение с условием присваивания. Например, если в сообщении отсутствует свойство в условии запроса маршрута. Дополнительные сведения о [синтаксисе запросов маршрутизации](../articles/iot-hub/iot-hub-devguide-routing-query-syntax.md).|
|раутивалуатионеррор|Ошибка|Ошибка при вычислении сообщения из-за проблемы с форматом сообщения. Например, эта ошибка будет записана в журнал, если в сообщении не указана кодировка содержимого или недопустимый тип содержимого. Они должны быть установлены в [свойствах системы](../articles/iot-hub/iot-hub-devguide-routing-query-syntax.md#system-properties).|
|дроппедмессаже|Ошибка|Сообщение было удалено, но не направлено. Это может быть вызвано тем, что сообщение не совпало ни с одним запросом маршрутизации, ни с одной конечной точкой и сообщение не было доставлено после нескольких повторных попыток. Рекомендуется получить дополнительные сведения о конечной точке с помощью REST API [получения работоспособности конечной точки](/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth).|
|ендпоинтунхеалси|Ошибка|Конечная точка не принимает сообщения из центра Интернета вещей, и центр Интернета вещей пытается повторно отправить эти сообщения. Мы рекомендуем наблюдать за последней известной ошибкой с помощью REST API [получения работоспособности конечной точки](/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth).|
|ендпоинтдеад|Ошибка|Конечная точка не принимает сообщения из центра Интернета вещей в течение часа. Мы рекомендуем наблюдать за последней известной ошибкой с помощью REST API [получения работоспособности конечной точки](/rest/api/iothub/iothubresource/getendpointhealth#iothubresource_getendpointhealth).|
|ендпоинсеалси|Сведения|Конечная точка находится в работоспособном состоянии и получает сообщения из центра Интернета вещей. Это сообщение не регистрируется непрерывно, но регистрируется только в том случае, если конечная точка снова становится работоспособной. Это сообщение означает, что центр Интернета вещей не смог отправить сообщения в конечную точку, но конечная точка теперь работоспособна.|
|орфанедмессаже|Сведения|Сообщение не соответствует ни одному маршруту.|
|инвалидмессаже|Ошибка|Сообщение недопустимо из-за несовместимости с конечной точкой. Рекомендуется проверить конфигурации конечной точки.|


Операции *ундефинедраутивалуатион*, *раутивалуатионеррор* и *орфанедмессаже* регулируемы и регистрируются не чаще одного раза в минуту на центр Интернета вещей.
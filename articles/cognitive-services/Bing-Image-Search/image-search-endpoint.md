---
title: Конечные точки для API Bing для поиска изображений
titleSuffix: Azure Cognitive Services
description: Поиск изображений API содержит три конечных точки. Конечная точка 1 возвращает изображения из Интернета. Конечная точка 2 возвращает ImageInsights (аналитические сведения об изображениях). Конечная точка 3 возвращает популярные видео.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: 67fec77136f5d593279be2846e63c51b60e16bb4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "94593524"
---
# <a name="endpoints-for-the-bing-image-search-api"></a>Конечные точки для API Bing для поиска изображений

> [!WARNING]
> API Поиска Bing будут перенесены из Cognitive Services в службы Поиска Bing. С **30 октября 2020 г.** подготовку всех новых экземпляров Поиска Bing необходимо будет выполнять в соответствии с процедурой, описанной [здесь](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> API-интерфейсы Поиска Bing, подготовленные с помощью Cognitive Services, будут поддерживаться в течение следующих трех лет или до завершения срока действия вашего Соглашения Enterprise (в зависимости от того, какой период окончится раньше).
> Инструкции по миграции см. в статье о [службах Поиска Bing](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

**API для поиска изображений** включает три конечные точки.  Конечная точка 1 возвращает изображения из Интернета на основе запроса. Конечная точка 2 возвращает [ImageInsights](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#imageinsightsresponse) (аналитические сведения об изображениях).  Конечная точка 3 возвращает популярные видео.

## <a name="endpoints"></a>Конечные точки

Чтобы получить результаты поиска изображений с помощью API Bing, отправьте запрос на одну из конечных точек, приведенных ниже. Для определения дополнительных спецификаций используйте заголовки и параметры URL-адреса.

**Конечная точка 1** возвращает результаты поиска изображений, соответствующие поисковому запросу пользователя, который определен с помощью `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search
```

**Конечная точка 2** возвращает аналитические сведения об изображении, используя `GET` или `POST`.
```
 GET or POST https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
Запрос GET возвращает аналитические сведения об изображении, например веб-страницы, содержащие данное изображение. Включите в запрос `GET` параметр [insightsToken](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#insightstoken).

Вы также можете включить в текст запроса `POST` двоичный файл изображения и задать для параметра [modules](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference#modulesrequested) значение `RecognizedEntities`. При этом будет возвращено значение [insightsToken](/rest/api/cognitiveservices-bingsearch/bing-images-api-v5-reference#insightstoken), используемое в качестве параметра в следующем запросе `GET`, который возвращает сведения о людях на данном изображении.  Задайте для параметра `modules` значение `All`, чтобы получить в результатах `POST` все аналитические сведения, кроме `RecognizedEntities`. При этом не потребуется выполнять второй вызов, используя параметр `insightsToken`.


**Конечная точка 3** возвращает изображения, которые набирают популярность. За основу берутся результаты поисковых запросов, выполненных другими пользователями. Изображения разделяются на различные категории, например, если в них фигурируют примечательные люди или события.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending
```

Список рынков, для которых поддерживается поиск популярных изображений, см. в статье [Получение изображений, набирающих популярность](./trending-images.md).

Дополнительные сведения о заголовках, параметрах, кодах рынков, объектах ответов, ошибках и т. п. вы найдете в справочнике [по API Bing для поиска изображений версии 7](/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference).
## <a name="response-json"></a>Ответ в формате JSON
Ответ на запрос на поиск изображений содержит результаты в виде объектов JSON. Примеры синтаксического анализа результатов см. в этом [руководстве](./tutorial-bing-image-search-single-page-app.md) и [исходном коде](./tutorial-bing-image-search-single-page-app.md).

## <a name="next-steps"></a>Дальнейшие действия
Интерфейсы API **Bing** поддерживают действия поиска, которые возвращают результаты определенного типа. Все конечные точки поиска возвращают результаты в виде объектов ответа JSON.  Все конечные точки поддерживают запросы, которые возвращают результаты с учетом языка и (или) местоположения по значениям долготы, широты и радиуса поиска.

Полные сведения о параметрах, поддерживаемых каждой конечной точкой, приведены в справочной документации по каждому типу.
Простые примеры запросов к API для поиска изображений см. в [кратких руководствах по поиску изображений](./overview.md).
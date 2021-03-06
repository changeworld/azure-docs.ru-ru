---
title: Поддержка языков — API анализа текста
titleSuffix: Azure Cognitive Services
description: Список естественных языков, поддерживаемых API анализа текста. В этой статье объясняется, какие языки поддерживаются для каждой операции.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/23/2021
ms.author: aahi
ms.openlocfilehash: f6a109c10491ad2eabb12069157e9e6f394bc1f4
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "101736616"
---
# <a name="text-analytics-api-v3-language-support"></a>Поддержка языков API анализа текста v3 

#### <a name="sentiment-analysis"></a>[анализ тональности](#tab/sentiment-analysis);

| Язык              | Код языка | Поддержка v3 | Начальная версия модели V3: |              Примечания |
|:----------------------|:-------------:|:----------:|:--------------------------:|-------------------:|
| Китайский (упрощенное письмо)    |   `zh-hans`   |     ✓      |         2019-10-01         | Также допускается `zh` |
| Китайский (традиционное письмо)   |   `zh-hant`   |    ✓      |         2019-10-01         |                    |
| Английский               |     `en`      |     ✓      |         2019-10-01         |                    |
| Французский                |     `fr`      |     ✓      |         2019-10-01         |                    |
| Немецкий                |     `de`      |     ✓      |         2019-10-01         |                    |
| Итальянский               |     `it`      |     ✓      |         2019-10-01         |                    |
| Японский              |     `ja`      |     ✓      |         2019-10-01         |                    |
| Корейский                |     `ko`      |    ✓      |         2019-10-01         |                    |
| Норвежский (букмол)   |     `no`      |     ✓      |         01.07.2020         |                    |
| Португальский (Бразилия)   |    `pt-BR`    |     ✓      |         2020-04-01         |                    |
| Португальский (Португалия) |    `pt-PT`    |     ✓      |         2019-10-01         | Также допускается `pt` |
| Испанский               |     `es`      |     ✓      |         2019-10-01         |                    |
| Турецкий               |     `tr`      |     ✓       |         01.07.2020        |                    |

### <a name="opinion-mining-v31-preview-only"></a>Интеллектуальный анализ данных с мнением (версия 3.1 — только предварительная версия)

| Язык              | Код языка | Начиная с версии модели V3: |              Примечания |
|:----------------------|:-------------:|:------------------------------------:|-------------------:|
| Английский               |     `en`      |              2020-04-01              |                    |


#### <a name="named-entity-recognition-ner"></a>[Распознавание именованных сущностей (NER)](#tab/named-entity-recognition)

> [!NOTE]
> * Для языков, помеченных как *, возвращаются только сущности Person, Location и Organization.

| Язык               | Код языка | Поддержка v3 | Начиная с версии модели V3: |       Примечания        |
|:-----------------------|:-------------:|:----------:|:-------------------------------:|:------------------:|
| Арабский                 |     `ar`      |      ✓*    |               2019-10-01        |                    |
| Китайский (упрощенное письмо)     |   `zh-hans`   |     ✓      |               2021-01-15        | Также допускается `zh` |
| Китайский (традиционное письмо)   |   `zh-hant`   |     ✓*      |               2019-10-01        |                    |
| Чешский                 |     `cs`      |     ✓*      |               2019-10-01        |                    |
| Датский                |     `da`      |     ✓*      |               2019-10-01        |                    |
| Нидерландский                 |     `nl`      |     ✓*      |               2019-10-01        |                    |
| Английский                |     `en`      |     ✓      |               2019-10-01        |                    |
| Финский               |     `fi`      |     ✓*      |               2019-10-01        |                    |
| Французский                 |     `fr`      |     ✓      |               2021-01-15        |                    |
| Немецкий                 |     `de`      |     ✓      |               2021-01-15        |                    |
| Иврит                |     `he`      |     ✓*      |               2019-10-01        |                    |
| Венгерский             |     `hu`      |     ✓*      |               2019-10-01        |                    |
| Итальянский               |     `it`      |     ✓       |               2021-01-15        |                    |
| Японский              |     `ja`      |     ✓       |               2021-01-15        |                    |
| Корейский                |     `ko`      |     ✓       |               2021-01-15        |                    |
| Норвежский (букмол)   |     `no`      |     ✓*      |               2019-10-01        | Также допускается `nb` |
| Польский                |     `pl`      |     ✓*      |               2019-10-01        |                    |
| Португальский (Бразилия)   |    `pt-BR`    |     ✓       |               2021-01-15        |                    |
| Португальский (Португалия) |    `pt-PT`    |     ✓       |               2021-01-15        | Также допускается `pt` |
| русском языке              |     `ru`      |     ✓*       |               2019-10-01        |                    |
| Испанский               |     `es`      |     ✓       |               2020-04-01        |                    |
| Шведский               |     `sv`      |     ✓*      |               2019-10-01        |                    |
| Турецкий               |     `tr`      |     ✓*      |               2019-10-01        |                    |

#### <a name="key-phrase-extraction"></a>[Пример. Как извлечь ключевые фразы с помощью Анализа текста](#tab/key-phrase-extraction)

| Язык              | Код языка |  Поддержка v3 | Доступно начиная с версии модели V3: |       Примечания        |
|:----------------------|:-------------:|:----------:|:-----------------------------------------:|:------------------:|
| Датский                |     `da`      |     ✓     |                2019-10-01                 |                    |
| Нидерландский                 |     `nl`      |     ✓      |                2019-10-01                 |                    |
| Английский               |     `en`      |     ✓      |                2019-10-01                 |                    |
| Финский               |     `fi`      |     ✓      |                2019-10-01                 |                    |
| Французский                |     `fr`      |     ✓      |                2019-10-01                 |                    |
| Немецкий                |     `de`      |     ✓      |                2019-10-01                 |                    |
| Итальянский               |     `it`      |     ✓      |                2019-10-01                 |                    |
| Японский              |     `ja`      |     ✓      |                2019-10-01                 |                    |
| Корейский                |     `ko`      |     ✓      |                2019-10-01                 |                    |
| Норвежский (букмол)   |     `no`      |     ✓      |                01.07.2020                 | Также допускается `nb` |
| Польский                |     `pl`      |    ✓      |                2019-10-01                 |                    |
| Португальский (Бразилия)   |    `pt-BR`    |     ✓      |                2019-10-01                 |                    |
| Португальский (Португалия) |    `pt-PT`    |    ✓      |                2019-10-01                 | Также допускается `pt` |
| Русский               |     `ru`      |     ✓      |                2019-10-01                 |                    |
| Испанский               |     `es`      |     ✓      |                2019-10-01                 |                    |
| Шведский               |     `sv`      |     ✓      |                2019-10-01                 |                    |

#### <a name="entity-linking"></a>[Связывание сущностей](#tab/entity-linking)

| Язык | Код языка |  Поддержка v3 | Доступно начиная с версии модели V3: | Примечания |
|:---------|:-------------:|:----------:|:-----------------------------------------:|:-----:|
| Английский  |     `en`      |     ✓      |                2019-10-01                 |       |
| Испанский  |     `es`      |    ✓      |                2019-10-01                 |       |

#### <a name="personally-identifiable-information-pii"></a>[личные сведения (PII).](#tab/pii)

| Язык               | Код языка | Поддержка v3 | Начиная с версии модели V3: |       Примечания        |
|:-----------------------|:-------------:|:----------:|:-------------------------------:|:------------------:|
| Китайский (упрощенное письмо)     |   `zh-hans`   |     ✓      |               2021-01-15        | Также допускается `zh` |
| Английский                |     `en`      |     ✓      |               01.07.2020        |                    |
| Французский                 |     `fr`      |     ✓      |               2021-01-15        |                    |
| Немецкий                 |     `de`      |     ✓      |               2021-01-15        |                    |
| Итальянский               |     `it`      |     ✓       |               2021-01-15        |                    |
| Японский              |     `ja`      |     ✓       |               2021-01-15        |                    |
| Корейский                |     `ko`      |     ✓       |               2021-01-15        |                    |
| Португальский (Бразилия)   |    `pt-BR`    |     ✓       |               2021-01-15        |                    |
| Португальский (Португалия) |    `pt-PT`    |     ✓       |               2021-01-15        | Также допускается `pt` |
| Испанский               |     `es`      |     ✓       |               2020-04-01        |                    |

#### <a name="language-detection"></a>[распознавание языка](#tab/language-detection);

API анализа текста может обнаруживать широкий спектр языков, разновидностей, диалектов и некоторых региональных и культурных языков, а также возвращать обнаруженные языки с их именем и кодом. Анализ текста параметры кода распознавание языка языка соответствуют стандарту [bcp-47](https://tools.ietf.org/html/bcp47) , большинство из которых соответствует идентификаторам [ISO-639-1](https://www.iso.org/iso-639-language-codes.html) . 

Если у вас имеется содержимое на менее распространенном языке, вы можете попробовать применить распознавание языка, чтобы узнать, вернет ли эта функция код. Для языков, которые не удалась распознать, возвращается ответ `unknown`.

| Язык | Код языка | Поддержка v3 | Доступно начиная с версии модели V3: |
|:-|:-:|:-:|:-:|
|Африкаанс|`af`|✓|    |
|Албанский|`sq`|✓|    |
|Амхарский|`am`|✓|2021-01-05|
|Арабский|`ar`|✓|    |
|Армянский|`hy`|✓|    |
|Ассамский|`as`|✓|2021-01-05|
|Азербайджанский|`az`|✓|2021-01-05|
|Баскский|`eu`|✓|    |
|Белорусский|`be`|✓|    |
|Бенгальский|`bn`|✓|    |
|Боснийский|`bs`|✓|2020-09-01|
|Болгарский|`bg`|✓|    |
|Barmština|`my`|✓|    |
|Каталонский|`ca`|✓|    |
|Центральная кхмерский|`km`|✓|    |
|Китайский|`zh`|✓|    |
|Китайский (упрощенный)|`zh_chs`|✓|    |
|Китайский (традиционное письмо)|`zh_cht`|✓|    |
|Corsican|`co`|✓|2021-01-05|
|Хорватский|`hr`|✓|    |
|Чешский|`cs`|✓|    |
|Датский|`da`|✓|    |
|Дари|`prs`|✓|2020-09-01|
|Мальдивский|`dv`|✓|    |
|Нидерландский|`nl`|✓|    |
|Английский|`en`|✓|    |
|Эсперанто|`eo`|✓|    |
|Эстонский|`et`|✓|    |
|Фиджийский|`fj`|✓|2020-09-01|
|Финский|`fi`|✓|    |
|Французский|`fr`|✓|    |
|Галисийский|`gl`|✓|    |
|Грузинский|`ka`|✓|    |
|Немецкий|`de`|✓|    |
|Греческий|`el`|✓|    |
|Гуджарати|`gu`|✓|    |
|Гаитянский|`ht`|✓|    |
|Хауса|`ha`|✓|2021-01-05|
|Иврит|`he`|✓|    |
|Hindi|`hi`|✓|    |
|Хмонг дау|`mww`|✓|2020-09-01|
|Венгерский|`hu`|✓|    |
|Исландский|`is`|✓|    |
|Игбо|`ig`|✓|2021-01-05|
|Индонезийский|`id`|✓|    |
|Инуктитут|`iu`|✓|    |
|Ирландский|`ga`|✓|    |
|Итальянский|`it`|✓|    |
|Японский|`ja`|✓|    |
|Яванская письменность|`jv`|✓|2021-01-05|
|Каннада|`kn`|✓|    |
|Казахский|`kk`|✓|2020-09-01|
|Киньяруанда|`rw`|✓|2021-01-05|
|Киргизский|`ky`|✓|2021-01-05|
|Корейский|`ko`|✓|    |
|Курдский|`ku`|✓|    |
|Лаосский|`lo`|✓|    |
|Латиница|`la`|✓|    |
|Латышский|`lv`|✓|    |
|Литовский|`lt`|✓|    |
|Люксембургский|`lb`|✓|2021-01-05|
|Macedonian|`mk`|✓|    |
|Малагасийский|`mg`|✓|2020-09-01|
|Малайский|`ms`|✓|    |
|Малаялам|`ml`|✓|    |
|Мальтийский|`mt`|✓|    |
|Маори|`mi`|✓|2020-09-01|
|Маратхи|`mr`|✓|2020-09-01|
|Монгольский|`mn`|✓|2021-01-05|
|Непальский|`ne`|✓|2021-01-05|
|Норвежский|`no`|✓|    |
|Норвежский (нюнорск)|`nn`|✓|    |
|Ория|`or`|✓|    |
|пашт|`ps`|✓|    |
|Персидский|`fa`|✓|    |
|Польский|`pl`|✓|    |
|Португальский|`pt`|✓|    |
|Панджаби|`pa`|✓|    |
|Керетарский диалект отоми|`otq`|✓|2020-09-01|
|Румынский|`ro`|✓|    |
|Русский|`ru`|✓|    |
|Самоанский|`sm`|✓|2020-09-01|
|Сербский|`sr`|✓|    |
|Шона|`sn`|✓|2021-01-05|
|Синдхи|`sd`|✓|2021-01-05|
|Сингальский|`si`|✓|    |
|Словацкий|`sk`|✓|    |
|Словенский|`sl`|✓|    |
|Сомалийский|`so`|✓|    |
|Испанский|`es`|✓|    |
|Сунданская письменность|`su`|✓|2021-01-05|
|Суахили|`sw`|✓|    |
|Шведский|`sv`|✓|    |
|Тагальский|`tl`|✓|    |
|Таитянский|`ty`|✓|2020-09-01|
|Таджикский|`tg`|✓|2021-01-05|
|Тамильский|`ta`|✓|    |
|Татарский|`tt`|✓|2021-01-05|
|Телугу|`te`|✓|    |
|Тайский|`th`|✓|    |
|Тибетский|`bo`|✓|2021-01-05|
|Тигринья|`ti`|✓|2021-01-05|
|Тонганский|`to`|✓|2020-09-01|
|Турецкий|`tr`|✓|2021-01-05|
|Туркменский|`tk`|✓|2021-01-05|
|Коса|`xh`|✓|2021-01-05|
|Йоруба|`yo`|✓|2021-01-05|
|Зулу|`zu`|✓|2021-01-05|

---

## <a name="see-also"></a>См. также раздел

* [Что такое API анализа текста?](overview.md)   

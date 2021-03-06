---
title: Фильтрация событий для службы "Сетка событий Azure"
description: В статье описывается, как фильтровать события при создании подписки на службу "Сетка событий Azure".
ms.topic: conceptual
ms.date: 03/04/2021
ms.openlocfilehash: fa63296f97bfa888cb0f425d0c03a5e4a7e46525
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103419853"
---
# <a name="understand-event-filtering-for-event-grid-subscriptions"></a>Общие сведения о фильтрации событий для подписок на службу "Сетка событий Azure"

В статье описываются различные способы фильтрации событий, отправляемых в конечную точку. При создании подписки на события предлагается три варианта фильтрации:

* Типы событий
* по тому, что находится в начале или конце темы;
* с помощью расширенных полей и операторов.

## <a name="event-type-filtering"></a>Фильтрация по типу события

По умолчанию все [типы событий](event-schema.md) для источника события отправляются в конечную точку. Можно сделать так, чтобы в конечную точку отправлялись только определенные типы событий. Например, можно получать уведомления об обновлениях ресурсов, но не получать уведомления о других операциях, таких как удаление. В этом случае фильтрация выполняется по типу события `Microsoft.Resources.ResourceWriteSuccess`. Предоставьте массив с типами событий или укажите `All`, чтобы получать все типы событий для источника событий.

При фильтрации по типу события используется такой синтаксис JSON:

```json
"filter": {
  "includedEventTypes": [
    "Microsoft.Resources.ResourceWriteFailure",
    "Microsoft.Resources.ResourceWriteSuccess"
  ]
}
```

## <a name="subject-filtering"></a>Фильтрация по теме

Для простой фильтрации по теме укажите начальное или конечное значение для этой темы. Например, можно указать, что тема заканчивается на `.txt`, чтобы получать только события, связанные с загрузкой текстового файла в учетную запись хранения. Или можно выполнять фильтрацию по теме, начинающейся с `/blobServices/default/containers/testcontainer`, чтобы получать все события для этого контейнера, но не для других контейнеров в этой учетной записи хранения.

При публикации событий в пользовательских разделах создавайте темы для событий, чтобы подписчикам было проще определить, представляет ли событие для них интерес. Подписчики могут использовать свойство темы для фильтрации и маршрутизации событий. Можно добавить путь к расположению, в котором произошло событие, чтобы подписчики могли выполнять фильтрацию по сегментам этого пути. Путь позволяет подписчикам сузить или расширить область фильтрации. Например, если указать в теме трехсегментный путь, такой как `/A/B/C`, подписчики смогут выполнить фильтрацию по первому сегменту `/A`, чтобы получить широкий набор событий. Эти подписчики получат события с такими темами, как `/A/B/C` или `/A/D/E`. Другие подписчики могут выполнить фильтрацию по `/A/B`, чтобы получить более узкий набор событий.

Для фильтрации по теме используется следующий синтаксис JSON:

```json
"filter": {
  "subjectBeginsWith": "/blobServices/default/containers/mycontainer/log",
  "subjectEndsWith": ".jpg"
}

```

## <a name="advanced-filtering"></a>Расширенная фильтрация

Вариант расширенной фильтрации позволяет выполнять фильтрацию по значениям в полях данных и указывать оператор сравнения. При расширенной фильтрации можно указать следующее:

* operatorType — тип сравнения;
* ключ — поле в данных события, которое используется для фильтрации Это может быть число, логическое значение, строка или массив.
* Values — значение или значения, сравниваемые с ключом.

## <a name="key"></a>Ключ
Key — это поле в данных события, которое используется для фильтрации. Это может быть один из следующих типов:

- Число
- Логическое
- Строка
- Array (массив). Чтобы использовать эту функцию, необходимо присвоить `enableAdvancedFilteringOnArrays` свойству значение true. В настоящее время портал Azure не поддерживает включение этой функции. 

    ```json
    "filter":
    {
        "subjectBeginsWith": "/blobServices/default/containers/mycontainer/log",
        "subjectEndsWith": ".jpg",
        "enableAdvancedFilteringOnArrays": true
    }
    ```

Для событий в **схеме сетки событий** используйте следующие значения для ключа: `ID` , `Topic` , `Subject` ,, `EventType` `DataVersion` или данные события (например, `data.key1` ).

Для событий в **схеме облачных событий** используйте следующие значения для ключа:,, `eventid` `source` `eventtype` , `eventtypeversion` или данные события (например, `data.key1` ).

Для **пользовательской входной схемы** используйте поля данных события (например, `data.key1` ). Чтобы получить доступ к полям в разделе данных, используйте `.` нотацию (с точкой). Например, `data.sitename` `data.appEventTypeDetail.action` для доступа к `sitename` или `action` для следующего события выборки.

```json
    "data": {
        "appEventTypeDetail": {
            "action": "Started"
        },
        "siteName": "<site-name>",
        "clientRequestId": "None",
        "correlationRequestId": "None",
        "requestId": "292f499d-04ee-4066-994d-c2df57b99198",
        "address": "None",
        "verb": "None"
    },
```

## <a name="values"></a>Значения
Возможны следующие значения: число, строка, логическое значение или массив.

## <a name="operators"></a>Операторы

Доступные для **чисел** операторы:

## <a name="numberin"></a>NumberIn;
Оператор Number возвращает значение true, если значение **ключа** является одним из указанных значений **фильтра** . В следующем примере проверяется, является ли значение `counter` атрибута в `data` разделе 5 или 1. 

```json
"advancedFilters": [{
    "operatorType": "NumberIn",
    "key": "data.counter",
    "values": [
        5,
        1
    ]
}]
```


Если ключ является массивом, все значения в массиве проверяются по массиву значений фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` и фильтр: `[a, b, c]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF filter == key
            MATCH
```

## <a name="numbernotin"></a>NumberNotIn.
Нумбернотин возвращает значение true, если значение **ключа** **не** является ни одним из указанных значений **фильтра** . В следующем примере проверяется, не имеет ли значение `counter` атрибута в `data` разделе 41 и 0. 

```json
"advancedFilters": [{
    "operatorType": "NumberNotIn",
    "key": "data.counter",
    "values": [
        41,
        0
    ]
}]
```

Если ключ является массивом, все значения в массиве проверяются по массиву значений фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` и фильтр: `[a, b, c]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF filter == key
            FAIL_MATCH
```

## <a name="numberlessthan"></a>NumberLessThan;
Оператор Нумберлесссан вычисляет значение true, если значение **ключа** **меньше** указанного значения **фильтра** . В следующем примере проверяется, меньше ли значение `counter` атрибута в `data` разделе 100. 

```json
"advancedFilters": [{
    "operatorType": "NumberLessThan",
    "key": "data.counter",
    "value": 100
}]
```

Если ключ является массивом, все значения в массиве проверяются по значению фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH key IN (v1, v2, v3)
    IF key < filter
        MATCH
```

## <a name="numbergreaterthan"></a>NumberGreaterThan;
Оператор Нумбергреатерсан возвращает значение true, если значение **ключа** **больше** указанного значения **фильтра** . В следующем примере проверяется, является ли значение `counter` атрибута в `data` разделе больше 20. 

```json
"advancedFilters": [{
    "operatorType": "NumberGreaterThan",
    "key": "data.counter",
    "value": 20
}]
```

Если ключ является массивом, все значения в массиве проверяются по значению фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH key IN (v1, v2, v3)
    IF key > filter
        MATCH
```

## <a name="numberlessthanorequals"></a>NumberGreaterThanOrEquals;
Оператор Нумберлесссанорекуалс вычисляет значение true, если значение **ключа** **меньше или равно** указанному значению **фильтра** . В следующем примере проверяется, является ли значение `counter` атрибута в `data` разделе меньше или равным 100. 

```json
"advancedFilters": [{
    "operatorType": "NumberLessThanOrEquals",
    "key": "data.counter",
    "value": 100
}]
```

Если ключ является массивом, все значения в массиве проверяются по значению фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH key IN (v1, v2, v3)
    IF key <= filter
        MATCH
```

## <a name="numbergreaterthanorequals"></a>NumberGreaterThanOrEquals;
Оператор Нумбергреатерсанорекуалс возвращает значение true, если значение **ключа** **больше или равно** указанному значению **фильтра** . В следующем примере проверяется, является ли значение `counter` атрибута в `data` разделе больше или равным 30. 

```json
"advancedFilters": [{
    "operatorType": "NumberGreaterThanOrEquals",
    "key": "data.counter",
    "value": 30
}]
```

Если ключ является массивом, все значения в массиве проверяются по значению фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH key IN (v1, v2, v3)
    IF key >= filter
        MATCH
```

## <a name="numberinrange"></a>нумберинранже
Оператор Нумберинранже возвращает значение true, если значение **ключа** находится в одном из указанных **диапазонов фильтров**. В следующем примере проверяется, находится ли значение `key1` атрибута в `data` разделе в одном из двух диапазонов: 3,14159-999,95, 3000-4000. 

```json
{
    "operatorType": "NumberInRange",
    "key": "data.key1",
    "values": [[3.14159, 999.95], [3000, 4000]]
}
```

`values`Свойство является массивом диапазонов. В предыдущем примере это массив из двух диапазонов. Ниже приведен пример массива с одним диапазоном для проверки. 

**Массив с одним диапазоном:** 
```json
{
    "operatorType": "NumberInRange",
    "key": "data.key1",
    "values": [[3000, 4000]]
}
```

Если ключ является массивом, все значения в массиве проверяются по массиву значений фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` и фильтр: массив диапазонов. В этом псевдокоде `a` и `b` являются низкими и большими значениями каждого диапазона в массиве. Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH (a,b) IN filter.Values
    FOR_EACH key IN (v1, v2, v3)
       IF key >= a AND key <= b
           MATCH
```


## <a name="numbernotinrange"></a>нумбернотинранже
Оператор Нумбернотинранже возвращает значение true, если значение **ключа** **не** входит ни в один из указанных **диапазонов фильтров**. В следующем примере проверяется, находится ли значение `key1` атрибута в `data` разделе в одном из двух диапазонов: 3,14159-999,95, 3000-4000. Если это так, оператор возвращает значение false. 

```json
{
    "operatorType": "NumberNotInRange",
    "key": "data.key1",
    "values": [[3.14159, 999.95], [3000, 4000]]
}
```
`values`Свойство является массивом диапазонов. В предыдущем примере это массив из двух диапазонов. Ниже приведен пример массива с одним диапазоном для проверки.

**Массив с одним диапазоном:** 
```json
{
    "operatorType": "NumberNotInRange",
    "key": "data.key1",
    "values": [[3000, 4000]]
}
```

Если ключ является массивом, все значения в массиве проверяются по массиву значений фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` и фильтр: массив диапазонов. В этом псевдокоде `a` и `b` являются низкими и большими значениями каждого диапазона в массиве. Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH (a,b) IN filter.Values
    FOR_EACH key IN (v1, v2, v3)
        IF key >= a AND key <= b
            FAIL_MATCH
```


Ниже приведен допустимый оператор для **логических** значений. 

## <a name="boolequals"></a>BoolEquals
Оператор Булекуалс возвращает значение true, если значение **ключа** является указанным **фильтром** логических значений. В следующем примере проверяется, имеет ли значение `isEnabled` атрибута в `data` разделе `true` . 

```json
"advancedFilters": [{
    "operatorType": "BoolEquals",
    "key": "data.isEnabled",
    "value": true
}]
```

Если ключ является массивом, все значения в массиве проверяются по логическому значению фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH key IN (v1, v2, v3)
    IF filter == key
        MATCH
```

Ниже приведены доступные операторы для **строк** .

## <a name="stringcontains"></a>StringContains;
**Стрингконтаинс** возвращает значение true, если значение **ключа** **содержит** любые из указанных значений **фильтра** (в виде подстрок). В следующем примере проверяется, содержит ли значение `key1` атрибута в `data` разделе одну из указанных подстрок: `microsoft` или `azure` . Например, `azure data factory` `azure` в нем есть. 

```json
"advancedFilters": [{
    "operatorType": "StringContains",
    "key": "data.key1",
    "values": [
        "microsoft", 
        "azure"
    ]
}]
```

Если ключ является массивом, все значения в массиве проверяются по массиву значений фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` и фильтр: `[a,b,c]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF key CONTAINS filter
            MATCH
```

## <a name="stringnotcontains"></a>стрингнотконтаинс
Оператор **стрингнотконтаинс** возвращает значение true, если **ключ** не **содержит** указанных значений **фильтров** в виде подстрок. Если ключ содержит одно из указанных значений в виде подстроки, оператор принимает значение false. В следующем примере оператор возвращает значение true только в том случае, если значение `key1` атрибута в `data` разделе не содержит `contoso` и `fabrikam` как подстроки. 

```json
"advancedFilters": [{
    "operatorType": "StringNotContains",
    "key": "data.key1",
    "values": [
        "contoso", 
        "fabrikam"
    ]
}]
```

Если ключ является массивом, все значения в массиве проверяются по массиву значений фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` и фильтр: `[a,b,c]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF key CONTAINS filter
            FAIL_MATCH
```
Текущее ограничение этого оператора см. в разделе [ограничения](#limitations) .

## <a name="stringbeginswith"></a>StringBeginsWith;
Оператор **стрингбегинсвис** возвращает значение true, если значение **ключа** **начинается с** любого из указанных значений **фильтра** . В следующем примере проверяется, начинается ли значение `key1` атрибута в `data` разделе `event` или `grid` . Например, `event hubs` начинается с `event` .  

```json
"advancedFilters": [{
    "operatorType": "StringBeginsWith",
    "key": "data.key1",
    "values": [
        "event", 
        "message"
    ]
}]
```

Если ключ является массивом, все значения в массиве проверяются по массиву значений фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` и фильтр: `[a,b,c]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF key BEGINS_WITH filter
            MATCH
```

## <a name="stringnotbeginswith"></a>стрингнотбегинсвис
Оператор **стрингнотбегинсвис** возвращает значение true, если значение **ключа** **не начинается с** какого либо из указанных значений **фильтра** . В следующем примере проверяется, `key1` не начинается ли значение атрибута в `data` разделе `event` или `message` .

```json
"advancedFilters": [{
    "operatorType": "StringNotBeginsWith",
    "key": "data.key1",
    "values": [
        "event", 
        "message"
    ]
}]
```

Если ключ является массивом, все значения в массиве проверяются по массиву значений фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` и фильтр: `[a,b,c]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF key BEGINS_WITH filter
            FAIL_MATCH
```

## <a name="stringendswith"></a>StringEndsWith;
Оператор **стринжендсвис** возвращает значение true, если значение **ключа** **заканчивается** одним из указанных значений **фильтра** . В следующем примере проверяется, заканчивается ли значение `key1` атрибута в `data` разделе на `jpg` или `jpeg` `png` . Например, `eventgrid.png` заканчивается на `png` .


```json
"advancedFilters": [{
    "operatorType": "StringEndsWith",
    "key": "data.key1",
    "values": [
        "jpg", 
        "jpeg", 
        "png"
    ]
}]
```

Если ключ является массивом, все значения в массиве проверяются по массиву значений фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` и фильтр: `[a,b,c]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF key ENDS_WITH filter
            MATCH
```

## <a name="stringnotendswith"></a>стрингнотендсвис
Оператор **стрингнотендсвис** возвращает значение true, если значение **ключа** **не заканчивается** ни одним из указанных значений **фильтра** . В следующем примере проверяется, `key1` не заканчивается ли значение атрибута в `data` разделе `jpg` или `jpeg` `png` . 


```json
"advancedFilters": [{
    "operatorType": "StringNotEndsWith",
    "key": "data.key1",
    "values": [
        "jpg", 
        "jpeg", 
        "png"
    ]
}]
```

Если ключ является массивом, все значения в массиве проверяются по массиву значений фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` и фильтр: `[a,b,c]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF key ENDS_WITH filter
            FAIL_MATCH
```

## <a name="stringin"></a>StringIn;
Оператор **String** проверяет, **совпадает** ли значение **ключа** с одним из указанных значений **фильтра** . В следующем примере проверяется, является ли значение `key1` атрибута в `data` разделе значением `exact` или `string` или `matches` . 

```json
"advancedFilters": [{
    "operatorType": "StringIn",
    "key": "data.key1",
    "values": [
        "contoso", 
        "fabrikam", 
        "factory"
    ]
}]
```

Если ключ является массивом, все значения в массиве проверяются по массиву значений фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` и фильтр: `[a,b,c]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF filter == key
            MATCH
```

## <a name="stringnotin"></a>StringNotIn.
Оператор **стрингнотин** проверяет, **не совпадает** ли значение **ключа** ни с одним из указанных значений **фильтра** . В следующем примере проверяется, `key1` не является ли значение атрибута в `data` разделе значением `aws` и `bridge` . 

```json
"advancedFilters": [{
    "operatorType": "StringNotIn",
    "key": "data.key1",
    "values": [
        "aws", 
        "bridge"
    ]
}]
```

Если ключ является массивом, все значения в массиве проверяются по массиву значений фильтра. Вот пример псевдокода с ключом: `[v1, v2, v3]` и фильтр: `[a,b,c]` . Все ключевые значения с типами данных, которые не соответствуют типу данных фильтра, игнорируются.

```
FOR_EACH filter IN (a, b, c)
    FOR_EACH key IN (v1, v2, v3)
        IF filter == key
            FAIL_MATCH
```


Все сравнения строк не учитывают регистр.

> [!NOTE]
> Если в JSON события не содержится ключ расширенного фильтра, Filter еваулатед, как **несоответствие** для следующих операторов: Нумбергреатерсан, Нумбергреатерсанорекуалс, Нумберлесссан, Нумберлесссанорекуалс, Numbers, BoolEquals, StringContains, StringNotContains, StringBeginsWith, StringNotBeginsWith, StringEndsWith, StringNotEndsWith, String.
> 
>Фильтр еваулатед **в соответствии со** следующими операторами: Нумбернотин, стрингнотин.


## <a name="isnullorundefined"></a>иснуллорундефинед
Оператор Иснуллорундефинед возвращает значение true, если значение ключа равно NULL или не определено. 

```json
{
    "operatorType": "IsNullOrUndefined",
    "key": "data.key1"
}
```

В следующем примере отсутствует Key1, поэтому оператор возвращает значение true. 

```json
{ 
    "data": 
    { 
        "key2": 5 
    } 
}
```

В следующем примере параметр key1 имеет значение null, поэтому оператор возвращает значение true.

```json
{
    "data": 
    { 
        "key1": null
    }
}
```

Если в этих примерах параметр key1 имеет любое другое значение, оператор возвращает значение false. 

## <a name="isnotnull"></a>IsNotNull
Оператор IsNotNull возвращает значение true, если значение ключа не равно NULL или не определено. 

```json
{
    "operatorType": "IsNotNull",
    "key": "data.key1"
}
```

## <a name="or-and-and"></a>OR и и
Если указать один фильтр с несколькими значениями, то выполняется операция **или** , поэтому значением ключевого поля должно быть одно из следующих значений. Пример:

```json
"advancedFilters": [
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/microsoft.devtestlab/",
            "/providers/Microsoft.Compute/virtualMachines/"
        ]
    }
]
```

Если задано несколько различных фильтров, операция **и** выполняется, поэтому должно выполняться каждое условие фильтра. Приведем пример: 

```json
"advancedFilters": [
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/microsoft.devtestlab/"
        ]
    },
    {
        "operatorType": "StringContains",
        "key": "Subject",
        "values": [
            "/providers/Microsoft.Compute/virtualMachines/"
        ]
    }
]
```

## <a name="cloudevents"></a>CloudEvents 
Для событий в **схеме клаудевентс** используйте следующие значения для ключа: `eventid` ,, `source` `eventtype` , `eventtypeversion` или данные события (например, `data.key1` ). 

Вы также можете использовать [атрибуты контекста расширения в клаудевентс 1,0](https://github.com/cloudevents/spec/blob/v1.0.1/spec.md#extension-context-attributes). В следующем примере `comexampleextension1` и `comexampleothervalue` являются атрибутами контекста расширения. 

```json
{
    "specversion" : "1.0",
    "type" : "com.example.someevent",
    "source" : "/mycontext",
    "id" : "C234-1234-1234",
    "time" : "2018-04-05T17:31:00Z",
    "subject": null,
    "comexampleextension1" : "value",
    "comexampleothervalue" : 5,
    "datacontenttype" : "application/json",
    "data" : {
        "appinfoA" : "abc",
        "appinfoB" : 123,
        "appinfoC" : true
    }
}
```

Ниже приведен пример использования атрибута контекста расширения в фильтре.

```json
"advancedFilters": [{
    "operatorType": "StringBeginsWith",
    "key": "comexampleothervalue",
    "values": [
        "5", 
        "1"
    ]
}]
```


## <a name="limitations"></a>Ограничения

Расширенная фильтрация имеет такие ограничения:

* 5 дополнительных фильтров и 25 значений фильтров по всем подпискам на одну подписку на сетку событий
* 512 знаков для значения строки;
* пять значений для операторов **in** и **not in**;
* В `StringNotContains` настоящее время оператор недоступен на портале.
* Ключи с символом **`.` (точкой)** . Пример: `http://schemas.microsoft.com/claims/authnclassreference` или `john.doe@contoso.com`. В настоящее время escape-символы в ключах не поддерживаются. 

Один ключ можно использовать в нескольких фильтрах.





## <a name="next-steps"></a>Следующие шаги

* См. дополнительные сведения о фильтрации событий для Сетки событий с помощью [PowerShell и Azure CLI](how-to-filter-events.md).
* Сведения о том, как быстро приступить к использованию службы "Сетка событий", см. в разделе [Создание и перенаправление пользовательского события со службой "Сетка событий Azure"](custom-event-quickstart.md).

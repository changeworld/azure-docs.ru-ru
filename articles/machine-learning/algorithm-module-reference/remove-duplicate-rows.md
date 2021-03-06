---
title: 'Удалить дублирующиеся строки: ссылка на модуль'
titleSuffix: Azure Machine Learning
description: Узнайте, как использовать модуль удаления повторяющихся строк в Машинное обучение Azure, чтобы удалить потенциальные дубликаты из набора данных.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: bf35d08128aa8a3e8f545ed7184866694219f2cb
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "90905211"
---
# <a name="remove-duplicate-rows-module"></a>Удалить модуль повторяющихся строк

В этой статье описывается модуль в конструкторе Машинное обучение Azure.

Используйте этот модуль, чтобы удалить потенциальные дубликаты из набора данных.

Например, предположим, что данные выглядят следующим образом и представляют несколько записей для пациентов. 

| патиентид | Инициалы| пол;|возраст;|Допуск|
|----|----|----|----|----|
|1|ф.м.| M| 53| Январь|
|2| ф.а.м.| M| 53| Январь|
|3| ф.а.м.| M| 24| Январь|
|3| ф.м.| M| 24| Февраль|
|4| ф.м.| M| 23| Февраль|
| | ф.м.| M| 23| |
|5| ф.а.м.| M| 53| |
|6| ф.а.м.| M| не число| |
|7| ф.а.м.| M| не число| |

Очевидно, что в этом примере есть несколько столбцов с потенциально повторяющимися данными. Действительно ли они дублируются, зависит от ваших знаний о данных. 

+ Например, может быть известно, что многие пациентов имеют одинаковое имя. Вы не исключите дубликаты с помощью столбцов имени, а только столбца **идентификаторов** . Таким образом отфильтровываются только строки с повторяющимися значениями идентификатора независимо от того, имеют ли пациентов имена.

+ Кроме того, можно разрешить дублирование в поле ID и использовать другое сочетание файлов для поиска уникальных записей, таких как имя, фамилия, возраст и пол.  

Чтобы задать критерии для того, является ли строка дубликатом, укажите один столбец или набор столбцов для использования в качестве **ключей**. Две строки считаются повторяющимися, только если значения во **всех** ключевых столбцах равны. Если в любой строке отсутствуют значения для **ключей**, они не будут считаться повторяющимися строками. Например, если пол и Age заданы в качестве ключей в приведенной выше таблице, строки 6 и 7 не будут содержать повторяющихся строк, в которых отсутствует значение Age.

При запуске модуля он создает набор данных-кандидат и возвращает набор строк, не имеющий дубликатов в наборе указанных столбцов.

> [!IMPORTANT]
> Исходный набор данных не изменяется; Этот модуль создает новый набор данных, который фильтруется для исключения дубликатов на основе указанных критериев.

## <a name="how-to-use-remove-duplicate-rows"></a>Использование удаления повторяющихся строк

1. Добавьте модуль в конвейер. Модуль **Удаление повторяющихся строк** можно найти в разделе **Преобразование данных**, **Обработка**.  

2. Подключите набор данных, который необходимо проверить на наличие повторяющихся строк.

3. В области **Свойства** в разделе **выражение фильтра выбора ключевых столбцов** щелкните **запустить селектор столбцов**, чтобы выбрать столбцы, используемые при обнаружении дубликатов.

    В этом контексте **ключ** не означает уникальности идентификатора. Все столбцы, выбранные с помощью селектора столбцов, назначаются в качестве **ключевых столбцов**. Все невыбранные столбцы считаются неключевыми столбцами. Сочетание столбцов, выбранных в качестве ключей, определяет уникальность записей. (Представьте, что это инструкция SQL, использующая несколько екуалитиес соединений.)

    Примеры:

    + «Я хочу убедиться, что идентификаторы уникальны: выберите только столбец ИДЕНТИФИКАТОРов.
    + "Я хочу убедиться, что сочетание имени, фамилии и идентификатора уникально": выбрать все три столбца.

4. Установите флажок **хранить первую строку** , чтобы указать, какая строка должна возвращаться при обнаружении дубликатов:

    + Если этот флажок установлен, возвращается первая строка и другие отбрасываются. 
    + Если снять этот флажок, последняя повторяющаяся строка будет сохранена в результатах, а другие будут удалены. 

5. Отправьте конвейер.

6. Чтобы просмотреть результаты, щелкните правой кнопкой мыши модуль и выберите **визуализировать**. 

> [!TIP]
> Если результаты сложно понять или вы хотите исключить некоторые столбцы из рассмотрения, можно удалить столбцы с помощью модуля [Выбор столбцов в наборе данных](./select-columns-in-dataset.md) .

## <a name="next-steps"></a>Дальнейшие действия

Ознакомьтесь с [набором доступных модулей](module-reference.md) в службе Машинного обучения Azure. 
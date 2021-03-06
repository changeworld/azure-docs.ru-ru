---
title: Фильтрация данных. Пользовательский переводчик
titleSuffix: Azure Cognitive Services
description: При отправке документов, которые будут использоваться для обучения пользовательской системы, документы проходят ряд этапов по обработке и фильтрации для подготовки к обучению.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 08/17/2020
ms.author: lajanuar
ms.topic: conceptual
ms.openlocfilehash: 53dea20e356f735a521dec8c22edf8cb2aa7122d
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "98895872"
---
# <a name="data-filtering"></a>Фильтрация данных

При отправке документов, которые будут использоваться для обучения пользовательской системы, документы проходят ряд этапов по обработке и фильтрации для подготовки к обучению. Эти шаги описаны ниже. Знания о фильтрации могут помочь вам понять количество предложений, отображаемых в пользовательском трансляторе, а также шаги, которые вы можете предпринять для подготовки документов для обучения с помощью настраиваемого переводчика.

## <a name="sentence-alignment"></a>Выравнивание предложений
Если документ не имеет формат XLIFF, TMX или ALIGN, пользовательский переводчик выравнивает предложения исходного и целевого документов рядом, по предложениям. Настраиваемый переводчик не выполняет выравнивание документов — он соответствует именам документов для поиска соответствующего документа на другом языке. Внутри документа пользовательский переводчик пытается найти соответствующие предложения на другом языке. Он использует разметку документа, например внедренные теги HTML, чтобы упростить выравнивание.  

При обнаружении большого расхождения между числом предложений в исходном и целевом документах документ может не быть параллельным в первую очередь или по другим причинам не удалось выполнить согласование. Если в паре документов разница в количестве предложений составляет более 10 %, проверьте документы и убедитесь, что они параллельны. Пользовательский переводчик отображает предупреждение рядом с документом, если количество предложений подозрительно отличается.  


## <a name="deduplication"></a>Дедупликация
Пользовательский переводчик удаляет предложения, присутствующие в тестовых документах, из данных обучения.Удаление происходит динамически на шаге обучения, а не обработки данных. Пользовательский переводчик сообщает о количестве предложений в обзоре проекта, прежде чем удалить такие предложения.  

## <a name="length-filter"></a>Фильтр длины
* Удалить предложения только из одного слова с обеих сторон.
* Удалить предложения более чем из 100 слов с обеих сторон.Кроме китайского, японского и корейского.
* Удалить предложения, где менее 3 знаков. Кроме китайского, японского и корейского.
* Удалить предложения, где более 2000 знаков (китайский, японский, корейский).
* Удалить предложения, где буквы занимают менее 1 %.
* Удалить записи словаря, содержащие более 50 слов.

## <a name="white-space"></a>Пробел
* Заменить любую последовательность символов пробела, включая табуляцию и переход на новую строку, на один пробел.
* Удалить начальные и конечные пробелы в предложении.

## <a name="sentence-end-punctuation"></a>Конечный знак препинания в предложении
Заменить несколько знаков препинания в конце предложения на один знак.  

## <a name="japanese-character-normalization"></a>Нормализация японских символов
Преобразует полноширинные буквы и цифры в символы половинной ширины.

## <a name="unescaped-xml-tags"></a>Неэкранированные теги XML
Фильтрация преобразует неэкранированные теги в экранированные:
* `&lt;` заменяется на `&amp;lt;`.
* `&gt;` заменяется на `&amp;gt;`.
* `&amp;` заменяется на `&amp;amp;`.

## <a name="invalid-characters"></a>Недопустимые знаки
Пользовательский переводчик удаляет предложения, содержащие символ Юникода U+FFFD. Символ U+FFFD указывает на сбой преобразования кодировки.

## <a name="next-steps"></a>Дальнейшие действия

- [Обучение модели](how-to-train-model.md) в пользовательском переводчике.

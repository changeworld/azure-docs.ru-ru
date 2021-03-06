---
title: Рекомендации по формату импорта документов — QnA Maker
description: Используйте эти рекомендации для импорта документов, чтобы получить лучшие результаты для вашего содержимого.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 04/06/2020
ms.openlocfilehash: 15ff2ec296cedc37b086a9ca2d0825fb20b4f05a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "99549547"
---
# <a name="format-guidelines-for-imported-documents-and-urls"></a>Формат рекомендаций для импортированных документов и URL-адресов

Ознакомьтесь с рекомендациями по форматированию, чтобы получить лучшие результаты для вашего содержимого.

## <a name="formatting-considerations"></a>Рекомендации по форматированию

После импорта файла или URL-адреса QnA Maker преобразует и сохраняет содержимое в [формате Markdown](https://en.wikipedia.org/wiki/Markdown). В процессе преобразования в текст добавляются новые строки, например `\n\n` . Знание формата Markdown поможет вам разобраться с преобразованным содержимым и управлять содержанием базы знаний.

Если вы добавляете или изменяете содержимое непосредственно в своей базе знаний, используйте **Форматирование Markdown** для создания форматированного текста или изменения содержимого формата Markdown, которое уже находится в ответе. QnA Maker поддерживает большую часть формата Markdown, чтобы обеспечить широкие возможности работы с текстом в содержимом. Однако клиентское приложение, например Bot чата, может не поддерживать одинаковый набор форматов Markdown. Важно протестировать отображение ответов клиентского приложения.

Полный список [типов содержимого и примеры](./concepts/data-sources-and-content.md#content-types-of-documents-you-can-add-to-a-knowledge-base)см. здесь.

## <a name="basic-document-formatting"></a>Базовое форматирование документов

QnA Maker определяет разделы и подразделы и связи в файле, используя визуальные подсказки, такие как:

* размер шрифта
* стиль шрифта
* Нумерация
* цвета

> [!NOTE]
> В настоящее время Извлечение изображений из отправленных документов не поддерживается.

### <a name="product-manuals"></a>Руководства по продукции

Руководством обычно называются справочные материалы, входящие в комплект поставки продукта. Они помогают пользователям устанавливать, использовать, обслуживать продукт и устранять возможные неполадки. Когда QnA Maker обрабатывает руководство, он извлекает заголовки и подзаголовки в качестве вопросов, а последующее содержимое — в качестве ответов. Пример вы можете найти [здесь](https://download.microsoft.com/download/2/9/B/29B20383-302C-4517-A006-B0186F04BE28/surface-pro-4-user-guide-EN.pdf).

Ниже приведен пример руководства со страницей индексов и иерархическим содержимым

 ![Пример руководства по продукту для базы знаний](./media/qnamaker-concepts-datasources/product-manual.png)

> [!NOTE]
> Извлечение лучше всего работает с руководствами с отдельным содержанием и (или) страницей индексов и хорошей иерархической структурой заголовков.

### <a name="brochures-guidelines-papers-and-other-files"></a>Брошюры, рекомендации, документы и другие файлы

Многие другие типы документов также могут быть обработаны для создания пар вопросов и ответов при условии, что они имеют четкую структуру и макет. К ним относятся буклеты, руководства, отчеты, технические документы, научные документы, политики, книги и т. д. См. пример [здесь](https://qnamakerstore.blob.core.windows.net/qnamakerdata/docs/Manage%20Azure%20Blob%20Storage.docx).

Ниже приведен пример слабоструктурированного документа без индекса:

 ![Слабоструктурированный документ хранилища BLOB-объектов](./media/qnamaker-concepts-datasources/semi-structured-doc.png)

### <a name="structured-qna-document"></a>Документ структурированных вопросов и ответов

Формат структурированных "вопросов — ответов" в DOC-файлах представлен в чередующемся формате — по одному вопросу в строке, за которым следует ответ в следующей строке, как показано ниже.

```text
Question1

Answer1

Question2

Answer2
```

Ниже приведен пример структурированного документа Word вопросов и ответов.

 ![Пример документа структурированных вопросов и ответов для базы знаний](./media/qnamaker-concepts-datasources/structured-qna-doc.png)

### <a name="structured-txt-tsv-and-xls-files"></a>Структурированные *TXT*, *TSV* и *XLS* файлы

Вопросы и ответы в форме структурированных файлов в формате *TXT*, *TSV* или *XLS* также могут быть загружены в QnA Maker для создания или расширения базы знаний.  Они могут быть либо обычным текстом, либо иметь содержимое в формате RTF или HTML.

| Вопрос  | Ответ  | Метаданные (1 ключ: 1 значение) |
|-----------|---------|-------------------------|
| Question1 | Answer1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 |      `Key:Value`           |

Все остальные столбцы в исходном файле игнорируются.

#### <a name="example-of-structured-excel-file"></a>Пример структурированного файла Excel

Ниже приведен пример структурированных вопросов и ответов файла формата *XLS* с содержимым HTML

 ![Пример структурированных вопросов и ответов в формате Excel для базы знаний](./media/qnamaker-concepts-datasources/structured-qna-xls.png)

#### <a name="example-of-alternate-questions-for-single-answer-in-excel-file"></a>Пример альтернативных вопросов для одного ответа в файле Excel

Ниже приведен пример структурированного файла QnA *. xls* с несколькими альтернативными вопросами для одного ответа:

 ![Пример альтернативных вопросов для одного ответа в файле Excel](./media/qnamaker-concepts-datasources/xls-alternate-question-example.png)

После импорта файла пара "вопрос-ответ" находится в базе знаний, как показано ниже.

 ![Снимок экрана с альтернативными вопросами для одного ответа, импортированного в базу знаний](./media/qnamaker-concepts-datasources/xls-alternate-question-example-after-import.png)

### <a name="structured-data-format-through-import"></a>Загрузка данных в структурированном формате через импорт

При импорте базы знаний содержимое существующей базы знаний полностью заменяется. Для импорта нужно предоставить структурированный TSV-файл с информацией об источнике данных. Эти сведения помогают QnA Maker сгруппировать пары "вопрос — ответ" и сопоставить их с конкретным источником данных.

| Вопрос  | Ответ  | Источник| Метаданные (1 ключ: 1 значение) |
|-----------|---------|----|---------------------|
| Question1 | Answer1 | Url1 | <code>Key1:Value1 &#124; Key2:Value2</code> |
| Question2 | Answer2 | Редактирование|    `Key:Value`       |

<a href="#formatting-considerations"></a>

### <a name="multi-turn-document-formatting"></a>Множественная переключка форматирования документов

* Используйте заголовки и подзаголовки для обозначения иерархии. Например, можно заметить, что в качестве родительского QnA и H2 вы можете обозначить QnA, который должен быть сделан запросом. Используйте Малый размер заголовка для обозначения последующей иерархии. Не используйте стиль, цвет или какой-либо другой механизм для включения структуры в документ, QnA Maker не извлекать многострочные запросы.
* Первый символ заголовка должен быть прописным.
* Не завершайте заголовок вопросительным знаком, `?` .

**Образцы документов**:<br>[Surface Pro (DOCX)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx)<br>[Преимущества Contoso (DOCX)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.docx)<br>[Преимущества Contoso (PDF)](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/Multiturn-ContosoBenefits.pdf)

## <a name="faq-urls"></a>URL-адреса вопросов и ответов

QnA Maker может поддерживать веб-страницы вопросов и ответов в трех разных формах:

* Простые страницы вопросов и ответов
* Страницы вопросов и ответов со ссылками
* Страницы вопросов и ответов с домашней страницей разделов

### <a name="plain-faq-pages"></a>Простые страницы вопросов и ответов

Это наиболее распространенный тип страниц с вопросами и ответами, в котором ответы содержатся на той же странице, сразу после каждого вопроса.

Ниже показан пример простой страницы вопросов и ответов.

![Пример простой страницы с вопросами и ответами для базы знаний](./media/qnamaker-concepts-datasources/plain-faq.png)


### <a name="faq-pages-with-links"></a>Страницы вопросов и ответов со ссылками

В этом типе страниц с вопросами и ответами все вопросы собраны вместе и содержат ссылки на ответы, расположенные либо в разных разделах на той же странице, либо на разных страницах.

Ниже показан пример страницы вопросов и ответов со ссылками на разделы, которые расположенные на той же странице.

 ![Ссылка на раздел на странице с вопросами и ответами для базы знаний](./media/qnamaker-concepts-datasources/sectionlink-faq.png)


### <a name="parent-topics-page-links-to-child-answers-pages"></a>Страницы родительских разделов ссылки на дочерние страницы ответов

Этот тип часто задаваемых вопросов содержит страницу разделов, где каждый раздел связан с соответствующим набором вопросов и ответов на другой странице. QnA Maker обходит все связанные страницы, чтобы извлечь соответствующие вопросы & ответы.

Ниже приведен пример страницы разделов со ссылками на разделы часто задаваемых вопросов на разных страницах.

 ![Прямая ссылка на страницу с вопросами и ответами для базы знаний](./media/qnamaker-concepts-datasources/topics-faq.png)

### <a name="support-urls"></a>URL-адреса для поддержки

QnA Maker может обрабатывать полуструктурированные веб-страницы поддержки, такие как статьи в Интернете, описывающие способы выполнения определенной задачи, способы диагностики и устранения проблемы и рекомендации для процесса. Извлечение лучше всего работает с документами, которые имеют четкую структуру с иерархической структурой заголовков.

> [!NOTE]
> Извлечение для статьи о поддержке — это новая функция, которая находится на ранних стадиях. Она лучше всего подходит для простых страниц, которые хорошо структурированы и не содержат сложные колонтитулы.

![QnA Maker поддерживает извлечение из частично структурированных веб-страниц, где есть четкая структура и иерархические заголовки.](./media/qnamaker-concepts-datasources/support-web-pages-with-heirarchical-structure.png)

## <a name="import-and-export-knowledge-base"></a>Импорт и экспорт базы знаний

**Файлы TSV и XLS**, полученные из экспортированных баз знаний, можно использовать только при импорте файлов со страницы **параметров** на портале QnA Maker. Их нельзя использовать в качестве источников данных во время создания базы знаний или с помощью функции **+ Добавить файл** или **+ Добавить URL-адрес** на странице **Параметры** . 

При импорте базы знаний с помощью этих **файлов TSV и XLS** пары QnA добавляются в Редакционный источник, а не в источники, из которых QnA были извлечены в экспортированной базе знаний. 


## <a name="next-steps"></a>Дальнейшие действия

См. полный список [типов содержимого и примеры](./concepts/data-sources-and-content.md#content-types-of-documents-you-can-add-to-a-knowledge-base)

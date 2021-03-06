---
title: Создание ресурса Cognitive Services на портале Azure
titleSuffix: Azure Cognitive Services
description: Начните работу с Azure Cognitive Services, создав ресурс и оформив подписку на него с помощью портала Azure.
services: cognitive-services
author: aahill
manager: nitinme
keywords: Cognitive Services, когнитивня аналитика, когнитивные решения, службы ИИ
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 03/15/2021
ms.author: aahi
ms.openlocfilehash: af957758918b99dcb44732eb536c0ca031231a7a
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104868228"
---
# <a name="quickstart-create-a-cognitive-services-resource-using-the-azure-portal"></a>Краткое руководство. Создание ресурса Cognitive Services с помощью портала Azure

Используйте это краткое руководство, чтобы начать работу с Azure Cognitive Services. После создания ресурса Cognitive Services на портале Azure вы получите конечную точку и ключ для проверки подлинности приложений.

Azure Cognitive Services — это облачные службы с REST API и пакетами SDK клиентских библиотек, которые помогают разработчикам без опыта работы со средствами искусственного интеллекта (ИИ) и обработки и анализа данных создавать когнитивные интеллектуальные приложения. С помощью Azure Cognitive Services разработчики могут без усилий добавлять в свои приложения когнитивные функции, создавая когнитивные решения, которые могут видеть, слышать, говорить, понимать и даже в некоторой степени размышлять.

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="prerequisites"></a>Предварительные требования

* Действующая подписка Azure ([создайте бесплатную учетную запись](https://azure.microsoft.com/free/cognitive-services/)).

## <a name="create-a-new-azure-cognitive-services-resource"></a>Создание ресурса Azure Cognitive Services

1. Создание ресурса.

### <a name="multi-service-resource"></a>[Ресурс для нескольких служб](#tab/multiservice)

Ресурс для нескольких служб на портале называется **Cognitive Services**. [Создайте ресурс Cognitive Services.](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne)

В настоящее время ресурс для нескольких служб предоставляет доступ к следующим службам Cognitive Services:

* **Зрение**. Компьютерное зрение, Пользовательское визуальное распознавание, Распознаватель документов, Распознавание лиц
* **Речь**. Речь
* **Язык**. Распознавание речи (LUIS), Анализ текста, Переводчик
* **Решение**. Персонализатор, Content Moderator

### <a name="single-service-resource"></a>[Ресурс для одной службы](#tab/singleservice)

Используйте следующие ссылки, чтобы создать ресурс для доступных служб Cognitive Services:

| Зрение                      | Речь                  | Язык                          | Решение             |
|-----------------------------|-------------------------|-----------------------------------|----------------------|
| [Компьютерное зрение](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision)         | [Службы "Речь"](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices)     | [Иммерсивное средство чтения](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesImmersiveReader)              | [Детектор аномалий](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) | 
| [Пользовательское визуальное распознавание](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesCustomVision) |  | [Распознавание речи (LUIS)](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesLUISAllInOne) | [Content Moderator](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) | 
| [Распознавание лиц](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFace)                    |                         | [QnA Maker](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker)                     | [Персонализатор](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)     |
| [Распознаватель документов](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesFormRecognizer)        |                         | [Анализ текста](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)                |  [Помощник по метрикам](https://go.microsoft.com/fwlink/?linkid=2142156)                    |
| | | [Переводчик](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextTranslation) | |

---

2. На странице **Создание** введите приведенные ниже сведения.
<!-- markdownlint-disable MD024 -->

### <a name="multi-service-resource"></a>[Ресурс для нескольких служб](#tab/multiservice)

|сведения о проекте;| Описание   |
|--|--|
| **Подписка** | Выберите одну из доступных подписок Azure. |
| **Группа ресурсов** | Группа ресурсов Azure, которая будет содержать ресурс Cognitive Services. Вы можете создать новую группу или добавить к имеющейся группе. |
| **Регион** | Расположение экземпляра Cognitive Service. Разные расположения могут увеличивать задержку, но не влияют на доступность среды выполнения ресурса. |
| **Имя** | Описательное имя для ресурса Cognitive Services. Например, *MyCognitiveServicesResource*. |
| **Ценовая категория** | Затраты на учетную запись Cognitive Services зависят от выбранных параметров и использования. Дополнительные сведения см. на [странице с ценами](https://azure.microsoft.com/pricing/details/cognitive-services/) API-интерфейсов.

<!--![Multi-service resource creation screen](media/cognitive-services-apis-create-account/resource_create_screen-multi.png)-->
:::image type="content" source="media/cognitive-services-apis-create-account/resource_create_screen-multi.png" alt-text="Экран создания ресурса для нескольких служб":::

Прочтите и примите условия (если применимо), а затем выберите **Просмотр и создание**.

### <a name="single-service-resource"></a>[Ресурс для одной службы](#tab/singleservice)

|сведения о проекте;| Описание   |
|--|--|
| **Подписка** | Выберите одну из доступных подписок Azure. |
| **Группа ресурсов** | Группа ресурсов Azure, которая будет содержать ресурс Cognitive Services. Вы можете создать новую группу или добавить к имеющейся группе. |
| **Регион** | Расположение экземпляра Cognitive Service. Разные расположения могут увеличивать задержку, но не влияют на доступность среды выполнения ресурса. |
| **Имя** | Описательное имя для ресурса Cognitive Services. Например, *MyCognitiveServicesResource*. |
| **Ценовая категория** | Затраты на учетную запись Cognitive Services зависят от выбранных параметров и использования. Дополнительные сведения см. на [странице с ценами](https://azure.microsoft.com/pricing/details/cognitive-services/) API-интерфейсов.

<!--![Single-service resource creation screen](media/cognitive-services-apis-create-account/resource_create_screen.png)-->
:::image type="content" source="media/cognitive-services-apis-create-account/resource_create_screen.png" alt-text="Экран создания ресурса для одной службы":::

Нажмите **Next: Virtual Network** (Далее: виртуальная сеть), выберите тип доступа к сети, который вы хотите разрешить для ресурса, а затем нажмите **Просмотр и создание**.

---

[!INCLUDE [Register Azure resource for subscription](./includes/register-resource-subscription.md)]

## <a name="get-the-keys-for-your-resource"></a>Получение ключей для ресурса

1. После успешного развертывания ресурса щелкните **Перейти к ресурсу** в разделе **Дальнейшие действия**.

    ![Поиск по запросу "Cognitive Services"](media/cognitive-services-apis-create-account/resource-next-steps.png)

2. Откроется область быстрого запуска, где вы можете получить ключ и конечную точку.

    ![Получение ключа и конечной точки](media/cognitive-services-apis-create-account/get-cog-serv-keys.png)

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы хотите очистить и удалить подписку Cognitive Services, вы можете удалить ресурс или группу ресурсов. При удалении группы ресурсов удаляются также все другие ресурсы, содержащиеся в этой группе.

1. На портале Azure разверните меню слева, чтобы открыть меню служб, и выберите **Resource Groups** (Группы ресурсов), чтобы просмотреть список групп ресурсов.
2. Найдите группу ресурсов, содержащую ресурс, который необходимо удалить.
3. Щелкните правой кнопкой мыши список групп ресурсов. Выберите **Удалить группу ресурсов** и подтвердите выбор.

## <a name="see-also"></a>См. также раздел

* Сведения о безопасной работе с Cognitive Services см. в статье **[Проверка подлинности запросов к Azure Cognitive Services](authentication.md)** .
* См. статью **[Общие сведения об Azure Cognitive Services](./what-are-cognitive-services.md)** , в которой можно увидеть список различных категорий в Cognitive Services.
* Список естественных языков, поддерживаемых Cognitive Services, см. в статье **[Поддержка естественного языка](language-support.md)** .
* Сведения об использовании Cognitive Services в локальной среде см. в статье **[Использование Cognitive Services в качестве контейнеров](cognitive-services-container-support.md)** .
* См. статью **[Планирование затрат и управление ими в Cognitive Services](plan-manage-costs.md)** , чтобы получить сведения о том, как оценивать затраты на использование Cognitive Services.

---
title: Реагирование на события служб мультимедиа Azure
description: В этой статье описывается, как подписываться на события служб мультимедиа с помощью службы "Сетка событий Azure".
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 03/17/2021
ms.author: inhenkel
ms.openlocfilehash: 9bed493ca37d21d0c3e5c289bb8c26607975bdc6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104609724"
---
# <a name="handling-event-grid-events"></a>Обработка событий Сетки событий

[!INCLUDE [media services api v3 logo](../includes/v3-hr.md)]

События Служб мультимедиа позволяют приложениям реагировать на различные события (например, событие изменения состояния задания) с помощью современных безсерверных архитектур. При этом не требуется сложный код или дорогостоящие и неэффективные службы опроса. Вместо этого события отправляются через службу [Сетка событий Azure](https://azure.microsoft.com/services/event-grid/) обработчикам событий, таким как [Функции Azure](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), или даже в веб-перехватчик. При этом вы оплачиваете только то, что используете. Дополнительные сведения о ценах см. на странице [с ценами на службу"Сетка событий Azure"](https://azure.microsoft.com/pricing/details/event-grid/).

Доступность событий Служб мультимедиа привязывается к [доступности](../../../event-grid/overview.md) службы "Сетка событий". Эта функция скоро станет доступной в других регионах вместе со службой "Сетка событий".  

## <a name="media-services-events-and-schemas"></a>События и схемы служб мультимедиа

Сетка событий использует [подписки на события](../../../event-grid/concepts.md#event-subscriptions) для маршрутизации сообщений о событиях подписчикам. События Служб мультимедиа содержат все сведения, необходимые для реагирования на изменения в данных. Событие Служб мультимедиа можно определить, так как свойство eventType начинается с Microsoft.Media.

Дополнительные сведения см. в статье [Схемы службы "Сетка событий Azure" для событий Служб мультимедиа](../media-services-event-schemas.md).

## <a name="practices-for-consuming-events"></a>Рекомендации по потреблению событий

Приложения, которые обрабатывают события Служб мультимедиа, должны следовать нескольким рекомендациям:

* Так как в один обработчик событий можно настроить маршрутизацию событий из нескольких подписок, не следует предполагать, что события поступают из определенного источника. Но необходимо проверять тему сообщения, чтобы убедиться, что оно поступает из ожидаемой учетной записи хранения.
* Подобным образом убедитесь, что вы готовы к обработке этого типа событий (eventType), а также не следует предполагать, что все получаемые события будут иметь ожидаемый тип.
* Игнорируйте поля, которые вам непонятны.  Такой подход поможет обеспечить устойчивость к новым функциям, которые могут быть добавлены в будущем.
* С помощью префикса и суффикса subject события можно ограничить до конкретного.

> [!NOTE]
> События подчиняются Соглашение об уровне обслуживанияу "Сетка событий" [(SLA)](https://azure.microsoft.com/support/legal/sla/event-grid/v1_0/). Если вы хотите получать уведомления о событиях с помощью интерфейсов API, см. примеры использования событий с [пакетом](https://github.com/Azure-Samples/media-services-v3-dotnet) SDK для .NET или [Java](https://github.com/Azure-Samples/media-services-v3-java).

## <a name="next-steps"></a>Дальнейшие действия

* [Мониторинг событий — портал](../monitor-events-portal-how-to.md)
* [Мониторинг событий (CLI)](../job-state-events-cli-how-to.md)

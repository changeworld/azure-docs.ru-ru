---
title: Рекомендации по программе для поставщиков облачных решений
description: Для партнеров CSP делегированное управление ресурсами Azure помогает улучшить безопасность и контроль за счет предоставления детализированных разрешений.
ms.date: 03/12/2021
ms.topic: conceptual
ms.openlocfilehash: 8736cf913739f2bd16fb519aed98fd336f6876a5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "103419394"
---
# <a name="azure-lighthouse-and-the-cloud-solution-provider-program"></a>Azure Lighthouse и программа поставщиков облачных решений

Если вы имеете статус [CSP (поставщика облачных решений)](/partner-center/csp-overview), вам доступны подписки Azure, созданные для клиентов в программе CSP, с помощью функции [Администрирование от имени (AOBO)](https://channel9.msdn.com/Series/cspdev/Module-11-Admin-On-Behalf-Of-AOBO). Это позволяет непосредственно обслуживать и настраивать подписки клиентов, а также управлять ими.

С помощью [Azure Lighthouse](../overview.md) вы можете использовать делегированное управление ресурсами в Azure вместе с AOBO. Это помогает улучшить безопасность и не допускать излишние попытки доступа с помощью более детализированных разрешений для пользователей. Кроме того, повышается эффективность и масштабируемость, так как пользователи могут работать с несколькими клиентскими подписками, используя одно имя входа в клиенте.

> [!TIP]
> Чтобы защитить ресурсы клиентов, обязательно ознакомьтесь с [рекомендациями по обеспечению безопасности](recommended-security-practices.md) и [требованиями к безопасности партнеров](/partner-center/partner-security-requirements).

## <a name="administer-on-behalf-of-aobo"></a>Администрирование от имени (AOBO)

С AOBO любой пользователь с ролью [агента администратора](/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) в своем арендаторе будет иметь доступ AOBO к подпискам Azure, которые создаются с помощью программы CSP. Пользователи, которым нужен доступ к подпискам клиентов, должны быть членами этой группы. AOBO не позволяет гибко создавать отдельные группы, которые работают с разными клиентами, или разрешать разные роли для групп или пользователей.

![Схема, показывающая управление клиентами с помощью АОBO.](../media/csp-1.jpg)

## <a name="azure-delegated-resource-management"></a>Делегированное управление ресурсами Azure

Используя Azure Lighthouse, можно назначать разные группы разным клиентам или ролям, как продемонстрировано на схеме ниже. Так как у пользователей будет соответствующий уровень доступа благодаря делегированному управлению ресурсами Azure, можно будет сократить число пользователей с ролью агента-администратора (т. е. имеющих полный доступ AOBO). Это поможет повысить безопасность, предоставляя только необходимый уровень доступа к ресурсам клиента. Кроме того, это повышает гибкость управления несколькими клиентами в соответствующем масштабе.

Чтобы подключить подписку, созданную с помощью программы CSP, выполните действия, описанные в разделе [Подключение подписки к Azure Lighthouse](../how-to/onboard-customer.md). Любой пользователь, имеющий роль агента администратора в своем арендаторе, может выполнить это подключение.

![Схема, изображающая управление арендаторами с помощью делегированного управления ресурсами AOBO и Azure.](../media/csp-2.jpg)

> [!TIP]
> [Предложения управляемых служб](managed-services-offers.md) с частными планами не поддерживаются с подписками, установленными через торгового посредника по программе поставщика облачных решений (CSP). Вы можете подключить эти подписки к Azure Lighthouse с [помощью шаблонов Azure Resource Manager](../how-to/onboard-customer.md).

> [!NOTE]
> Страница [**Мои клиенты** на портале Azure](../how-to/view-manage-customers.md) теперь включает в себя раздел **Поставщик облачных решений (предварительная версия)** , где можно просмотреть сведения о выставлении счетов и ресурсы для клиентов CSP, которые [заключили Клиентское соглашение Майкрософт (MCA)](/partner-center/confirm-customer-agreement) и на которых распространяется [план Azure](/partner-center/azure-plan-get-started). Чтобы узнать больше, ознакомьтесь с разделом [Начало работы с учетной записью выставления счетов Соглашения с партнером Майкрософт](../../cost-management-billing/understand/mpa-overview.md).
>
> Клиенты CSP могут появиться в этом разделе независимо от того, были ли они подключены к Azure Lighthouse. Если были, то они также будут отображаться в разделе **Клиенты**, как описано в статье [Просмотр клиентов и делегированных ресурсов, а также управление ими](../how-to/view-manage-customers.md). Аналогичным образом, клиент CSP не должен отображаться в разделе **Поставщик облачных решений (предварительная версия)** на странице **Мои клиенты**, чтобы вы могли подключить их к Azure Lighthouse.

## <a name="next-steps"></a>Дальнейшие действия

- Узнайте больше об [интерфейсах управления для различных клиентов](cross-tenant-management-experience.md).
- Узнайте, как [подключить подписку к Azure Lighthouse](../how-to/onboard-customer.md).
- Узнайте больше о [программе для поставщиков облачных решений](/partner-center/csp-overview).

---
title: Обновление терминологии Azure Monitor | Документация Майкрософт
description: В этой статье описаны последние изменения в терминологии для служб мониторинга Azure.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/08/2019
ms.openlocfilehash: fea85a85db1e555adea63f5b65b09a9285d714da
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102031979"
---
# <a name="azure-monitor-naming-and-terminology-changes"></a>Изменения терминологии и именования Azure Monitor
Недавно в Azure Monitor были внесены значительные изменения, а также произошло слияние различных служб, для того чтобы упростить мониторинг для клиентов Azure. В этой статье описываются недавние изменения имени и терминологии в документации Azure Monitor.

## <a name="october-2019---diagnostic-log-to-resource-log"></a>Октябрь 2019 — журнал диагностики для журнала ресурсов
"Журналы диагностики" были изменены на "Журналы ресурсов", чтобы они лучше соответствовали собираемым данным. Термин "параметры диагностики" остается неизменным.  

## <a name="february-2019---log-analytics-terminology"></a>Терминология Log Analytics, февраль 2019 г.
Чтобы лучше описать службу Azure Monitor и ее различные компоненты после слияния различных служб в Azure Monitor, мы делаем следующий шаг, изменяя терминологию в нашей документации. 

### <a name="log-analytics"></a>Log Analytics
Данные журнала Azure Monitor по-прежнему хранятся в рабочей области Log Analytics, собираются и анализируются той же службой, но мы меняем термин _Log Analytics_ во многих местах на _журналы Azure Monitor_. Этот термин лучше отражает свою роль в Azure Monitor и обеспечивает лучшую согласованность с [метриками в Azure Monitor](essentials/data-platform-metrics.md).

Термин _log analytics_ теперь в первую очередь применяется к странице на портале Azure, которая используется для записи и выполнения запросов, а также для анализа данных журнала. Это функциональный эквивалент [обозревателя метрик](essentials/metrics-charts.md), который является страницей на портале Azure, используемой для анализа данных метрик.

### <a name="log-analytics-workspaces"></a>Рабочие области Log Analytics
[Рабочие области](logs/manage-access.md), которые содержат данные журнала в Azure Monitor, по-прежнему называются рабочими областями Log Analytics. Меню **Log Analytics** на портале Azure было переименовано в **Рабочие области Log Analytics**. Здесь вы [создаете новые рабочие области](logs/quick-create-workspace.md) и настраиваете источники данных. Анализ журналов и других данных мониторинга в **Azure Monitor** и настройка **рабочих областей Log Analytics**.

### <a name="management-solutions"></a>Решения для управления
[Решения для управления](insights/solutions.md) были переименованы на _Решения для мониторинга_, что лучше описывает их функциональность.


## <a name="august-2018---consolidation-of-monitoring-services-into-azure-monitor"></a>Консолидация служб мониторинга в Azure Monitor, август 2018 г.
Log Analytics и Application Insights были объединены в Azure Monitor, чтобы предоставить единую интегрированную среду для мониторинга ресурсов Azure и гибридных сред. Никакие функциональные возможности не были удалены из этих служб, и пользователи могут выполнять те же сценарии, которые всегда выполняли, без потери или компрометации каких-либо возможностей.

Документация для каждой из этих служб была объединена в единый набор содержимого для Azure Monitor. Это поможет читателю в поиске содержимого для конкретного сценария мониторинга в одном месте, в отличие от необходимости ссылаться на несколько наборов контента. По мере развития консолидированной службы содержимое станет более интегрированным.

Другие возможности, которые считались частью службы Log Analytics, например агенты и представления, также размещены как и компоненты Azure Monitor. Их функциональность осталась прежней. Единственное изменение — потенциальные улучшения интерфейса на портале Azure.


## <a name="april-2018---retirement-of-operations-management-suite-brand"></a>Снятие с эксплуатации торговой марки Operations Management Suite, апрель 2018
В целях лицензирования Operations Management Suite (OMS) объединял в себе следующие службы управления Azure.

- Application Insights
- Служба автоматизации Azure
- Azure Backup
- Log Analytics
- Site Recovery

[Для этих служб были введены новые цены](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/), и объединение OMS больше не доступно для новых клиентов. Ни одна из служб, которая была элементом OMS, не изменилась, за исключением консолидации в Azure Monitor, описанной выше. 




## <a name="next-steps"></a>Дальнейшие действия

- Ознакомьтесь с [обзором Azure Monitor](overview.md), в котором описаны его различные компоненты и функции.
- Дополнительные сведения о [перемещении портала OMS](./logs/oms-portal-transition.md).

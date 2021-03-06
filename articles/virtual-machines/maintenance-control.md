---
title: Общие сведения об управлении обслуживанием для виртуальных машин Azure с помощью портал Azure
description: Узнайте, как управлять применением обслуживания к виртуальным машинам Azure с помощью управления обслуживанием.
author: cynthn
ms.service: virtual-machines
ms.subservice: maintenance-control
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 11/19/2020
ms.author: cynthn
ms.openlocfilehash: 290a1e8da4e9b3e8eff171ab2d5837bfc9c381b9
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "102552428"
---
# <a name="managing-platform-updates-with-maintenance-control"></a>Управление обновлениями платформы с помощью управления обслуживанием 

Управление обновлениями платформы, которые не нуждаются в перезагрузке, с помощью управления обслуживанием. Azure часто обновляет свою инфраструктуру для повышения надежности, производительности, безопасности или запуска новых функций. Большинство обновлений прозрачны для пользователей. Некоторые конфиденциальные рабочие нагрузки, например игры, потоковая передача мультимедиа и финансовые транзакции, не допускают даже несколько секунд заморозки или отключения виртуальной машины для обслуживания. Управление обслуживанием дает возможность ожидать обновления платформы и применять их в течение интервала в течение 35 дней. 

Управление обслуживанием позволяет решить, когда следует применять обновления к изолированным виртуальным машинам и выделенным узлам Azure.

С помощью управления обслуживанием можно:
- Пакетных обновлений в одном пакете обновления.
- Подождите 35 дней, чтобы применить обновления. 
- Автоматизируйте обновление платформы, настроив расписание обслуживания или используя [функции Azure](https://github.com/Azure/azure-docs-powershell-samples/tree/master/maintenance-auto-scheduler).
- Конфигурации обслуживания работают в подписках и группах ресурсов. 

## <a name="limitations"></a>Ограничения

- Виртуальные машины должны находиться на [выделенном узле](./dedicated-hosts.md)или быть созданы с использованием [ИЗОЛИРОВАННОГО размера виртуальной машины](isolation.md).
- Если расписание обслуживания объявлено, оно должно быть не менее 2 часов.
- Через 35 дней будет автоматически применено обновление.
- Пользователь должен иметь доступ к **участнику ресурсов** .

## <a name="management-options"></a>Параметры управления

Вы можете создавать конфигурации обслуживания и управлять ими с помощью любого из следующих параметров:

- [Azure CLI](maintenance-control-cli.md)
- [Azure PowerShell](maintenance-control-powershell.md)
- [Портал Azure](maintenance-control-portal.md)

Пример использования функций Azure см. в разделе [Планирование обновлений обслуживания с помощью функции управления обслуживанием и функций Azure](https://github.com/Azure/azure-docs-powershell-samples/tree/master/maintenance-auto-scheduler).

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в разделе [обслуживание и обновления](maintenance-and-updates.md).

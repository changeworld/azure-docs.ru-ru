---
title: Введение в автоматизацию в Azure Sentinel | Документация Майкрософт
description: В этой статье рассказывается о возможностях оркестрации, автоматизации и реагирования (ВЗЛЕТЕЛ) Azure Sentinel, а также описываются его компоненты ВЗЛЕТЕЛ и модули PlayBook.
services: sentinel
cloud: na
documentationcenter: na
author: yelevin
manager: rkarlin
ms.assetid: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2021
ms.author: yelevin
ms.openlocfilehash: 54d7c997ce17c927a692e84f6094e3a08f707af7
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104609804"
---
# <a name="security-orchestration-automation-and-response-soar-in-azure-sentinel"></a>Оркестрации безопасности, Автоматизация и ответ (ВЗЛЕТЕЛ) в Azure Sentinel

В этой статье описываются возможности оркестрации в Azure по обеспечению безопасности, автоматизации и реагированию (ВЗЛЕТЕЛ), а также показано, как использование правил автоматизации и модули Playbook в ответ на угрозы безопасности повышает эффективность SOC и экономит время и ресурсы.

## <a name="azure-sentinel-as-a-soar-solution"></a>Azure Sentinel как решение ВЗЛЕТЕЛ

### <a name="the-problem"></a>задачи;

Группы SIEM/SOC обычно засыпаны с оповещениями системы безопасности и инцидентами на регулярной основе, так как тома настолько велики, что доступный персонал является огромным. Это приводит к слишком частому результату в ситуациях, когда много предупреждений пропускается и многие инциденты не изучаются, в результате чего Организация становится уязвимой для атак, которые не были замечены.

### <a name="the-solution"></a>Решение

Метка Azure, помимо системы управления сведениями о безопасности и событиями (SIEM), также является платформой для оркестрации безопасности, автоматизации и реагирования (ВЗЛЕТЕЛ). Одной из ее основных целей является автоматизация повторяющихся и предсказуемых задач, которые являются обязанностью центра деятельности по обеспечению безопасности и персонала (SOC/SecOps), освобождая время и ресурсы для более глубокого изучения и поиском сложных угроз. Автоматизация берет несколько различных форм в Azure Sentinel, от правил автоматизации, централизованно управляющих автоматизацией обработки инцидентов и реагирования, на модули PlayBook, которые выполняют предварительно определенные последовательности действий, чтобы обеспечить эффективную и гибкую автоматизацию для задач реагирования на угрозы.

## <a name="automation-rules"></a>Правила автоматизации

Правила автоматизации — это новая концепция в Azure Sentinel. Эта функция позволяет пользователям централизованно управлять автоматизацией обработки инцидентов. Помимо того, что вы можете назначать модули PlayBook инцидентам (а не только предупреждениям, как раньше), правила автоматизации позволяют автоматизировать ответы для нескольких правил аналитики одновременно, автоматически размечать, назначать или закрывать инциденты без необходимости модули playbook и управлять порядком выполнения выполняемых действий. Правила автоматизации упрощают использование службы автоматизации в Azure Sentinel и позволяют упростить сложные рабочие процессы для процессов оркестрации инцидентов.

Дополнительные сведения о [правилах автоматизации](automate-incident-handling-with-automation-rules.md)см. в этом подробном описании.

> [!IMPORTANT]
>
> - В настоящее время функция **правил автоматизации** доступна в **предварительной версии**. Ознакомьтесь с дополнительными [условиями использования Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) предварительных версий для дополнительных юридических условий, которые относятся к функциям Azure, которые доступны в бета-версии, предварительном просмотре или еще не выпущены в общедоступную версию.

## <a name="playbooks"></a>Сборники тренировочных заданий

Сборник тренировочных заданий — это набор действий по отклику и исправлению, который можно запустить из Sentinel в качестве подпрограммы. Сборник тренировочных заданий может помочь автоматизировать и управлять откликом угроз, он может интегрироваться с другими системами как внутренними, так и внешними, и его можно настроить для автоматического запуска в ответ на определенные предупреждения или инциденты, которые вызываются правилом аналитики или правилом автоматизации соответственно. Его также можно запускать вручную по запросу в ответ на предупреждения на странице инциденты.

Модули Playbook в Azure Sentinel основаны на рабочих процессах, встроенных в [Azure Logic Apps](../logic-apps/logic-apps-overview.md), облачной службе, которая помогает планировать, автоматизировать и управлять задачами и рабочими процессами во всей системе предприятия. Это означает, что модули PlayBook может воспользоваться всеми преимуществами возможностей интеграции и управления Logic Apps ", а также простыми в использовании инструментами проектирования, масштабируемостью, надежностью и уровнем обслуживания для службы Azure уровня 1.

Подробнее об этом подробном описании [модули PlayBook](automate-responses-with-playbooks.md).

## <a name="next-steps"></a>Дальнейшие действия

В этом документе вы узнали, как Azure Sentinel использует автоматизацию для повышения эффективности и эффективности работы SOC.

- Дополнительные сведения об автоматизации обработки инцидентов см. [в разделе Автоматизация обработки инцидентов в Azure Sentinel](automate-incident-handling-with-automation-rules.md).
- Дополнительные сведения о расширенных возможностях автоматизации см. [в статье Автоматизация реагирования на угрозы с помощью модули Playbook в Azure Sentinel](automate-responses-with-playbooks.md).
- Справку по реализации правил автоматизации и модули PlayBook см. в разделе [учебник. Использование модули PlayBook для автоматизации реагирования угроз в Azure Sentinel](tutorial-respond-threats-playbook.md).

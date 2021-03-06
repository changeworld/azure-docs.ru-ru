---
title: Соединитель управления ИТ-услугами — безопасный экспорт в Azure Monitor-Configuration с помощью ServiceNow
description: В этой статье показано, как подключить продукты и службы ITSM с ServiceNow по безопасному экспорту в Azure Monitor.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/31/2020
ms.openlocfilehash: cf19911bd8126bfb73f848c086aa817a7d7adb00
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "103563697"
---
# <a name="connect-servicenow-to-azure-monitor"></a>Подключение ServiceNow к Azure Monitor

В следующих разделах приводятся сведения о подключении продукта ServiceNow и безопасном экспорте в Azure.

## <a name="prerequisites"></a>Предварительные требования

Убедитесь, что выполнены следующие предварительные требования:

* Служба Azure AD зарегистрирована.
* У вас установлена поддерживаемая версия управления событиями ServiceNow — ИТОМ (версия Нью-Йорк или более поздняя).
* [Приложение](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ac4c9c57dbb1d090561b186c1396191a/1.3.1?referer=%2Fstore%2Fsearch%3Flistingtype%3Dallintegrations%25253Bancillary_app%25253Bcertified_apps%25253Bcontent%25253Bindustry_solution%25253Boem%25253Butility%26q%3DEvent%2520Management%2520Connectors&sl=sh) , установленное в экземпляре ServiceNow.

## <a name="configure-the-servicenow-connection"></a>Настройка подключения ServiceNow

1. Используйте ссылку HTTPS://(имя экземпляра). Service-Now. com/API/sn_em_connector/ем/inbound_event? Source = азуремонитор URI для определения безопасного экспорта.

2. Следуйте инструкциям в соответствии с версией:
   * [Париж](https://docs.servicenow.com/bundle/paris-it-operations-management/page/product/event-management/concept/azure-integration.html)
   * [Орландо](https://docs.servicenow.com/bundle/orlando-it-operations-management/page/product/event-management/concept/azure-integration.html)
   * [Нью-Йорк](https://docs.servicenow.com/bundle/newyork-it-operations-management/page/product/event-management/concept/azure-integration.html)

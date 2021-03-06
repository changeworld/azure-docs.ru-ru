---
title: Что такое шлюз приложений Azure
description: Сведения об использовании шлюза приложений Azure для управления веб-трафиком для вашего приложения.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.custom: mvc
ms.date: 08/26/2020
ms.author: victorh
ms.openlocfilehash: 4344cd38d9a58eec27c6202e81b8ef678a510681
ms.sourcegitcommit: 24a12d4692c4a4c97f6e31a5fbda971695c4cd68
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2021
ms.locfileid: "102176014"
---
# <a name="what-is-azure-application-gateway"></a>Что такое шлюз приложений Azure?

Шлюз приложений Azure — это подсистема балансировки нагрузки веб-трафика, предназначенная для управления трафиком веб-приложений. Традиционные подсистемы балансировки нагрузки работают на транспортном уровне (уровень 4 OSI — протоколы TCP и UDP) и маршрутизируют трафик к IP-адресу и порту назначения в зависимости от исходного IP-адреса и порта.

Шлюза приложений умеет принимать решения о маршрутизации на основе дополнительных атрибутов HTTP-запроса, например пути URI или заголовков узла. Например, вы можете маршрутизировать трафик на основе входящего URL-адреса. Поэтому, если во входящем URL-адресе есть элемент `/images`, вы можете направлять трафик к определенному набору (пулу) серверов, настроенных для изображений. Если в URL-адресе есть элемент `/video`, трафик направляется к другому пулу, который оптимизирован для видео.

![imageURLroute](./media/application-gateway-url-route-overview/figure1-720.png)

Этот тип маршрутизации называется балансировкой нагрузки на уровне приложения (уровень 7 OSI). Шлюз приложений Azure может выполнять маршрутизацию на основе URL-адреса и многие другие действия.

>[!NOTE]
> Azure предоставляет набор полностью управляемых решений балансировки нагрузки для пользовательских сценариев. 
> * Если вам необходима глобальная маршрутизация на основе DNS и вам **не** нужны обработка подключений по протоколу TLS (разгрузка SSL), обработка прикладного уровня HTTP- или HTTPS-запросов, вам стоит обратить внимание на [Диспетчер трафика](../traffic-manager/traffic-manager-overview.md). 
> * Если нужно оптимизировать глобальную маршрутизацию для вашего веб-трафика, а также обеспечить лучшую производительность и надежность для пользователей посредством быстрой глобальной отработки отказа, ознакомьтесь с возможностями службы [Front Door](../frontdoor/front-door-overview.md).
> * Чтобы выполнить балансировку нагрузки на сетевом уровне, ознакомьтесь с [Load Balancer](../load-balancer/load-balancer-overview.md). 
> 
> В комплексных сценариях может быть целесообразно объединить эти решения.
> Сравнение параметров балансировки нагрузки Azure см. в статье [Overview of load-balancing options in Azure](/azure/architecture/guide/technology-choices/load-balancing-overview) (Общие сведения о параметрах балансировки нагрузки в Azure).


## <a name="features"></a>Компоненты

Сведения о функциях Шлюза приложений см. в [этой статье](features.md).

## <a name="pricing-and-sla"></a>Цены и соглашение об уровне обслуживания

Сведения о ценах на Шлюз приложений см. на [этой странице](https://azure.microsoft.com/pricing/details/application-gateway/).

Соглашение об уровне обслуживания для Шлюза приложений представлено [здесь](https://azure.microsoft.com/support/legal/sla/application-gateway/v1_2/).

## <a name="whats-new"></a>Новые возможности

О новых возможностях Шлюза приложений Azure, можно узнать на странице [Обновления Azure](https://azure.microsoft.com/updates/?category=networking&query=Application%20Gateway).

## <a name="next-steps"></a>Дальнейшие шаги

В зависимости от требований и среды тестовый шлюз приложений можно создать с помощью портала Azure, Azure PowerShell или Azure CLI:

- [Краткое руководство. Направление веб-трафика с помощью Шлюза приложений Azure на портале Azure](quick-create-portal.md).
- [Краткое руководство. Направление веб-трафика с помощью шлюза приложений Azure в Azure PowerShell](quick-create-powershell.md)
- [Краткое руководство. Направление веб-трафика с помощью Шлюза приложений Azure и Azure CLI.](quick-create-cli.md)
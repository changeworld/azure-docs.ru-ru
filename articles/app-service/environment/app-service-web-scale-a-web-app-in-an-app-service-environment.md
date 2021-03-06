---
title: Масштабирование приложения в ASE v1
description: Масштабирование приложения в Среда службы приложений. Этот документ предоставляется только для клиентов, использующих устаревшую версию ASE (версию 1).
author: ccompy
ms.assetid: 78eb1e49-4fcd-49e7-b3c7-f1906f0f22e3
ms.topic: article
ms.date: 10/17/2016
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 0e665ec27da0a898e754817f946b965ac7360fda
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "86220564"
---
# <a name="scaling-apps-in-an-app-service-environment-v1"></a>Масштабирование приложений в Среда службы приложений v1
Обычное в службе приложений Azure можно масштабировать три параметра:

* план ценообразования;
* размер рабочей роли; 
* количество экземпляров.

В среде ASE нет необходимости выбирать или изменять план ценообразования.  С точки зрения возможностей среда уже находится на максимальном ценовом уровне.  

Что касается объема ресурсов, для каждой рабочей роли администратор ASE может назначать размер вычислительного ресурса для каждого пула исполнителей.  Это значит, что при необходимости вы можете иметь пул исполнителей 1 с вычислительными ресурсами P4 и пул исполнителей 2 с вычислительными ресурсами P1.  Они не должны быть расположены по размеру.  Подробные сведения о размерах и ценах см. на странице с [ценами на использование службы приложений Azure][AppServicePricing].  То есть для масштабирования веб-приложений и планов службы приложений в среде службы приложений остаются такие параметры:

* выбор пула исполнителей;
* количество экземпляров.

Изменение любого элемента осуществляется через соответствующий пользовательский интерфейс, который отображается для ваших планов службы приложений, размещенных в ASE.  

![Снимок экрана, на котором показано, где можно просмотреть сведения о плане службы масштабирования и плане службы рабочего пула.][1]

Невозможно увеличить масштаб ASP больше, чем позволяет количество доступных вычислительных ресурсов в рабочем пуле, в котором находится ASP.  Если вам нужны дополнительные вычислительные ресурсы в этом рабочем пуле, попросите администратора ASE добавить их.  Сведения о перенастройке среды ASE см. в статье [Масштабирование приложений в среде службы приложений][HowtoConfigureASE].  Можно также воспользоваться функциями автоматического масштабирования ASE, чтобы добавлять емкость с учетом расписания или других показателей.  Дополнительные сведения о настройке автоматического масштабирования среды ASE см. в статье [Автомасштабирование и среда службы приложений версии 1][ASEAutoscale].

Можно создать несколько планов службы приложений, использующих вычислительные ресурсы из разных рабочих пулов. Кроме того, можно использовать один и тот же рабочий пул.  Например, если в рабочем пуле 1 имеется (10) доступных вычислительных ресурсов, вы можете создать один план службы приложений, использующий (6) вычислительных ресурсов, и другой план службы приложений, использующий (4) вычислительных ресурса.

### <a name="scaling-the-number-of-instances"></a>Масштабирование количества экземпляров
При первом создании веб-приложения в среде службы приложений запускается один его экземпляр.  Затем можно развернуть его, добавляя экземпляры, чтобы предоставить приложению дополнительные вычислительные ресурсы.   

Если среда ASE располагает достаточными ресурсами, сделать это будет очень просто.  Перейдите в план службы приложений, содержащий сайты, которые нужно масштабировать, и выберите "Масштаб".  Откроется пользовательский интерфейс, где можно вручную задать шкалу для ASP или настроить правила автомасштабирования ASP.  Чтобы вручную масштабировать приложение, просто установите ***Scale by** _ в значение _ *_число экземпляров, введенное вручную_* *.  Отсюда перетащите ползунок на желаемое количество или введите нужное число в поле рядом с ползунком.  

![Снимок экрана, на котором показано, где можно задать масштаб для кода ASP или настроить правила автомасштабирования для ASP.][2] 

Правила автоматического масштабирования для ASP в ASE работают как обычно.  Вы можете выбрать ***процент загрузки ЦП** _ в разделе _*_масштабировать по_*_ и создать правила автомасштабирования для ASP на основе процента использования ЦП или создать более сложные правила, используя _ *_Расписание и правила производительности_* *.  Более полные сведения о настройке автомасштабирования см. в руководстве [Увеличение масштаба приложения в Azure][AppScale]. 

### <a name="worker-pool-selection"></a>выбор пула исполнителей;
Как отмечено ранее, выбор пула исполнителей осуществляется через пользовательский интерфейс ASP.  Откройте колонку для ASP, который требуется масштабировать, и выберите пул исполнителей.  Вы увидите все пулы исполнителей, настроенные в среде службы приложений.  Если в среде настроен только один пул исполнителей, в списке будет только один пул.  Чтобы изменить расположение ASP в определенном пуле ресурсов, достаточно выбрать пул ресурсов, в который нужно перенести план службы приложений.  

![Снимок экрана, на котором показано, где можно изменить пул рабочих процессов, в котором находится ASP.][3]

Перед перемещением ASP из одного пула исполнителей в другой нужно убедиться в наличии в последнем достаточного количества ресурсов для вашего ASP.  В списке пулов исполнителей отображается не только его имя, но и количество доступных исполнителей в этому пуле.  Убедитесь, что в пуле достаточно экземпляров для содержания вашего плана службы приложений.  Если вам нужно больше вычислительных ресурсов в пуле исполнителей, в который вы перемещаете план, попросите администратора ASE добавить их.  

> [!NOTE]
> Перемещение ASP из одного рабочего пула приведет к холодному запуску приложений в этом ASP.  Это может замедлить обработку запросов, так как холодный запуск приложения выполняется на новых вычислительных ресурсах.  Холодного запуска можно избежать с помощью функции [подготовки приложения][AppWarmup] в службе приложений Azure.  Модуль инициализации приложений, описанный в этой статье, тоже подходит для холодного запуска, так как процесс инициализации также вызывается при холодном запуске приложений на новых вычислительных ресурсах. 
> 
> 

## <a name="getting-started"></a>Начало работы
Чтобы приступить к работе со средами службы приложений, изучите статью [Создание среды службы приложений][HowtoCreateASE].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[ScaleWebapp]: ../manage-scale-up.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[HowtoConfigureASE]: app-service-web-configure-an-app-service-environment.md
[CreateWebappinASE]: app-service-web-how-to-create-a-web-app-in-an-ase.md
[Appserviceplans]: ../overview-hosting-plans.md
[AppServicePricing]: https://azure.microsoft.com/pricing/details/app-service/ 
[ASEAutoscale]: app-service-environment-auto-scale.md
[AppScale]: ../manage-scale-up.md
[AppWarmup]: https://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/

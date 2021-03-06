---
title: Оптимизация кэша Gen2
description: Узнайте, как выполнять мониторинг кэша 2-го поколения на портале Azure.
services: synapse-analytics
author: gaursa
manager: craigg
ms.service: synapse-analytics
ms.subservice: sql-dw
ms.topic: conceptual
ms.date: 11/20/2020
ms.author: gaursa
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: fed3ed2c87342d557872e97bfc2a6c4d142b5a3a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104585627"
---
# <a name="how-to-monitor-the-adaptive-cache"></a>Мониторинг адаптивного кэша

В этой статье описывается, как отслеживать и устранять производительность медленных запросов, определяя, оптимально ли Рабочая нагрузка использует адаптивный кэш для выделенных пулов SQL.

Архитектура хранилища выделенного пула SQL автоматически размещает наиболее часто запрашиваемые сегменты columnstore в кэше, находящегося на твердотельных накопителях на основе NVMe. Если запросы извлекают сегменты, находящиеся в кэше, производительность будет выше.
 
## <a name="troubleshoot-using-the-azure-portal"></a>Устранение неполадок с помощью портала Azure

Azure Monitor можно использовать для просмотра метрик кэша для устранения неполадок с производительностью запросов. Сначала перейдите к портал Azure и щелкните **монитор**, **метрики** и **+ Выбор области**:

![Снимок экрана — выберите область, выбранную из метрик в портал Azure.](./media/sql-data-warehouse-how-to-monitor-cache/cache-0.png)

Используйте полосы поиска и раскрывающиеся панели, чтобы найти выделенный пул SQL. Затем нажмите кнопку Применить.

![На снимке экрана показана панель Выбор области, на которой можно выбрать хранилище данных.](./media/sql-data-warehouse-how-to-monitor-cache/cache-1.png)

Ключевые метрики для устранения неполадок кэша — **Процент попаданий в кэш** и **процент использования кэша**. Выберите **Процент попаданий в кэш** и нажмите кнопку **Добавить метрику** , чтобы добавить **процент использования кэша**. 

![Метрики кэша](./media/sql-data-warehouse-how-to-monitor-cache/cache-2.png)

## <a name="cache-hit-and-used-percentage"></a>Процент попаданий и процент использования кэша

В матрице ниже описаны сценарии,основанные на значениях метрик кэша:

|                                | **Высокий процент попаданий в кэш** | **Низкий процент попаданий в кэш** |
| :----------------------------: | :---------------------------: | :--------------------------: |
| **Высокий процент использования кэша** |          Сценарий 1           |          Сценарий 2          |
| **Низкий процент использования кэша**  |          Сценарий 3           |          Сценарий 4          |

**Сценарий 1.** Вы оптимально используете кэш. [Устранение неполадок](sql-data-warehouse-manage-monitor.md) в других областях, которые могут замедлить выполнение запросов.

**Сценарий 2.** Текущий рабочий набор данных не помещается в кэш, что приводит к низкому проценту попаданий в кэш из-за физических операций чтения. Попробуйте увеличить уровень производительности и повторно запустите рабочую нагрузку для заполнения кэша.

**Сценарий 3.** Вполне вероятно, что запрос выполняется медленно из-за причин, которые не относятся к кэшу. [Устранение неполадок](sql-data-warehouse-manage-monitor.md) в других областях, которые могут замедлить выполнение запросов. Вы можете также рассмотреть [уменьшение масштаба экземпляра](sql-data-warehouse-manage-monitor.md), чтобы уменьшить размер кэша для сокращения затрат. 

**Сценарий 4**. У вас есть холодный кэш, который может быть причиной медленного выполнения запроса. Подумайте о том, чтобы повторно выполнить свой запрос, так как ваш рабочий набор данных теперь должен быть в режиме кэширования. 

> [!IMPORTANT]
> Если процент попаданий в кэш или процент использования кэша не обновляется после повторного выполнения рабочей нагрузки, Рабочий набор может уже находиться в памяти. Кэшируются только кластеризованные таблицы columnstore.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о настройке производительности общих запросов см. в разделе [Наблюдение за выполнением запросов](sql-data-warehouse-manage-monitor.md#monitor-query-execution).

---
title: Высокая доступность — база данных Azure для PostgreSQL — один сервер
description: В этой статье приведены сведения о высокой доступности в базе данных Azure для PostgreSQL — один сервер.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: conceptual
ms.date: 6/15/2020
ms.openlocfilehash: aa9f38b2cefa60a0c3341c1317cf45fbcb735301
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "92485449"
---
# <a name="high-availability-in-azure-database-for-postgresql--single-server"></a>Высокий уровень доступности в базе данных Azure для PostgreSQL — один сервер
Служба "база данных Azure для PostgreSQL" обеспечивает гарантированный высокий уровень доступности при использовании соглашения об уровне обслуживания с финансовыми гарантиями (SLA) на [99,99%](https://azure.microsoft.com/support/legal/sla/postgresql) времени работы. База данных Azure для PostgreSQL обеспечивает высокий уровень доступности во время запланированных событий, таких как операция вычислений User-инитатед Scale, а также при незапланированных событиях, таких как оборудование, программное обеспечение или сбои сети. База данных Azure для PostgreSQL может быстро восстановиться после большинства критических обстоятельств, обеспечивая практически немедленный доступ к приложениям при использовании этой службы.

База данных Azure для PostgreSQL подходит для выполнения критически важных баз данных, требующих высокой отказоустойчивости. Служба, созданная на основе архитектуры Azure, обладает высокой доступностью, избыточностью и устойчивостью, чтобы уменьшить время простоя базы данных от запланированных и незапланированных простоев, не требуя настройки каких бы то ни было дополнительных компонентов. 

## <a name="components-in-azure-database-for-postgresql--single-server"></a>Компоненты в базе данных Azure для PostgreSQL — один сервер

| **Компонент** | **Описание**|
| ------------ | ----------- |
| <b>Сервер базы данных PostgreSQL | База данных Azure для PostgreSQL обеспечивает безопасность, изоляцию, защиту ресурсов и возможность быстрого перезапуска для серверов баз данных. Эти возможности упрощают выполнение операций, таких как масштабирование и восстановление сервера базы данных, после сбоя в секундах. <br/> Изменения данных в сервере базы данных обычно происходят в контексте транзакции базы данных. Все изменения базы данных записываются синхронно в виде упреждающего журнала (WAL) в службе хранилища Azure, подключенной к серверу базы данных. Во время процесса [контрольной точки](https://www.postgresql.org/docs/11/sql-checkpoint.html) базы данных страницы данных из памяти сервера базы данных также сбрасываются в хранилище. |
| <b>Удаленное хранилище | Все файлы физических данных PostgreSQL и файлы WAL хранятся в службе хранилища Azure, которая является архитектурой для хранения трех копий данных в регионе для обеспечения избыточности, доступности и надежности данных. Уровень хранилища также не зависит от сервера базы данных. Его можно отсоединить от сервера базы данных, на котором произошел сбой, и повторно подключить к новому серверу базы данных в течение нескольких секунд. Кроме того, служба хранилища Azure постоянно отслеживает ошибки хранилища. При обнаружении повреждения блока оно автоматически фиксируется путем создания экземпляра новой копии хранилища. |
| <b>Шлюз | Шлюз выступает в качестве прокси-сервера базы данных, направляет все клиентские подключения к серверу базы данных. |

## <a name="planned-downtime-mitigation"></a>Запланированное снижение времени простоя
Служба "база данных Azure для PostgreSQL" спроектирована так, чтобы обеспечить высокий уровень доступности во время запланированных операций простоя. 

:::image type="content" source="./media/concepts-high-availability/azure-postgresql-elastic-scaling.png" alt-text="представление эластичного масштабирования в Azure PostgreSQL":::

1. Увеличение и уменьшение масштаба серверов баз данных PostgreSQL за считаные секунды
2. Шлюз, который выступает в качестве прокси-сервера для маршрутизации клиента к соответствующему серверу базы данных
3. Масштабирование хранилища может выполняться без простоев. Удаленное хранилище обеспечивает быструю отсоединение и повторное подключение после отработки отказа.
Ниже приведены некоторые сценарии планового обслуживания.

| **Сценарий** | **Описание**|
| ------------ | ----------- |
| <b>Увеличение/уменьшение масштаба вычислений | Когда пользователь выполняет операцию увеличения или уменьшения масштаба вычислений, новый сервер базы данных подготавливается с помощью масштабируемой конфигурации вычислений. На старом сервере базы данных активные контрольные точки могут быть завершены, клиентские подключения преобразуются, все незафиксированные транзакции отменяются, а затем завершает работу. Затем хранилище отсоединяется от старого сервера базы данных и прикрепляется к новому серверу базы данных. Когда клиентское приложение повторяет подключение или пытается создать новое соединение, шлюз направляет запрос на подключение к новому серверу базы данных.|
| <b>Масштабирование хранилища | Увеличение масштаба хранилища является оперативной операцией и не прерывает работу сервера базы данных.|
| <b>Новое развертывание программного обеспечения (Azure) | Новые компоненты или исправления ошибок автоматически происходят в рамках планового обслуживания службы. Дополнительные сведения см. в [документации](./concepts-monitoring.md#planned-maintenance-notification), а также на [портале](https://aka.ms/servicehealthpm).|
| <b>Обновления дополнительных версий | База данных Azure для PostgreSQL автоматически запланирует серверы баз данных до дополнительной версии, определенной Azure. Это происходит в рамках планового обслуживания службы. Это приведет к короткому простою в секундах, а сервер базы данных автоматически перезапускается с новой дополнительной версией. Дополнительные сведения см. в [документации](./concepts-monitoring.md#planned-maintenance-notification), а также на [портале](https://aka.ms/servicehealthpm).|


##  <a name="unplanned-downtime-mitigation"></a>Незапланированное снижение времени простоя

Незапланированные простои могут возникнуть в результате непредвиденных сбоев, включая базовые сбои оборудования, сетевые проблемы и программные ошибки. В случае непредвиденного отключения сервера базы данных новый сервер базы данных автоматически подготавливается в течение нескольких секунд. Удаленное хранилище автоматически прикрепляется к новому серверу базы данных. Подсистема PostgreSQL выполняет операцию восстановления с использованием файлов WAL и базы данных и открывает сервер базы данных, чтобы разрешить подключение клиентов. Незафиксированные транзакции теряются, и их необходимо повторить с помощью приложения. Хотя незапланированные простои не могут быть устранены, база данных Azure для PostgreSQL сокращает время простоя, автоматически выполняя операции восстановления как на сервере базы данных, так и на уровне хранилища, не требуя вмешательства человека. 


:::image type="content" source="./media/concepts-high-availability/azure-postgresql-built-in-high-availability.png" alt-text="Просмотр высокого уровня доступности в Azure PostgreSQL":::

1. Серверы Azure PostgreSQL с возможностями быстрого масштабирования.
2. Шлюз, который выступает в качестве прокси-сервера для маршрутизации клиентских подключений к правильному серверу базы данных
3. Служба хранилища Azure с тремя копиями для обеспечения надежности, доступности и избыточности.
4. Удаленное хранилище также обеспечивает быструю отсоединение и повторное подключение после отработки отказа сервера.
   
### <a name="unplanned-downtime-failure-scenarios-and-service-recovery"></a>Незапланированный простой: сценарии сбоя и восстановление службы
Ниже приведены некоторые сценарии сбоя и автоматическое восстановление базы данных Azure для PostgreSQL.

| **Сценарий** | **Автоматическое восстановление** |
| ---------- | ---------- |
| <B>Сбой сервера базы данных | Если сервер базы данных не работает из-за некоторой базовой ошибки оборудования, активные соединения удаляются, а все порядковых транзакции прерываются. Автоматически развертывается новый сервер базы данных, а удаленное хранилище данных присоединяется к новому серверу базы данных. После завершения восстановления базы данных клиенты могут подключаться к новому серверу базы данных через шлюз. <br /> <br /> Время восстановления (RTO) зависит от различных факторов, включая действия во время сбоя, такие как большие транзакции и объем операций восстановления, выполняемых в процессе запуска сервера базы данных. <br /> <br /> Приложения, использующие базы данных PostgreSQL, должны быть созданы таким образом, чтобы они определяли и повторно пропустили соединения и неудачные транзакции.  Когда приложение повторяет попытку, шлюз прозрачно перенаправляет подключение к только что созданному серверу базы данных. |
| <B>Сбой хранилища | Приложения не видят никакого влияния на связанные с хранилищем проблемы, такие как сбой диска или повреждение физического блока. Поскольку данные хранятся в 3 копиях, копия данных обрабатывается оставшимся хранилищем. Повреждение блоков автоматически исправляется. Если копия данных потеряна, автоматически создается новая копия данных. |

Ниже приведены некоторые сценарии сбоев, для восстановления которых требуется действие пользователя.

| **Сценарий** | **План восстановления** |
| ---------- | ---------- |
| <b> Сбой региона | Сбой региона является редким событием. Однако, если требуется защита от сбоя в регионе, можно настроить одну или несколько реплик чтения в других регионах для аварийного восстановления (DR). (Дополнительные сведения о создании реплик чтения и управлении ими см. в [этой статье](./howto-read-replicas-portal.md) .) В случае сбоя уровня области можно вручную повысить реплику чтения, настроенную в другом регионе, в качестве сервера рабочей базы данных. |
| <b> Ошибки логических или пользовательских пользователей | Восстановление после ошибок пользователя, таких как случайно удаленные таблицы или неправильно обновленные данные, включает в себя выполнение [восстановления на момент времени](./concepts-backup.md) (PITR) путем восстановления данных до момента времени, предшествующего возникновению ошибки.<br> <br>  Если вы хотите восстановить только подмножество баз данных или определенные таблицы, а не все базы данных на сервере базы данных, можно восстановить сервер базы данных в новом экземпляре, экспортировать таблицы с помощью [pg_dump](https://www.postgresql.org/docs/11/app-pgdump.html), а затем использовать [pg_restore](https://www.postgresql.org/docs/11/app-pgrestore.html) для восстановления этих таблиц в базе данных. |



## <a name="summary"></a>Итоги

База данных Azure для PostgreSQL предоставляет возможность быстрого перезапуска серверов баз данных, избыточного хранилища и эффективной маршрутизации из шлюза. Для дополнительной защиты данных можно настроить георепликацию резервных копий, а также развернуть одну или несколько реплик чтения в других регионах. Благодаря встроенным возможностям высокого уровня доступности база данных Azure для PostgreSQL защищает базы данных от большинства распространенных простоев, а также предлагает ведущие в отрасли, [99,99% времени соглашения об уровне обслуживания](https://azure.microsoft.com/support/legal/sla/postgresql). Все эти возможности доступности и надежности позволяют платформе Azure быть идеальной платформой для выполнения критически важных приложений.

## <a name="next-steps"></a>Дальнейшие действия
- Дополнительные сведения о [регионах Azure](../availability-zones/az-overview.md)
- Ознакомьтесь с дополнительными сведениями об [обработке временных ошибок подключения](concepts-connectivity.md).
- Чтобы узнать, как реплицировать данные с репликами для чтения, см. [здесь](howto-read-replicas-portal.md).
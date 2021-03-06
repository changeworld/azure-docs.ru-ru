---
title: Функции оптимизации производительности действий копирования
description: Сведения о ключевых функциях, помогающих оптимизировать производительность действий копирования в фабрике данных Azure Marketplace.
ms.author: jingwang
author: linda33wj
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/24/2020
ms.openlocfilehash: ecb4550b218b069273cba2e3d70a9510c1cc74ca
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "100387807"
---
# <a name="copy-activity-performance-optimization-features"></a>Функции оптимизации производительности действий копирования

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

В этой статье описываются функции оптимизации производительности действий копирования, которые можно использовать в фабрике данных Azure.

## <a name="data-integration-units"></a>Единицы интеграции данных

Единица интеграции данных — это мера, представляющая мощность (сочетание ЦП, памяти и выделения ресурсов сети) одного блока в фабрике данных Azure. Единица интеграции данных применяется только к [среде выполнения интеграции Azure](concepts-integration-runtime.md#azure-integration-runtime), но не к локальной [среде выполнения интеграции](concepts-integration-runtime.md#self-hosted-integration-runtime).

Допустимый диус для повышения производительности действия копирования составляет **от 2 до 256**. Если параметр не указан или в пользовательском интерфейсе выбрано значение "Авто", фабрика данных динамически применяет оптимальную настройку Диу на основе пары "источник-приемник" и шаблона данных. В следующей таблице перечислены поддерживаемые диапазоны Диу и поведение по умолчанию в различных сценариях копирования.

| Сценарий копирования | Поддерживаемый диапазон Диу | Число единиц интеграции данных, устанавливаемое по умолчанию службой |
|:--- |:--- |---- |
| Между хранилищами файлов |- **Копирование из одного файла или в** него: 2-4 <br>- **Копировать из и в несколько файлов**: 2-256 в зависимости от числа и размера файлов <br><br>Например, если скопировать данные из папки с 4 большими файлами и выбрать сохранение иерархии, то максимальный эффективный Диу равен 16; При выборе объединения файла максимальный действующий Диу имеет значение 4. |От 4 до 32 в зависимости от числа и размера файлов |
| Из хранилища файлов в хранилище, отличное от файлов |- **Копирование из одного файла**: 2-4 <br/>- **Копирование из нескольких файлов**: 2-256 в зависимости от числа и размера файлов <br/><br/>Например, если скопировать данные из папки с 4 большими файлами, максимальная эффективная Диу — 16. |- **Копирование в базу данных SQL Azure или Azure Cosmos DB**: от 4 до 16 в зависимости от уровня приемника (DTU/RUs) и шаблона исходного файла<br>- **Копирование в Azure синапсе Analytics** с помощью polybase или инструкции Copy: 2<br>— Другой сценарий: 4 |
| Из нефайлового хранилища в хранилище файлов |- **Копирование из хранилищ данных с поддержкой параметров разделов** (включая [базу данных sql Azure](connector-azure-sql-database.md#azure-sql-database-as-the-source), [управляемый экземпляр Azure SQL](connector-azure-sql-managed-instance.md#sql-managed-instance-as-a-source), [Azure синапсе Analytics](connector-azure-sql-data-warehouse.md#azure-synapse-analytics-as-the-source), [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [SQL Server](connector-sql-server.md#sql-server-as-a-source)и [Teradata](connector-teradata.md#teradata-as-source)): 2-256 при записи в папку и 2-4 при записи в один файл. Примечание. для каждого исходного раздела данных можно использовать до 4 диус.<br>- **Другие сценарии**: 2-4 |- **Копировать из RESTful или HTTP**: 1<br/>- **Копирование из Amazon RedShift** с помощью выгрузки: 2<br>- **Другой сценарий**: 4 |
| Между нефайловыми хранилищами |- **Копирование из хранилищ данных с поддержкой параметров разделов** (включая [базу данных sql Azure](connector-azure-sql-database.md#azure-sql-database-as-the-source), [управляемый экземпляр Azure SQL](connector-azure-sql-managed-instance.md#sql-managed-instance-as-a-source), [Azure синапсе Analytics](connector-azure-sql-data-warehouse.md#azure-synapse-analytics-as-the-source), [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [SQL Server](connector-sql-server.md#sql-server-as-a-source)и [Teradata](connector-teradata.md#teradata-as-source)): 2-256 при записи в папку и 2-4 при записи в один файл. Примечание. для каждого исходного раздела данных можно использовать до 4 диус.<br/>- **Другие сценарии**: 2-4 |- **Копировать из RESTful или HTTP**: 1<br>- **Другой сценарий**: 4 |

Диус, используемый для каждой копии, можно просмотреть в представлении Мониторинг действия копирования или в выходных данных действия. Дополнительные сведения см. в разделе [мониторинг действий копирования](copy-activity-monitoring.md). Для переопределения этого значения по умолчанию укажите значение для `dataIntegrationUnits` свойства следующим образом. *Фактическое число единиц интеграции данных*, используемых при копировании, не превышает заданного значения, в зависимости от формата данных.

Вам будет выставляться плата **за использование диус \* единицы длительности копирования ( \* Цена за единицу/Диу-час)**. Текущие цены см. [здесь](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/). Местная валюта и отдельные скидки могут применяться для каждого типа подписки.

**Пример**.

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "dataIntegrationUnits": 128
        }
    }
]
```

## <a name="self-hosted-integration-runtime-scalability"></a>Масштабируемость локальной среды выполнения интеграции

Если вы хотите добиться более высокой пропускной способности, можно увеличить или уменьшить масштаб для локальной среды IR:

- Если ЦП и доступная память на автономном узле IR не используются полностью, но выполнение параллельных заданий достигает предела, необходимо увеличить масштаб, увеличив число параллельных заданий, которые могут выполняться на узле.  Инструкции см. [здесь](create-self-hosted-integration-runtime.md#scale-up) .
- С другой стороны, высокая загрузка ЦП на локально размещенный IR-узел или доступная память мала, можно добавить новый узел, чтобы расширить нагрузку между несколькими узлами.  Инструкции см. [здесь](create-self-hosted-integration-runtime.md#high-availability-and-scalability) .

Обратите внимание, что в следующих сценариях выполнение одного действия копирования может использовать несколько саморазмещенных IR-узлов:

- Копирование данных из файловых хранилищ в зависимости от числа и размера файлов.
- Скопируйте данные из хранилища данных с поддержкой параметров разделов (включая [базу данных SQL Azure](connector-azure-sql-database.md#azure-sql-database-as-the-source), [управляемый экземпляр Azure SQL](connector-azure-sql-managed-instance.md#sql-managed-instance-as-a-source), [Azure синапсе Analytics](connector-azure-sql-data-warehouse.md#azure-synapse-analytics-as-the-source), [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [открытый центр SAP](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source), [таблицу SAP](connector-sap-table.md#sap-table-as-source), [SQL Server](connector-sql-server.md#sql-server-as-a-source)и [Teradata](connector-teradata.md#teradata-as-source)) в зависимости от количества секций данных.

## <a name="parallel-copy"></a>Параллельное копирование

Можно задать Параллельное копирование ( `parallelCopies` свойство) в действии копирования, чтобы указать параллелизм, который будет использоваться действием копирования. Это свойство можно считать максимальным числом потоков в действии копирования, считываемых из источника или записываемых в хранилище данных приемника параллельно.

Параллельная копия является ортогональной к [единицам интеграции данных](#data-integration-units) или ЛОКАЛЬным [инфракрасным узлам](#self-hosted-integration-runtime-scalability). Он учитывается на всех диус или на автономных узлах IR.

Для каждого выполнения действия копирования по умолчанию фабрика данных Azure динамически применяет оптимальный параметр параллельного копирования на основе пары источник-приемник и шаблона данных. 

> [!TIP]
> По умолчанию Параллельное копирование обычно дает наилучшую пропускную способность, которая автоматически определяется службой ADF на основе пары источников-приемников, шаблона данных и количества диус, а также количества ресурсов ЦП, памяти и узлов для самостоятельно размещенного IR. См. статью [Устранение неполадок с производительностью действия копирования](copy-activity-performance-troubleshooting.md) при настройке параллельной копии.

В следующей таблице перечислены принципы параллельного копирования.

| Сценарий копирования | Поведение параллельного копирования |
| --- | --- |
| Между хранилищами файлов | `parallelCopies` Определяет параллелизм **на уровне файлов**. Фрагментирование внутри каждого файла происходит автоматически и прозрачно. Он предназначен для использования наиболее подходящего размера блока для заданного типа хранилища данных для параллельной загрузки данных. <br/><br/>Фактическое число параллельных копий, используемых действием копирования во время выполнения, не превышает количество файлов, которое у вас есть. Если поведение копирования **mergeFile** в приемник файла, действие копирования не сможет воспользоваться параллелизмом на уровне файлов. |
| Из хранилища файлов в хранилище, отличное от файлов | — При копировании данных в базу данных SQL Azure или Azure Cosmos DB параллельная копия по умолчанию также зависит от уровня приемника (количество DTU/RUs).<br>— При копировании данных в таблицу Azure параллельная копия по умолчанию имеет значение 4. |
| Из нефайлового хранилища в хранилище файлов | — При копировании данных из хранилища данных с поддержкой параметров раздела (включая [базу данных SQL](connector-azure-sql-database.md#azure-sql-database-as-the-source)azure, [управляемый экземпляр Azure SQL](connector-azure-sql-managed-instance.md#sql-managed-instance-as-a-source), [Azure синапсе Analytics](connector-azure-sql-data-warehouse.md#azure-synapse-analytics-as-the-source), [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [открытый центр SAP](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source), [Таблица SAP](connector-sap-table.md#sap-table-as-source), [SQL Server](connector-sql-server.md#sql-server-as-a-source)и [Teradata](connector-teradata.md#teradata-as-source)), по умолчанию используется параллельная копия 4. Фактическое число параллельных копий, используемых действием копирования во время выполнения, не превышает количество секций данных, которые у вас есть. При использовании автономного Integration Runtime и копировании в BLOB-объект или ADLS 2-го поколения Azure, обратите внимание, что максимальная эффективная параллельная копия — 4 или 5 на каждый узел IR.<br>Для других сценариев параллельная копия не вступит в силу. Даже если указан параллелизм, он не применяется. |
| Между нефайловыми хранилищами | — При копировании данных в базу данных SQL Azure или Azure Cosmos DB параллельная копия по умолчанию также зависит от уровня приемника (количество DTU/RUs).<br/>— При копировании данных из хранилища данных с поддержкой параметров раздела (включая [базу данных SQL](connector-azure-sql-database.md#azure-sql-database-as-the-source)azure, [управляемый экземпляр Azure SQL](connector-azure-sql-managed-instance.md#sql-managed-instance-as-a-source), [Azure синапсе Analytics](connector-azure-sql-data-warehouse.md#azure-synapse-analytics-as-the-source), [Oracle](connector-oracle.md#oracle-as-source), [Netezza](connector-netezza.md#netezza-as-source), [SAP HANA](connector-sap-hana.md#sap-hana-as-source), [открытый центр SAP](connector-sap-business-warehouse-open-hub.md#sap-bw-open-hub-as-source), [Таблица SAP](connector-sap-table.md#sap-table-as-source), [SQL Server](connector-sql-server.md#sql-server-as-a-source)и [Teradata](connector-teradata.md#teradata-as-source)), по умолчанию используется параллельная копия 4.<br>— При копировании данных в таблицу Azure параллельная копия по умолчанию имеет значение 4. |

Для управления нагрузкой на компьютеры, на которых размещаются хранилища данных, или для настройки производительности копирования можно переопределить значение по умолчанию и указать значение для `parallelCopies` Свойства. Значение должно быть целым числом больше или равно 1. Во время выполнения для наилучшей производительности действие копирования использует значение, меньшее или равное заданному значению.

При указании значения для `parallelCopies` Свойства переведите нагрузку в хранилище данных источника и приемника в учетную запись. Также учтите увеличение нагрузки на локальную среду выполнения интеграции, если она предоставляет действие копирования. Это увеличение нагрузки происходит в особенности при наличии нескольких действий или параллельных запусков тех же действий, которые выполняются с одним и тем же хранилищем данных. Если вы заметили, что хранилище данных или локальная среда выполнения интеграции перегружена, уменьшите `parallelCopies` значение, чтобы освободить нагрузку.

**Пример**.

```json
"activities":[
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "BlobSource",
            },
            "sink": {
                "type": "AzureDataLakeStoreSink"
            },
            "parallelCopies": 32
        }
    }
]
```

## <a name="staged-copy"></a>промежуточное копирование

При копировании данных из исходного хранилища данных в хранилище приемников можно выбрать использование хранилища BLOB-объектов Azure или Azure Data Lake Storage 2-го поколения в качестве промежуточного хранилища. Промежуточное хранилище очень удобно в следующих ситуациях.

- **Вы хотите получать данные из различных хранилищ данных в Azure синапсе Analytics через Polybase, копировать данные из или в снежинки или принимать данные из Amazon RedShift/HDFS перформантли.** Дополнительные сведения:
  - [Используйте polybase для загрузки данных в Azure синапсе Analytics](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-synapse-analytics).
  - [Соединитель «снежинка»](connector-snowflake.md)
  - [Соединитель Amazon Redshift](connector-amazon-redshift.md)
  - [Соединитель HDFS](connector-hdfs.md)
- **Вы не хотите открывать порты, отличные от порта 80 и порта 443, в брандмауэре из-за корпоративных ИТ-политик.** Например, при копировании данных из локального хранилища данных в базу данных SQL Azure или Azure синапсе Analytics необходимо активировать исходящий TCP-трафик через порт 1433 как для брандмауэра Windows, так и для корпоративного брандмауэра. В этом сценарии поэтапное копирование может использовать локальную среду выполнения интеграции, чтобы сначала скопировать данные в промежуточное хранилище по протоколу HTTP или HTTPS через порт 443, а затем загрузить данные из промежуточного хранения в базу данных SQL или Azure синапсе Analytics. В таком сценарии не нужно включать порт 1433.
- **Иногда для выполнения гибридного перемещения данных (т. е. копирования из локального хранилища данных в облачное хранилище данных) через низкое сетевое подключение требуется некоторое время.** Для повышения производительности можно использовать промежуточное копирование для сжатия данных в локальной среде, чтобы уменьшить время на перемещение данных в промежуточное хранилище данных в облаке. Затем можно распаковать данные в промежуточном хранилище перед загрузкой в целевое хранилище данных.

### <a name="how-staged-copy-works"></a>Принцип промежуточного копирования

При активации функции промежуточного хранения данные копируются из исходного хранилища данных в промежуточное хранилище (с помощью собственного большого двоичного объекта Azure или Azure Data Lake Storage 2-го поколения). Далее данные копируются из промежуточного хранения в хранилище данных-приемник. Действие копирования фабрики данных Azure автоматически управляет процессом с двумя этапами, а также удаляет временные данные из промежуточного хранилища после завершения перемещения данных.

![промежуточное копирование](media/copy-activity-performance/staged-copy.png)

При активации перемещения данных с помощью промежуточного хранилища можно указать, следует ли сжимать данные перед перемещением данных из исходного хранилища данных в промежуточное и затем распаковать перед перемещением данных из промежуточного или промежуточного хранилища в хранилище данных-приемник.

В настоящее время нельзя копировать данные между двумя хранилищами данных, которые подключены через разные собственные данные IRs, ни с промежуточным копированием, так и без него. Для такого сценария можно настроить два явно связанных действия копирования для копирования из источника в промежуточную среду, а затем из промежуточного хранения в приемник.

### <a name="configuration"></a>Конфигурация

Настройте параметр **enableStaging** в действии копирования, чтобы указать, должны ли данные быть помещены в хранилище перед их загрузкой в целевое хранилище данных. При установке **enableStaging** в `TRUE` Укажите дополнительные свойства, перечисленные в следующей таблице. 

| Свойство. | Описание | Значение по умолчанию | Обязательно |
| --- | --- | --- | --- |
| enableStaging |Укажите, следует ли копировать данные в промежуточное хранилище. |False |нет |
| linkedServiceName |Укажите имя [хранилища BLOB-объектов Azure](connector-azure-blob-storage.md#linked-service-properties) или связанной службы [Azure Data Lake Storage 2-го поколения](connector-azure-data-lake-storage.md#linked-service-properties) , которая ссылается на экземпляр хранилища, используемый в качестве промежуточного хранилища. |Н/Д |Да, если для параметра **enableStaging** задано значение true |
| path |Укажите путь, который будет содержать промежуточные данные. Если не указать путь, служба создает контейнер для хранения временных данных. |Недоступно |Нет |
| enableCompression |Указывает, следует ли сжимать данные перед их копированием в место назначения. Этот параметр позволяет уменьшить объем передаваемых данных. |False |нет |

>[!NOTE]
> Если используется промежуточное копирование с включенным сжатием, проверка подлинности субъекта-службы или MSI для связанной службы BLOB-объектов не поддерживается.

Ниже приведен пример определения действия копирования со свойствами, описанными в предыдущей таблице.

```json
"activities":[
    {
        "name": "CopyActivityWithStaging",
        "type": "Copy",
        "inputs": [...],
        "outputs": [...],
        "typeProperties": {
            "source": {
                "type": "OracleSource",
            },
            "sink": {
                "type": "SqlDWSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": {
                    "referenceName": "MyStagingStorage",
                    "type": "LinkedServiceReference"
                },
                "path": "stagingcontainer/path"
            }
        }
    }
]
```

### <a name="staged-copy-billing-impact"></a>Принцип выставления счетов за промежуточное копирование

Плата наследуется на основе двух шагов: копирование длительности и типа копирования.

* При использовании промежуточного хранения во время копирования в облако, которое копирует данные из облачного хранилища данных в другое облачное хранилище данных, для обоих этапов, которые доставляются службой интеграции Azure, задается [сумма длительности копирования для шага 1 и шаг 2] x [Цена за единицу облачного копирования].
* При использовании промежуточного хранения во время гибридной копии, которая копирует данные из локального хранилища данных в облачное хранилище данных, на один этап, предоставляемый локальной средой выполнения интеграции, выставляются счета за [Длительность гибридной копии] x [Цена за единицу гибридного копирования] + [Длительность облачного копирования] x [Цена за единицу облачного копирования].

## <a name="next-steps"></a>Дальнейшие действия
См. другие статьи о действии копирования:

- [Действие копирования в фабрике данных Azure](copy-activity-overview.md)
- [Руководство по производительности и масштабируемости действия копирования](copy-activity-performance.md)
- [Устранение неполадок с производительностью действия копирования](copy-activity-performance-troubleshooting.md)
- [Перенос данных из хранилища данных в Azure с помощью фабрики данных Azure](data-migration-guidance-overview.md)
- [Миграция данных из Amazon S3 в службу хранилища Azure](data-migration-guidance-s3-azure-storage.md)
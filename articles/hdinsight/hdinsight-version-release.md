---
title: Обзор HDInsight 4.0 в Azure
description: Сравнение функций и ограничений HDInsight 3.6 и HDInsight 4.0, а также рекомендации по обновлению.
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/21/2020
ms.openlocfilehash: 694acc0005e90d8299d46528f83ba68ea3fcf1c0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "98931184"
---
# <a name="azure-hdinsight-40-overview"></a>Обзор Azure HDInsight 4.0

Azure HDInsight является одной из самых популярных служб среди корпоративных клиентов для Apache Hadoop и Apache Spark. HDInsight 4.0 является облачным дистрибутивом компонентов Apache Hadoop. Эта статья содержит сведения о последнем выпуске Azure HDInsight и инструкции по обновлению.

## <a name="whats-new-in-hdinsight-40"></a>Что нового в HDInsight 4.0?

### <a name="apache-hive-30-and-low-latency-analytical-processing"></a>Apache Hive 3.0 и аналитическая обработка с низкой задержкой

Для аналитической обработки с низкой задержкой (LLAP) Apache Hive используются постоянные серверы запросов и выполняющееся в памяти кэширование. Это ускоряет выполнение SQL-запросов к данным, размещенным в удаленном облачном хранилище. Hive LLAP использует набор постоянных управляющих программ, которые выполняют фрагменты запросов Hive. Выполнение запросов в LLAP аналогично выполнению запросов в Hive без LLAP: рабочие задачи выполняются в управляющих программах LLAP, а не в контейнерах.

Преимущества Hive LLAP:

* Возможность выполнять глубокую аналитику SQL без ущерба для производительности и адаптируемости. Например, сложные объединения, вложенные запросы, функции управления окнами, сортировка, пользовательские функции и сложные агрегаты.

* Интерактивные запросы к данным в том же хранилище, в котором выполняется их подготовка, что исключает необходимость перемещать данные из хранилища в другую подсистему для аналитической обработки.

* Кэширование результатов запроса позволяет повторно использовать ранее вычисленные результаты запроса. Кэш позволяет сэкономить время и ресурсы, затраченные на выполнение задач кластера, необходимых для запроса.

### <a name="hive-dynamic-materialized-views"></a>Динамические материализованные представления Hive

Hive теперь поддерживает динамические материализованные представления или предварительное вычисление соответствующих сводок. Эти представления ускоряют обработку запросов в хранилищах данных. Материализованные представления могут храниться в собственном коде в Hive и прозрачно использовать ускорение обработки LLAP.

### <a name="hive-transactional-tables"></a>Транзакционные таблицы Hive

HDI 4.0 включает Apache Hive 3. Для Hive 3 требуется соответствие требованиям к атомарности, согласованности, изоляции и устойчивости для транзакционных таблиц, которые размещены в хранилище данных Hive. Доступ к совместимым с ACID таблицам и табличным данным и управление ими осуществляет Hive. Данные в таблицах, поддерживающих операции создания, извлечения, обновления и удаления (CRUD), должны быть представлены в формате ORC. Таблицы только для вставки поддерживают все форматы файлов. 

> [!Note]
> Поддержка ACID и транзакций работает только для управляемых таблиц, а не для внешних таблиц. Внешние таблицы Hive разработаны таким образом, чтобы внешние стороны могли считывать и записывать данные таблиц, не перфоминг при этом изменения базовых данных. Для таблиц ACID Hive может изменять базовые данные с помощью сжатия и транзакций.

Ниже приведены некоторые преимущества таблиц ACID.

* В ACID версии 2 повышена производительность формата хранения и подсистемы выполнения.

* Компонент ACID включен по умолчанию для обеспечения полной поддержки обновления данных.

* Улучшенные возможности ACID дают возможность обновлять и удалять данные на уровне строки.

* Снижение производительности при этом отсутствует.

* Группирование не требуется.

* Служба Spark может считывать и записывать данные в совместимых с ACID таблицах Hive с помощью соединителя хранилища данных Hive.

Узнайте больше об [Apache Hive 3](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/hive-overview/content/hive_whats_new_in_this_release_hive.html).

### <a name="apache-spark"></a>Apache Spark

Apache Spark получает обновляемые таблицы и транзакции ACID с помощью соединителя хранилища данных Hive. Соединитель хранилища данных Hive позволяет зарегистрировать транзакционные таблицы Hive в качестве внешних таблиц в Spark для получения доступа ко всем функциям транзакций. Предыдущие версии поддерживали только оперирование секциями таблиц. Hive Warehouse Connector также поддерживает пакеты DataFrames для потоковой передачи данных.  В ходе этого процесса выполняется потоковая передача операций чтения и записи в транзакционные таблицы и таблицы потоковой передачи данных Hive из Spark.

Исполнители Spark могут подключаться непосредственно к управляющим программам Hive LLAP, чтобы извлекать и обновлять данные посредством транзакций, что позволяет Hive сохранить контроль над данными.

Apache Spark в HDInsight 4.0 поддерживает следующие сценарии:

* Обучение моделей машинного обучения с помощью транзакционной таблицы, используемой для создания отчетов.
* Безопасное добавление столбцов из Spark ML в таблицу Hive с помощью транзакций ACID.
* Запуск задания потоковой передачи Spark в канале изменений из таблицы потоковой передачи Hive.
* Создание ORC-файлов непосредственно в задании структурированной потоковой передачи Spark.

Вам больше не нужно беспокоиться о случайных попытках доступа к транзакционным таблицам Hive непосредственно из Spark, которые приводят к несогласованным результатам, возникновению повторяющихся данных или повреждению данных. В HDInsight 4.0 таблицы Spark и таблицы Hive хранятся в отдельных хранилищах метаданных. Используйте соединитель хранилища данных Hive, чтобы явно регистрировать транзакционные таблицы Hive как внешние таблицы Spark.

Узнайте больше об [Apache Spark](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/spark-overview/content/analyzing_data_with_apache_spark.html).

### <a name="apache-oozie"></a>Apache Oozie

Apache Oozie 4.3.1 входит в состав HDI 4.0 со следующими изменениями:

* Oozie больше не выполняет действия Hive. Интерфейс командной строки Hive удален и заменен BeeLine.

* Вы можете исключить нежелательные зависимости из общей библиотеки, добавив шаблон исключения в файл **job.properties**.

Узнайте больше об [Apache Oozie](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/patch_oozie.html).

## <a name="how-to-upgrade-to-hdinsight-40"></a>Как выполнить обновление до HDInsight 4.0

Важно тщательно протестировать компоненты перед внедрением последней версии в рабочей среде. После этого можно начать процесс обновления до версии HDInsight 4.0. HDInsight 3.6 является версией по умолчанию, позволяющей предотвратить случайные сбои.

Поддерживаемый способ обновления более ранних версий HDInsight до HDInsight 4.0 отсутствует. Из-за изменения форматов данных в хранилище метаданных и больших двоичных объектах версия 4.0 несовместима с предыдущими версиями. Важно отделить новую среду HDInsight 4.0 от текущей рабочей среды. Если развернуть HDInsight 4.0 в текущей среде, то хранилище метаданных будет обновлено и это действие станет необратимым.  

## <a name="limitations"></a>Ограничения

* HDInsight 4.0 не поддерживает MapReduce для Apache Hive. Вместо этого используйте Apache Tez. Узнайте больше об [Apache Tez](https://tez.apache.org/).
* HDInsight 4.0 не поддерживает Apache Storm.
* HDInsight 4,0 не поддерживает тип кластера служб ML.
* Представление Hive доступно только в кластерах HDInsight 4,0 с номером версии, превышающим или равным 4,1. Этот номер версии доступен в Ambari Admin-> версии.
* Интерпретатор оболочки в Apache Zeppelin не поддерживается в кластерах Spark и Interactive Query.
* Вы не можете *запретить* использование LLAP в кластере Spark LLAP. LLAP можно только выключить.
* Azure Data Lake Storage 2-го поколения не может сохранять записные книжки Jupyter в кластере Spark.
* Apache Pig работает в Tez по умолчанию, однако его можно заменить на MapReduce
* Интеграция Spark SQL Ranger для обеспечения безопасности строк и столбцов не рекомендуется
* Spark 2.4 и Kafka 2.1 доступны в HDInsight 4.0, поэтому Spark 2.3 и Kafka 1.1 больше не поддерживаются. Мы рекомендуем использовать Spark 2.4 и Kafka 2.1 и более поздних версий в HDInsight 4.0.

## <a name="next-steps"></a>Дальнейшие действия

* [Руководством по миграции HBase](./hbase/apache-hbase-migrate-new-version.md)
* [Руководством по миграции Hive](./interactive-query/apache-hive-migrate-workloads.md)
* [Kafka по миграции](./kafka/migrate-versions.md)
* [Руководством по миграции Spark](./spark/migrate-versions.md)
* [Документация по Azure HDInsight](index.yml)
* [Заметки о выпуске](hdinsight-release-notes.md)

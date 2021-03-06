---
title: Включение дампов кучи для служб Apache Hadoop в HDInsight в Azure
description: Включение дампов кучи для служб Apache Hadoop с кластеров HDInsight под управлением Linux для отладки и анализа.
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/02/2020
ms.openlocfilehash: fe5b2a1f083e246ea61854c9cbe03932e6655fdb
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104866596"
---
# <a name="enable-heap-dumps-for-apache-hadoop-services-on-linux-based-hdinsight"></a>Включение дампов кучи для служб Apache Hadoop в HDInsight под управлением Linux

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Дампы кучи содержат снимок памяти приложения, включая значения переменных на момент создания дампа. Они полезны для диагностики проблем, происходящих во время выполнения.

## <a name="services"></a>Службы

Вы можете включить дампы кучи для следующих служб:

* **Apache Hcatalog** — tempelton;
* **Apache Hive** — hiveserver2, metastore, derbyserver;
* **Mapreduce** — jobhistoryserver;
* **Apache Yarn** — resourcemanager, nodemanager, timelineserver;
* **Apache HDFS** — datanode, secondarynamenode, namenode.

Вы можете также включить дампы кучи для процессов сопоставления и уменьшения, запущенных HDInsight.

## <a name="understanding-heap-dump-configuration"></a>Основные сведения о настройке дампа кучи

Дампы кучи включаются путем передачи параметров в виртуальную машину Java при запуске службы. Чтобы передать эти параметры, для большинства служб [Apache Hadoop](https://hadoop.apache.org/) можно изменить скрипт оболочки, используемый для запуска службы.

В каждом скрипте есть экспорт, который содержит **\* \_** параметры, передаваемые в виртуальной машины Java. Например, в сценарии **hadoop-env.sh** строка, начинающаяся с `export HADOOP_NAMENODE_OPTS=`, содержит параметры для службы NameNode.

Процессы сопоставления и уменьшения немного отличаются друг от друга, поскольку эти операции являются дочерним процессом службы MapReduce. Каждый процесс сопоставления или уменьшения запускается в дочернем контейнере и существуют две записи, содержащие параметры виртуальной машины Java. Оба содержатся в файле **mapred-site.xml**:

* **MapReduce.Admin.map.child.Java.opts**
* **mapreduce.admin.reduce.child.java.opts**

> [!NOTE]  
> Мы рекомендуем использовать [Apache Ambari](https://ambari.apache.org/) , чтобы изменить как скрипты, так и параметры mapred-site.xml, так как Ambari обрабатывает репликацию изменений между узлами в кластере. Подробные сведения см. в разделе об [использовании Apache Ambari](#using-apache-ambari).

### <a name="enable-heap-dumps"></a>Включение дампов кучи

Следующий параметр включает дампы кучи при возникновении ошибки OutOfMemoryError:

`-XX:+HeapDumpOnOutOfMemoryError`

**+** Указывает, что этот параметр включен. По умолчанию — отключен.

> [!WARNING]  
> Дампы кучи для службы Hadoop в HDInsight не включены по умолчанию, поскольку файлы дампов могут быть очень большими. Включив их для устранения неполадок, не забудьте потом отключить, когда проблема будет воспроизведена и собраны файлы дампа.

### <a name="dump-location"></a>Расположение дампа

По умолчанию файл дампа располагается в текущем рабочем каталоге. Место сохранения файла можно выбрать, используя следующий параметр:

`-XX:HeapDumpPath=/path`

Например, с помощью `-XX:HeapDumpPath=/tmp` задается место для сохранения дампов в каталоге /tmp.

### <a name="scripts"></a>Скрипты

Вы можете также запустить сценарий при возникновении ошибки **OutOfMemoryError** . Например, можно запустить уведомление, благодаря которому можно будет узнать, что произошла ошибка. При возникновении ошибки __OutOfMemoryError__ используйте следующий параметр для запуска сценария:

`-XX:OnOutOfMemoryError=/path/to/script`

> [!NOTE]  
> Так как Apache Hadoop является распределенной системой, каждый используемый скрипт необходимо помещать во все узлы кластера, на которых запущена служба.
> 
> Сценарий также должен быть в расположении, доступном из учетной записи, с использованием которой запущена служба, и необходимо предоставить разрешение на выполнение. Например, вы можете сохранить сценарии в `/usr/local/bin` и использовать `chmod go+rx /usr/local/bin/filename.sh` для предоставления прав на чтение и выполнение.

## <a name="using-apache-ambari"></a>Использование Apache Ambari

Чтобы изменить конфигурацию службы, выполните следующие действия.

1. В веб-браузере перейдите на страницу `https://CLUSTERNAME.azurehdinsight.net`, где `CLUSTERNAME` — это имя вашего кластера.

2. Используя список слева, выберите область службы, которую требуется изменить. Например, **HDFS**. В центральной области выберите вкладку **Конфигурации** .

    :::image type="content" source="./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdi-service-config-tab.png" alt-text="Сеть Ambari с выбранной вкладкой конфигурации HDFS":::

3. Используя запись **Фильтр...**, введите **параметры**. Отобразятся только те элементы, в которых есть этот текст.

    :::image type="content" source="./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdinsight-filter-list.png" alt-text="Отфильтрованный список настройки Apache Ambari":::

4. Найдите запись **\* \_ для** службы, для которой нужно включить дампы кучи, и добавьте параметры, которые необходимо включить. На изображении ниже вы можете увидеть, что я добавил `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` в запись **HADOOP\_NAMENODE\_OPTS**:

    :::image type="content" source="./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hadoop-namenode-opts.png" alt-text="Apache Ambari Hadoop-namenode-намерена":::

   > [!NOTE]  
   > При включении дампов кучи для дочерних процессов сопоставления и уменьшения найдите поля **mapreduce.admin.map.child.java.opts** и **mapreduce.admin.reduce.child.java.opts**.

    Используйте кнопку **Сохранить** , чтобы сохранить изменения. Вы можете ввести краткое примечание, описывающее изменения.

5. После применения изменений появится значок **Требуется перезапуск** рядом с одной или несколькими службами.

    :::image type="content" source="./media/hdinsight-hadoop-collect-debug-heap-dump-linux/restart-required-icon.png" alt-text="Значок «Требуется перезапуск» и кнопка «Перезапуск»":::

6. Выберите все службы, которые требуют перезагрузки, и используйте кнопку **Действия службы** для **включения режима обслуживания**. Режим обслуживания не позволит создавать предупреждения от службы при ее перезапуске.

    :::image type="content" source="./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdi-maintenance-mode.png" alt-text="Включить меню режима обслуживания HDi":::

7. После включения режима обслуживания используйте кнопку **Перезапуск** для службы, чтобы **перезапустить все затронутые записи**.

    :::image type="content" source="./media/hdinsight-hadoop-collect-debug-heap-dump-linux/hdi-restart-all-button.png" alt-text="Apache Ambari перезапускает все затронутые записи":::

   > [!NOTE]  
   > Записи для кнопки **перезапуска** могут отличаться для других служб.

8. После перезапуска служб используйте кнопку **Действия службы** для **отключения режима обслуживания**. Эта Ambari для возобновления наблюдения за оповещениями для службы.
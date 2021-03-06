---
title: Масштабирование размеров кластера в Azure HDInsight
description: Гибкое масштабирование кластера Apache Hadoop в соответствии с рабочей нагрузкой в Azure HDInsight
ms.author: ashish
ms.service: hdinsight
ms.topic: how-to
ms.custom: seoapr2020
ms.date: 04/29/2020
ms.openlocfilehash: 6e6c692e8fc13d1703df44c99e9969ba4db5f119
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872104"
---
# <a name="scale-azure-hdinsight-clusters"></a>Масштабирование кластеров Azure HDInsight

HDInsight обеспечивает эластичность с помощью параметров для увеличения и уменьшения количества рабочих узлов в кластерах. Такая эластичность позволяет сжимать кластер после нескольких часов или выходных дней. И разверните его во время пиковых бизнес-требований.

Увеличьте масштаб кластера перед периодической пакетной обработкой, чтобы кластер получил достаточно ресурсов.  После завершения обработки и использования уменьшается масштаб кластера HDInsight до меньшего количества рабочих узлов.

Вы можете масштабировать кластер вручную, используя один из методов, описанных ниже. Можно также использовать параметры [автомасштабирования](hdinsight-autoscale-clusters.md) для автоматического увеличения и уменьшения масштаба в ответ на определенные метрики.

> [!NOTE]  
> Поддерживаются только кластеры HDInsight версии 3.1.3 или более поздней. Если вы не знаете версию кластера, см. страницу «Свойства».

## <a name="utilities-to-scale-clusters"></a>Служебные программы для масштабирования кластеров

Корпорация Майкрософт предоставляет следующие служебные программы для масштабирования кластеров:

|Служебная программа | Описание|
|---|---|
|[PowerShell Az](/powershell/azure)|[`Set-AzHDInsightClusterSize`](/powershell/module/az.hdinsight/set-azhdinsightclustersize) `-ClusterName CLUSTERNAME -TargetInstanceCount NEWSIZE`|
|[PowerShell AzureRM](/powershell/azure/azurerm) |[`Set-AzureRmHDInsightClusterSize`](/powershell/module/azurerm.hdinsight/set-azurermhdinsightclustersize) `-ClusterName CLUSTERNAME -TargetInstanceCount NEWSIZE`|
|[Azure CLI](/cli/azure/) | [`az hdinsight resize`](/cli/azure/hdinsight#az-hdinsight-resize) `--resource-group RESOURCEGROUP --name CLUSTERNAME --workernode-count NEWSIZE`|
|[Классический Azure CLI](hdinsight-administer-use-command-line.md)|`azure hdinsight cluster resize CLUSTERNAME NEWSIZE` |
|[Портал Azure](https://portal.azure.com)|Откройте панель кластера HDInsight, выберите **Размер кластера** в меню слева, затем на панели размер кластера введите число рабочих узлов и нажмите кнопку Сохранить.|  

:::image type="content" source="./media/hdinsight-scaling-best-practices/azure-portal-settings-nodes.png" alt-text="Параметр масштабирования кластера портал Azure":::

С помощью любого из этих методов можно увеличивать или уменьшать масштаб кластера HDInsight за считанные минуты.

> [!IMPORTANT]  
> * Классический интерфейс командной строки Azure является устаревшим и должен использоваться только с классической моделью развертывания. Для всех остальных развертываний используйте [Azure CLI](/cli/azure/).
> * Модуль PowerShell AzureRM является устаревшим.  При возможности используйте [модуль AZ](/powershell/azure/new-azureps-module-az) .

## <a name="impact-of-scaling-operations"></a>Влияние операций масштабирования

При **добавлении** узлов в кластер HDInsight (масштабирование) это не повлияет на работу заданий. Во время масштабирования можно безопасно передать новые задания. Если операция масштабирования завершается неудачно, произойдет сбой кластера в функциональном состоянии.

Если **Удалить** узлы (уменьшить масштаб), ожидающие или выполняющиеся задания завершатся ошибкой по завершении операции масштабирования. Эта ошибка вызвана тем, что некоторые службы перезапускаются во время процесса масштабирования. Кластер может зависнуть в защищенном режиме во время ручной операции масштабирования.

Ниже представлены возможности, связанные с изменением количества узлов данных в кластере каждого типа, поддерживаемого в HDInsight:

* Apache Hadoop

    Можно легко увеличить количество рабочих узлов в работающем кластере Hadoop, не влияя на какие-либо задания. В ходе выполнения операции можно также отправлять новые задания. Сбои в операции масштабирования корректно обрабатываются. Кластер всегда остается в функциональном состоянии.

    При уменьшении масштаба кластера Hadoop с меньшим числом узлов данных некоторые службы перезапускаются. Это приведет к сбою всех выполняющихся и ожидающих заданий при завершении операции масштабирования. Однако после завершения операции вы можете повторно отправить задания.

* Apache HBase

    Вы можете без проблем добавлять или удалять узлы в кластере HBase во время его работы. Балансировка региональных серверов выполняется автоматически в течение нескольких минут после завершения операции масштабирования. Однако можно вручную сбалансировать региональные серверы. Войдите в кластер головного узла и выполните следующие команды:

    ```bash
    pushd %HBASE_HOME%\bin
    hbase shell
    balancer
    ```

    Дополнительные сведения об использовании оболочки HBase см. в статье [Начало работы с примером Apache HBase в HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md).

* Apache Storm

    Вы можете без проблем добавлять или удалять узлы данных во время работы. Однако после успешного завершения операции масштабирования необходимо будет перераспределить топологию. Повторная балансировка позволяет топологии изменять [параметры параллелизма](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) на основе нового числа узлов в кластере. Чтобы повторно выполнить балансировку выполняющихся топологий, используйте один из следующих вариантов.

  * с помощью веб-интерфейса Storm;

    чтобы запустить балансировку для топологии из пользовательского интерфейса Storm, выполните следующие действия.

    1. Откройте `https://CLUSTERNAME.azurehdinsight.net/stormui` в веб-браузере, где `CLUSTERNAME` — это имя кластера с расширением. При появлении запроса введите имя администратора кластера HDInsight (admin) и пароль, указанный при создании кластера.

    1. Выберите топологию, для которой нужно выполнить повторную балансировку, и нажмите кнопку **Перераспределить**. Введите задержку до завершения операции перераспределения.

        :::image type="content" source="./media/hdinsight-scaling-best-practices/hdinsight-portal-scale-cluster-storm-rebalance.png" alt-text="Повторное распределение масштабирования в HDInsight":::

  * с помощью программы командной строки.

    подключитесь к серверу и запустите балансировку для топологии, используя следующую команду:

    ```bash
     storm rebalance TOPOLOGYNAME
    ```

    Можно также указать параметры для переопределения подсказок параллелизма, изначально предоставляемых топологией. Например, приведенный ниже код перестраивает `mytopology` топологию до 5 рабочих процессов, 3 исполнителей для spout компонента и 10 исполнителей для компонента с желтым молнией.

    ```bash
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

* Kafka

    выполните повторную балансировку реплик разделов после масштабирования. Дополнительные сведения см. в документе о [высоком уровне доступности данных при использовании Apache Kafka в HDInsight](./kafka/apache-kafka-high-availability.md).

* Apache Hive LLAP

    После масштабирования на `N` рабочие узлы HDInsight автоматически настроит следующие конфигурации и перезапустите Hive.

  * Максимальное число одновременных запросов: `hive.server2.tez.sessions.per.default.queue = min(N, 32)`
  * Число узлов, используемых LLAP Hive: `num_llap_nodes  = N`
  * Число узлов для запуска управляющей программы Hive LLAP: `num_llap_nodes_for_llap_daemons = N`

## <a name="how-to-safely-scale-down-a-cluster"></a>Безопасное масштабирование кластера

### <a name="scale-down-a-cluster-with-running-jobs"></a>Уменьшение масштаба кластера с выполняющимися заданиями

Чтобы избежать сбоя выполняющихся заданий во время операции уменьшения масштаба, можно попробовать выполнить три действия:

1. Дождитесь завершения заданий, прежде чем масштабировать кластер.
1. Завершите задания вручную.
1. Повторно отправьте задания после завершения операции масштабирования.

Чтобы просмотреть список ожидающих и выполняющихся заданий, можно использовать **Пользовательский интерфейс YARN диспетчер ресурсов**, выполнив следующие действия.

1. На [портале Azure](https://portal.azure.com/) выберите свой кластер.  Кластер откроется на новой странице портала.
2. В главном представлении перейдите к **панели мониторинга кластера**  >  **Ambari Домашняя страница**. Введите учетные данные кластера.
3. В пользовательском интерфейсе Ambari выберите **YARN** в списке служб в меню слева.  
4. На странице YARN выберите **быстрые ссылки** и наведите указатель мыши на активный головной узел, а затем выберите **Диспетчер ресурсов Пользовательский интерфейс**.

    :::image type="content" source="./media/hdinsight-scaling-best-practices/resource-manager-ui1.png" alt-text="Быстрые ссылки Apache Ambari диспетчер ресурсов UI":::

Вы можете получить прямой доступ к диспетчер ресурсов пользовательскому интерфейсу с помощью `https://<HDInsightClusterName>.azurehdinsight.net/yarnui/hn/cluster` .

Вы увидите список заданий и их текущее состояние. На снимке экрана в настоящее время выполняется одно задание:

:::image type="content" source="./media/hdinsight-scaling-best-practices/resourcemanager-ui-applications.png" alt-text="диспетчер ресурсов приложений пользовательского интерфейса":::

Чтобы вручную завершить работу запущенного приложения, из оболочки SSH выполните команду ниже:

```bash
yarn application -kill <application_id>
```

Например:

```bash
yarn application -kill "application_1499348398273_0003"
```

### <a name="getting-stuck-in-safe-mode"></a>Возникла проблема в безопасном режиме

При уменьшении масштаба кластера HDInsight использует интерфейсы управления Apache Ambari для первого списания дополнительных рабочих узлов. Узлы реплицируют свои блоки HDFS на другие сетевые рабочие узлы. После этого HDInsight будет безопасно масштабировать кластер. HDFS переходит в защищенный режим во время операции масштабирования. После завершения масштабирования должен быть создан HDFS. Однако в некоторых случаях HDFS зависает в защищенном режиме во время операции масштабирования из-за блокирования файла при репликации.

По умолчанию HDFS настроен с `dfs.replication` параметром 1, который определяет, сколько копий каждого блока файлов доступно. Каждая копия блока файла хранится на другом узле кластера.

Если ожидаемое число блочных копий недоступно, HDFS переходит в защищенный режим, а Ambari создает предупреждения. HDFS может войти в защищенный режим для операции масштабирования. Кластер может зависнуть в защищенном режиме, если для репликации не обнаружено необходимое количество узлов.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Примеры ошибок, когда включен безопасный режим

```output
org.apache.hadoop.hdfs.server.namenode.SafeModeException: Cannot create directory /tmp/hive/hive/819c215c-6d87-4311-97c8-4f0b9d2adcf0. Name node is in safe mode.
```

```output
org.apache.http.conn.HttpHostConnectException: Connect to active-headnode-name.servername.internal.cloudapp.net:10001 [active-headnode-name.servername. internal.cloudapp.net/1.1.1.1] failed: Connection refused
```

Вы можете изучить журналы узла имени в папке `/var/log/hadoop/hdfs/` примерно в то же время, когда было выполнено масштабирование кластера, чтобы проследить, когда был активирован безопасный режим. Файлам журнала присвоено имя `Hadoop-hdfs-namenode-<active-headnode-name>.*`.

Основная причина в том, что Hive зависит от временных файлов в HDFS во время выполнения запросов. Когда HDFS переходит в защищенный режим, Hive не может выполнять запросы, так как не может выполнить запись в HDFS. Временные файлы в HDFS находятся на локальном диске, подключенном к виртуальным машинам отдельных рабочих узлов. Файлы реплицируются между другими рабочими узлами в трех репликах, как минимум.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode"></a>Как предотвратить зависание HDInsight в защищенном режиме

Есть несколько способов предотвратить зависание HDInsight в безопасном режиме.

* Остановите все задания Hive перед уменьшением масштаба HDInsight. Кроме того, можно запланировать операцию уменьшения масштаба, чтобы избежать конфликта с запущенными заданиями Hive.
* Вручную очистите пустые файлы в каталоге `tmp` Hive в HDFS, а затем приступите к уменьшению масштаба.
* Выполните уменьшение масштаба HDInsight как минимум до трех рабочих узлов. Старайтесь избежать масштабирования до одного рабочего узла.
* При необходимости выполните команду, чтобы отключить безопасный режим.

В разделах ниже описываются эти параметры.

#### <a name="stop-all-hive-jobs"></a>Остановка всех заданий Hive

Остановите все задания Hive перед уменьшением масштаба до одного рабочего узла. Если запланирована рабочая нагрузка, выполните уменьшение масштаба после завершения работы Hive.

Остановка заданий Hive перед масштабированием помогает максимально снизить количество временных файлов в папке TMP (если они есть).

#### <a name="manually-clean-up-hives-scratch-files"></a>Очистка пустых файлов Hive вручную

Если при работе Hive остались временные файлы, их можно вручную очистить, а затем выполнить уменьшение масштаба, чтобы избежать активации безопасного режима.

1. Проверьте, какое расположение используется для временных файлов Hive, просмотрев `hive.exec.scratchdir` свойство конфигурации. Этот параметр задается в `/etc/hive/conf/hive-site.xml` :

    ```xml
    <property>
        <name>hive.exec.scratchdir</name>
        <value>hdfs://mycluster/tmp/hive</value>
    </property>
    ```

1. Остановите службы Hive и убедитесь, что выполнены все задания и запросы.

1. Список содержимого вспомогательного каталога, найденного выше, `hdfs://mycluster/tmp/hive/` чтобы узнать, содержит ли он какие либо файлы:

    ```bash
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```

    Ниже приведен пример выходных данных при наличии файлов:

    ```output
    sshuser@scalin:~$ hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/_tmp_space.db
    -rw-r--r--   3 hive hdfs         27 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info
    -rw-r--r--   3 hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.lck
    drwx------   - hive hdfs          0 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699
    -rw-r--r--   3 hive hdfs         26 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699/inuse.info
    ```

1. Если эти файлы обработаны Hive, их можно удалить. Убедитесь, что в Hive нет запросов, выполняемых на странице Yarn диспетчер ресурсов UI.

    Пример командной строки для удаления файлов из HDFS:

    ```bash
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```

#### <a name="scale-hdinsight-to-three-or-more-worker-nodes"></a>Масштабирование HDInsight до трех или более рабочих узлов

Если кластеры постоянно задерживаются в защищенном режиме при уменьшении масштаба до трех рабочих узлов, следует удерживать по крайней мере три рабочих узла.

Наличие трех рабочих узлов является более дорогостоящим, чем масштабирование до одного рабочего узла. Однако это действие не позволит вашему кластеру зависнуть в защищенном режиме.

### <a name="scale-hdinsight-down-to-one-worker-node"></a>Масштабирование HDInsight до одного рабочего узла

Даже если кластер масштабируется до одного узла, Рабочий узел 0 по-прежнему будет оставаться нестабильным. Рабочий узел 0 не может быть списан.

#### <a name="run-the-command-to-leave-safe-mode"></a>Выполнение команды для отключения безопасного режима

Последний вариант — выполнить команду "выйти из безопасного режима". Если HDFS вошел в защищенный режим из-за файла Hive в процессе репликации, выполните следующую команду, чтобы выйти из безопасного режима:

```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

### <a name="scale-down-an-apache-hbase-cluster"></a>Уменьшение масштаба кластера Apache HBase

Серверы регионов автоматически распределяются в течение нескольких минут после завершения операции масштабирования. Чтобы вручную сбалансировать серверы регионов, выполните следующие действия.

1. Подключитесь к кластеру HDInsight по протоколу SSH. Дополнительные сведения см. в статье [Использование SSH с Hadoop на основе Linux в HDInsight из Linux, Unix или OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Запустите оболочку HBase:

    ```bash
    hbase shell
    ```

3. Используйте команду ниже, чтобы вручную распределить нагрузку между региональными серверами:

    ```bash
    balancer
    ```

## <a name="next-steps"></a>Дальнейшие действия

* [Автоматическое масштабирование кластеров Azure HDInsight](hdinsight-autoscale-clusters.md)

Подробные сведения о масштабировании кластера HDInsight см. в следующих статьях.

* [Управление кластерами Apache Hadoop в HDInsight с помощью портала Azure](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Управление кластерами Apache Hadoop в HDInsight с помощью Azure CLI](hdinsight-administer-use-command-line.md#scale-clusters)
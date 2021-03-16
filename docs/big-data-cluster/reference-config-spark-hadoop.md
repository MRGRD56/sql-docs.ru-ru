---
title: Свойства конфигурации Apache Spark и Apache Hadoop
titleSuffix: SQL Server big data clusters
description: Справочная статья по свойствам конфигурации для Apache Spark и Apache Hadoop (HDFS).
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 08/04/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c6a73d8cf4a110990260db4917d565c33bd59766
ms.sourcegitcommit: 765262cdc6352a5325148afc22fa4f1499fe1aa3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514891"
---
# <a name="apache-spark--apache-hadoop-hdfs-configuration-properties"></a>Свойства конфигурации Apache Spark и Apache Hadoop (HDFS)

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Платформа "Кластеры больших данных" поддерживает настройку компонентов Apache Spark и Hadoop в областях служб и ресурсов как во время развертывания, так и после него. Для большинства параметров платформа использует те же значения конфигурации по умолчанию, что и соответствующий проект с открытым кодом. Ниже перечислены те параметры, которые мы будем изменять, а также приведены их описания и значения по умолчанию. Помимо ресурса шлюза между параметрами, которые можно настроить в области действия службы и в области действия ресурса, нет никаких различий.

Все возможные конфигурации и значения по умолчанию для каждой категории можно найти на соответствующем сайте документации по Apache.
- Apache Spark: https://spark.apache.org/docs/latest/configuration.html
- Apache Hadoop:
  - HDFS HDFS-Site: https://hadoop.apache.org/docs/r2.7.1/hadoop-project-dist/hadoop-hdfs/hdfs-default.xml
  - HDFS Core-Site: https://hadoop.apache.org/docs/r2.8.0/hadoop-project-dist/hadoop-common/core-default.xml  
  - Yarn: https://hadoop.apache.org/docs/r3.1.1/hadoop-yarn/hadoop-yarn-site/ResourceModel.html
- Hive: https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties#ConfigurationProperties-MetaStore
- Livy: https://github.com/cloudera/livy/blob/master/conf/livy.conf.template
- Шлюз Apache Knox: https://knox.apache.org/books/knox-0-14-0/user-guide.html#Gateway+Details

Параметры, настройку которых мы не поддерживаем, также перечислены ниже.

> [!NOTE]
> Чтобы включить Spark в пул носителей, задайте логическое значение `includeSpark` в файле конфигурации `bdc.json` в `spec.resources.storage-0.spec.settings.spark`. Инструкции см. в разделе [Настройка Apache Spark и Apache Hadoop в кластерах больших данных](configure-spark-hdfs.md).


##  <a name="big-data-clusters-specific-default-spark-settings"></a>Параметры Spark по умолчанию для кластеров больших данных
Ниже приведены параметры Spark и их значения по умолчанию, специфичные для платформы "Кластеры больших данных", которые пользователь может изменить. Сюда не входят параметры, управляемые системой.

| Имя параметра                                                                                 | Описание                                                                                                                                                           | Type   | Значение по умолчанию                                                                                                                              |
| ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ------------------------------------------------------------------------------------------------------------------------------------------ |
| capacity-scheduler.yarn.scheduler.capacity.maximum-applications                      | Максимальное число одновременно активных приложений в системе, включая выполняемые и находящиеся в состоянии ожидания.                                                               | INT    | 10000                                                                                                                                      |
| capacity-scheduler.yarn.scheduler.capacity.resource-calculator                       | Реализация ResourceCalculator, используемая для сравнения ресурсов в планировщике.                                                                               | строка | org.apache.hadoop.yarn.util.resource.DominantResourceCalculator                                                                            |
| capacity-scheduler.yarn.scheduler.capacity.root.queues                               | Планировщик ресурсов с предопределенной очередью с именем root.                                                                                                              | строка | default                                                                                                                                    |
| capacity-scheduler.yarn.scheduler.capacity.root.default.capacity                     | Емкость очереди в процентах (%) как минимальная емкость очереди ресурсов в абсолютном выражении для корневой очереди.                                                                          | INT    | 100                                                                                                                                        |
| spark-defaults-conf.spark.driver.cores                                               | Число ядер, используемых для процесса драйвера (только в режиме кластера).                                                                                                  | INT    | 1                                                                                                                                          |
| spark-defaults-conf.spark.driver.memoryOverhead                                      | Объем памяти вне кучи, выделяемой для каждого драйвера в режиме кластера.                                                                                             | INT    | 384                                                                                                                                        |
| spark-defaults-conf.spark.executor.instances                                         | Число исполнителей для статического выделения.                                                                                                                        | INT    | 1                                                                                                                                          |
| spark-defaults-conf.spark.executor.cores                                             | Число ядер, используемых для каждого исполнителя.                                                                                                                          | INT    | 1                                                                                                                                          |
| spark-defaults-conf.spark.driver.memory                                              | Объем памяти, используемый для процесса драйвера.                                                                                                                       | строка | 1 ГБ                                                                                                                                         |
| spark-defaults-conf.spark.executor.memory                                            | Объем памяти, используемый для каждого процесса исполнителя.                                                                                                                         | строка | 1 ГБ                                                                                                                                         |
| spark-defaults-conf.spark.executor.memoryOverhead                                    | Объем памяти вне кучи, выделяемой для каждого исполнителя.                                                                                                           | INT    | 384                                                                                                                                        |
| yarn-site.yarn.nodemanager.resource.memory-mb                                        | Объем физической памяти (в МБ), который может выделяться для контейнеров.                                                                                               | INT    | 8192                                                                                                                                       |
| yarn-site.yarn.scheduler.maximum-allocation-mb                                       | Максимальное выделение памяти для каждого запроса контейнера в диспетчере ресурсов.                                                                                           | INT    | 8192                                                                                                                                       |
| yarn-site.yarn.nodemanager.resource.cpu-vcores                                       | Количество ядер ЦП, которое может быть выделено для контейнеров.                                                                                                             | INT    | 32                                                                                                                                         |
| yarn-site.yarn.scheduler.maximum-allocation-vcores                                   | Максимальное выделение памяти для каждого запроса контейнера в диспетчере ресурсов, выраженное в количестве виртуальных ядер ЦП.                                                            | INT    | 8                                                                                                                                          |
| yarn-site.yarn.nodemanager.linux-container-executor.secure-mode.pool-user-count      | Число пользователей пула для исполнителя контейнера Linux в безопасном режиме.                                                                                             | INT    | 6                                                                                                                                          |
| yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent                        | Максимальный процент ресурсов в кластере, который может использоваться для запуска мастер-приложений.                                                                              | FLOAT  | 0.1                                                                                                                                        |
| yarn-site.yarn.nodemanager.container-executor.class                                  | Исполнители контейнеров для конкретных операционных систем.                                                                                                               | строка | org.apache.hadoop.yarn.server.nodemanager.LinuxContainerExecutor                                                                           |
| capacity-scheduler.yarn.scheduler.capacity.root.default.user-limit-factor            | Кратность емкости очереди, которую можно настроить для разрешения одному пользователю запрашивать дополнительные ресурсы.                                                          | INT    | 1                                                                                                                                          |
| capacity-scheduler.yarn.scheduler.capacity.root.default.maximum-capacity             | Максимальная емкость очереди в процентах (%) с плавающей запятой или в абсолютном выражении. Значение –1 для этого параметра указывает на максимальную емкость 100 %.           | INT    | 100                                                                                                                                        |
| capacity-scheduler.yarn.scheduler.capacity.root.default.state                        | Состояние очереди может принимать значения Running (Выполняется) и Stopped (Остановлено).                                                                                                                      | строка | RUNNING                                                                                                                                    |
| capacity-scheduler.yarn.scheduler.capacity.root.default.maximum-application-lifetime | Максимальное время (в секундах) существования приложения, которое отправляется в очередь. Любое значение меньше нуля или равное нулю, означает отключение этого параметра.                     | INT    | -1                                                                                                                                         |
| capacity-scheduler.yarn.scheduler.capacity.root.default.default-application-lifetime | Время по умолчанию (в секундах) существования приложения, которое отправляется в очередь. Любое значение меньше нуля или равное нулю, означает отключение этого параметра.                     | INT    | -1                                                                                                                                         |
| capacity-scheduler.yarn.scheduler.capacity.node-locality-delay                       | Количество пропущенных возможностей планирования, после которого CapacityScheduler пытается запланировать контейнеры локально в стойке.                                               | INT    | 40                                                                                                                                         |
| capacity-scheduler.yarn.scheduler.capacity.rack-locality-additional-delay            | Количество пропущенных возможностей планирования сверх значения node-locality-delay, после которого CapacityScheduler пытается запланировать контейнеры вне коммутатора. | INT    | -1                                                                                                                                         |
| hadoop-env.HADOOP_HEAPSIZE_MAX                                                       | Максимальный размер кучи по умолчанию для всех процессов виртуальной машины Java Hadoop.                                                                                                                 | INT    | 2048                                                                                                                                       |
| yarn-env.YARN_RESOURCEMANAGER_HEAPSIZE                                               | Размер кучи для диспетчера ресурсов Yarn.                                                                                                                                     | INT    | 2048                                                                                                                                       |
| yarn-env.YARN_NODEMANAGER_HEAPSIZE                                                   | Размер кучи для диспетчера узлов Yarn.                                                                                                                                         | INT    | 2048                                                                                                                                       |
| mapred-env.HADOOP_JOB_HISTORYSERVER_HEAPSIZE                                         | Размер кучи для сервера журнала заданий Hadoop.                                                                                                                                 | INT    | 2048                                                                                                                                       |
| hive-env.HADOOP_HEAPSIZE                                                             | Размер кучи Hadoop для Hive.                                                                                                                                          | INT    | 2048                                                                                                                                       |
| livy-conf.livy.server.session.timeout-check                                          | Проверка времени ожидания сеанса для сервера Livy.                                                                                                                                | bool   | Да                                                                                                                                       |
| livy-conf.livy.server.session.timeout-check.skip-busy                                | Режим "пропускать, если занято" для проверки времени ожидания сеанса сервера Livy.                                                                                                                  | bool   | Да                                                                                                                                       |
| livy-conf.livy.server.session.timeout                                                | Время ожидания для сеанса сервера Livy (в мс, с, | мин, ч, д, г).                                                                                                              | строка | 2 ч                                                                                                                                         |
| livy-conf.livy.server.yarn.poll-interval                                             | Интервал опроса для Yarn на сервере Livy (в мс, с, | мин, ч, д, г).                                                                                                     | строка | 500 мс                                                                                                                                      |
| livy-conf.livy.rsc.jars                                                              | JAR-файлы RSC для Livy.                                                                                                                                                        | строка | local:/opt/livy/rsc-jars/livy-api.jar,local:/opt/livy/rsc-jars/livy-rsc.jar,local:/opt/livy/rsc-jars/netty-all.jar                         |
| livy-conf.livy.repl.jars                                                             | JAR-файлы репликации Livy.                                                                                                                                                       | строка | local:/opt/livy/repl_2.11-jars/livy-core.jar,local:/opt/livy/repl_2.11-jars/livy-repl.jar,local:/opt/livy/repl_2.11-jars/commons-codec.jar |
| livy-conf.livy.rsc.sparkr.package                                                    | Пакет SparkR для Livy RSC.                                                                                                                                              | строка | hdfs:///system/livy/sparkr.zip                                                                                                             |
| livy-env.LIVY_SERVER_JAVA_OPTS                                                       | Параметры Java для сервера Livy.                                                                                                                                             | строка | -Xmx2g                                                                                                                                     |
| spark-defaults-conf.spark.r.backendConnectionTimeout                                 | Время ожидания подключения, используемое процессом R для подключения к RBackend (в секундах).                                                                                         | INT    | 86400                                                                                                                                      |
| spark-defaults-conf.spark.pyspark.python                                             | Параметр Python для Spark.                                                                                                                                              | строка | /opt/bin/python3                                                                                                                           |
| spark-defaults-conf.spark.yarn.jars                                                  | JAR-файлы Yarn.                                                                                                                                                            | строка | local:/opt/spark/jars/*                                                                                                                    |
| spark-history-server-conf.spark.history.fs.cleaner.maxAge                            | Максимальный возраст для файлов журнала заданий, после которого они удаляются средством очистки журналов файловой системы (в мс, с, | мин, ч, д, г).                                                   | строка | 7 дн.                                                                                                                                         |
| spark-history-server-conf.spark.history.fs.cleaner.interval                          | Интервал очистки журнала Spark (в мс, с, | мин, ч, д, г).                                                                                                        | строка | 12 ч                                                                                                                                        |
| hadoop-env.HADOOP_CLASSPATH                                                          | Задает дополнительный путь к классам для Hadoop.                                                                                                                                 | строка |                                                                                                                                            |
| spark-env.SPARK_DAEMON_MEMORY                                                        | Память управляющей программы Spark.                                                                                                                                                  | строка | 2 ГБ                                                                                                                                         |
| yarn-site.yarn.log-aggregation.retain-seconds                                        | Если включено объединение журналов, это свойство определяет количество секунд, в течение которых должны храниться журналы.                                                                       | INT    | 604800                                                                                                                                     |
| yarn-site.yarn.nodemanager.log-aggregation.compression-type                          | Тип сжатия для объединения журналов в диспетчере узлов Yarn.                                                                                                            | строка | gz                                                                                                                                         |
| yarn-site.yarn.nodemanager.log-aggregation.roll-monitoring-interval-seconds          | Интервал в секундах для мониторинга наката журналов при объединении в диспетчере узлов.                                                                                                  | INT    | 3600                                                                                                                                       |
| yarn-site.yarn.scheduler.minimum-allocation-mb                                       | Минимальное выделение памяти для каждого запроса контейнера в диспетчере ресурсов (в МБ).                                                                                   | INT    | 512                                                                                                                                        |
| yarn-site.yarn.scheduler.minimum-allocation-vcores                                   | Минимальное выделение памяти для каждого запроса контейнеров в диспетчере ресурсов, выраженное в количестве виртуальных ядер ЦП.                                                             | INT    | 1                                                                                                                                          |
| yarn-site.yarn.nm.liveness-monitor.expiry-interval-ms                                | Время ожидания, по истечении которого диспетчер узлов считается неработающим.                                                                                                             | INT    | 180000                                                                                                                                     |
| yarn-site.yarn.resourcemanager.zk-timeout-ms                                         | Время ожидания сеанса ZooKeeper (в миллисекундах).                                                                                                                          | INT    | 40 000                                                                                                                                      |
| capacity-scheduler.yarn.scheduler.capacity.root.default.acl_application_max_priority | Список управления доступом для пользователей, которые могут отправлять приложения с настроенным приоритетом. Например, [user={name} group={name} max_priority={priority} default_priority={priority}].             | строка | *                                                                                                                                          |
| includeSpark                                                                         | Логическое значение, которое определяет возможность выполнения заданий Spark в пуле носителей.                                                                                           | bool   | Да                                                                                                                                       |
| enableSparkOnK8s                                                                     | Логическое значение, которое определяет использование Spark в Kubernetes, что позволяет добавить контейнеры для Kubernetes в заголовок Spark.                                                               | bool   | false                                                                                                                                      |
| sparkVersion                                                                         | Версия Spark.                                                                                                                                                  | строка | 2.4                                                                                                                                        |
| spark-env.PYSPARK_ARCHIVES_PATH                                                      | Путь к JAR-файлам архива PySpark, которые используются в заданиях Spark.                                                                                                                      | строка | local:/opt/spark/python/lib/pyspark.zip,local:/opt/spark/python/lib/py4j-0.10.7-src.zip                                                    |

В следующих разделах указаны неподдерживаемые конфигурации.

##  <a name="big-data-clusters-specific-default-hdfs-settings"></a>Параметры HDFS по умолчанию для кластеров больших данных
Ниже приведены параметры HDFS и их значения по умолчанию, специфичные для платформы "Кластеры больших данных", которые пользователь может изменить. Сюда не входят параметры, управляемые системой.

| Имя параметра                                                                       | Описание                                                                                         | Type   | Значение по умолчанию                                                                    |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------ | -------------------------------------------------------------------------------------- |
| hdfs-site.dfs.replication                                                  | Репликация блоков по умолчанию.                                                                          | INT    | 2                                                                                      |
| hdfs-site.dfs.namenode.provided.enabled                                    | Разрешает узлу имени обрабатывать предоставленные хранилища.                                                   | bool   | Да                                                                                   |
| hdfs-site.dfs.datanode.provided.enabled                                    | Разрешает узлу данных обрабатывать предоставленные хранилища.                                                   | bool   | Да                                                                                   |
| hdfs-site.dfs.datanode.provided.volume.lazy.load                           | Разрешает отложенную загрузку на узле данных для предоставленных хранилищ.                                                 | bool   | Да                                                                                   |
| hdfs-site.dfs.provided.aliasmap.inmemory.enabled                           | Разрешает карту псевдонимов в памяти для предоставленных хранилищ.                                                     | bool   | Да                                                                                   |
| hdfs-site.dfs.provided.aliasmap.class                                      | Класс, используемый для указания формата входных данных для блоков в предоставленных хранилищах.              | строка | org.apache.hadoop.hdfs.server.common.blockaliasmap.impl.InMemoryLevelDBAliasMapClient  |
| hdfs-site.dfs.namenode.provided.aliasmap.class                             | Класс, используемый для указания формата входных данных для блоков в предоставленных хранилищах узла имен. | строка | org.apache.hadoop.hdfs.server.common.blockaliasmap.impl.NamenodeInMemoryAliasMapClient |
| hdfs-site.dfs.provided.aliasmap.load.retries                               | Число повторных попыток на узле данных при загрузке предоставленной карты псевдонимов.                                    | INT    | 0                                                                                      |
| hdfs-site.dfs.provided.aliasmap.inmemory.batch-size                        | Размер пакета при переборе базы данных, в которой хранится карта псевдонимов.                               | INT    | 500                                                                                    |
| hdfs-site.dfs.datanode.provided.volume.readthrough                         | Включение сквозного чтения для предоставленных хранилищ на узле данных.                                               | bool   | Да                                                                                   |
| hdfs-site.dfs.provided.cache.capacity.mount                                | Включение подключения емкости кэша для предоставленных хранилищ.                                                  | bool   | Да                                                                                   |
| hdfs-site.dfs.provided.overreplication.factor                              | Коэффициент перерепликации для предоставленных хранилищ.                                                       | FLOAT  | 1                                                                                      |
| hdfs-site.dfs.provided.cache.capacity.fraction                             | Доля емкости кэша для предоставленного хранилища.                                                       | FLOAT  | 0,01                                                                                   |
| hdfs-site.dfs.ls.limit                                                     | Ограничение числа файлов, выводимых командой ls.                                                            | INT    | 500                                                                                    |
| hdfs-env.HDFS_NAMENODE_OPTS                                                | Параметры узла имен HDFS.                                                                              | строка | -Dhadoop.security.logger=INFO,RFAS -Xmx2g                                              |
| hdfs-env.HDFS_DATANODE_OPTS                                                | Параметры узла данных HDFS.                                                                              | строка | -Dhadoop.security.logger=ERROR,RFAS -Xmx2g                                             |
| hdfs-env.HDFS_ZKFC_OPTS                                                    | Параметры HDFS ZKFC.                                                                                  | строка | -Xmx1g                                                                                 |
| hdfs-env.HDFS_JOURNALNODE_OPTS                                             | Параметры узла журналов HDFS.                                                                           | строка | -Xmx2g                                                                                 |
| hdfs-env.HDFS_AUDIT_LOGGER                                                 | Параметры средства ведения журнала аудита HDFS.                                                                          | строка | INFO,RFAAUDIT                                                                          |
| core-site.hadoop.security.group.mapping.ldap.search.group.hierarchy.levels | Уровни иерархии для группы поиска Hadoop LDAP для основного сайта.                                            | INT    | 10                                                                                     |
| core-site.fs.permissions.umask-mode                                        | Режим umask для разрешений.                                                                              | строка | 077                                                                                    |
| core-site.hadoop.security.kms.client.failover.max.retries                  | Максимальное число повторных попыток при отработке отказа клиента.                                                                    | INT    | 20                                                                                     || zoo-cfg.tickTime                                       | Время такта для конфигурации ZooKeeper.                         | INT    | 2000                |
| zoo-cfg.initLimit                                      | Время инициализации для конфигурации ZooKeeper.                         | INT    | 10                  |
| zoo-cfg.syncLimit                                      | Время синхронизации для конфигурации ZooKeeper.                         | INT    | 5                   |
| zoo-cfg.maxClientCnxns                                 | Максимальное число клиентских подключений для конфигурации ZooKeeper.            | INT    | 60                  |
| zoo-cfg.minSessionTimeout                              | Минимальное время ожидания сеанса для конфигурации ZooKeeper.           | INT    | 4000                |
| zoo-cfg.maxSessionTimeout                              | Максимальное время ожидания сеанса для конфигурации ZooKeeper.           | INT    | 40 000               |
| zoo-cfg.autopurge.snapRetainCount                      | Число сохраненных прикреплений для конфигурации автоочистки ZooKeeper.       | INT    | 3                   |
| zoo-cfg.autopurge.purgeInterval                        | Интервал очистки для конфигурации автоочистки ZooKeeper.          | INT    | 0                   |
| zookeeper-java-env.JVMFLAGS                            | Флаги виртуальной машины Java для среды Java в ZooKeeper.            | строка | -Xmx1G -Xms1G       |
| zookeeper-log4j-properties.zookeeper.console.threshold | Пороговое значение для консоли log4j в ZooKeeper.               | строка | ИНФОРМАЦИЯ                |
| zoo-cfg.zookeeper.request.timeout                      | Управляет временем ожидания запроса ZooKeeper (в миллисекундах). | INT    | 40 000               |
| kms-site.hadoop.security.kms.encrypted.key.cache.size | Размер кэша для зашифрованного ключа в Hadoop KMS. | INT  | 500 |
    

##  <a name="big-data-clusters-specific-default-gateway-settings"></a>Параметры шлюза по умолчанию для платформы "Кластеры больших данных"
Ниже приведены параметры шлюза и их значения по умолчанию, специфичные для платформы "Кластеры больших данных", которые пользователь может изменить. Сюда не входят параметры, управляемые системой. Параметры шлюза можно настроить только в области действия для **ресурса**.

| Имя параметра                                          | Описание                                            | Type   | Значение по умолчанию |
| --------------------------------------------- | ------------------------------------------------------ | ------ | ------------------- |
| gateway-site.gateway.httpclient.socketTimeout | Время ожидания сокета для HTTP-клиента в шлюзе (в мс, с, мин). | строка | 90 c                 |
| gateway-site.sun.security.krb5.debug          | Отладка для безопасности Kerberos.                           | bool   | Да                |
| knox-env.KNOX_GATEWAY_MEM_OPTS                | Параметры памяти для шлюза Knox.                           | строка | -Xmx2g              |

## <a name="unsupported-spark-configurations"></a>Неподдерживаемые конфигурации Spark

Перечисленные ниже конфигурации `spark` не поддерживаются и не могут быть изменены в контексте кластера больших данных.

| Категория  | Подкатегория               | Файл                       | Неподдерживаемые конфигурации                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|           | yarn-site                  | yarn-site.xml              | yarn.log-aggregation-enable                                             |
|           |                            |                            | yarn.log.server.url                                                     |
|           |                            |                            | yarn.nodemanager.pmem-check-enabled                                     |
|           |                            |                            | yarn.nodemanager.vmem-check-enabled                                     |
|           |                            |                            | yarn.nodemanager.aux-services                                           |
|           |                            |                            | yarn.resourcemanager.address                                            |
|           |                            |                            | yarn.nodemanager.address                                                |
|           |                            |                            | yarn.client.failover-no-ha-proxy-provider                               |
|           |                            |                            | yarn.client.failover-proxy-provider                                     |
|           |                            |                            | yarn.http.policy                                                        |
|           |                            |                            | yarn.nodemanager.linux-container-executor.secure-mode.use-pool-user     |
|           |                            |                            | yarn.nodemanager.linux-container-executor.secure-mode.pool-user-prefix  |
|           |                            |                            | yarn.nodemanager.linux-container-executor.nonsecure-mode.local-user     |
|           |                            |                            | yarn.acl.enable                                                         |
|           |                            |                            | yarn.admin.acl                                                          |
|           |                            |                            | yarn.resourcemanager.hostname                                           |
|           |                            |                            | yarn.resourcemanager.principal                                          |
|           |                            |                            | yarn.resourcemanager.keytab                                             |
|           |                            |                            | yarn.resourcemanager.webapp.spnego-keytab-file                          |
|           |                            |                            | yarn.resourcemanager.webapp.spnego-principal                            |
|           |                            |                            | yarn.nodemanager.principal                                              |
|           |                            |                            | yarn.nodemanager.keytab                                                 |
|           |                            |                            | yarn.nodemanager.webapp.spnego-keytab-file                              |
|           |                            |                            | yarn.nodemanager.webapp.spnego-principal                                |
|           |                            |                            | yarn.resourcemanager.ha.enabled                                         |
|           |                            |                            | yarn.resourcemanager.cluster-id                                         |
|           |                            |                            | yarn.resourcemanager.zk-address                                         |
|           |                            |                            | yarn.resourcemanager.ha.rm-ids                                          |
|           |                            |                            | yarn.resourcemanager.hostname.*                                         |
|           | capacity-scheduler         | capacity-scheduler.xml     | yarn.scheduler.capacity.root.acl_submit_applications                    |
|           |                            |                            | yarn.scheduler.capacity.root.acl_administer_queue                       |
|           |                            |                            | yarn.scheduler.capacity.root.default.acl_application_max_priority       |
|           | yarn-env                   | yarn-env.sh                |                                                                         |
|           | spark-defaults-conf        | spark-defaults.conf        | spark.yarn.archive                                                      |
|           |                            |                            | spark.yarn.historyServer.address                                        |
|           |                            |                            | spark.eventLog.enabled                                                  |
|           |                            |                            | spark.eventLog.dir                                                      |
|           |                            |                            | spark.sql.warehouse.dir                                                 |
|           |                            |                            | spark.sql.hive.metastore.version                                        |
|           |                            |                            | spark.sql.hive.metastore.jars                                           |
|           |                            |                            | spark.extraListeners                                                    |
|           |                            |                            | spark.metrics.conf                                                      |
|           |                            |                            | spark.ssl.enabled                                                       |
|           |                            |                            | spark.authenticate                                                      |
|           |                            |                            | spark.network.crypto.enabled                                            |
|           |                            |                            | spark.ssl.keyStore                                                      |
|           |                            |                            | spark.ssl.keyStorePassword  
|           |                            |                            | spark.ui.enabled                                                        |
|           | spark-env                  | spark-env.sh               | SPARK_NO_DAEMONIZE                                                      |
|           |                            |                            | SPARK_DIST_CLASSPATH                                                    |
|           | spark-history-server-conf  | spark-history-server.conf  | spark.history.fs.logDirectory                                           |
|           |                            |                            | spark.ui.proxyBase                                                      |
|           |                            |                            | spark.history.fs.cleaner.enabled                                        |
|           |                            |                            | spark.ssl.enabled                                                       |
|           |                            |                            | spark.authenticate                                                      |
|           |                            |                            | spark.network.crypto.enabled                                            |
|           |                            |                            | spark.ssl.keyStore                                                      |
|           |                            |                            | spark.ssl.keyStorePassword                                              |
|           |                            |                            | spark.history.kerberos.enabled                                          |
|           |                            |                            | spark.history.kerberos.principal                                        |
|           |                            |                            | spark.history.kerberos.keytab                                           |
|           |                            |                            | spark.ui.filters                                                        |
|           |                            |                            | spark.acls.enable                                                       |
|           |                            |                            | spark.history.ui.acls.enable                                            |
|           |                            |                            | spark.history.ui.admin.acls                                             |
|           |                            |                            | spark.history.ui.admin.acls.groups                                      |
|           | livy-conf                  | livy.conf                  | livy.keystore                                                           |
|           |                            |                            | livy.keystore.password                                                  |
|           |                            |                            | livy.spark.master                                                       |
|           |                            |                            | livy.spark.deploy-mode                                                  |
|           |                            |                            | livy.rsc.jars                                                           |
|           |                            |                            | livy.repl.jars                                                          |
|           |                            |                            | livy.rsc.pyspark.archives                                               |
|           |                            |                            | livy.rsc.sparkr.package                                                 |
|           |                            |                            | livy.repl.enable-hive-context                                           |
|           |                            |                            | livy.superusers                                                         |
|           |                            |                            | livy.server.auth.type                                                   |
|           |                            |                            | livy.server.launch.kerberos.keytab                                      |
|           |                            |                            | livy.server.launch.kerberos.principal                                   |
|           |                            |                            | livy.server.auth.kerberos.principal                                     |
|           |                            |                            | livy.server.auth.kerberos.keytab                                        |
|           |                            |                            | livy.impersonation.enabled                                              |
|           |                            |                            | livy.server.access-control.enabled                                      |
|           |                            |                            | livy.server.access-control.*                                            |
|           | livy-env                   | livy-env.sh                |                                                                         |
|           | hive-site                  | Hive-site.xml              | javax.jdo.option.ConnectionURL                                          |
|           |                            |                            | javax.jdo.option.ConnectionDriverName                                   |
|           |                            |                            | javax.jdo.option.ConnectionUserName                                     |
|           |                            |                            | javax.jdo.option.ConnectionPassword                                     |
|           |                            |                            | hive.metastore.uris                                                     |
|           |                            |                            | hive.metastore.pre.event.listeners                                      |
|           |                            |                            | hive.security.authorization.enabled                                     |
|           |                            |                            | hive.security.metastore.authenticator.manager                           |
|           |                            |                            | hive.security.metastore.authorization.manager                           |
|           |                            |                            | hive.metastore.use.SSL                                                  |
|           |                            |                            | hive.metastore.keystore.path                                            |
|           |                            |                            | hive.metastore.keystore.password                                        |
|           |                            |                            | hive.metastore.truststore.path                                          |
|           |                            |                            | hive.metastore.truststore.password                                      |
|           |                            |                            | hive.metastore.kerberos.keytab.file                                     |
|           |                            |                            | hive.metastore.kerberos.principal                                       |
|           |                            |                            | hive.metastore.sasl.enabled                                             |
|           |                            |                            | hive.metastore.execute.setugi                                           |
|           |                            |                            | hive.cluster.delegation.token.store.class                               |
|           | hive-env                   | hive-env.sh                

## <a name="unsupported-hdfs-configurations"></a>Неподдерживаемые конфигурации HDFS

Перечисленные ниже конфигурации `hdfs` не поддерживаются и не могут быть изменены в контексте кластера больших данных.

| Категория  | Подкатегория               | Файл                       | Неподдерживаемые конфигурации                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|          | core-site                   | core-site.xml                 | fs.defaultFS                                          |
|          |                             |                               | ha.zookeeper.quorum                                   |
|          |                             |                               | hadoop.tmp.dir                                        |
|          |                             |                               | hadoop.rpc.protection                                 |
|          |                             |                               | hadoop.security.auth_to_local                         |
|          |                             |                               | hadoop.security.authentication                        |
|          |                             |                               | hadoop.security.authorization                         |
|          |                             |                               | hadoop.http.authentication.simple.anonymous.allowed   |
|          |                             |                               | hadoop.http.authentication.type                       |
|          |                             |                               | hadoop.http.authentication.kerberos.principal         |
|          |                             |                               | hadoop.http.authentication.kerberos.keytab            |
|          |                             |                               | hadoop.http.filter.initializers                       |
|          |                             |                               | hadoop.security.group.mapping.*                       |                               |
|          |                             |                               | hadoop.security.key.provider.path                     |                               |
|          | mapred-env                  | mapred-env.sh                 |                                                       |
|          | hdfs-site                   | hdfs-site.xml                 | dfs.namenode.name.dir                                 |
|          |                             |                               | dfs.datanode.data.dir                                 |
|          |                             |                               | dfs.namenode.acls.enabled                             |
|          |                             |                               | dfs.namenode.datanode.registration.ip-hostname-check  |
|          |                             |                               | dfs.client.retry.policy.enabled                       |
|          |                             |                               | dfs.permissions.enabled                               |
|          |                             |                               | dfs.nameservices                                      |
|          |                             |                               | dfs.ha.namenodes.nmnode-0                             |
|          |                             |                               | dfs.namenode.rpc-address.nmnode-0.*                   |
|          |                             |                               | dfs.namenode.shared.edits.dir                         |
|          |                             |                               | dfs.ha.automatic-failover.enabled                     |
|          |                             |                               | dfs.ha.fencing.methods                                |
|          |                             |                               | dfs.journalnode.edits.dir                             |
|          |                             |                               | dfs.client.failover.proxy.provider.nmnode-0           |
|          |                             |                               | dfs.namenode.http-address                             |
|          |                             |                               | dfs.namenode.httpS-address                            |
|          |                             |                               | dfs.http.policy                                       |
|          |                             |                               | dfs.encrypt.data.transfer                             |
|          |                             |                               | dfs.block.access.token.enable                         |
|          |                             |                               | dfs.data.transfer.protection                          |
|          |                             |                               | dfs.encrypt.data.transfer.cipher.suites               |
|          |                             |                               | dfs.https.port                                        |
|          |                             |                               | dfs.namenode.keytab.file                              |
|          |                             |                               | dfs.namenode.kerberos.principal                       |
|          |                             |                               | dfs.namenode.kerberos.internal.spnego.principal       |
|          |                             |                               | dfs.datanode.data.dir.perm                            |
|          |                             |                               | dfs.datanode.address                                  |
|          |                             |                               | dfs.datanode.http.address                             |
|          |                             |                               | dfs.datanode.ipc.address                              |
|          |                             |                               | dfs.datanode.https.address                            |
|          |                             |                               | dfs.datanode.keytab.file                              |
|          |                             |                               | dfs.datanode.kerberos.principal                       |
|          |                             |                               | dfs.journalnode.keytab.file                           |
|          |                             |                               | dfs.journalnode.kerberos.principal                    |
|          |                             |                               | dfs.journalnode.kerberos.internal.spnego.principal    |
|          |                             |                               | dfs.web.authentication.kerberos.keytab                |
|          |                             |                               | dfs.web.authentication.kerberos.principal             |
|          |                             |                               | dfs.webhdfs.enabled                                   |
|          |                             |                               | dfs.permissions.superusergroup                        |
|          | hdfs-env                    | hdfs-env.sh                   | HADOOP_HEAPSIZE_MAX                                   |
|          | zoo-cfg                     | zoo.cfg                       | secureClientPort                                      |
|          |                             |                               | clientPort                                            |
|          |                             |                               | dataDir                                               |
|          |                             |                               | dataLogDir                                            |
|          |                             |                               | 4lw.commands.whitelist                                |
|          | zookeeper-java-env          | java.env                      | ZK_LOG_DIR                                            |
|          |                             |                               | SERVER_JVMFLAGS                                       |
|          | zookeeper-log4j-properties  | log4j.properties (zookeeper)  | log4j.rootLogger                                      |
|          |                             |                               | log4j.appender.CONSOLE.*                              |

> [!NOTE]
> В этой статье содержится термин *список разрешений*, который корпорация Майкрософт считает некорректным для данного контекста. Термин присутствует в этой статье, так как в настоящее время он присутствует в программном обеспечении. При удалении термина из программного обеспечения мы удалим его из статьи.

## <a name="unsupported-gateway-configurations"></a>Неподдерживаемые конфигурации `gateway`

Перечисленные ниже конфигурации `gateway` не поддерживаются и не могут быть изменены в контексте кластера больших данных.

| Категория  | Подкатегория               | Файл                       | Неподдерживаемые конфигурации                                              |
|-----------|----------------------------|----------------------------|-------------------------------------------------------------------------|
|          | gateway-site                | gateway-site.xml              | gateway.port                                          |
|          |                             |                               | gateway.path                                          |
|          |                             |                               | gateway.gateway.conf.dir                              |
|          |                             |                               | gateway.hadoop.kerberos.secured                       |
|          |                             |                               | java.security.krb5.conf                               |
|          |                             |                               | java.security.auth.login.config                       |
|          |                             |                               | gateway.websocket.feature.enabled                     |
|          |                             |                               | gateway.scope.cookies.feature.enabled                 |
|          |                             |                               | ssl.exclude.protocols                                 |
|          |                             |                               | ssl.include.ciphers                                   |

## <a name="next-steps"></a>Дальнейшие действия

[Настройка кластеров больших данных SQL Server](configure-bdc-overview.md)

---
title: Руководство по потоковой передаче Spark для кластеров больших данных SQL Server
titleSuffix: SQL Server Big Data Clusters
description: В этом руководстве рассматриваются варианты использования потоковой передачи и их реализация с помощью возможностей кластеров больших данных SQL Server.
author: dacoelho
ms.author: dacoelho
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 04/06/2021
ms.topic: guide
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 05e35cf861a46a667ce25c88e9d6d2ea17b1183b
ms.sourcegitcommit: 14b97028da137f872a0a35cfe9d5a639a2d116a8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2021
ms.locfileid: "107221773"
---
# <a name="sql-server-big-data-clusters-spark-streaming-guide"></a>Руководство по потоковой передаче Spark для кластеров больших данных SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

В этом руководстве рассматриваются варианты использования потоковой передачи и их реализация с помощью кластеров больших данных SQL Server Spark.

Из этого руководства вы узнаете, как выполнить следующие задачи:

> [!div class="checklist"]
> * Загрузка библиотек потоковой передачи для использования с PySpark и Scala Spark.
> * Реализация трех распространенных шаблонов потоковой передачи с помощью кластеров больших данных SQL Server.

## <a name="prerequisites"></a>Предварительные условия

* Развертывание кластеров больших данных SQL Server
* Один из двух приведенных ниже вариантов:
    * кластер Apache Kafka 2.0 и выше;
    * Центры событий Azure — пространство имен и Центр событий.

Для выполнения действий в этом руководстве предполагается наличие хорошего уровня знаний о понятиях и архитектурах технологии потоковой передачи.
С основными понятиями можно ознакомиться в приведенных ниже статьях.

* [Руководство по архитектуре данных. Обработка в реальном времени](/azure/architecture/data-guide/big-data/real-time-processing)
* [Использование Центров событий Azure из приложений Apache Kafka](/azure/event-hubs/event-hubs-for-kafka-ecosystem-overview)
* [Руководство по архитектуре данных. Выбор технологии приема сообщений в реальном времени в Azure](/azure/architecture/data-guide/technology-choices/real-time-ingestion)

### <a name="apache-kafka-and-azure-event-hub-conceptual-mapping"></a>Сопоставление понятий Apache Kafka и Центра событий Azure

|Понятия Apache Kafka|Понятия Центров событий|
|--------------------|------------------|
|Кластер|Пространство имен|
|Раздел|Концентратор событий|
|Partition (Раздел)|Partition (Раздел)|
|Группа потребителей|Группа потребителей|
|Offset|Offset|

### <a name="reproducibility"></a>Воспроизводимость

В этом руководстве используется приложение производителя, предоставляемое в [кратком руководстве по потоковой передаче данных по протоколу Kafka с помощью Центров событий](/azure/event-hubs/event-hubs-quickstart-kafka-enabled-event-hubs). Кроме того, на странице [Центры событий Azure для Apache Kafka](https://github.com/Azure/azure-event-hubs-for-kafka) на сайте GitHub приведены примеры приложений на многих языках программирования, которые позволят быстро выполнять сценарии потоковой передачи.

Ниже показан измененный файл `producer.py`, передающий в потоковом режиме смоделированные данные JSON датчика в модуль потоковой передачи с помощью совместимого с Kafka клиента. Важно отметить, что Центры событий Azure совместимы с протоколом Kafka. Следуйте инструкциям по установке, приведенным в [репозитории GitHub](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/quickstart/python), чтобы получить пример для работы. В словаре `conf` содержится вся информация о подключении, и значения могут меняться в зависимости от среды. Замените хотя бы `bootstrap.servers` и `sasl.password`, так как эта конфигурация является наиболее оптимальной в примере кода ниже.

```python
#!/usr/bin/env python
#
# Copyright (c) Microsoft Corporation. All rights reserved.
# Copyright 2016 Confluent Inc.
# Licensed under the MIT License.
# Licensed under the Apache License, Version 2.0
#
# Original Confluent sample modified for use with Azure Event Hubs for Apache Kafka Ecosystems

from confluent_kafka import Producer
import sys
import random
import time
import json

sensors = ["Sensor 1", "Sensor 2", "Sensor 3"]

if __name__ == '__main__':
    if len(sys.argv) != 2:
        sys.stderr.write('Usage: %s <topic>\n' % sys.argv[0])
        sys.exit(1)
    topic = sys.argv[1]

    # Producer configuration
    # See https://github.com/edenhill/librdkafka/blob/master/CONFIGURATION.md
    # See https://github.com/edenhill/librdkafka/wiki/Using-SSL-with-librdkafka#prerequisites for SSL issues
    conf = {
        'bootstrap.servers': '',                     #replace!
        'security.protocol': 'SASL_SSL',
        'ssl.ca.location': '/usr/lib/ssl/certs/ca-certificates.crt',
        'sasl.mechanism': 'PLAIN',
        'sasl.username': '$ConnectionString',
        'sasl.password': '',                         #replace!
        'client.id': 'python-sample-producer'
    }

    # Create Producer instance
    p = Producer(**conf)

    def delivery_callback(err, msg):
        if err:
            sys.stderr.write('%% Message failed delivery: %s\n' % err)
        else:
            sys.stderr.write('%% Message delivered to %s [%d] @ %o\n' % (msg.topic(), msg.partition(), msg.offset()))

    # Simulate stream
    for i in range(0, 10000):
        try:
            payload = {
                'sensor': random.choice(sensors),
                'measure1': random.gauss(37, 7),
                'measure2': random.random(),
            }
            p.produce(topic, json.dumps(payload).encode('utf-8'), callback=delivery_callback)
            #p.produce(topic, str(i), callback=delivery_callback)
        except BufferError as e:
            sys.stderr.write('%% Local producer queue is full (%d messages awaiting delivery): try again\n' % len(p))
        p.poll(0)
        time.sleep(2)

    # Wait until all messages have been delivered
    sys.stderr.write('%% Waiting for %d deliveries\n' % len(p))
    p.flush()

```

Выполните пример приложения производителя, используя следующую команду, заменив `<my-sample-topic>` сведениями о вашей среде.

```bash
python producer.py <my-sample-topic>
```

## <a name="streaming-scenarios"></a>Сценарии потоковой передачи

|Шаблон потоковой передачи             |Описание сценария и его реализация         |
|------------------------------|------------------------------------------------|
|Извлечение из Kafka или Центров событий |Создайте задание потоковой передачи Spark, которое непрерывно извлекает данные из обработчика потоковой передачи, выполняя необязательные преобразования и реализуя логику аналитики.|
|Прием потоковой передачи данных в HDFS |В целом это соотносится с предыдущим шаблоном. После реализации логики потоковой передачи и преобразования данные могут быть записаны во множество расположений данных, что позволяет выполнить требование к сохраняемости данных.|
|Отправка из Spark в Kafka или Центров событий |После обработки с помощью Spark данные можно отправить обратно в модуль внешней потоковой передачи. Существует множество сценариев, в которых это действие является желательным, например при рекомендации продукта в реальном времени, мошенничестве с микропакетами, обнаружении аномалий и т. д.|

## <a name="sample-spark-streaming-application"></a>Пример приложения потоковой передачи Spark

В этом примере приложения реализуются все три шаблона потоковой передачи, описанные в предыдущем разделе. Это приложение выполняет следующие задачи:

1. настройка переменных конфигурации для подключения к службе потоковой передачи;
1. создание кадра данных потоковой передачи Spark для извлечения данных;
1. локальная запись агрегированных данных в HDFS;
1. запись агрегированных данных в другой раздел в службе потоковой передачи.

Вот как выглядит готовый код `sample-spark-streaming-python.py`:
```python
from pyspark import SparkContext, SparkConf
from pyspark.streaming import StreamingContext
from pyspark.sql import SparkSession
from pyspark.sql.types import *
from pyspark.sql.functions import *

# Sets up batch size to 15 seconds
duration_ms = 15000
# Changes Spark Session into a Structured Streaming context
sc = SparkContext.getOrCreate()
ssc = StreamingContext(sc, duration_ms)
spark = SparkSession(sc)

# Connection Information
bootstrap_servers = "" # Replace!
sasl = "" # Replace!
# Topic we will consume from
topic = "sample-topic"
# Topic we will write toa
topic_to = "sample-topic-processed"

# Define the schema to speed up processing
jsonSchema = StructType([StructField("sensor", StringType(), True), StructField("measure1", DoubleType(), True), StructField("measure2", DoubleType(), True)])

streaming_input_df = (
    spark.readStream \
    .format("kafka") \
    .option("subscribe", topic) \
    .option("kafka.bootstrap.servers", bootstrap_servers) \
    .option("kafka.sasl.mechanism", "PLAIN") \
    .option("kafka.security.protocol", "SASL_SSL") \
    .option("kafka.sasl.jaas.config", sasl) \
    .option("kafka.request.timeout.ms", "60000") \
    .option("kafka.session.timeout.ms", "30000") \
    .option("failOnDataLoss", "true") \
    .option("startingOffsets", "latest") \
    .load()
)

def foreach_batch_function(df, epoch_id):
    # Transform and write batchDF
    if df.count() <= 0:
        None
    else:
        # Create a data frame to be written to HDFS
        sensor_df = df.selectExpr('CAST(value AS STRING)').select(from_json("value", jsonSchema).alias("value")).select("value.*")
        # root
        #  |-- sensor: string (nullable = true)
        #  |-- measure1: double (nullable = true)
        #  |-- measure2: double (nullable = true)
        sensor_df.persist()
        # Write to HDFS:
        sensor_df.write.format('parquet').mode('append').saveAsTable('sensor_data')
        # Create a summarization dataframe
        sensor_stats_df = (sensor_df.groupBy('sensor').agg({'measure1':'avg', 'measure2':'avg', 'sensor':'count'}).withColumn('ts', current_timestamp()).withColumnRenamed('avg(measure1)', 'measure1_avg').withColumnRenamed('avg(measure2)', 'measure2_avg').withColumnRenamed('avg(measure1)', 'measure1_avg').withColumnRenamed('count(sensor)', 'count_sensor'))
        # root
        # |-- sensor: string (nullable = true)
        # |-- measure2_avg: double (nullable = true)
        # |-- measure1_avg: double (nullable = true)
        # |-- count_sensor: long (nullable = false)
        # |-- ts: timestamp (nullable = false)
        sensor_stats_df.write.format('parquet').mode('append').saveAsTable('sensor_data_stats')
        # Group by and send metrics to an output kafka topic:
        sensor_stats_df.writeStream
            .format("kafka")
            .option("topic", topic_to)
            .option("kafka.bootstrap.servers", bootstrap_servers)
            .option("kafka.sasl.mechanism", "PLAIN")
            .option("kafka.security.protocol", "SASL_SSL")
            .option("kafka.sasl.jaas.config", sasl)
            .save()
        # For example, you could write to SQL Server
        # df.write.format('com.microsoft.sqlserver.jdbc.spark').mode('append').option('url', url).option('dbtable', datapool_table).save()
        sensor_df.unpersist()


writer = streaming_input_df.writeStream.foreachBatch(foreach_batch_function).start().awaitTermination()

```

Ниже приведены таблицы, которые требуется создать с помощью __Spark SQL__:
```sql
CREATE TABLE IF NOT EXISTS sensor_data (sensor string, measure1 double, measure2 double)
USING PARQUET;

CREATE TABLE IF NOT EXISTS sensor_data_stats (sensor string, measure2_avg double, measure1_avg double, count_sensor long, ts timestamp)
USING PARQUET;
```


### <a name="copy-the-application-to-hdfs"></a>Копирование приложения в HDFS

```bash
azdata bdc hdfs cp --from-path sample-spark-streaming-python.py --to-path "hdfs:/apps/ETL-Pipelines/sample-spark-streaming-python.py"
```

### <a name="configuring-kafka-libraries"></a>Настройка библиотек Kafka

Самый важный шаг заключается в настройке клиентских библиотек Kafka для приложения перед отправкой.

Есть __две необходимые библиотеки__:

* [kafka-clients](https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients) — основная библиотека, которая обеспечивает поддержку протокола Kafka и возможность подключения.
* [spark-sql-kafka](https://mvnrepository.com/artifact/org.apache.spark/spark-sql-kafka-0-10) — включает функцию кадров данных SQL Spark в потоках Kafka.

Обе библиотеки должны соответствовать следующим требованиям:

* использоваться для Scala 2.11 и Spark 2.4.7. Это требование кластера BDC SQL Server в соответствии с CU9 или выше.
* быть совместимыми с сервером потоковой передачи.

> [!CAUTION]
   > Как правило, следует использовать самую последнюю совместимую библиотеку. Код, приведенный в этом руководстве, был протестирован с использованием Apache Kafka для Центров событий Azure и предоставляется "как есть", а не в виде заявления о поддержке. Apache Kafka предлагает двунаправленную совместимость клиентов, но реализации библиотек различаются в зависимости от языка программирования. Для обеспечения надлежащей совместимости всегда обращайтесь к документации по платформе Kafka.

#### <a name="shared-library-locations-for-jobs-on-hdfs"></a>Расположения общих библиотек для заданий в HDFS

Если несколько приложений подключаются к одному и тому же кластеру Kafka, или в вашей организации есть один кластер Kafka с несколькими версиями, скопируйте соответствующие JAR-файлы библиотеки в общее расположение в HDFS. Затем все задания должны ссылаться на одни и те же файлы библиотеки.

Скопируйте библиотеки в общее расположение:

```bash
azdata bdc hdfs cp --from-path kafka-clients-2.7.0.jar --to-path "hdfs:/apps/jars/kafka-clients-2.7.0.jar"
azdata bdc hdfs cp --from-path spark-sql-kafka-0-10_2.11-2.4.7.jar --to-path "hdfs:/apps/jars/spark-sql-kafka-0-10_2.11-2.4.7.jar"
```

#### <a name="dynamically-install-the-libraries"></a>Динамическая установка библиотек

Пакеты можно динамически устанавливать при отправке задания с помощью [функций управления пакетами](spark-install-packages.md), доступных в кластерах больших данных SQL Server. Время запуска задания задерживается из-за повторяющихся скачиваний файлов библиотеки при каждой отправке задания.

### <a name="submit-the-spark-streaming-job-using-azdata"></a>Отправка задания потоковой передачи Spark с помощью `azdata`

В первом примере используются JAR-файлы общей библиотеки в HDFS:

```bash
azdata bdc spark batch create -f hdfs:/apps/ETL-Pipelines/sample-spark-streaming-python.py \
-j '["/apps/jars/kafka-clients-2.7.0.jar","/apps/jars/spark-sql-kafka-0-10_2.11-2.4.7.jar"]' \
--config '{"spark.streaming.concurrentJobs":"3","spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"}' \
-n MyStreamingETLPipelinePySpark --executor-count 2 --executor-cores 2 --executor-memory 1664m
```

В этом примере для установки зависимостей используется динамическое управление пакетами:

```bash
azdata bdc spark batch create -f hdfs:/apps/ETL-Pipelines/sample-spark-streaming-python.py \
--config '{"spark.jars.packages": "org.apache.kafka:kafka-clients:2.7.0,org.apache.spark:spark-sql-kafka-0-10_2.11:2.4.7","spark.streaming.concurrentJobs":"3","spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"}' \
-n MyStreamingETLPipelinePySpark --executor-count 2 --executor-cores 2 --executor-memory 1664m
```

## <a name="next-steps"></a>Дальнейшие шаги

Сведения об отправке заданий Spark в кластер больших данных SQL Server с помощью конечных точек azdata или Livy см. в статье об [отправке заданий Spark с помощью программ командной строки](spark-submit-job-command-line.md).

Дополнительные сведения о кластере больших данных SQL Server и связанных сценариях см. в статье [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md).

---
title: Отправка заданий Spark с помощью командной строки
titleSuffix: SQL Server big data clusters
description: Отправка заданий Spark в кластер больших данных SQL Server с помощью программ командной строки.
author: dacoelho
ms.author: dacoelho
ms.reviewer: MikeRayMSFT
ms.date: 04/01/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3983f6354c84d11809b37d784d64c2f8c5b28f10
ms.sourcegitcommit: 14b97028da137f872a0a35cfe9d5a639a2d116a8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2021
ms.locfileid: "107221770"
---
# <a name="submit-spark-jobs-using-command-line-tools"></a>Отправка заданий Spark с помощью программ командной строки

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

В этой статье приводятся рекомендации по использованию программ командной строки для выполнения заданий Spark в кластерах больших данных SQL Server.

## <a name="prerequisites"></a>Предварительные условия

* [Средства по работе с большими данными SQL Server 2019](deploy-big-data-tools.md), настроенные и внедренные в кластер:
  * **azdata** 
  * приложение **curl** для выполнения вызовов REST API к Livy

## <a name="spark-jobs-using-__azdata__-or-livy"></a>Отправка заданий Spark с использованием __azdata__ или Livy

В этой статье приведены примеры использования шаблонов командной строки для отправки приложений Spark в кластеры больших данных SQL Server.

[Команды `azdata bdc spark`](../azdata/reference/reference-azdata-bdc-spark.md) Azure Data CLI реализуют все возможности кластеров больших данных SQL Spark из командной строки. Хотя в этом руководстве основное внимание уделяется отправке заданий, `azdata bdc spark` также поддерживает интерактивные режимы для Python, Scala, SQL и R с помощью команды `azdata bdc spark session`.

Если требуется непосредственная интеграция с REST API, используйте для отправки заданий стандартные вызовы Livy. Мы будем использовать программу командной строки `curl` в примерах Livy для выполнения вызова REST API. Подробный пример взаимодействия с конечной точкой Livy Spark с помощью кода Python см. в на странице [Использование Spark из конечной точки Livy](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/restful-api-access/accessing_spark_via_livy.ipynb) на сайте GitHub.

## <a name="simple-etl-using-sql-server-bdc-spark"></a>Выполнение простых операций извлечения, преобразования и загрузки (ETL) с использованием BDC Spark SQL Server

Это приложение служит примером общего шаблона инжиниринга данных — оно загружает табличные данные из пути целевой зоны HDFS, а затем выполняет запись, используя формат таблицы, в путь обработанной зоны HDFS. Набор данных, используемый в этом примере приложения, можно скачать [здесь](https://ailab.criteo.com/download-criteo-1tb-click-logs-dataset/). Приложения PySpark можно создать с помощью PySpark, Spark Scala или Spark SQL. В следующих примерах приведены упражнения для каждого решения. Используйте вкладки ниже для выбора нужной платформы и последующего выполнения приложения с помощью `azdata` или `curl`.

### <a name="pyspark"></a>[PySpark](#tab/pyspark)

В этом примере мы будем использовать приложение PySpark, сохраненное в виде файла Python с именем ```parquet_etl_sample.py``` на локальном компьютере.

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.getOrCreate()

# Read clickstream_data from Storage Pool HDFS into a Spark data frame. Applies column renames.
df = spark.read.option("inferSchema", "true").csv('/securelake/landing/criteo/test.txt', sep='\t', 
    header=False).toDF("feat1","feat2","feat3","feat4","feat5","feat6","feat7","feat8",
    "feat9","feat10","feat11","feat12","feat13","catfeat1","catfeat2","catfeat3","catfeat4",
    "catfeat5","catfeat6","catfeat7","catfeat8","catfeat9","catfeat10","catfeat11","catfeat12",
    "catfeat13","catfeat14","catfeat15","catfeat16","catfeat17","catfeat18","catfeat19",
    "catfeat20","catfeat21","catfeat22","catfeat23","catfeat24","catfeat25","catfeat26")

# Prints the data frame inferred schema:
df.printSchema()

tot_rows = df.count()
print("Number of rows:", tot_rows)

# Drop the managed table
spark.sql("DROP TABLE dl_clickstream")

# Write data frame to HDFS managed table using optimized Delta Lake table format
df.write.format("parquet").mode("overwrite").saveAsTable("dl_clickstream")

print("Sample ETL pipeline completed")
```

#### <a name="copy-the-pyspark-application-to-hdfs"></a>Копирование приложения PySpark в HDFS

Приложение должно храниться в HDFS, чтобы кластер мог обращаться к нему и выполнять его. В целях упрощения администрирования рекомендуется стандартизировать расположения приложений в кластере и управлять ими. В этом примере все приложения конвейера ETL будут храниться в расположении по пути `hdfs:/apps/ETL-Pipelines`. Пример приложения будет храниться в расположении по пути `hdfs:/apps/ETL-Pipelines/parquet_etl_sample.py`.

Выполните следующую команду, чтобы отправить __`parquet_etl_sample.py`__ с локального компьютера разработки или промежуточного хранения в кластер HDFS. 

```bash
azdata bdc hdfs cp --from-path parquet_etl_sample.py  --to-path "hdfs:/apps/ETL-Pipelines/parquet_etl_sample.py"
```

### <a name="spark-scala"></a>[Spark Scala](#tab/scala)

В этом примере мы будем использовать приложение Spark, написанное на Scala Spark.

```scala
import org.apache.spark.sql.SparkSession

object ParquetETLSample {
    def main(args: Array[String]) {
        val spark = SparkSession.builder.getOrCreate()
        
        val df = spark.read.
            option("inferSchema", "true").
            option("header", "false").
            option("delimiter", "\t").
            csv("/securelake/landing/criteo/test.txt").
            toDF("feat1","feat2","feat3","feat4","feat5","feat6","feat7","feat8","feat9","feat10","feat11","feat12","feat13","catfeat1","catfeat2","catfeat3","catfeat4","catfeat5","catfeat6","catfeat7","catfeat8","catfeat9","catfeat10","catfeat11","catfeat12","catfeat13","catfeat14","catfeat15","catfeat16","catfeat17","catfeat18","catfeat19","catfeat20","catfeat21","catfeat22","catfeat23","catfeat24","catfeat25","catfeat26")
        
        val tot_rows = df.count()
        println(s"Number of rows: $tot_rows")

        spark.sql("DROP TABLE dl_clickstream")

        df.write.format("parquet").mode("overwrite").saveAsTable("dl_clickstream")

        println("Sample ETL pipeline completed")
        
        spark.stop()
    }
}
```

#### <a name="bundle-and-copy-the-spark-application-to-hdfs"></a>Создание пакета приложения и копирование приложения Spark в HDFS

Согласно документации по Spark рекомендует создать JAR-файл __сборки__ (или пакет), содержащий приложение и все его зависимости. Это обязательный шаг для отправки пакета приложений в кластер для выполнения. Настройка полной среды разработки Scala Spark не рассматривается в этом руководстве. Дополнительные сведения см. в официальной [документации по Spark о создании автономных приложений](https://spark.apache.org/docs/latest/quick-start.html#self-contained-applications).

В этом примере предполагается, что пакет JAR приложения с именем `parquet-etl-sample.jar` скомпилирован и доступен. Выполните следующую команду, чтобы отправить пакет с локального компьютера разработки или промежуточного хранения в кластер HDFS.

```bash
azdata bdc hdfs cp --from-path parquet-etl-sample.jar  --to-path "hdfs:/apps/ETL-Pipelines/parquet-etl-sample.jar"
```

### <a name="spark-sql"></a>[SQL Spark](#tab/sql)

В этом примере используется Spark SQL для выполнения логики приема с использованием таблиц и представлений в целях реализации ориентированного на SQL подхода к ETL.

```sql
DROP VIEW IF EXISTS etl_clickstream;

CREATE TEMPORARY VIEW etl_clickstream
USING CSV
OPTIONS (path "/securelake/landing/criteo/test.txt", header "false", delimiter "\t", mode "FAILFAST");

DROP TABLE IF EXISTS dl_clickstream;

CREATE TABLE dl_clickstream (
    feat1 integer,
    feat2 integer,
    feat3 integer,
    feat4 integer,
    feat5 integer,
    feat6 integer,
    feat7 integer,
    feat8 integer,
    feat9 integer,
    feat10 integer,
    feat11 integer,
    feat12 integer,
    feat13 integer,
    catfeat1 string,
    catfeat2 string,
    catfeat3 string,
    catfeat4 string,
    catfeat5 string,
    catfeat6 string,
    catfeat7 string,
    catfeat8 string,
    catfeat9 string,
    catfeat10 string,
    catfeat11 string,
    catfeat12 string,
    catfeat13 string,
    catfeat14 string,
    catfeat15 string,
    catfeat16 string,
    catfeat17 string,
    catfeat18 string,
    catfeat19 string,
    catfeat20 string,
    catfeat21 string,
    catfeat22 string,
    catfeat23 string,
    catfeat24 string,
    catfeat25 string,
    catfeat26 string
) 
USING PARQUET
AS SELECT * FROM etl_clickstream;
```

#### <a name="copy-the-spark-sql-application-to-hdfs"></a>Копирование приложения SQL Spark в HDFS

Выполните следующую команду, чтобы отправить __```parquet-etl-sample.sql```__ с локального компьютера разработки или промежуточного хранения в кластер HDFS.

```bash
azdata bdc hdfs cp --from-path parquet-etl-sample.sql --to-path "hdfs:/apps/ETL-Pipelines/parquet-etl-sample.sql"
```

---

#### <a name="execute-the-spark-application"></a>Выполнение приложения Spark

Используйте следующую команду, чтобы отправить приложение в SQL Server BDC Spark для выполнения.


# <a name="pyspark-and-azdata"></a>[PySpark и azdata](#tab/azdata/pyspark)

Команда __`azdata`__ выполняет приложение с использованием часто задаваемых параметров. Полный список параметров для `azdata bdc spark batch create` см. в статье [`azdata bdc spark`](../azdata/reference/reference-azdata-bdc-spark.md).

Для этого приложения требуется параметр конфигурации Spark `spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation`, поэтому команда использует параметр `--config`. Это было сделано намеренно, чтобы продемонстрировать, как передавать конфигурации в сеанс Spark. Вы можете использовать параметр `--config`, чтобы указать несколько параметров конфигурации. Это можно также сделать в сеансе приложения, задав конфигурацию в объекте SparkSession.

```bash
azdata bdc spark batch create -f hdfs:/apps/ETL-Pipelines/parquet_etl_sample.py \
--config '{"spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"}' \
-n MyETLPipelinePySpark --executor-count 2 --executor-cores 2 --executor-memory 1664m
```

# <a name="pyspark-and-curl-using-livy"></a>[PySpark и curl с использованием Livy](#tab/curl/pyspark)

Команда __`curl`__ выполняет приложение с помощью Livy. Не забудьте заменить значения USER, PASSWORD и LIVY_ENDPOINT в соответствии с вашей средой.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches \
-H 'Content-Type: application/json; charset=utf-8' \
--data-binary @- << EOF
{
    "file": "/apps/ETL-Pipelines/parquet_etl_sample.py",
    "name": "MyETLPipelinePySpark",
    "numExecutors": 2,
    "executorCores": 2,
    "executorMemory": "1664m",
    "conf": {
        "spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"
    }
}
EOF
```

# <a name="scala-and-azdata"></a>[Scala и azdata](#tab/azdata/scala)

Команда __`azdata`__ выполняет приложение с использованием часто задаваемых параметров. Полный список параметров для `azdata bdc spark batch create` см. в статье [`azdata bdc spark`](../azdata/reference/reference-azdata-bdc-spark.md).

Для этого приложения требуется параметр конфигурации Spark `spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation`, поэтому команда использует параметр `--config`. Это было сделано намеренно, чтобы продемонстрировать, как передавать конфигурации в сеанс Spark. Вы можете использовать параметр `--config`, чтобы указать несколько параметров конфигурации. Это можно также сделать в сеансе приложения, задав конфигурацию в объекте SparkSession.

```bash
azdata bdc spark batch create -f hdfs:/apps/ETL-Pipelines/parquet-etl-sample.jar \
--class "ParquetETLSample" \
--config '{"spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"}' \
-n MyETLPipeline --executor-count 2 --executor-cores 2 --executor-memory 1664m
```

# <a name="scala-and-curl-using-livy"></a>[Scala и curl с использованием Livy](#tab/curl/scala)

Команда __`curl`__ выполняет приложение с помощью Livy. Не забудьте заменить значения USER, PASSWORD и LIVY_ENDPOINT в соответствии с вашей средой.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches \
-H 'Content-Type: application/json; charset=utf-8' \
--data-binary @- << EOF
{
    "file": "/apps/ETL-Pipelines/parquet-etl-sample.jar",
    "class": "ParquetETLSample",
    "name": "MyETLPipeline",
    "numExecutors": 2,
    "executorCores": 2,
    "executorMemory": "1664m",
    "conf": {
        "spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"
    }
}
EOF
```

# <a name="sql-and-azdata"></a>[SQL и azdata](#tab/azdata/sql)

Команда __`azdata`__ выполняет приложение с использованием часто задаваемых параметров. Полный список параметров для `azdata bdc spark batch create` см. в статье [`azdata bdc spark`](../azdata/reference/reference-azdata-bdc-spark.md).

Как и в примере с PySpark, этому приложению также требуется параметр конфигурации Spark `spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation`, поэтому команда использует параметр `--config`. Это было сделано намеренно, чтобы продемонстрировать, как передавать конфигурации в сеанс Spark. Вы можете использовать параметр `--config`, чтобы указать несколько параметров конфигурации. Это можно также сделать в сеансе приложения, задав конфигурацию в объекте SparkSession.

```bash
azdata bdc spark batch create -f hdfs:/apps/ETL-Pipelines/parquet_etl_sample.sql \
--config '{"spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"}' \
-n MyETLPipelineSQL --executor-count 2 --executor-cores 2 --executor-memory 1664m
```

# <a name="sql-and-curl-using-livy"></a>[SQL и curl с использованием Livy](#tab/curl/sql)

Команда __`curl`__ выполняет приложение с помощью Livy. Не забудьте заменить значения USER, PASSWORD и LIVY_ENDPOINT в соответствии с вашей средой.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches \
-H 'Content-Type: application/json; charset=utf-8' \
--data-binary @- << EOF
{
    "file": "/apps/ETL-Pipelines/parquet_etl_sample.sql",
    "name": "MyETLPipelineSQL",
    "numExecutors": 2,
    "executorCores": 2,
    "executorMemory": "1664m",
    "conf": {
        "spark.sql.legacy.allowCreatingManagedTableUsingNonemptyLocation":"true"
    }
}
EOF
```

---

## <a name="monitoring-spark-jobs"></a>Мониторинг заданий Spark

[Команды `azdata bdc spark batch`](../azdata/reference/reference-azdata-bdc-spark.md) содержат действия по управлению пакетными заданиями Spark.

Чтобы получить __список всех выполняющихся заданий__, выполните следующую команду.

Это команда `azdata`:

```bash
azdata bdc spark batch list -o table
```

Это команда `curl`, использующая Livy.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches
```

Чтобы __получить сведения__ для пакета Spark с заданным идентификатором, выполните следующую команду. После выполнения команды spark batch create возвращается `batch id`.

Это команда `azdata`:

```bash
azdata bdc spark batch info --batch-id 0
```

Это команда `curl`, использующая Livy.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches/<BATCH_ID>
```

Чтобы __получить сведения о состоянии__ для пакета Spark с заданным идентификатором, выполните следующую команду.

Это команда `azdata`:

```bash
azdata bdc spark batch state --batch-id 0
```

Это команда `curl`, использующая Livy.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches/<BATCH_ID>/state
```

Чтобы __получить журналы__ для пакета Spark с заданным идентификатором, выполните следующую команду.

Это команда `azdata`:

```bash
azdata bdc spark batch log --batch-id 0
```

Это команда `curl`, использующая Livy.

```bash
curl -k -u <USER>:<PASSWORD> -X POST <LIVY_ENDPOINT>/batches/<BATCH_ID>/log
```

## <a name="next-steps"></a>Дальнейшие шаги

Дополнительные сведения об устранении неполадок с кодом Spark см. в статье [Устранение неполадок с записной книжкой pyspark](troubleshoot-pyspark-notebook.md).

Полный набор примеров кода Spark доступен на странице [примеров Spark для кластеров больших данных SQL Server](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark) на сайте GitHub.

Дополнительные сведения о кластере больших данных SQL Server и связанных сценариях см. в статье [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md).

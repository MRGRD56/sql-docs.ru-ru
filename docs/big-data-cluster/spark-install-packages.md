---
title: Управление библиотекой Spark
titleSuffix: SQL Server big data clusters
description: Управление библиотекой Spark
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 01/25/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 70611d3f7d4ed6825911908942d9ed707dabbd2e
ms.sourcegitcommit: 8bdb5a51f87a6ff3b94360555973ca0cd0b6223f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100549972"
---
# <a name="spark-library-management"></a>Управление библиотекой Spark

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

В этой статье приводятся инструкции по импорту и установке пакетов для сеанса Spark с помощью конфигураций сеансов и записных книжек.

## <a name="built-in-tools"></a>Встроенные средства
Базовые пакеты Python 3.7 и Python 2.7 Pandas, Sklearn, Numpy и другие пакеты обработки данных для Spark и Hadoop.
Пакеты R и MRO Sparklyr

## <a name="install-packages-from-a-maven-repository-onto-the-spark-cluster-at-runtime"></a>Установка пакетов из репозитория Maven в кластер Spark во время выполнения
Пакеты Maven можно установить в кластер Spark с помощью конфигурации ячеек записной книжки в начале сеанса Spark. Перед запуском сеанса Spark в Azure Data Studio выполните следующий код.

```
%%configure -f \
{"conf": {"spark.jars.packages": "com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1"}}
```

## <a name="install-python-packages-at-pyspark-job-submission-time"></a>Установка пакетов Python во время отправки задания PySpark
1. Укажите путь к файлу requirements.txt в HDFS со ссылками на устанавливаемые пакеты.
```
%%configure -f \
{"conf": {
    "spark.pyspark.virtualenv.enabled" : "true",
    "spark.pyspark.virtualenv.type" : "conda",
    "spark.pyspark.virtualenv.requirements" : "requirements.txt",
    "spark.pyspark.virtualenv.bin.path" : "/opt/mls/python/bin/conda"
    }, 
"files": ["hdfs://nmnode-0/tmp/requirements.txt"]
}
```
2. Создайте виртуальную среду conda без файла требований и динамически добавьте пакеты во время сеанса Spark.
```
%%configure -f \
{"conf": {
    'spark.pyspark.virtualenv.enabled' : 'true',
    'spark.pyspark.virtualenv.type' : 'conda',
    'spark.pyspark.virtualenv.bin.path' : '/opt/mls/python/bin/conda',
    'spark.pyspark.virtualenv.python_version': '3.6'
 }
 ```

 ```python
sc.install_packages("numpy==1.11.0")
import numpy as np
```

## <a name="import-jar-from-hdfs-for-use-at-runtime"></a>Импорт JAR-файла из HDFS для использования во время выполнения
Импортируйте JAR-файл во время выполнения через конфигурацию ячеек записной книжки Azure Data Studio.

```
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

### <a name="import-jar-at-runtime-through-azure-data-studio-notebook-cell-configuration"></a>Импорт JAR-файла во время выполнения через конфигурацию ячеек записной книжки Azure Data Studio
```
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о кластере больших данных SQL Server и связанных сценариях см. в статье [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md).
---
title: Управление библиотекой Spark
titleSuffix: SQL Server big data clusters
description: Управление библиотекой Spark
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/25/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bed687cb003bfc7748aefa3c98ae5e19089f9685
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837061"
---
# <a name="spark-library-management"></a>Управление библиотекой Spark

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

В этой статье приводятся инструкции по импорту и установке пакетов для сеанса Spark с помощью конфигураций сеансов и записных книжек.

## <a name="built-in-tools"></a>Встроенные средства

Базовые пакеты Scala Spark (Scala 2.11) и Hadoop. 

PySpark (Python 3.7). Pandas, Sklearn, NumPy и другие пакеты для обработки данных и машинного обучения.

Пакеты MRO 3.5.2. Sparklyr и SparkR для рабочих нагрузок R Spark.

## <a name="install-packages-from-a-maven-repository-onto-the-spark-cluster-at-runtime"></a>Установка пакетов из репозитория Maven в кластер Spark во время выполнения

Пакеты Maven можно установить в кластер Spark с помощью конфигурации ячеек записной книжки в начале сеанса Spark. Перед запуском сеанса Spark в Azure Data Studio выполните следующий код.

```python
%%configure -f \
{"conf": {"spark.jars.packages": "com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1"}}
```

## <a name="install-python-packages-at-pyspark-at-runtime"></a>Установка пакетов Python в PySpark во время выполнения

Управление пакетами на уровне сеансов и заданий гарантирует согласованность и изоляцию библиотеки. Конфигурация — это конфигурация стандартной библиотеки Spark, которую можно применить к сеансам Livy. __azdata spark__ поддерживает эти конфигурации. Примеры ниже представляются в виде ячеек конфигурации __записных книжек Azure Data Studio__, которые должны быть выполнены после подключения к кластеру с ядром PySpark.

Если конфигурация __"spark.pyspark.virtualenv.enabled" : "true"__ не задана, сеанс будет использовать установленные библиотеки и библиотеки кластера Python по умолчанию.

### <a name="sessionjob-configuration-with-requirementstxt"></a>Конфигурация сеанса и задания с использованием requirements.txt

Укажите путь к файлу requirements.txt в HDFS со ссылками на устанавливаемые пакеты.

```python
%%configure -f \
{
    "conf": {
        "spark.pyspark.virtualenv.enabled" : "true",
        "spark.pyspark.virtualenv.python_version": "3.7",
        "spark.pyspark.virtualenv.requirements" : "hdfs://user/project-A/requirements.txt"
    }
}
```

### <a name="sessionjob-configuration-with-different-python-versions"></a>Конфигурация сеанса и задания с разными версиями Python

Создайте виртуальную среду conda без файла требований и динамически добавьте пакеты во время сеанса Spark.

```python
%%configure -f \
{
    "conf": {
        "spark.pyspark.virtualenv.enabled" : "true",
        "spark.pyspark.virtualenv.python_version": "3.6"
    }
}
```

### <a name="library-installation"></a>Установка библиотеки

Выполните __sc.install_packages__, чтобы динамически установить библиотеки в сеансе. Библиотеки будут установлены на узле драйвера и на всех узлах исполнителя.

 ```python
sc.install_packages("numpy==1.11.0")
import numpy as np
```

Также можно установить несколько библиотек в одной команде, используя массив.

 ```python
sc.install_packages(["numpy==1.11.0", "xgboost"])
import numpy as np
import xgboost as xgb
```

## <a name="import-jar-from-hdfs-for-use-at-runtime"></a>Импорт JAR-файла из HDFS для использования во время выполнения
Импортируйте JAR-файл во время выполнения через конфигурацию ячеек записной книжки Azure Data Studio.

```python
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

### <a name="import-jar-at-runtime-through-azure-data-studio-notebook-cell-configuration"></a>Импорт JAR-файла во время выполнения через конфигурацию ячеек записной книжки Azure Data Studio

```python
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о кластере больших данных SQL Server и связанных сценариях см. в статье [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md).
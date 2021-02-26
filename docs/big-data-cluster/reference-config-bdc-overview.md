---
title: Свойства конфигурации кластеров больших данных SQL Server
titleSuffix: SQL Server big data clusters
description: Справочная статья о свойствах конфигурации кластеров больших данных
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ecaba704d9c08619f42c5cdf8d726917ccc61b9c
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343544"
---
# <a name="sql-server-big-data-clusters-configuration-properties"></a>Свойства конфигурации кластеров больших данных SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Параметры конфигурации кластеров больших данных можно определить в следующих областях: `cluster`, `service` и `resource`. Иерархия параметров также следует этому порядку — от высшего к низшему. Компоненты Кластеров больших данных принимают значение параметра, определенное на самом низком уровне. Если параметр не определен в заданной области, он наследует значение из более высокой родительской области. Ниже приведен список доступных параметров для каждого компонента кластера больших данных в различных областях. Настраиваемые параметры кластера больших данных можно также просмотреть с помощью azdata.

## <a name="bdc-cluster-scope-settings"></a>Параметры в области кластера больших данных
В области кластера можно настроить указанные ниже параметры.

|Свойство|Параметры|
| --- | --- |
|`mssql.telemetry`|`customerfeedback = { true | false }` |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="sql-service-scope-settings"></a>Параметры в области службы SQL
В области службы SQL можно настроить указанные ниже параметры.

|Свойство|Параметры|
| --- | --- |
|`mssql.language`|`lcid = <language_identifier>` |

## <a name="spark-service-scope-settings"></a>Параметры в области службы Spark
Сведения обо всех поддерживаемых и неподдерживаемых параметрах см. в [статье о конфигурации Apache Spark и Apache Hadoop](reference-config-spark-hadoop.md).

## <a name="hdfs-service-scope-settings"></a>Параметры в области службы HDFS
Сведения обо всех поддерживаемых и неподдерживаемых параметрах см. в [статье о конфигурации Apache Spark и Apache Hadoop](reference-config-spark-hadoop.md).

## <a name="gateway-service-scope-settings"></a>Параметры в области службы шлюза
В области службы шлюза нет настраиваемых параметров. Настройте параметры в области ресурса шлюза.

## <a name="app-service-scope-settings"></a>Параметры в области службы приложений
Недоступно.

## <a name="master-pool-resource-scope-settings"></a>Параметры в области ресурсов главного пула
|Свойство|Параметры|
| --- | --- |
|`mssql.sqlagent`|`enabled = { true | false }` |
|`mssql.licensing`|`pid = { Enterprise | Developer }` |
<!-- |`mssql.collation`|`x = <language_identifier>` | -->

> [!NOTE]
> Изменение параметров сортировки по умолчанию для экземпляра SQL Server — сложная операция. Помимо изменения параметра `mssql.collation`, может потребоваться заново создать пользовательские базы данных и все объекты в них. Инструкции см. в статье [Задание или изменение параметров сортировки сервера](../relational-databases/collations/set-or-change-the-server-collation.md#changing-the-server-collation-in-sql-server).

## <a name="storage-pool-resource-scope-settings"></a>Параметры в области ресурсов пула носителей
Пул носителей состоит из компонентов SQL, Spark и HDFS.

### <a name="available-sql-configurations"></a>Доступные конфигурации SQL
|Свойство|Параметры|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.storagePoolCacheSize`| |
|`mssql.storagePoolMaxCacheSize`| |
|`mssql.storagePoolCacheAutogrowth`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |


### <a name="available-apache-spark-and-hadoop-configurations"></a>Доступные конфигурации Apache Spark и Hadoop
Сведения обо всех поддерживаемых и неподдерживаемых параметрах см. в [статье о конфигурации Apache Spark и Apache Hadoop](reference-config-spark-hadoop.md).

## <a name="data-pool-resource-scope-settings"></a>Параметры в области ресурсов пула данных
|Свойство|Параметры|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="compute-pool-resource-scope-settings"></a>Параметры в области ресурсов вычислительного пула
|Свойство|Параметры|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="spark-pool-resource-scope-settings"></a>Параметры в области ресурсов пула Spark
Сведения обо всех поддерживаемых и неподдерживаемых параметрах см. в [статье о конфигурации Apache Spark и Apache Hadoop](reference-config-spark-hadoop.md).

## <a name="gateway-resource-scope-settings"></a>Параметры в области ресурса шлюза
Сведения обо всех поддерживаемых и неподдерживаемых параметрах см. в [статье о конфигурации Apache Spark и Apache Hadoop](reference-config-spark-hadoop.md).

## <a name="sparkhead-resource-scope-settings"></a>Параметры в области ресурса `Sparkhead`
Сведения обо всех поддерживаемых и неподдерживаемых параметрах см. в [статье о конфигурации Apache Spark и Apache Hadoop](reference-config-spark-hadoop.md).

## <a name="zookeeper-resource-scope-settings"></a>Параметры в области ресурса Zookeeper
Сведения обо всех поддерживаемых и неподдерживаемых параметрах см. в [статье о конфигурации Apache Spark и Apache Hadoop](reference-config-spark-hadoop.md).

## <a name="namenode-resource-scope-settings"></a>Параметры в области ресурса Namenode
Сведения обо всех поддерживаемых и неподдерживаемых параметрах см. в [статье о конфигурации Apache Spark и Apache Hadoop](reference-config-spark-hadoop.md).

## <a name="app-proxy-resource-scope-settings"></a>Параметры в области ресурса прокси приложения
Недоступно.

## <a name="next-steps"></a>Дальнейшие действия

[Настройка кластеров больших данных SQL Server](configure-bdc-overview.md)
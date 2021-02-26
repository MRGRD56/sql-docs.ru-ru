---
title: Общие сведения о настройке кластеров больших данных SQL Server после развертывания
titleSuffix: SQL Server big data clusters
description: Общие сведения о настройке кластеров больших данных после развертывания
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 478ecc9888bbd3c8f51ee96c6c796856472f93d5
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343909"
---
# <a name="how-to-configure-bdc-settings-post-deployment"></a>Настройка параметров кластера больших данных после развертывания

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

> [!NOTE]
> Настройка параметров после развертывания возможно только в накопительном пакете обновления 9 и более поздних версий. Настройка параметров не распространяется на конфигурацию масштаба, хранилища или конечных точек. Параметры и инструкции по настройке кластера больших данных с накопительным пакетом обновления до версии 9 см. [здесь](configure-bdc-pre-configuration.md).

Параметры кластеров больших данных на уровне кластера, службы и ресурса можно настроить после развертывания с помощью интерфейса командной строки (CLI) azdata. Эта функция позволяет администраторам кластеров больших данных настраивать конфигурации в соответствии с требованиями рабочих нагрузок. В этой статье приводятся пошаговые инструкции по настройке кластера больших данных в соответствии с требованиями рабочей нагрузки Spark. Для настройки после развертывания используется процесс "задание — определение различий — применение".

## <a name="step-by-step-configure-bdc-to-meet-your-spark-workload-requirements"></a>Пошаговое руководство. Настройка кластера больших данных в соответствии с требованиями рабочей нагрузки Spark

### <a name="view-the-current-configurations-of-the-big-data-cluster-spark-service"></a>Просмотр текущих конфигураций для службы Spark кластера больших данных
В приведенном ниже примере показано, как можно просмотреть настроенные пользователем параметры службы Spark. С помощью дополнительных параметров можно просмотреть все возможные настраиваемые параметры, параметры, управляемые системой, и ожидающие параметры. Дополнительные сведения см. в [справке по командам `azdata bdc spark` statement](../azdata/reference/reference-azdata-bdc-spark-statement.md).

```bash
azdata bdc spark settings show
```
#### <a name="sample-output"></a>Пример выходных данных
Служба Spark 

|Параметр|Текущее значение|
| --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1` |
|`spark-defaults-conf.spark.driver.memory`|`1664m` |

### <a name="change-the-default-number-of-cores-and-memory-for-the-spark-driver-across-all-resources-with-spark-ie-for-the-spark-service"></a>Изменение числа ядер и размера памяти по умолчанию для драйвера Spark во всех ресурсах Spark (то есть для службы Spark)
Измените число ядер по умолчанию на 2 и размер памяти по умолчанию на 7424m для службы Spark.

```bash
azdata bdc spark settings set spark-defaults-conf.spark.driver.cores=2, spark-defaults-conf.spark.driver.memory=7424m
```

### <a name="change-the-default-number-of-cores-and-memory-for-the-spark-executors-in-the-storage-pool"></a>Изменение числа ядер и размера памяти по умолчанию для исполнителей Spark в пуле носителей
Измените число ядер исполнителей по умолчанию на 4 для пула носителей.

```bash
azdata bdc spark settings set spark-defaults-conf.spark.executor.cores=4 --resource=storage-0
```

### <a name="view-the-pending-settings-changes-staged-in-the-bdc"></a>Просмотр ожидающих изменений параметров, подготовленных в кластере больших данных
Вы можете просмотреть ожидающие изменения параметров только для службы Spark и для всего кластера больших данных.

#### <a name="pending-spark-service-settings"></a>Ожидающие параметры службы Spark
```bash
azdata bdc spark settings show --filter-option=pending --include-details
```

### <a name="spark-service"></a>Служба Spark

|Параметр|Текущее значение|Настроенное значение|Настраивается|Настроено |Время последнего изменения|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1`| `2` | `true` | `true` |
|`spark-defaults-conf.spark.driver.memory`|`1664m`| `7424m` | `true` | `true` |

#### <a name="all-pending-settings"></a>Все ожидающие параметры
```bash
azdata bdc settings show --filter-option=pending --include-details --recursive
```

Ожидающие параметры службы Spark

|Параметр|Текущее значение|Настроенное значение|Настраивается|Настроено|Время последнего изменения|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.driver.cores`|`1`| `2` | `true` | `true` |
|`spark-defaults-conf.spark.driver.memory`|`1664m`| `7424m` | `true` | `true` |

Ожидающие параметры ресурса Spark Storage-0

|Параметр|Текущее значение|Настроенное значение|Настраивается|Настроено|Время последнего изменения|
| --- | --- | --- | --- | --- | --- |
|`spark-defaults-conf.spark.executor.cores`|`1`| `4` | `true` | `true` |

### <a name="apply-the-pending-settings-to-the-bdc"></a>Применение ожидающих параметров к кластеру больших данных

```bash
azdata bdc settings apply
```

### <a name="monitor-the-status-of-the-bdc-configuration-update"></a>Отслеживание состояния обновления конфигурации для кластера больших данных

```bash
azdata bdc status show -all
```

## <a name="optional-steps"></a>Другие возможные шаги

### <a name="revert-pending-configuration-settings"></a>Отмена ожидающих параметров конфигурации

Если вы решите, что больше не хотите применять ожидающие параметры конфигурации, их подготовку можно отменить. При этом ожидающие параметры отменяются на всех уровнях.

```bash
azdata bdc settings revert
```

### <a name="abort-the-configuration-upgrade"></a>Прерывание обновления конфигурации

Если обновление конфигурации для любого из компонентов завершилось сбоем, можно отменить процесс обновления и восстановить предыдущие конфигурации кластера больших данных. Параметры, которые были подготовлены к изменению во время обновления, снова перейдут в состояние ожидания.

```bash
azdata bdc settings cancel-apply
```

## <a name="next-steps"></a>Дальнейшие действия

[Настройка кластера больших данных SQL Server](configure-bdc-overview.md)
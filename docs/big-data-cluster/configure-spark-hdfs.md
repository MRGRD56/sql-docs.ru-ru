---
title: Apache Spark и Apache Hadoop
titleSuffix: Configure Apache Spark and Apache Hadoop in Big Data Clusters
description: Кластеры больших данных SQL Server позволяют использовать решения Spark и HDFS. Узнайте, как их настроить.
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.date: 08/04/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3c018c3882f5a74bb55a3b4cb5fc23881b0daec8
ms.sourcegitcommit: 765262cdc6352a5325148afc22fa4f1499fe1aa3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2021
ms.locfileid: "102514861"
---
# <a name="configure-apache-spark-and-apache-hadoop-in-big-data-clusters"></a>Настройка Apache Spark и Apache Hadoop в Кластерах больших данных

Чтобы настроить Apache Spark и Apache Hadoop в Кластерах больших данных, необходимо изменить профиль кластера во время развертывания.

Кластер больших данных имеет четыре категории конфигураций: 

- `sql` 
- `hdfs` 
- `spark` 
- `gateway` 

`sql`, `hdfs`, `spark`, `sql` являются службами. Каждая служба соответствует одноименной категории конфигураций. Все конфигурации шлюзов относятся к категории `gateway`. 

Например, все конфигурации в службе `hdfs` относятся к категории `hdfs`. Обратите внимание, что все конфигурации Hadoop (core-site), HDFS и Zookeeper относятся к категории `hdfs`; все конфигурации Livy, Spark, Yarn и Hive Metastore относятся к категории `spark`. 

В разделе [Поддерживаемые конфигурации](reference-config-spark-hadoop.md) перечислены свойства Apache Spark и Hadoop, которые можно настроить при развертывании кластера больших данных SQL Server.

В следующих разделах перечислены свойства, которые **невозможно** изменить в кластере.

- [Неподдерживаемые `spark`конфигурации](reference-config-spark-hadoop.md#unsupported-spark-configurations)
- [Неподдерживаемые `hdfs`конфигурации](reference-config-spark-hadoop.md#unsupported-hdfs-configurations)
- [Неподдерживаемые `gateway`конфигурации](reference-config-spark-hadoop.md#unsupported-gateway-configurations)


## <a name="configurations-via-cluster-profile"></a>Настройка конфигураций через профиль кластера

В профиле кластера имеются ресурсы и службы. Во время развертывания конфигурации можно указывать одним из двух способов. 

* Во-первых, на уровне ресурса. 

   Ниже приведены примеры файлов исправлений для профиля. 

   ```json
   { 
         "op": "add", 
         "path": "spec.resources.zookeeper.spec.settings", 
         "value": { 
           "hdfs": { 
             "zoo-cfg.syncLimit": "6" 
           } 
         } 
   }
   ```

   Или сделайте так: 

   ```json
   { 
         "op": "add", 
         "path": "spec.resources.gateway.spec.settings", 
         "value": { 
           "gateway": { 
             "gateway-site.gateway.httpclient.socketTimeout": "95s" 
           } 
         } 
   } 
   ```

* Во-вторых, на уровне службы. Назначьте службе несколько ресурсов и укажите конфигурации службы.

Ниже приведен пример файла исправления для настройки размера блока HDFS в профиле: 

   ```json
   { 
         "op": "add", 
         "path": "spec.services.hdfs.settings", 
         "value": { 
           "hdfs-site.dfs.block.size": "268435456" 
        } 
   } 
   ```

Служба `hdfs` определяется следующим образом:

```json
{ 
  "spec": { 
   "services": { 
     "hdfs": { 
        "resources": [ 
          "nmnode-0", 
          "zookeeper", 
          "storage-0", 
          "sparkhead" 
        ], 
        "settings":{ 
          "hdfs-site.dfs.block.size": "268435456" 
        } 
      } 
    } 
  } 
} 
```
 
> [!NOTE]
> Конфигурации на уровне ресурсов переопределяют конфигурации на уровне службы. Один ресурс можно назначить нескольким службам.

## <a name="enable-spark-in-the-storage-pool"></a>Включение Spark в пуле носителей
Помимо поддерживаемых конфигураций Apache, мы предлагаем возможность настройки выполнения заданий Spark в пуле носителей. Это логическое значение, `includeSpark`, находится в файле конфигурации `bdc.json` в `spec.resources.storage-0.spec.settings.spark`.

Пример определения пула носителей в bdc.json может выглядеть следующим образом:
```json
...
"storage-0": {
                "metadata": {
                    "kind": "Pool",
                    "name": "default"
                },
                "spec": {
                    "type": "Storage",
                    "replicas": 2,
                    "settings": {
                        "spark": {
                            "includeSpark": "true"
                        }
                    }
                }
            }
```


## <a name="limitations"></a>Ограничения

Конфигурации можно задавать только на уровне категории. Для указания нескольких конфигураций одной подкатегории нельзя извлечь общий префикс в профиле кластера. 

```json
{ 
      "op": "add", 
      "path": "spec.services.hdfs.settings.core-site.hadoop", 
      "value": { 
        “proxyuser.xyz.users”: “*”, 
        “proxyuser.abc.users”: “*” 
     } 
} 
```

## <a name="next-steps"></a>Дальнейшие действия

- [Свойства конфигурации Apache Spark и Apache Hadoop (HDFS).](reference-config-spark-hadoop.md)
- [Справочник [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/reference/reference-azdata.md)
- [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)

---
title: Настройка развертываний
titleSuffix: SQL Server big data clusters
description: Сведения о том, как настроить развертывание кластера больших данных с помощью файлов конфигурации, встроенных в средство управления azdata.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rajmera3
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ffb41a559c6f262fe6d25a50c51b6e6553a2a7b8
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343923"
---
# <a name="configure-deployment-settings-for-cluster-resources-and-services"></a>Настройка параметров развертывания для кластерных ресурсов и служб

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]
> [!Note]
> Кластеры больших данных с накопительным пакетом обновления 9 или более поздней версии поддерживают функцию управления конфигурацией. Она обеспечивает настройку кластера после развертывания, улучшает видимость и расширяет возможности настройки. В версиях накопительного пакета обновления 8 и более ранних этой функции нет, и настройку можно производить только во время развертывания.

Начиная с предопределенного набора профилей конфигурации, встроенных в инструмент управления [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], можно легко изменять параметры по умолчанию для достижения лучшего соответствия требованиям рабочих нагрузок BDC. Структура файлов конфигурации позволяет детально обновлять параметры для каждой службы в ресурсе.

Просмотрите это 13-минутное видео, в котором представлены общие сведения о конфигурации кластеров больших данных:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Big-Data-Cluster-Configuration/player?WT.mc_id=dataexposed-c9-niner]

> [!TIP]
> Дополнительные сведения о развертывании высокодоступных служб см. в статьях, посвященных настройке **высокой доступности** для критически важных компонентов, таких как [главный экземпляр SQL Server](deployment-high-availability.md) или [узел имен HDFS](deployment-high-availability-hdfs-spark.md).

> [!TIP]
> Чтобы узнать, какие параметры можно настроить, см. статью [Свойства конфигурации кластеров больших данных SQL Server](reference-config-bdc-overview.md). Для версий накопительного пакета обновления 8 и более ранних конфигурации, доступные для главного экземпляра SQL Server, см. в статье [Свойства конфигурации главного экземпляра SQL Server (до накопительного пакета обновления 9)](reference-config-master-instance.md), а свойства Apache Spark и Hadoop — в статье [Свойства конфигурации Apache Spark и Apache Hadoop (HDFS)](reference-config-spark-hadoop.md).

Можно также задавать конфигурации уровня ресурсов или обновлять конфигурации для всех служб в ресурсе. Ниже приведена сводка по структуре для `bdc.json`.

```json
{
    "apiVersion": "v1",
    "metadata": {
        "kind": "BigDataCluster",
        "name": "mssql-cluster"
    },
    "spec": {
        "resources": {
            "nmnode-0": {...
            },
            "sparkhead": {...
            },
            "zookeeper": {...
            },
            "gateway": {...
            },
            "appproxy": {...
            },
            "master": {...
            },
            "compute-0": {...
            },
            "data-0": {...
            },
            "storage-0": {...
        },
        "services": {
            "sql": {
                "resources": [
                    "master",
                    "compute-0",
                    "data-0",
                    "storage-0"
                ]
            },
            "hdfs": {
                "resources": [
                    "nmnode-0",
                    "zookeeper",
                    "storage-0",
                    "sparkhead"
                ],
                "settings": {...
            },
            "spark": {
                "resources": [
                    "sparkhead",
                    "storage-0"
                ],
                "settings": {...
            }
        }
    }
}
```

Для обновления конфигураций уровня ресурсов, таких как число экземпляров в пуле, необходимо обновить спецификацию ресурсов. Например, чтобы обновить число экземпляров в пуле вычислений, измените этот раздел в файле конфигурации `bdc.json`:

```json
"resources": {
    ...
    "compute-0": {
        "metadata": {
            "kind": "Pool",
            "name": "default"
        },
        "spec": {
            "type": "Compute",
            "replicas": 4
        }
    }
    ...
}
``` 

Аналогичным образом выполняется изменение параметров отдельной службы в определенном ресурсе. Например, если вы хотите изменить параметры памяти Spark только для компонента Spark в пуле носителей, обновите ресурс `storage-0`, добавив в файл конфигурации `bdc.json` раздел `settings` для службы `spark`.

```json
"resources":{
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
                    "spark-defaults-conf.spark.driver.memory": "2g",
                    "spark-defaults-conf.spark.driver.cores": "1",
                    "spark-defaults-conf.spark.executor.instances": "3",
                    "spark-defaults-conf.spark.executor.memory": "1536m",
                    "spark-defaults-conf.spark.executor.cores": "1",
                    "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
                    "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
                    "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
                    "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
                    "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
                }
            }
        }
    }
    ...
}
```

Если вы хотите применить одни и те же конфигурации для службы, связанной с несколькими ресурсами, обновите соответствующие разделы `settings` в разделе `services`. Например, если нужно задать одинаковые параметры для Spark в обоих пулах носителей и пулах Spark, следует обновить раздел `settings` в разделе службы `spark` в файле конфигурации `bdc.json`.

```json
"services": {
    ...
    "spark": {
        "resources": [
            "sparkhead",
            "storage-0"
        ],
        "settings": {
            "spark-defaults-conf.spark.driver.memory": "2g",
            "spark-defaults-conf.spark.driver.cores": "1",
            "spark-defaults-conf.spark.executor.instances": "3",
            "spark-defaults-conf.spark.executor.memory": "1536m",
            "spark-defaults-conf.spark.executor.cores": "1",
            "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
            "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
            "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
            "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
            "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
        }
    }
    ...
}
```

Чтобы настроить файлы конфигурации развертывания кластера, можно использовать любой редактор формата JSON, например VSCode. Чтобы внести эти изменения в скрипты для автоматизации, используйте команду `azdata bdc config`. В этой статье описано, как настроить развертывания кластера больших данных, изменив файлы конфигурации развертывания. Она содержит примеры изменения конфигурации для различных сценариев. Дополнительные сведения об использовании файлов конфигурации используются в развертываниях, см. в [руководстве по развертыванию](deployment-guidance.md#configfile).

## <a name="prerequisites"></a>Предварительные требования

- [Установка azdata](../azdata/install/deploy-install-azdata.md).

- В каждом из примеров в этом разделе предполагается, что вы создали копию одной из стандартных конфигураций. Дополнительные сведения см. в статье [Создание настраиваемой конфигурации](deployment-guidance.md#customconfig). Например, следующая команда создает каталог с именем `custom-bdc`, содержащий два файла конфигурации развертывания JSON — `bdc.json` и `control.json`, основанных на конфигурации по умолчанию `aks-dev-test`:

   ```bash
   azdata bdc config init --source aks-dev-test --target custom-bdc
   ```

## <a name="change-default-docker-registry-repository-and-images-tag"></a><a id="docker"></a> Изменение тегов реестра, репозитория и образов Docker по умолчанию

Во встроенных файлах конфигурации, в частности в control.json, имеется раздел `docker`, в котором содержатся предварительно заполненные теги реестра, репозитория и образов контейнеров. По умолчанию образы, необходимые для кластеров больших данных, находятся в реестре контейнеров Майкрософт (`mcr.microsoft.com`), в репозитории `mssql/bdc`:

```json
{
    "apiVersion": "v1",
    "metadata": {
        "kind": "Cluster",
        "name": "mssql-cluster"
    },
    "spec": {
        "docker": {
            "registry": "mcr.microsoft.com",
            "repository": "mssql/bdc",
            "imageTag": "2019-GDR1-ubuntu-16.04",
            "imagePullPolicy": "Always"
        },
        ...
    }
}
```

Перед развертыванием можно настроить параметры `docker`, изменив их непосредственно в файле конфигурации `control.json` или с помощью команд `azdata bdc config`. Например, следующие команды обновляют файл конфигурации control.json в каталоге `custom-bdc`, задавая другие теги `<registry>`, `<repository>` и `<image_tag>`:

```bash
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.registry=<registry>"
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.repository=<repository>"
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.imageTag=<image_tag>"
```

> [!TIP]
> Рекомендуется задавать тег образа с указанием конкретной версии и не использовать тег образа `latest`, так как это может привести к несовпадению версий и как следствие к проблемам с работоспособностью кластера.

> [!TIP]
> Развертывание кластеров больших данных должно иметь доступ к реестру контейнеров и репозиторию, из которого извлекаются образы контейнеров. Если ваша среда не имеет доступа к реестру контейнеров Майкрософт по умолчанию, можно выполнить автономную установку, в которой необходимые образы сначала помещаются в частный репозиторий Docker. Дополнительные сведения об автономных установках см. в разделе [Выполнение автономного развертывания кластера больших данных SQL Server](deploy-offline.md). Обратите внимание, что перед выполнением развертывания необходимо задать [переменные среды](deployment-guidance.md#env) `DOCKER_USERNAME` и `DOCKER_PASSWORD`, чтобы рабочий процесс развертывания имел доступ к вашему частному репозиторию для извлечения из него образов.

## <a name="change-cluster-name"></a><a id="clustername"></a> Изменение имени кластера

Имя кластера — это имя кластера больших данных и пространство имен Kubernetes, которое будет создано при развертывании. Оно указывается в следующей части файла конфигурации развертывания `bdc.json`:

```json
"metadata": {
    "kind": "BigDataCluster",
    "name": "mssql-cluster"
},
```

Следующая команда отправляет пару "ключ — значение" в параметр `--json-values`, чтобы изменить имя кластера больших данных на `test-cluster`.

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "metadata.name=test-cluster"
```

> [!IMPORTANT]
> Имя кластера больших данных должно содержать только строчные буквы и цифры, без пробелов. Все артефакты Kubernetes (контейнеры, pod, наборы с отслеживанием состояния, службы) для кластера будут созданы в пространстве имен с именем, соответствующим указанному имени кластера.

## <a name="update-endpoint-ports"></a><a id="ports"></a> Обновление портов конечной точки

Конечные точки задаются для контроллера в файле `control.json`, а для шлюза и главного экземпляра SQL Server — в соответствующих разделах файла `bdc.json`. В следующей части файла конфигурации `control.json` показаны определения конечных точек для контроллера.

```json
{
  "endpoints": [
    {
      "name": "Controller",
      "serviceType": "LoadBalancer",
      "port": 30080
    },
    {
      "name": "ServiceProxy",
      "serviceType": "LoadBalancer",
      "port": 30777
    }
  ]
}
```

В следующем примере с помощью встроенного JSON изменяется порт для конечной точки `controller`.

```bash
azdata bdc config replace --config-file custom-bdc/control.json --json-values "$.spec.endpoints[?(@.name==""Controller"")].port=30000"
```

## <a name="configure-scale"></a><a id="replicas"></a> Настройка масштаба

Конфигурации каждого ресурса, такого как пул носителей, задаются в файле конфигурации `bdc.json`. Например, в следующей части `bdc.json` показано определение ресурса `storage-0`.

```json
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
                "spark-defaults-conf.spark.driver.memory": "2g",
                "spark-defaults-conf.spark.driver.cores": "1",
                "spark-defaults-conf.spark.executor.instances": "3",
                "spark-defaults-conf.spark.executor.memory": "1536m",
                "spark-defaults-conf.spark.executor.cores": "1",
                "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
                "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
                "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
                "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
                "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
            }
        }
    }
}
```

Можно настроить несколько экземпляров пула носителей, вычислений или данных, изменив значение `replicas` для каждого пула. В следующем примере с помощью встроенного JSON изменяются эти значения для пулов носителей вычислений и данных на `10`, `4` и `4` соответственно.

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.storage-0.spec.replicas=10"
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.compute-0.spec.replicas=4"
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.data-0.spec.replicas=4"
```

> [!NOTE]
> Максимальное число экземпляров, допустимое для пулов вычислений и данных, составляет `8` для каждого. Это ограничение не применяется во время развертывания, однако не рекомендуется настраивать более высокие значения в рабочих развертываниях.

## <a name="configure-storage"></a><a id="storage"></a> Настройка хранилища

Вы также может изменить характеристики и класс хранилища, используемые для каждого пула. В следующем примере пулам носителей и данных назначается пользовательский класс хранения, а также изменяется размер утверждения постоянного тома для хранения данных на 500 ГБ для HDFS (пула хранения) и на 100 ГБ для главного пула и пула данных. 

> [!TIP]
> Дополнительные сведения о конфигурации хранилища см. в статье [Сохраняемость данных при использовании кластера больших данных SQL Server в Kubernetes](concept-data-persistence.md).

Сначала создайте файл patch.json, как показано ниже, чтобы настроить параметры *хранения*.

```json
{
        "patch": [
                {
                        "op": "add",
                        "path": "spec.resources.storage-0.spec.storage",
                        "value": {
                                "data": {
                                        "size": "500Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                },
        {
                        "op": "add",
                        "path": "spec.resources.master.spec.storage",
                        "value": {
                                "data": {
                                        "size": "100Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                },
                {
                        "op": "add",
                        "path": "spec.resources.data-0.spec.storage",
                        "value": {
                                "data": {
                                        "size": "100Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                }
        ]
}

```

Затем можно использовать команду `azdata bdc config patch`, чтобы обновить файл конфигурации `bdc.json`.
```bash
azdata bdc config patch --config-file custom-bdc/bdc.json --patch ./patch.json
```

> [!NOTE]
> В файле конфигурации на основе `kubeadm-dev-test` отсутствует определение хранилища для каждого пула, но при необходимости его можно добавить с помощью приведенной выше процедуры.

## <a name="configure-storage-pool-without-spark"></a><a id="sparkstorage"></a> Настройка пула носителей без Spark

Кроме того, можно настроить пулы носителей для работы без Spark и создать отдельный пул Spark. Такая конфигурация позволяет масштабировать вычислительные мощности Spark независимо от хранилища. О том, как настроить пул Spark, см. в разделе [Создание пула Spark](#sparkpool) этой статьи.

> [!NOTE]
> Развертывание кластера больших данных без Spark не поддерживается. Поэтому вы должны либо задать для `includeSpark` значение `true`, либо создать отдельный [пул Spark](#sparkpool) по крайней мере с одним экземпляром. Также можно одновременно запустить Spark в пуле носителей (`includeSpark` имеет значение `true`) и иметь отдельный пул Spark.

По умолчанию параметр `includeSpark` для ресурса пула носителей имеет значение true, поэтому для внесения изменений необходимо добавить поле `includeSpark` в конфигурацию хранилища. Следующая команда демонстрирует, как изменить это значение с помощью встроенного JSON.

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.storage-0.spec.settings.spark.includeSpark=false"
```

## <a name="create-a-spark-pool"></a><a id="sparkpool"></a> Создание пула Spark

Пул Spark можно создать в дополнение к экземплярам Spark, работающим в пуле носителей, или вместо них. В следующем примере показано, как создать пул Spark с двумя экземплярами путем исправления файла конфигурации `bdc.json`. 

Сначала создайте файл `spark-pool-patch.json`, как показано ниже.

```json
{
    "patch": [
        {
            "op": "add",
            "path": "spec.resources.spark-0",
            "value": {
                "metadata": {
                    "kind": "Pool",
                    "name": "default"
                },
                "spec": {
                    "type": "Spark",
                    "replicas": 2
                }
            }
        },
        {
            "op": "add",
            "path": "spec.services.spark.resources/-",
            "value": "spark-0"
        },
        {
            "op": "add",
            "path": "spec.services.hdfs.resources/-",
            "value": "spark-0"
        }
    ]
}
```

Затем выполните следующую команду `azdata bdc config patch`:

```bash
azdata bdc config patch -c custom-bdc/bdc.json -p spark-pool-patch.json
```

## <a name="configure-pod-placement-using-kubernetes-labels"></a><a id="podplacement"></a> Настройка размещения pod с помощью меток Kubernetes

Вы можете управлять размещением pod на узлах Kubernetes, имеющих определенные ресурсы для удовлетворения различных типов требований к рабочей нагрузке. С помощью меток Kubernetes можно задать, какие узлы в кластере Kubernetes будут использоваться для развертывания ресурсов кластера больших данных, а также ограничить круг узлов, используемых для конкретных ресурсов.
Например, может потребоваться, чтобы объекты pod ресурса пула носителей размещались на узлах с увеличенным объемом хранилища, а главные экземпляры SQL Server — на узлах с увеличенными ресурсами ЦП и памяти. В этом случае сначала вы создадите разнородный кластер Kubernetes с различными типами оборудования, а затем [назначите метки узлов](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) соответствующим образом. Во время развертывания кластера больших данных можно задать одинаковые метки на уровне кластера, чтобы указать, какие узлы используются для кластера больших данных, с помощью атрибута `clusterLabel` в файле `control.json`. Затем для размещения на уровне пула будут использоваться разные метки. Эти метки можно указать в файлах конфигурации развертывания кластера больших данных с помощью атрибута `nodeLabel`. Kubernetes назначает объекты pod на узлы, соответствующие заданным меткам. Конкретные ключи меток, которые необходимо добавить к узлам в кластере Kubernetes: `mssql-cluster` (чтобы указать узлы, используемые для кластера больших данных) и `mssql-resource` (чтобы указать конкретные узлы, на которых размещаются объекты pod, для различных ресурсов). В качестве значений этих меток можно задать любые выбранные вами строки.

> [!NOTE]
> В силу особенностей pod, которые выполняют сбор метрик на уровне узла, pod `metricsdc` развертываются на всех узлах с меткой `mssql-cluster`, и к этим pod метка `mssql-resource` относиться не будет.

В следующем примере показано, как изменить пользовательский файл конфигурации, чтобы включить метку узла `bdc` для всего кластера больших данных, метку `bdc-master` для размещения объектов pod главного экземпляра SQL Server на определенном узле, метку `bdc-storage-pool` для ресурсов пула носителей, метку `bdc-compute-pool` для объектов pod пула вычислений и пула данных, а также метку `bdc-shared` для остальных ресурсов. 

Сначала включите метки узлов Kubernetes:

```bash
kubectl label node <kubernetesNodeName1> mssql-cluster=bdc mssql-resource=bdc-shared --overwrite=true
kubectl label node <kubernetesNodeName2> mssql-cluster=bdc mssql-resource=bdc-master --overwrite=true
kubectl label node <kubernetesNodeName3> mssql-cluster=bdc mssql-resource=bdc-compute-pool --overwrite=true
kubectl label node <kubernetesNodeName4> mssql-cluster=bdc mssql-resource=bdc-compute-pool --overwrite=true
kubectl label node <kubernetesNodeName5> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName6> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName7> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName8> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
```

Затем обновите файлы конфигурации развертывания кластера, чтобы включить значения меток. В этом примере предполагается, что файлы конфигурации настраиваются в профиле `custom-bdc`. По умолчанию во встроенных конфигурациях отсутствуют ключи `nodeLabel` и `clusterLabel`, поэтому необходимо либо изменить пользовательский файл конфигурации вручную, либо внести нужные изменения с помощью команд `azdata bdc config add`.

```bash
azdata bdc config add -c custom-bdc/control.json -j "$.spec.clusterLabel=bdc"
azdata bdc config add -c custom-bdc/control.json -j "$.spec.nodeLabel=bdc-shared"

azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.master.spec.nodeLabel=bdc-master"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.compute-0.spec.nodeLabel=bdc-compute-pool"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.data-0.spec.nodeLabel=bdc-compute-pool"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.storage-0.spec.nodeLabel=bdc-storage-pool"

# below can be omitted in which case we will take the node label default from the control.json
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.nmnode-0.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.sparkhead.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.zookeeper.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.gateway.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.appproxy.spec.nodeLabel=bdc-shared"
```
>[!NOTE]
> Рекомендуется избегать предоставления главному узлу Kubernetes любой из указанных выше ролей BDC. Если вы все равно планируете назначить эти роли главному узлу Kubernetes, потребуется [удалить его отметку ``master:NoSchedule``.](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) Имейте в виду, что это может привести к перегрузке главного узла и препятствовать осуществлению им управления Kubernetes в больших кластерах. В общем случае в любом развертывании можно наблюдать, что для главного узла запланированы некоторые объекты pod: они уже допускают отметку ``master:NoSchedule`` и в основном используются для помощи при управлении кластером. 

## <a name="other-customizations-using-json-patch-files"></a><a id="jsonpatch"></a> Другие настройки с использованием файлов исправлений JSON

Файлы исправлений JSON настраивают несколько параметров одновременно. Дополнительные сведения об исправлениях JSON см. в статьях [Исправления JSON в Python](https://github.com/stefankoegl/python-json-patch) и [JSONPath Online Evaluator](https://jsonpath.com/).

Следующие файлы `patch.json` выполняют следующие изменения.

- Обновление порта отдельной конечной точки в `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "$.spec.endpoints[?(@.name=='Controller')].port",
      "value": 30000
    }
  ]
}
```

- Обновление всех конечных точек (`port` и `serviceType`) в `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.endpoints",
      "value": [
        {
          "serviceType": "LoadBalancer",
          "port": 30001,
          "name": "Controller"
        },
        {
          "serviceType": "LoadBalancer",
          "port": 30778,
          "name": "ServiceProxy"
        }
      ]
    }
  ]
}
```

- Обновление параметров хранения контроллера в `control.json`. Эти параметры применимы ко всем компонентам кластера, если только они не переопределены на уровне пула.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.storage",
      "value": {
        "data": {
          "className": "managed-premium",
          "accessMode": "ReadWriteOnce",
          "size": "100Gi"
        },
        "logs": {
          "className": "managed-premium",
          "accessMode": "ReadWriteOnce",
          "size": "32Gi"
        }
      }
    }
  ]
}
```

- Обновление имени класса хранения в `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.storage.data.className",
      "value": "managed-premium"
    }
  ]
}
```

- Обновление параметров хранения для пула носителей в `bdc.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.resources.storage-0.spec",
      "value": {
        "type": "Storage",
        "replicas": 2,
        "storage": {
          "data": {
            "size": "100Gi",
            "className": "myStorageClass",
            "accessMode": "ReadWriteOnce"
          },
          "logs": {
            "size": "32Gi",
            "className": "myStorageClass",
            "accessMode": "ReadWriteOnce"
          }
        }
      }
    }
  ]
}
```

- Обновление параметров Spark для пула носителей в `bdc.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.services.spark.settings",
      "value": {
            "spark-defaults-conf.spark.driver.memory": "2g",
            "spark-defaults-conf.spark.driver.cores": "1",
            "spark-defaults-conf.spark.executor.instances": "3",
            "spark-defaults-conf.spark.executor.memory": "1536m",
            "spark-defaults-conf.spark.executor.cores": "1",
            "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
            "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
            "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
            "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
            "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
      }
    }
  ]
}
```

> [!TIP]
> Дополнительные сведения о структуре и способах изменения файла конфигурации развертывания см. в [справочнике по файлу конфигурации развертывания для кластеров больших данных](reference-deployment-config.md).

Используйте команды `azdata bdc config`, чтобы применить эти изменения в файле исправления JSON. В следующем примере файл `patch.json` применяется к целевому файлу конфигурации развертывания `custom-bdc/bdc.json`.

```bash
azdata bdc config patch --config-file custom-bdc/bdc.json --patch-file ./patch.json
```

## <a name="disable-elasticsearch-to-run-in-privileged-mode"></a>Отключение запуска ElasticSearch в привилегированном режиме

По умолчанию контейнер ElasticSearch запускается в привилегированном режиме в кластере больших данных. Этот параметр гарантирует, что во время инициализации контейнер будет иметь достаточные разрешения для обновления параметра на узле, который требуется, когда ElasticSearch обрабатывает большее количество журналов. Дополнительные сведения по этому вопросу см. в [этой статье](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html). 

Чтобы отключить контейнер, запускающий ElasticSearch в привилегированном режиме, необходимо обновить раздел `settings` в файле `control.json` и указать для `vm.max_map_count` значение `-1`. Ниже приведен пример, как мог бы выглядеть этот раздел.

```json
{
    "apiVersion": "v1",
    "metadata": {...},
    "spec": {
        "docker": {...},
        "storage": {...},
        "endpoints": [...],
        "settings": {
            "ElasticSearch": {
                "vm.max_map_count": "-1"
            }
        }
    }
}
```

Можно вручную изменить файл `control.json` и добавить в `spec` приведенный выше раздел или создать файл исправления `elasticsearch-patch.json`, подобный показанному ниже, и с помощью [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] исправить файл `control.json`.

```json
{
  "patch": [
    {
      "op": "add",
      "path": "spec.settings",
      "value": {
            "ElasticSearch": {
                "vm.max_map_count": "-1"
        }
      }
    }
  ]
}
```

Чтобы исправить файл конфигурации, выполните следующую команду:

```bash
azdata bdc config patch --config-file custom-bdc/control.json --patch-file elasticsearch-patch.json
```

> [!IMPORTANT]
> Рекомендуется вручную обновить параметр `max_map_count` на каждом узле в кластере Kubernetes в соответствии с инструкциями из [этой статьи](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html).

## <a name="turn-pods-and-nodes-metrics-collection-onoff"></a>Включение и отключение сбора метрик для модулей pod и узлов

В SQL Server 2019 с накопительным пакетом обновления 5 (CU5) появились два параметра для управления сбором метрик для модулей pod и узлов. Если вы используете различные решения для мониторинга инфраструктуры Kubernetes, вы можете отключить встроенный сбор метрик для модулей pod и узлов, присвоив параметрам *allowNodeMetricsCollection* и *allowPodMetricsCollection* значения *false* в файле конфигурации развертывания *control.json*. Для сред OpenShift для этих параметров по умолчанию задано значение *false* во встроенных профилях развертывания, так как сбор метрик модулей pod и узлов требует привилегированных возможностей.
Выполните следующую команду, чтобы обновить значения этих параметров в пользовательском файле конфигурации с помощью CLI *azdata*.

```bash
 azdata bdc config replace -c custom-bdc/control.json -j "$.security.allowNodeMetricsCollection=false"
 azdata bdc config replace -c custom-bdc/control.json -j "$.security.allowPodMetricsCollection=false"
 ```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения об использовании файлов конфигурации в развертываниях кластеров больших данных см. в статье [Развертывание [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в Kubernetes](deployment-guidance.md#configfile).
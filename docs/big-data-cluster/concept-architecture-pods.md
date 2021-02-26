---
title: Развертываемые ресурсы
titleSuffix: SQL Server Big Data Clusters
description: Описание объектов pod, которые обычно развертываются в кластере больших данных SQL Server.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 20f31d6cbf4bcdbbdc666b50dea626f79e298db1
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343936"
---
# <a name="resources-deployed-with-big-data-cluster"></a>Ресурсы, развертываемые с помощью кластера больших данных

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Эта статья описывает ресурсы, развертываемые кластером больших данных SQL Server.

Кластер больших данных развертывает объекты pod на основе профиля развертывания. Дополнительные сведения см. в разделе [Конфигурации по умолчанию](deployment-guidance.md#configfile). 

Эта статья описывает объекты pod, развертываемые с использованием профиля `aks-dev-test-ha`, и пул Spark. Для просмотра объектов pod, развернутых в кластере, отправьте запрос в Kubernetes. Следующий пример возвращает список объектов pod в определенном пространстве имен.

```bash
kubectl get pods -n <namespace>
```

Замените `<namespace>` именем кластера больших данных. 

Дополнительные сведения см. в статье [Развертывание [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в Kubernetes](deployment-guidance.md#configfile).

На следующей схеме показаны компоненты, развернутые в кластере больших данных:

:::image type="content" source="media/big-data-cluster-overview/architecture-diagram-overview.png" alt-text="big-data-cluster-diagram":::

Дополнительные сведения об архитектуре см. в разделе [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md).

## <a name="deployed-pods"></a>Развернутые модули

В следующей таблице приведен список объектов pod, развернутых в кластере больших данных.

|Имя  |Область|
|---------|---------|
|`control-<nnnn>`        |[Управление](#control)|
|`controldb-<#>`         |[Управление](#control)|
|`controlwd-<nnnn>`      |[Управление](#control)|
|`logsdb-<#>`            |[Управление](#control)|
|`logsui-<nnnn>`         |[Управление](#control)|
|`metricsdb-<#>`         |[Управление](#control)|
|`metricsdc-<nnnn>`      |[Управление](#control)|
|`metricsui-<nnnn>`      |[Управление](#control)|
|`mgmtproxy-<nnnn>`      |[Управление](#control)|
|`zookeeper-<#>`         |[Управление](#control)|
|`dns-<nnnn>`            |[Управление](#control)|
|`master-<#n>`           |[Главный экземпляр](#master-instance)|
|`operator-<nnnn>`       |[Главный экземпляр](#master-instance)
|`compute-<#n>-<#m>`     |[Вычислительный пул](#compute-pool)|
|`data-<#>-<#>`          |[Пул данных](#data-pool) |
|`storage-<#>-<#>`       |[Пул носителей](#storage-pool)|
|`nmnode-<#>-<#>`        |[Пул носителей](#storage-pool)|
|`sparkhead-<#>`         |[Пул носителей](#storage-pool)|
|`appproxy-<#m>`         |[Пул приложений](#application-pool)|
|`gateway-<#>`           |[Служба шлюза](#gateway-service)|

Не все объекты pod включены в каждый кластер BDC. Развертывания с высоким уровнем доступности или интеграции с Active Directory включают специальные объекты pod. 

### <a name="high-availability-specific-pods"></a>Объекты pod, связанные с высокой доступностью:

- `operator-<nnnn>`
- `zookeeper-<#>`

### <a name="active-directory-specific-pods"></a>Объекты pod, связанные с Active Directory:

- `dns-<nnnn>`

В следующих разделах описаны объекты pod и перечислены контейнеры в каждом из объектов.

## <a name="control"></a>Control

Объекты pod управления предоставляют службу управления.

|Имя объекта pod |Count| Тип контроллера Kubernetes | Контейнеры |
|--------|----|------|--------|-------|
|`control-#`|1| ReplicaSet |- `controller`<br><br>- `security-support`<br><br>- `fluentbit`
|`controldb`|1| StatefulSet |- `mssql-server`<br><br>- `fluentbit`
|`controlwd`|1|  ReplicaSet |- `controlwatchdog`
|`logsdb-#` |1| StatefulSet |- `elasticsearch`
|`logsui`   |1| ReplicaSet |- `kibana`
|`metricsdb-#`|1| StatefulSet |- `influxdb`
|`metricsdc`|1 на узел Kubernetes. | DaemonSet |- `telegraf` |
|`metricsui-nnnn`|1| ReplicaSet |- `grafana` |
|`mgmtproxy-nnnn`|1| ReplicaSet |- `service-proxy`<br><br>- `fluentbit`|
|`dns-nnnn`|0 или 1 для интеграции с Active Directory| ReplicaSet |- `dns`<br><br>- `fluentbit`|

## <a name="master-instance"></a>Главный экземпляр

`master-<#n>` — это главный экземпляр SQL Server.

- Управляет пулом данных с помощью языка DDL.
- Управляет данными в пуле данных с помощью языка DML.
- Разгружает выполнение аналитического запроса в пул данных.

|Имя объекта pod |Count| Тип контроллера Kubernetes | Контейнеры |
|--------|----|------|--------|
|`master-<#n>`|1 или более для высокого уровня доступности.| StatefulSet|- `mssql-server`<br><br>- `fluentbit`<br><br>- `collectd`<br><br>- `mssql-ha-supervisor` <sup>*</sup>|
|`operator`<sup>*</sup>| 0 или 1 для высокого уровня доступности | ReplicaSet |- `mssql-ha-operator`

<sup>*</sup> Только развертывания с высоким уровнем доступности. Оператор реализует и регистрирует определение пользовательского ресурса для SQL Server и ресурсы группы доступности. При развертывании оператор регистрирует себя в качестве прослушивателя для уведомлений о ресурсах SQL Server, развертываемых в кластере Kubernetes. `mssql-ha-supervisor` поддерживает группу доступности.

Каждый объект pod `master` содержит один экземпляр SQL Server. Развертывание с высоким уровнем доступности включает 3 объекта pod. Каждый объект pod содержит экземпляр SQL Server с базами данных в группе доступности AlwaysOn SQL Server.

Вы можете включать дополнительные объекты pod во время развертывания, в зависимости от рабочей нагрузки. 

## <a name="compute-pool"></a>Вычислительный пул

Вычислительный пул предоставляет экземпляр SQL Server для вычислений.

|Имя объекта pod |Count| Тип контроллера Kubernetes | Контейнеры |
|--------|----|------|--------|
|`compute-<#n>-<#m>`|1 или более.| StatefulSet |- `mssql-server`<br><br>- `fluentbit`<br><br>- `collectd`

- `#n` идентифицирует вычислительный пул.
- `#m` идентифицирует идентификатор экземпляра в пуле.

Экземпляры SQL Server вычислительного пула не учитывают состояние. Им требуется хранилище только для `tempdb`.

Вы можете включать дополнительные объекты pod во время развертывания, в зависимости от рабочей нагрузки. 

## <a name="data-pool"></a>Пул данных

Пул данных предоставляет экземпляры SQL Server для хранения и вычислений.

|Имя объекта pod |Count| Тип контроллера Kubernetes | Контейнеры |
|--------|----|------|--------|-------|
|`data-<#n>-<#m>` | 0 или больше | StatefulSet | -` mssql-server` <br><br>- `fluentbit`<br><br>- `collectd`|

- `#n` идентифицирует пул данных.
- `#m` идентифицирует идентификатор экземпляра в пуле.

Вы можете включать дополнительные объекты pod во время развертывания, в зависимости от рабочей нагрузки.

## <a name="storage-pool"></a>Пул носителей

Пул носителей обеспечивает прием данных через Spark, хранилище в HDFS, доступ к данным через HDFS и конечные точки SQL Server.

|Имя объекта pod |Count| Тип контроллера Kubernetes | Контейнеры |
|--------|----|------|--------|
|`storage-0-#`|1 или более. Вы можете включать дополнительные объекты pod во время развертывания, в зависимости от рабочей нагрузки. | StatefulSet |- `hadoop`<br><br>- `mssql-server`<br><br>- `fluentbit`<br><br>
|`nmnode-0-#`|1 или более для высокого уровня доступности.| StatefulSet |- `hadoop`<br><br>- `fluentbit`
|`sparkehead-#`|1 или более для высокого уровня доступности.| StatefulSet |- `hadoop-yarn-jobhistory`<br><br>- `hadoop-livy-sparkhistory`<br><br>- `hadoop-hivemetastore`<br><br>-- `fluentbit`
|`zookeeper`|0 или 3 для высокого уровня доступности. | StatefulSet |- `zookeeper`<br><br>- `fluentbit`

## <a name="application-pool"></a>Пул приложений

Пул приложений включен в некоторые профили конфигурации теста. В пуле приложений размещаются прокси-серверы службы приложений, которые вы определяете при развертывании приложений для кластеров больших данных. 

`appproxy` — это веб-API, располагающийся перед приложениями пула приложений. Он выполняет проверку подлинности пользователей, а затем направляет запросы в приложения.

|Имя объекта pod | Тип контроллера Kubernetes | Контейнеры  |
|--------|----|------|
|`appproxy`| ReplicaSet |- `app-service-proxy`<br><br>- `fluentbit`

Дополнительные сведения см. в разделе [Развертывание приложения в кластере больших данных](concept-application-deployment.md).

Вы можете включать дополнительные объекты pod во время развертывания, в зависимости от рабочей нагрузки. 

## <a name="gateway-service"></a>Служба шлюза

Службы шлюза предоставляют шлюз Knox для Spark, HDFS, [Yarn](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), пользовательский интерфейс Yarn и пользовательский интерфейс Spark.

|Имя объекта pod | Тип контроллера Kubernetes | Контейнеры |
|--------|-----|-----|
|`gateway-<#>`| StatefulSet |- `knox`<br><br>- `fluentbit`

Поддерживается только один шлюз.

## <a name="open-source-container-references"></a>Ссылки на контейнеры с открытым исходным кодом

Список проектов с открытым кодом и версий см. в статье [Справочник по ПО с открытым кодом](reference-open-source-software.md).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] см. в следующих ресурсах.

- [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Семинар. Архитектура [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Майкрософт](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
- [Развертывание [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в Kubernetes](deployment-guidance.md#configfile)

---
title: Выполнение мониторинга кластера с помощью панели мониторинга Grafana
titleSuffix: SQL Server Big Data Clusters
description: Мониторинг кластера больших данных SQL Server 2019 с помощью панели мониторинга Grafana.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 04/06/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 52ca3c49f2efbfa72a5f3353f306584e9422de4e
ms.sourcegitcommit: a177a1e17200400a70f1d61b737481c83249e9a3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2021
ms.locfileid: "107583961"
---
# <a name="monitor-cluster-with-azdata-and-grafana-dashboard"></a>Выполнение мониторинга кластера с помощью azdata и панели мониторинга Grafana

В этой статье описывается мониторинг приложений в кластерах больших данных SQL Server.

## <a name="prerequisites"></a>Предварительные требования

- [Кластер больших данных SQL Server 2019](deployment-guidance.md)
- [Служебная программа командной строки azdata](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Возможности

В SQL Server 2019 можно создать, удалить, описать, инициализировать, перечислить, запустить и обновить приложение. В следующей таблице описаны команды развертывания приложения, которые можно использовать с **azdata**.

|Get-Help |Описание |
|:---|:---|
|`azdata bdc endpoint list` | Список конечных точек для кластера больших данных. |


Следующий пример можно использовать для перечисления конечной точки **панели мониторинга Grafana**:

```bash
azdata bdc endpoint list --endpoint-name metricsui 
```

В выходных данных будет предоставлена конечная точка, в которой можно использовать имя пользователя и пароль для входа в кластер. 

![Панель мониторинга Grafana](media/big-data-cluster-monitor-apps/grafana-dashboard-endpoint.png)

Значения `nodeMetricsUrl` и `sqlMetricsUrl` ссылаются на панель мониторинга Grafana для мониторинга метрик узла Kubernetes и метрик службы кластеров больших данных.

![Панель мониторинга Grafana](./media/view-cluster-status/grafana-dashboard.png)

![SQL](./media/view-cluster-status/grafana-sql-status.png)



## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] см. в статье [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
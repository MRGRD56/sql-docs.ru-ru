---
title: Извлечение журналов кластера с помощью панели мониторинга Kibana
titleSuffix: SQL Server Big Data Clusters
description: Мониторинг кластера больших данных SQL Server 2019 с помощью панели мониторинга Kibana.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 04/06/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 75a7d58a25e7027450b53d1ce387635847bfdfe2
ms.sourcegitcommit: 554497d604e0c63c055bf6d572d92fdadb027dbf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2021
ms.locfileid: "107571326"
---
# <a name="check-out-cluster-logs--with-kibana-dashboard"></a>Извлечение журналов кластера с помощью панели мониторинга Kibana

В этой статье описывается мониторинг приложений в [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)].

## <a name="prerequisites"></a>Предварительные требования

- [[!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)
- [Служебная программа командной строки azdata](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Возможности

В [!INCLUDE[sssql19-md](../includes/sssql19-md.md)] можно создать, удалить, описать, инициализировать, перечислить, запустить и обновить приложение. В следующей таблице описаны команды развертывания приложения, которые можно использовать с **azdata**.

|Get-Help |Описание |
|:---|:---|
|`azdata bdc endpoint list` | Выводит список конечных точек для [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)]. |


Следующий пример можно использовать для вывода конечной точки **панели мониторинга Kibana**:

```bash
azdata bdc endpoint list --endpoint-name logsui 
```

В выходных данных будет предоставлена конечная точка, в которой можно использовать имя пользователя и пароль для входа в кластер. 

![Панель мониторинга Kibana](media/big-data-cluster-monitor-cluster/kibana-dashboard-endpoint.png)


Ссылка на панель мониторинга Kibana:

![Панель мониторинга Kibana](./media/view-cluster-status/kibana-dashboard.png)

> [!NOTE]
> Браузер Microsoft Edge старой версии несовместим с Kibana. Чтобы эта панель мониторинга отображалась корректно, необходимо использовать браузер Edge на базе Chromium. При загрузке панелей мониторинга в неподдерживаемом браузере вы увидите пустую страницу (см. [поддерживаемые Kibana браузеры](https://www.elastic.co/support/matrix#matrix_browsers)).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] см. в статье [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).



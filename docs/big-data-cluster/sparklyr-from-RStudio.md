---
title: Использование sparklyr из RStudio
titleSuffix: SQL Server big data clusters
description: Узнайте, как использовать sparklyr в кластере больших данных SQL Server для подключения к Spark через интерфейс R.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 614b456c40588af2c134f5d3a347fec9f74ae476
ms.sourcegitcommit: d1051f05a7db81ec62d9785bb6af572408f3d4e0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2020
ms.locfileid: "88680439"
---
# <a name="use-sparklyr-in-sql-server-big-data-cluster"></a>Использование sparklyr в кластерах больших данных SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Sparklyr предоставляет интерфейс R для Apache Spark. Sparklyr является популярным инструментом среди разработчиков R, использующих Spark. В этой статье описывается, как использовать sparklyr в [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] с помощью RStudio.

## <a name="prerequisites"></a>предварительные требования

- [Разверните кластер больших данных SQL Server 2019](quickstart-big-data-cluster-deploy.md).

### <a name="install-rstudio-desktop"></a>Установите RStudio Desktop.

Установите и настройте **RStudio Desktop**, выполнив следующие действия.

1. Если вы используете клиент Windows, [скачайте и установите R 3.4.4](https://cran.rstudio.com/bin/windows/base/old/3.4.4).

1. [Скачайте и установите RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

1. После завершения установки выполните следующие команды в RStudio Desktop, чтобы установить необходимые пакеты.

   ```RStudioDesktop
   install.packages("DBI", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   install.packages("dplyr", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   install.packages("sparklyr", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   ```

## <a name="connect-to-spark-in-a-big-data-cluster"></a>Подключение к Spark в кластере больших данных

Sparklyr можно использовать для подключения из клиента к кластеру больших данных с помощью Livy и шлюза HDFS/Spark. 

В RStudio создайте скрипт R и подключитесь к Spark, как показано в следующем примере.

> [!TIP]
> В качестве значений `<AZDATA_USERNAME>` и `<AZDATA_PASSWORD>` используйте имя пользователя и пароль, заданные при развертывании кластера больших данных.

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

Значения `<IP>` и `<PORT>` см. в документации по [подключению к кластеру больших данных](connect-to-big-data-cluster.md).

```r
library(sparklyr)
library(dplyr)
library(DBI)

#Specify the Knox username and password
config <- livy_config(user = "<AZDATA_USERNAME>", password = "<AZDATA_PASSWORD>")

httr::set_config(httr::config(ssl_verifypeer = 0L, ssl_verifyhost = 0L))

sc <- spark_connect(master = "https://<IP>:<PORT>/gateway/default/livy/v1",
                    method = "livy",
                    config = config)
```

## <a name="run-sparklyr-queries"></a>Выполнение запросов sparklyr

После подключения к Spark можно запускать sparklyr. В следующем примере выполняется запрос к набору данных iris с помощью sparklyr.

```r
iris_tbl <- copy_to(sc, iris)

iris_count <- dbGetQuery(sc, "SELECT COUNT(*) FROM iris")

iris_count
```

## <a name="distributed-r-computations"></a>Распределенные вычисления R

Одной из функций sparklyr является возможность [распределять вычисления R](https://spark.rstudio.com/guides/distributed-r/) с помощью [spark_apply](https://spark.rstudio.com/guides/distributed-r/#apply-an-r-function-to-a-spark-object).

Так как кластеры больших данных используют подключения Livy, необходимо задать `packages = FALSE` в вызове **spark_apply**. Дополнительные сведения см. в [разделе Livy](https://spark.rstudio.com/guides/distributed-r/#livy) документации sparklyr по распределенным вычислениям R. С помощью этого параметра можно использовать только те пакеты R, которые уже установлены в кластере Spark в коде R, переданном в **spark_apply**. Следующий пример демонстрирует использование этой функции.

```r
iris_tbl %>% spark_apply(function(e) nrow(e), names = "nrow", group_by = "Species", packages = FALSE)
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о кластерах больших данных см. в статье [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]](big-data-cluster-overview.md).

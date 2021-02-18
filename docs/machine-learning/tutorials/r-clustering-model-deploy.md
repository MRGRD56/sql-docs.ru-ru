---
title: Руководство по Развертывание модели кластеризации на языке R
titleSuffix: SQL machine learning
description: В четвертой части этого цикла учебников вы развернете модель кластеризации в R с помощью машинного обучения SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: b1dc019023546a4d1912480bfe452b3067f36bf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339622"
---
# <a name="tutorial-deploy-a-clustering-model-in-r-with-sql-machine-learning"></a>Руководство по развертыванию модели кластеризации в R с помощью машинного обучения SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
В четвертой части этого цикла учебников вы развернете модель кластеризации, разработанную на R, в базе данных с помощью Служб машинного обучения SQL Server или в Кластерах больших данных.
::: moniker-end
::: moniker range="=sql-server-2017"
В четвертой части этого цикла учебников вы развернете модель кластеризации, разработанную на R, в базе данных с помощью Служб машинного обучения SQL Server.
::: moniker-end
::: moniker range="=sql-server-2016"
В четвертой части этого цикла учебников вы развернете модель кластеризации, разработанную на R, в базе данных с помощью служб SQL Server R Services.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
В четвертой части этого цикла учебников вы развернете модель кластеризации, разработанную на Python, в базе данных с помощью Служб машинного обучения в Управляемом экземпляре SQL Azure.
::: moniker-end

Чтобы регулярно выполнять кластеризацию по мере регистрации новых клиентов, необходимо иметь возможность вызывать сценарий R из любого приложения. Для этого можно развернуть сценарий R в базе данных, поместив его в хранимую процедуру SQL. Так как модель выполняется в базе данных, ее можно легко обучать на данных, хранящихся в базе данных.

В этой статье вы узнаете, как выполнять следующие задачи.

> [!div class="checklist"]
> * Создание хранимой процедуры, которая формирует модель
> * Выполнение кластеризации
> * Использование сведений кластеризации

В [первой части](r-clustering-model-introduction.md) были установлены необходимые компоненты и восстановлена демонстрационная база данных.

Во [второй части](r-clustering-model-prepare-data.md) вы узнали, как подготовить данные из базы данных для выполнения кластеризации.

В [третьей части](r-clustering-model-build.md) вы узнали, как создать и обучить модель кластеризации на основе метода k-средних в R.

## <a name="prerequisites"></a>Предварительные требования

* В четвертой части этого цикла учебников предполагается, что вы уже выполнили предварительные требования из [**первой части**](r-clustering-model-introduction.md), а также действия, указанные во [**второй**](r-clustering-model-build.md) и [**третьей**](r-clustering-model-build.md) частях.

## <a name="create-a-stored-procedure-that-generates-the-model"></a>Создание хранимой процедуры, которая формирует модель

Чтобы создать хранимую процедуру, выполните следующий скрипт T-SQL. Эта процедура воссоздает шаги разработки, которые вы выполнили во второй и третьей частях этого цикла учебников:

* классификация клиентов на основе журнала покупок и возвратов;
* создание четырех кластеров клиентов с помощью алгоритма k-средних.

Процедура сохраняет полученные сопоставления клиентов с кластерами в таблице базы данных **customer_return_clusters**.

```sql
USE [tpcxbb_1gb]
DROP PROC IF EXISTS generate_customer_return_clusters;
GO
CREATE procedure [dbo].[generate_customer_return_clusters]
AS
/*
  This procedure uses R to classify customers into different groups
  based on their purchase & return history.
*/
BEGIN
    DECLARE @duration FLOAT
    , @instance_name NVARCHAR(100) = @@SERVERNAME
    , @database_name NVARCHAR(128) = db_name()
-- Input query to generate the purchase history & return metrics
    , @input_query NVARCHAR(MAX) = N'
SELECT ss_customer_sk AS customer,
    round(CASE 
            WHEN (
                    (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio,
    round(CASE 
            WHEN (
                    (orders_items = 0)
                    OR (returns_items IS NULL)
                    OR (orders_items IS NULL)
                    OR ((returns_items / orders_items) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio,
    round(CASE 
            WHEN (
                    (orders_money = 0)
                    OR (returns_money IS NULL)
                    OR (orders_money IS NULL)
                    OR ((returns_money / orders_money) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio,
    round(CASE 
            WHEN (returns_count IS NULL)
                THEN 0.0
            ELSE returns_count
            END, 0) AS frequency
FROM (
    SELECT ss_customer_sk,
        -- return order ratio
        COUNT(DISTINCT (ss_ticket_number)) AS orders_count,
        -- return ss_item_sk ratio
        COUNT(ss_item_sk) AS orders_items,
        -- return monetary amount ratio
        SUM(ss_net_paid) AS orders_money
    FROM store_sales s
    GROUP BY ss_customer_sk
    ) orders
LEFT OUTER JOIN (
    SELECT sr_customer_sk,
        -- return order ratio
        count(DISTINCT (sr_ticket_number)) AS returns_count,
        -- return ss_item_sk ratio
        COUNT(sr_item_sk) AS returns_items,
        -- return monetary amount ratio
        SUM(sr_return_amt) AS returns_money
    FROM store_returns
    GROUP BY sr_customer_sk
    ) returned ON ss_customer_sk = sr_customer_sk
 '
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
# Define the connection string

connStr <- paste("Driver=SQL Server; Server=", instance_name,
                 "; Database=", database_name,
                 "; uid=Username;pwd=Password; ",
                 sep="" )

# Input customer data that needs to be classified.
# This is the result we get from the query.
library(RODBC)

ch <- odbcDriverConnect(connStr);

customer_data <- sqlQuery(ch, input_query)

sqlDrop(ch, "customer_return_clusters")

## create clustering model
clust <- kmeans(customer_data[,2:5],4)

## create clustering output for table
customer_cluster <- data.frame(cluster=clust$cluster,customer=customer_data$customer,orderRatio=customer_data$orderRatio,
            itemsRatio=customer_data$itemsRatio,monetaryRatio=customer_data$monetaryRatio,frequency=customer_data$frequency)

## write cluster output to DB table
sqlSave(ch, customer_cluster, tablename = "customer_return_clusters")

## clean up
odbcClose(ch)
'
    , @input_data_1 = N''
    , @params = N'@instance_name nvarchar(100), @database_name nvarchar(128), @input_query nvarchar(max), @duration float OUTPUT'
    , @instance_name = @instance_name
    , @database_name = @database_name
    , @input_query = @input_query
    , @duration = @duration OUTPUT;
END;

GO
```

## <a name="perform-clustering"></a>Выполнение кластеризации

Теперь, создав хранимую процедуру, выполните следующий сценарий для выполнения кластеризации.

```sql
--Empty table of the results before running the stored procedure
TRUNCATE TABLE customer_return_clusters;

--Execute the clustering
--This will load the table customer_return_clusters with cluster mappings
EXECUTE [dbo].[generate_customer_return_clusters];
```

Убедитесь, что он работает и что у нас действительно имеется список клиентов и их сопоставлений с кластерами.

```sql
--Select data from table customer_return_clusters
--to verify that the clustering data was loaded
SELECT TOP (5) *
FROM customer_return_clusters;
```

```result
cluster  customer  orderRatio  itemsRatio  monetaryRatio  frequency
1        29727     0           0           0              0
4        26429     0           0           0.041979       1
2        60053     0           0           0.065762       3
2        97643     0           0           0.037034       3
2        32549     0           0           0.031281       4
```

## <a name="use-the-clustering-information"></a>Использование сведений кластеризации

Так как процедура кластеризации сохранена в базе данных, она может эффективно выполнять кластеризацию для данных клиента, хранящихся в той же базе данных. Эту процедуру можно выполнять при каждом обновлении данных клиента и использовать обновленные сведения кластеризации.

Предположим, требуется отправить рекламное сообщение электронной почты клиентам в кластере 0, неактивной группе (описание четырех кластеров см. в [третьей части](r-clustering-model-build.md#analyze-the-results) этого учебника). Следующий код выбирает адреса электронной почты клиентов в кластере 0.

```sql
USE [tpcxbb_1gb]
--Get email addresses of customers in cluster 0 for a promotion campaign
SELECT customer.[c_email_address], customer.c_customer_sk
  FROM dbo.customer
  JOIN
  [dbo].[customer_clusters] as c
  ON c.Customer = customer.c_customer_sk
  WHERE c.cluster = 0
```

Чтобы получить адреса электронной почты клиентов в других кластерах, можно изменить значение **c.cluster**.

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы не собираетесь продолжать работу с этим учебником, можно удалить базу данных tpcxbb_1gb.

## <a name="next-steps"></a>Дальнейшие действия

В четвертой части этого цикла учебников вы узнали, как выполнять следующие задачи:

* Создание хранимой процедуры, которая формирует модель
* Выполнение кластеризации с помощью машинного обучения SQL
* Использование сведений кластеризации

Дополнительные сведения об использовании R в Службах машинного обучения:

* [Запуск простых скриптов R](quickstart-r-create-script.md)
* [Структуры данных, типы данных и объекты R](quickstart-r-data-types-and-objects.md)
* [Функции R](quickstart-r-functions.md)

---
title: Руководство по подготовке данных к выполнению кластеризации на языке R
titleSuffix: SQL machine learning
description: Во второй части этого цикла учебников, состоящего из четырех частей, вы подготовите данные из базы данных для выполнения кластеризации в R с помощью машинного обучения SQL.
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 358f34e281c5c5b1a750135117886247c5e504c0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348066"
---
# <a name="tutorial-prepare-data-to-perform-clustering-in-r-with-sql-machine-learning"></a>Руководство по подготовке данных для кластеризации в R с помощью машинного обучения SQL
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
Во второй части этого цикла учебников, состоящего из четырех частей, вы подготовите данные из базы данных для выполнения кластеризации в R с помощью Служб машинного обучения SQL Server или в кластерах больших данных.
::: moniker-end
::: moniker range="=sql-server-2017"
Во второй части этого цикла учебников, состоящего из четырех частей, вы подготовите данные из базы данных для выполнения кластеризации в R с помощью служб машинного обучения SQL Server.
::: moniker-end
::: moniker range="=sql-server-2016"
Во второй части этого цикла учебников, состоящего из четырех частей, вы подготовите данные из базы данных для выполнения кластеризации в R с помощью служб SQL Server 2016 R.
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
Во второй части этого цикла учебников, состоящего из четырех частей, вы подготовите данные из базы данных для выполнения кластеризации в R с помощью служб машинного обучения управляемого экземпляра SQL Azure.
::: moniker-end

В этой статье вы узнаете, как выполнять следующие задачи.

> [!div class="checklist"]
> * Разделение клиентов по разным измерениям с использованием языка R.
> * Загрузка данных из базы данных в кадр данных R.

В [первой части](r-clustering-model-introduction.md) были установлены необходимые компоненты и восстановлена демонстрационная база данных.

В [третьей части](r-clustering-model-build.md) вы узнаете, как создать и обучить модель кластеризации на основе k-средних в R.

В [четвертой части](r-clustering-model-deploy.md) вы узнаете, как создать хранимую процедуру в базе данных, которая может выполнять кластеризацию в R на основе новых данных.

## <a name="prerequisites"></a>Предварительные требования

* Во второй части этого цикла учебников предполагается, что вы уже выполнили [**первую часть**](r-clustering-model-introduction.md).

## <a name="separate-customers"></a>Разделение клиентов

Создайте файл RScript в RStudio и выполните следующий сценарий.
В SQL-запросе разделение клиентов выполняется по таким измерениям:

* **orderRatio** = коэффициент возвратов заказов (отношение числа частично или полностью возвращенных заказов к общему числу заказов)
* **itemsRatio** = коэффициент возврата единицы товара (отношение возвращенных единиц товара к общему числу проданных единиц товара)
* **monetaryRatio** = коэффициент возврата в денежном выражении (отношение общего объема возвратов к общему объему покупок в денежном выражении)
* **frequency** = частота возвратов

В функции **connStr** замените **ServerName** собственными данными для подключения.

```r
# Define the connection string to connect to the tpcxbb_1gb database

connStr <- "Driver=SQL Server;Server=ServerName;Database=tpcxbb_1gb;uid=Username;pwd=Password"

#Define the query to select data
input_query <- "
SELECT ss_customer_sk AS customer
    ,round(CASE 
            WHEN (
                       (orders_count = 0)
                    OR (returns_count IS NULL)
                    OR (orders_count IS NULL)
                    OR ((returns_count / orders_count) IS NULL)
                    )
                THEN 0.0
            ELSE (cast(returns_count AS NCHAR(10)) / orders_count)
            END, 7) AS orderRatio
    ,round(CASE 
            WHEN (
                     (orders_items = 0)
                  OR (returns_items IS NULL)
                  OR (orders_items IS NULL)
                  OR ((returns_items / orders_items) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_items AS NCHAR(10)) / orders_items)
            END, 7) AS itemsRatio
    ,round(CASE 
            WHEN (
                     (orders_money = 0)
                  OR (returns_money IS NULL)
                  OR (orders_money IS NULL)
                  OR ((returns_money / orders_money) IS NULL)
                 )
            THEN 0.0
            ELSE (cast(returns_money AS NCHAR(10)) / orders_money)
            END, 7) AS monetaryRatio
    ,round(CASE 
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
    ) returned ON ss_customer_sk = sr_customer_sk";
```

## <a name="load-the-data-into-a-data-frame"></a>Загрузка данных в кадр данных

Теперь воспользуйтесь приведенным ниже сценарием, чтобы вернуть результаты из запроса в кадр данных R.

```r
# Query using input_query and get the results back
# to data frame customer_data

library(RODBC)

ch <- odbcDriverConnect(connStr)

customer_data <- sqlQuery(ch, input_query)

# Take a look at the data just loaded
head(customer_data, n = 5);
```

Результат должен иметь следующий вид.

```results
  customer orderRatio itemsRatio monetaryRatio frequency
1    29727          0          0      0.000000         0
2    26429          0          0      0.041979         1
3    60053          0          0      0.065762         3
4    97643          0          0      0.037034         3
5    32549          0          0      0.031281         4
```

## <a name="clean-up-resources"></a>Очистка ресурсов

Если вы не собираетесь продолжать работу с этим учебником, удалите базу данных tpcxbb_1gb.

## <a name="next-steps"></a>Дальнейшие действия

Во второй части этого цикла учебников вы узнали, как выполнять следующие задачи:

* Разделение клиентов по разным измерениям с использованием языка R.
* Загрузка данных из базы данных в кадр данных R.

Чтобы создать модель машинного обучения, которая использует эти данные о клиентах, перейдите к третьей части этого учебника:

> [!div class="nextstepaction"]
> [Создание прогнозной модели в R с помощью машинного обучения SQL](r-clustering-model-build.md)

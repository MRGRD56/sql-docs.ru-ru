---
title: 'Запрос данных HDFS: пул носителей'
titleSuffix: SQL Server Big Data Clusters
description: В этом учебнике показано, как выполняется запрос данных HDFS в кластере больших данных SQL Server 2019. Для этого создается внешняя таблица на основе данных в пуле носителей, после чего выполняется запрос.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: aa376549bf3d6430ab5967e193069d35650be76b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043554"
---
# <a name="tutorial-query-hdfs-in-a-sql-server-big-data-cluster"></a>Руководство по Запрос данных HDFS в кластере больших данных SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

В этом руководстве описывается, каким образом выполняется запрос данных HDFS в [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].

В этом руководстве описано следующее:

> [!div class="checklist"]
> * Создание внешней таблицы, указывающей на данные HDFS в кластере больших данных.
> * Объединение этих данных с важными данными в главном экземпляре.

> [!TIP]
> При необходимости вы можете скачать и выполнить скрипт, содержащий команды из этого руководства. См. инструкции в разделе [Примеры виртуализации данных](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/data-virtualization) на сайте GitHub.

Это 7-минутное видео посвящено обращению к данным HDFS в кластере больших данных:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Query-HDFS-data-inside-SQL-Server-big-data-cluster/player?WT.mc_id=dataexposed-c9-niner]

## <a name="prerequisites"></a><a id="prereqs"></a> Предварительные требования

- [Средства работы с большими данными](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Расширение SQL Server 2019**
- [Загрузка примера данных в кластер больших данных](tutorial-load-sample-data.md)

## <a name="create-an-external-table-to-hdfs"></a>Создание внешней таблицы в HDFS

В пуле носителей располагается CSV-файл с данными о посещениях веб-страниц, хранящийся в HDFS. Выполните следующие действия, чтобы определить внешнюю таблицу, которая сможет получать доступ к данным в этом файле.

1. В Azure Data Studio установите подключение к главному экземпляру SQL Server в кластере больших данных. Дополнительные сведения см. в разделе [Подключение к главному экземпляру SQL Server](connect-to-big-data-cluster.md#master).

1. Дважды щелкните подключение в окне **Серверы**, чтобы открыть панель мониторинга сервера для главного экземпляра SQL Server. Выберите **Создать запрос**.

   ![Запрос главного экземпляра SQL Server](./media/tutorial-query-hdfs-storage-pool/sql-server-master-instance-query.png)

1. Выполните следующую команду Transact-SQL, чтобы изменить контекст на базу данных **Sales** в главном экземпляре.

   ```sql
   USE Sales
   GO
   ```

1. Определите формат CSV-файла для чтения из HDFS. Нажмите клавишу F5, чтобы выполнить инструкцию.

   ```sql
   CREATE EXTERNAL FILE FORMAT csv_file
   WITH (
       FORMAT_TYPE = DELIMITEDTEXT,
       FORMAT_OPTIONS(
           FIELD_TERMINATOR = ',',
           STRING_DELIMITER = '"',
           FIRST_ROW = 2,
           USE_TYPE_DEFAULT = TRUE)
   );
   ```

1. Создайте внешний источник данных для пула носителей, если это не было сделано ранее.

   ```sql
   IF NOT EXISTS(SELECT * FROM sys.external_data_sources WHERE name = 'SqlStoragePool')
   BEGIN
     CREATE EXTERNAL DATA SOURCE SqlStoragePool
     WITH (LOCATION = 'sqlhdfs://controller-svc/default');
   END
   ```

1. Создайте внешнюю таблицу, которая может считывать `/clickstream_data` из пула носителей. К **SqlStoragePool** может осуществляться доступ из главного экземпляра кластера больших данных.

   ```sql
   CREATE EXTERNAL TABLE [web_clickstreams_hdfs]
   ("wcs_click_date_sk" BIGINT , "wcs_click_time_sk" BIGINT , "wcs_sales_sk" BIGINT , "wcs_item_sk" BIGINT , "wcs_web_page_sk" BIGINT , "wcs_user_sk" BIGINT)
   WITH
   (
       DATA_SOURCE = SqlStoragePool,
       LOCATION = '/clickstream_data',
       FILE_FORMAT = csv_file
   );
   GO
   ```

## <a name="query-the-data"></a>Запрос данных

Выполните следующий запрос, чтобы объединить данные HDFS во внешней таблице `web_clickstream_hdfs` с реляционными данными в локальной базе данных `Sales`.

```sql
SELECT  
    wcs_user_sk,
    SUM( CASE WHEN i_category = 'Books' THEN 1 ELSE 0 END) AS book_category_clicks,
    SUM( CASE WHEN i_category_id = 1 THEN 1 ELSE 0 END) AS [Home & Kitchen],
    SUM( CASE WHEN i_category_id = 2 THEN 1 ELSE 0 END) AS [Music],
    SUM( CASE WHEN i_category_id = 3 THEN 1 ELSE 0 END) AS [Books],
    SUM( CASE WHEN i_category_id = 4 THEN 1 ELSE 0 END) AS [Clothing & Accessories],
    SUM( CASE WHEN i_category_id = 5 THEN 1 ELSE 0 END) AS [Electronics],
    SUM( CASE WHEN i_category_id = 6 THEN 1 ELSE 0 END) AS [Tools & Home Improvement],
    SUM( CASE WHEN i_category_id = 7 THEN 1 ELSE 0 END) AS [Toys & Games],
    SUM( CASE WHEN i_category_id = 8 THEN 1 ELSE 0 END) AS [Movies & TV],
    SUM( CASE WHEN i_category_id = 9 THEN 1 ELSE 0 END) AS [Sports & Outdoors]
  FROM [dbo].[web_clickstreams_hdfs]
  INNER JOIN item it ON (wcs_item_sk = i_item_sk
                        AND wcs_user_sk IS NOT NULL)
GROUP BY  wcs_user_sk;
GO
```

## <a name="clean-up"></a>Очистка

Выполните следующую команду, чтобы удалить внешнюю таблицу, используемую в этом руководстве.

```sql
DROP EXTERNAL TABLE [dbo].[web_clickstreams_hdfs];
GO
```

## <a name="next-steps"></a>Дальнейшие действия

В следующей статье вы сможете ознакомиться с выполнением запросов к Oracle из кластера больших данных.
> [!div class="nextstepaction"]
> [Запрос внешних данных в Oracle](tutorial-query-oracle.md)

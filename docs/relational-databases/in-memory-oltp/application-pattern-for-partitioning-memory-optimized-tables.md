---
title: Модель приложения — секционирование таблиц, оптимизированных для памяти
description: Познакомьтесь со схемой создания приложения с выполняющейся в памяти OLTP, где текущие активные данные хранятся в оптимизированной для памяти таблице, а более старые данные — в секционированной таблице.
ms.custom: seo-dt-2019,issue-PR=4700-14820
ms.date: 05/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 3f867763-a8e6-413a-b015-20e9672cc4d1
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f7cda5a657e6efd7ea6d9607d03eb8185455afba
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489685"
---
# <a name="application-pattern-for-partitioning-memory-optimized-tables"></a>Модель приложения для секционирования таблиц, оптимизированных для памяти

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[hek_2](../../includes/hek-2-md.md)] поддерживает шаблон разработки приложения, который тратит ресурсы производительности на относительно актуальные данные. Этот шаблон может применяться, когда текущие данные считываются или обновляются гораздо чаще, чем старые данные. В этом случае мы называем текущие данные *активными* или *горячими*, а более старые данные — *холодными*.

Основная идея заключается в хранении *горячих* данных в таблице, оптимизированной для памяти. Каждую неделю или месяц более старые данные, которые стали *холодными*, перемещаются в секционированную таблицу. Данные секционированной таблицы хранятся на диске, например на жестком диске, а не в памяти.

Как правило, в этой структуре используется ключ **datetime**, позволяющий процессу перемещения эффективно различать горячие и холодные данные.

## <a name="advanced-partitioning"></a>Расширенное секционирование

В этой структуре предполагается имитировать секционированную таблицу, которая также содержит один раздел, оптимизированный для памяти. Чтобы эта структура работала, необходимо убедиться, что все таблицы совместно используют общую схему. В примере кода ниже в этой статье демонстрируется этот метод.

Новые данные считаются горячими по определению. Горячие данные вставляются в таблицу, оптимизированную для памяти, и обновляются там же. Холодные данные хранятся в традиционной секционированной таблице. Периодически хранимая процедура добавляет новую секцию. Секция содержит последние холодные данные, которые были перемещены из таблицы, оптимизированной для памяти.

Если операция требует только горячих данных, она может использовать скомпилированные в собственном коде хранимые процедуры для доступа к ним. Операции, которым нужны горячие или холодные данные, должны использовать интерпретируемые [!INCLUDE[tsql](../../includes/tsql-md.md)], чтобы присоединить оптимизированную для памяти таблицу с секционированной таблицей.

### <a name="add-a-partition"></a>Добавление секции

Данные, которые недавно стали холодным, должны быть перемещены в секционированную таблицу. Ниже приведены шаги для этого периодического переключения секций:

1. Для данных в таблице, оптимизированной для памяти, определите дату и время, которые являются границей, или разделом, между горячими и новыми холодными данными.
2. Вставьте новые данные из выполняющейся в памяти таблицы OLTP в таблицу *cold\_staging*.
3. Удаление тех же холодных данных из оптимизированной для памяти таблицы.
4. Переключите таблицу cold\_staging в секцию.
5. Добавьте секцию.

#### <a name="maintenance-window"></a>Период обслуживания

Один из предыдущих шагов состоит в том, чтобы удалить новые холодные данные из таблицы, оптимизированной для памяти. Между удалением и последним этапом, на котором добавляется новая секция, проходит интервал времени. В течение этого интервала все приложения, пытающиеся прочитать новые холодные данные, завершатся ошибкой.

Подходящий пример см. в разделе [Секционирование уровня приложения](../../relational-databases/in-memory-oltp/application-level-partitioning.md).

## <a name="code-sample"></a>Образец кода

Следующий пример Transact-SQL отображается в последовательности небольших блоков кода только для простоты представления. Их можно добавить в один большой блок кода для тестирования.

В целом пример T-SQL показывает, как использовать таблицу, оптимизированную для памяти, с секционированной таблицей на диске.

Первые этапы примера T-SQL создают базу данных, а затем такие объекты, как таблицы в базе данных. На последующих этапах показано перемещение данных из таблицы, оптимизированной для памяти, в секционированную таблицу.

### <a name="create-a-database"></a>Создание базы данных

В этой части примера T-SQL создается тестовая база данных. База данных настроена для поддержки оптимизированных для памяти таблиц и секционированных таблиц.

```sql
CREATE DATABASE PartitionSample;
GO

-- Add a FileGroup, enabled for In-Memory OLTP.
-- Change file path as needed.

ALTER DATABASE PartitionSample
    ADD FILEGROUP PartitionSample_mod
    CONTAINS MEMORY_OPTIMIZED_DATA;

ALTER DATABASE PartitionSample
    ADD FILE(
        NAME = 'PartitionSample_mod',
        FILENAME = 'c:\data\PartitionSample_mod')
    TO FILEGROUP PartitionSample_mod;
GO
```

### <a name="create-a-memory-optimized-table-for-hot-data"></a>Создание оптимизированной для памяти таблицы для горячих данных

В этом разделе создается таблица, оптимизированная для памяти, содержащая последние данные, которые в основном являются горячими.

```sql
USE PartitionSample;
GO

-- Create a memory-optimized table for the HOT Sales Order data.
-- Notice the index that uses datetime2.

CREATE TABLE dbo.SalesOrders_hot (
   so_id INT IDENTITY PRIMARY KEY NONCLUSTERED,
   cust_id INT NOT NULL,
   so_date DATETIME2 NOT NULL INDEX ix_date NONCLUSTERED,
   so_total MONEY NOT NULL,
   INDEX ix_date_total NONCLUSTERED (so_date desc, so_total desc)
) WITH (MEMORY_OPTIMIZED=ON);
GO
```

### <a name="create-a-partitioned-table-for-cold-data"></a>Создание секционированной таблицы для холодных данных

В этом разделе создается секционированная таблица, содержащая холодные данные.

```sql
-- Create a partition and table for the COLD Sales Order data.
-- Notice the index that uses datetime2.

CREATE PARTITION FUNCTION [ByDatePF](datetime2) AS RANGE RIGHT
   FOR VALUES();
GO

CREATE PARTITION SCHEME [ByDateRange]
   AS PARTITION [ByDatePF]
   ALL TO ([PRIMARY]);
GO

CREATE TABLE dbo.SalesOrders_cold (
   so_id INT NOT NULL,
   cust_id INT NOT NULL,
   so_date DATETIME2 NOT NULL,
   so_total MONEY NOT NULL,
   CONSTRAINT PK_SalesOrders_cold PRIMARY KEY (so_id, so_date),
   INDEX ix_date_total NONCLUSTERED (so_date desc, so_total desc)
) ON [ByDateRange](so_date);
GO
```

### <a name="create-a-table-to-store-cold-data-during-move"></a>Создание таблицы для хранения холодных данных во время перемещения

В этом разделе создается таблица cold\_staging. Также создается представление, которое объединяет горячие и холодные данные из двух таблиц.

```sql
-- A table used to briefly stage the newly cold data, during moves to a partition.

CREATE TABLE dbo.SalesOrders_cold_staging (
   so_id INT NOT NULL,
   cust_id INT NOT NULL,
   so_date datetime2 NOT NULL,
   so_total MONEY NOT NULL,
   CONSTRAINT PK_SalesOrders_cold_staging PRIMARY KEY (so_id, so_date),
   INDEX ix_date_total NONCLUSTERED (so_date desc, so_total desc),
   CONSTRAINT CHK_SalesOrders_cold_staging CHECK (so_date >= '1900-01-01')
);
GO

-- A view, for retrieving the aggregation of hot plus cold data.

CREATE VIEW dbo.SalesOrders
AS SELECT so_id,
          cust_id,
          so_date,
          so_total,
          1 AS 'is_hot'
       FROM dbo.SalesOrders_hot
   UNION ALL
   SELECT so_id,
          cust_id,
          so_date,
          so_total,
          0 AS 'is_cold'
       FROM dbo.SalesOrders_cold;
GO
```

### <a name="create-the-stored-procedure"></a>Создание хранимой процедуры

В этом разделе создается хранимая процедура, которая выполняется периодически. Процедура перемещает новые холодные данные из таблицы, оптимизированной для памяти, в секционированную таблицу.

```sql
-- A stored procedure to move all newly cold sales orders data
-- to its staging location.

CREATE PROCEDURE dbo.usp_SalesOrdersOffloadToCold @splitdate datetime2
   AS
   BEGIN
      BEGIN TRANSACTION;

      -- Insert the cold data as a temporary heap.
      INSERT INTO dbo.SalesOrders_cold_staging WITH (TABLOCKX)
      SELECT so_id , cust_id , so_date , so_total
         FROM dbo.SalesOrders_hot WITH (serializable)
         WHERE so_date <= @splitdate;

      -- Delete the moved data from the hot table.
      DELETE FROM dbo.SalesOrders_hot WITH (SERIALIZABLE)
         WHERE so_date <= @splitdate;

      -- Update the partition function, and switch in the new partition.
      ALTER PARTITION SCHEME [ByDateRange] NEXT USED [PRIMARY];

      DECLARE @p INT = (
        SELECT MAX(partition_number)
            FROM sys.partitions
            WHERE object_id = OBJECT_ID('dbo.SalesOrders_cold'));

      EXEC sp_executesql
        N'ALTER TABLE dbo.SalesOrders_cold_staging
            SWITCH TO dbo.SalesOrders_cold partition @i',
        N'@i int',
        @i = @p;

      ALTER PARTITION FUNCTION [ByDatePF]()
      SPLIT RANGE( @splitdate);

      -- Modify a constraint on the cold_staging table, to align with new partition.
      ALTER TABLE dbo.SalesOrders_cold_staging
         DROP CONSTRAINT CHK_SalesOrders_cold_staging;

      DECLARE @s nvarchar( 100) = CONVERT( nvarchar( 100) , @splitdate , 121);
      DECLARE @sql nvarchar( 1000) = N'alter table dbo.SalesOrders_cold_staging 
         add constraint CHK_SalesOrders_cold_staging check (so_date > ''' + @s + ''')';
      PRINT @sql;
      EXEC sp_executesql @sql;

      COMMIT;
END;
GO
```

### <a name="prepare-sample-data-and-demo-the-stored-procedure"></a>Подготовка примера данных и демонстрация хранимой процедуры

Этот раздел создает и вставляет пример данных, а затем запускает хранимую процедуру как демонстрацию.

```sql
-- Insert sample values into the hot table.
INSERT INTO dbo.SalesOrders_hot VALUES(1,SYSDATETIME(), 1);
GO
INSERT INTO dbo.SalesOrders_hot VALUES(1, SYSDATETIME(), 1);
GO
INSERT INTO dbo.SalesOrders_hot VALUES(1, SYSDATETIME(), 1);
GO

-- Verify that the hot data is in the table, by selecting from the view.
SELECT * FROM dbo.SalesOrders;
GO

-- Treat all data in the hot table as cold data:
-- Run the stored procedure, to move (offload) all sales orders to date to cold storage.
DECLARE @t datetime2 = SYSDATETIME();
EXEC dbo.usp_SalesOrdersOffloadToCold @t;

-- Again, read hot plus cold data from the view.
SELECT * FROM dbo.SalesOrders;
GO

-- Retrieve the name of every partition.
SELECT OBJECT_NAME( object_id) , * FROM sys.dm_db_partition_stats ps
   WHERE object_id = OBJECT_ID( 'dbo.SalesOrders_cold');

-- Insert more data into the hot table.
INSERT INTO dbo.SalesOrders_hot VALUES(2, SYSDATETIME(), 1);
GO
INSERT INTO dbo.SalesOrders_hot VALUES(2, SYSDATETIME(), 1);
GO
INSERT INTO dbo.SalesOrders_hot VALUES(2, SYSDATETIME(), 1);
GO

-- Read hot plus cold data from the view.
SELECT * FROM dbo.SalesOrders;
GO

-- Again, run the stored procedure, to move all sales orders to date to cold storage.
DECLARE @t datetime2 = SYSDATETIME();
EXEC dbo.usp_SalesOrdersOffloadToCold @t;

-- Read hot plus cold data from the view.
SELECT * FROM dbo.SalesOrders;
GO

-- Again, retrieve the name of every partition.
-- The stored procedure can modify the partitions.
SELECT OBJECT_NAME( object_id) , partition_number , row_count
  FROM sys.dm_db_partition_stats ps
  WHERE object_id = OBJECT_ID( 'dbo.SalesOrders_cold')
    AND index_id = 1;
```

### <a name="drop-all-the-demo-objects"></a>Удаление всех демонстрационных объектов

Не забудьте очистить демонстрационную тестовую базу данных для тестовой системы.

```sql
-- You must first leave the context of the PartitionSample database.

-- USE <A-Database-Name-Here>;
GO

DROP DATABASE PartitionSample;
GO
```

## <a name="see-also"></a>См. также:

[Таблицы, оптимизированные для памяти](./sample-database-for-in-memory-oltp.md)
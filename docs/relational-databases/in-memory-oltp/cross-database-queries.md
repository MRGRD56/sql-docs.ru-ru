---
title: Межбазовые запросы | Документация Майкрософт
description: Узнайте, как использовать табличные переменные, оптимизированные для памяти, в запросах между базами данных, чтобы перемещать данные из таблицы в одной базе данных в оптимизированные для памяти таблицы в другой базе данных в SQL Server.
ms.custom: ''
ms.date: 08/04/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: a0305f5b-91bd-4d18-a2fc-ec235b062fd3
author: rothja
ms.author: jroth
ms.openlocfilehash: 2e4a5b80032b0b76e72e9bdf226a833f00220a81
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489783"
---
# <a name="cross-database-queries"></a>Межбазовые запросы
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Начиная с [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], оптимизированные для памяти таблицы не поддерживают межбазовые транзакции. Нельзя получить доступ к другой базе данных из той же транзакции или того же запроса, которые также получают доступ к оптимизированной для памяти таблицы. Нельзя скопировать данные из одной таблицы в базе данных в оптимизированную для памяти таблицу в другой базе данных.  
  
 Табличные переменные не является транзакционными. Поэтому табличные переменные, оптимизированные для памяти, можно использовать в запросах между базами данных. Такие переменные упрощают перемещение данных из таблицы в одной базе данных в оптимизированные для памяти таблицы в другой базе данных. Можно использовать две транзакции. В первой транзакции вставьте данные из удаленной таблицы в переменную. Во второй транзакции вставьте данные в локальную оптимизированную для памяти таблицу из переменной.  Дополнительные сведения о табличных переменных, оптимизированных для памяти, см. в разделе [Улучшение производительности временной таблицы и табличной переменной с помощью оптимизации памяти](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md).
  
## <a name="example"></a>Пример
В этом примере показан способ передачи данных из одной базы данных в оптимизированную для памяти таблицу в другой базе данных.

1. Создайте тестовые объекты.  Выполните приведенный ниже код [!INCLUDE[tsql](../../includes/tsql-md.md)] в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  

    ```sql

    USE master;
    GO
    
    SET NOCOUNT ON;
    
    -- Create simple database
    CREATE DATABASE SourceDatabase;
    ALTER DATABASE SourceDatabase SET RECOVERY SIMPLE;
    GO

    -- Create a table and insert a few records
    USE SourceDatabase;
    
    CREATE TABLE SourceDatabase.[dbo].[SourceTable] (
        [ID] [int] PRIMARY KEY CLUSTERED,
        [FirstName] nvarchar(8)
        );
    
    INSERT [SourceDatabase].[dbo].[SourceTable]
    VALUES (1, N'Bob'),
        (2, N'Susan');
    GO

    -- Create a database with a MEMORY_OPTIMIZED_DATA filegroup

    CREATE DATABASE DestinationDatabase
     ON  PRIMARY 
    ( NAME = N'DestinationDatabase_Data', FILENAME = N'D:\DATA\DestinationDatabase_Data.mdf',   SIZE = 8MB), 
     FILEGROUP [DestinationDatabase_mod] CONTAINS MEMORY_OPTIMIZED_DATA  DEFAULT
    ( NAME = N'DestinationDatabase_mod', FILENAME = N'D:\DATA\DestinationDatabase_mod', MAXSIZE = UNLIMITED)
     LOG ON 
    ( NAME = N'DestinationDatabase_Log', FILENAME = N'D:\LOG\DestinationDatabase_Log.ldf', SIZE = 8MB);
    
    ALTER DATABASE DestinationDatabase SET RECOVERY SIMPLE;
    GO
    
    USE DestinationDatabase;
    GO

    -- Create a memory-optimized table
    CREATE TABLE [dbo].[DestTable_InMem] (
        [ID] [int] PRIMARY KEY NONCLUSTERED,
        [FirstName] nvarchar(8)
        )
    WITH ( MEMORY_OPTIMIZED = ON, DURABILITY = SCHEMA_AND_DATA );
    GO
    ```

2.  Попытайтесь выполнить межбазовый запрос. Выполните приведенный ниже код [!INCLUDE[tsql](../../includes/tsql-md.md)] в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].
  
    ```sql  
    INSERT [DestinationDatabase].[dbo].[DestTable_InMem]
    SELECT * FROM [SourceDatabase].[dbo].[SourceTable]
    ```  

    Должно появиться следующее сообщение об ошибке:
    > Сообщение 41317, уровень 16, состояние 5  
    > Пользовательская транзакция, обращающаяся к оптимизированным в памяти таблицам или модулям, скомпилированным в собственном коде, не может получить доступ к нескольким пользовательским базам данных или модели баз данных и msdb. Кроме того, такая транзакция не может выполнять запись в базу данных master.

3.  Создание типа оптимизированной для памяти таблицы.  Выполните приведенный ниже код [!INCLUDE[tsql](../../includes/tsql-md.md)] в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].

    ```sql
    USE DestinationDatabase;
    GO
    
    CREATE TYPE [dbo].[MemoryType]  
        AS TABLE  
        (  
        [ID] [int] PRIMARY KEY NONCLUSTERED,
        [FirstName] nvarchar(8)
        )  
        WITH  
            (MEMORY_OPTIMIZED = ON);  
    GO
    ```

4.  Еще раз попытайтесь выполнить межбазовый запрос.  На этот раз исходные данные сначала будут переданы в табличную переменную, оптимизированную для памяти.  Затем данные из табличной переменной будут переданы в таблицу, оптимизированную для памяти.
    ```sql
    -- Declare table variable utilizing the newly created type - MemoryType
    DECLARE @InMem dbo.MemoryType;
    
    -- Populate table variable
    INSERT @InMem SELECT * FROM SourceDatabase.[dbo].[SourceTable];
    
    -- Populate the destination memory-optimized table
    INSERT [DestinationDatabase].[dbo].[DestTable_InMem] SELECT * FROM @InMem;
    GO 
    ```
   
## <a name="see-also"></a>См. также:  
 [Миграция в In-Memory OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  

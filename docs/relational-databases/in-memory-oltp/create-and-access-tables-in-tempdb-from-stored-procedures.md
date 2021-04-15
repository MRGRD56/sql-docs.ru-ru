---
title: Создание таблиц в TempDB и доступ к ним из хранимых процедур
description: TempDB не поддерживает создание таблиц и обращение к ним из скомпилированных в собственном коде хранимых процедур. Используйте оптимизированные для памяти таблицы, либо табличные типы и табличные переменные.
ms.custom: seo-dt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 12be8011-b76c-45c1-8f55-7f46e0e374e9
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 55987ab8038eb1a7703e7cbf25abd894a35fb9c1
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489668"
---
# <a name="create-and-access-tables-in-tempdb-from-stored-procedures"></a>Создание таблиц в TempDB и доступ к ним из хранимых процедур
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Создание и обращение к таблицам в базе данных TempDB из скомпилированных в собственном коде хранимых процедур не поддерживается. Вместо этого используйте оптимизированные для памяти таблицы с параметром DURABILITY=SCHEMA_ONLY или табличные типы и переменные. 

Дополнительную информацию об оптимизации временных таблиц для памяти и табличных переменных см. в разделе: [Улучшение производительности временной таблицы и табличной переменной с помощью оптимизации памяти](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md).
  
  В следующем примере показано, как временную таблицу с тремя столбцами (ID, ProductID, Quantity) можно заменить табличной переменной **\@OrderQuantityByProduct** типа **dbo.OrderQuantityByProduct**:  
  
```sql  
CREATE TYPE dbo.OrderQuantityByProduct   
  AS TABLE   
   (id INT NOT NULL PRIMARY KEY NONCLUSTERED HASH WITH (BUCKET_COUNT=100000),   
    ProductID INT NOT NULL,   
    Quantity INT NOT NULL) WITH (MEMORY_OPTIMIZED=ON)  
GO  
CREATE PROCEDURE dbo.usp_OrderQuantityByProduct   
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH   
(  
    TRANSACTION ISOLATION LEVEL = SNAPSHOT,  
    LANGUAGE = N'ENGLISH'  
)  
  -- declare table variables for the list of orders   
  DECLARE @OrderQuantityByProduct dbo.OrderQuantityByProduct  
  
  -- populate input  
  INSERT @OrderQuantityByProduct SELECT ProductID, Quantity FROM dbo.[Order Details]  
  end  
```  
  
## <a name="see-also"></a>См. также:  
 [Проблемы миграции, связанные с хранимыми процедурами, скомпилированными в собственном коде](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [Конструкции языка Transact-SQL, не поддерживаемые в выполняющейся в памяти OLTP](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)  
  

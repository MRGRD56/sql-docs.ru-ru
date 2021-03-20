---
description: STATS_DATE (Transact-SQL)
title: STATS_DATE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STATS_DATE_TSQL
- STATS_DATE
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], last time updated
- STATS_DATE function
- query optimization statistics [SQL Server], last time updated
- last time statistics updated
- stats update date
ms.assetid: f9ec3101-1e41-489d-b519-496a0d6089fb
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 49aba3a5d9162bc4b48bb2961ae13b17914a9854
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748024"
---
# <a name="stats_date-transact-sql"></a>STATS_DATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает дату последнего обновления статистики для таблицы или индексированного представления.  
  
 Дополнительные сведения об обновлении статистики см. в разделе [Статистика](../../relational-databases/statistics/statistics.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
STATS_DATE ( object_id , stats_id )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *object_id*  
 Идентификатор таблицы или индексированного представления, содержащего статистику.  
  
 *stats_id*  
 Идентификатор объекта статистики.  
  
## <a name="return-types"></a>Типы возвращаемых значений  
 Возвращает **datetime** в случае успешного выполнения. Возвращает **NULL**, если большой двоичный объект статистики не был создан.  
  
## <a name="remarks"></a>Примечания  
 Системные функции можно использовать в списке выбора, в предложении WHERE и в любом месте, где может быть использовано выражение.  
 
 Дата обновления статистики хранится в [большом двоичном объекте статистики](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) вместе с [гистограммой](../../relational-databases/statistics/statistics.md#histogram) и [вектором плотности](../../relational-databases/statistics/statistics.md#density), а не в метаданных. Если нет данных для создания статистических данных, большой двоичный объект статистики не создается и дата недоступна. Это происходит с отфильтрованной статистикой, для которой предикат не возвращает ни одной строки, или с новыми пустыми таблицами.
 
 Если статистика соответствует индексу, то значение *stats_id* в представлении каталога [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) совпадает со значением *index_id* в представлении каталога [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).
  
## <a name="permissions"></a>Разрешения  
 Необходимо членство в предопределенной роли базы данных db_owner или разрешение на просмотр метаданных для таблицы или индексированного представления.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-return-the-dates-of-the-most-recent-statistics-for-a-table"></a>A. Возвращение даты последнего обновления статистики для таблицы  
 В следующем примере возвращается дата последнего обновления для каждого объекта статистики по таблице `Person.Address`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_update_date  
FROM sys.stats   
WHERE object_id = OBJECT_ID('Person.Address');  
GO  
```  
  
 Если статистика соответствует индексу, то значение *stats_id* в представлении каталога [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) совпадает со значением *index_id* в представлении каталога [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) и следующий запрос возвращает те же результаты, что и предшествующий. Если статистика не соответствует индексу, то она будет содержаться в результатах sys.stats, но не в результатах sys.indexes.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name AS index_name,   
    STATS_DATE(object_id, index_id) AS statistics_update_date  
FROM sys.indexes   
WHERE object_id = OBJECT_ID('Person.Address');  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-learn-when-a-named-statistics-was-last-updated"></a>Б. Определение времени последнего обновления именованной статистики  
 В приведенном ниже примере создается статистика по столбцу LastName таблицы DimCustomer. Затем выполняется запрос для отображения даты статистики. После этого статистика обновляется, и запрос выполняется еще раз для отображения обновленной даты.  
  
```sql
--First, create a statistics object  
USE AdventureWorksPDW2012;  
GO  
CREATE STATISTICS Customer_LastName_Stats  
ON AdventureWorksPDW2012.dbo.DimCustomer (LastName)  
WITH SAMPLE 50 PERCENT;  
GO  
  
--Return the date when Customer_LastName_Stats was last updated  
USE AdventureWorksPDW2012;  
GO  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer')  
    AND s.name = 'Customer_LastName_Stats';  
GO  
  
--Update Customer_LastName_Stats so it will have a different timestamp in the next query  
GO  
UPDATE STATISTICS dbo.dimCustomer (Customer_LastName_Stats);  
  
--Return the date when Customer_LastName_Stats was last updated.  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer')  
    AND s.name = 'Customer_LastName_Stats';  
GO    
```  
  
### <a name="c-view-the-date-of-the-last-update-for-all-statistics-on-a-table"></a>В. Просмотр даты последнего обновления для всех объектов статистики по таблице  
 В этом примере возвращается дата последнего обновления для каждого объекта статистики в таблице DimCustomer.  
  
```sql  
--Return the dates all statistics on the table were last updated.  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer');  
GO  
```  
  
 Если статистика соответствует индексу, то значение *stats_id* в представлении каталога [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) совпадает со значением *index_id* в представлении каталога [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md) и следующий запрос возвращает те же результаты, что и предшествующий. Если статистика не соответствует индексу, то она будет содержаться в результатах sys.stats, но не в результатах sys.indexes.  
  
```sql  
USE AdventureWorksPDW2012;  
GO  
SELECT name AS index_name,   
    STATS_DATE(object_id, index_id) AS statistics_update_date  
FROM sys.indexes   
WHERE object_id = OBJECT_ID('dbo.DimCustomer');  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Системные функции (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [UPDATE STATISTICS (Transact-SQL)](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_autostats (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)   
 [Статистика](../../relational-databases/statistics/statistics.md)    
 [sys.dm_db_stats_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
  
  


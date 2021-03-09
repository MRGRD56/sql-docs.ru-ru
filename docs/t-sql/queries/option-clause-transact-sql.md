---
description: Предложение OPTION (Transact-SQL)
title: Предложение OPTION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPTION clause
- OPTION_TSQL
- OPTION
- OPTION_clause_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- clauses [SQL Server], OPTION
- OPTION clause
ms.assetid: f47e2f3f-9302-4711-9d66-16b1a2a7ffe3
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 559b9572d95347d4ef14dfe9664cbaf6fb39bbed
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186591"
---
# <a name="option-clause-transact-sql"></a>Предложение OPTION (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Указывает, что показанное указание запроса должно быть использовано во всем запросе. Каждое указание запроса может быть задано только один раз, однако разрешены множественные указания запроса. В выражении может быть использовано только одно предложение OPTION.  
  
 Это предложение может быть указано в инструкциях SELECT, DELETE, UPDATE и MERGE.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
### <a name="syntax-for-ssnoversion-mdmd-and-ssazure_mdmd"></a>Синтаксис для [!INCLUDE[ssnoversion-md.md](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssazure_md.md](../../includes/ssazure_md.md)].

```syntaxsql

[ OPTION ( <query_hint> [ ,...n ] ) ]   
```  
  
### <a name="syntax-for-sssdw-mdmd-and-sspdw-mdmd"></a>Синтаксис для [!INCLUDE[sssdw-md.md](../../includes/sssdw-md.md)] и [!INCLUDE[sspdw-md.md](../../includes/sspdw-md.md)]

```syntaxsql
  
OPTION ( <query_option> [ ,...n ] )  
  
<query_option> ::=  
    LABEL = label_name |  
    <query_hint>  
  
<query_hint> ::=  
    HASH JOIN   
    | LOOP JOIN   
    | MERGE JOIN  
    | FORCE ORDER  
    | { FORCE | DISABLE } EXTERNALPUSHDOWN  
```  
  
### <a name="syntax-for-sssodfull-mdmd"></a>Синтаксис для [!INCLUDE[sssodfull-md.md](../../includes/sssodfull-md.md)]

```syntaxsql

OPTION ( <query_option> [ ,...n ] )

<query_option> ::=
    LABEL = label_name
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *query_hint*  
 Ключевые слова, которые указывают, какие указания оптимизатора применяются при настройке способа обработки инструкции ядром СУБД. Дополнительные сведения см. в разделе [Указания запросов (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-an-option-clause-with-a-group-by-clause"></a>A. Использование предложения OPTION с предложением GROUP BY  
 В следующем примере демонстрируется совместное использование предложений `OPTION` и `GROUP BY`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (HASH GROUP, FAST 10);  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-select-statement-with-a-label-in-the-option-clause"></a>Б. Инструкция SELECT с меткой в предложении OPTION  
 В приведенном ниже примере показана простая инструкция [!INCLUDE[ssDW](../../includes/ssdw-md.md)] SELECT с меткой в предложении OPTION.  
  
```sql
-- Uses AdventureWorks  
  
SELECT * FROM FactResellerSales  
  OPTION ( LABEL = 'q17' );  
```  
  
### <a name="c-select-statement-with-a-query-hint-in-the-option-clause"></a>В. Инструкция SELECT с указанием запроса в предложении OPTION  
 В приведенном ниже примере показана инструкция SELECT, в которой используется указание запроса HASH JOIN в предложении OPTION.  
  
```sql
-- Uses AdventureWorks  
  
SELECT COUNT (*) FROM dbo.DimCustomer a  
INNER JOIN dbo.FactInternetSales b   
ON (a.CustomerKey = b.CustomerKey)  
OPTION (HASH JOIN);  
```  
  
### <a name="d-select-statement-with-a-label-and-multiple-query-hints-in-the-option-clause"></a>Г. Инструкция SELECT с меткой и несколькими указаниями запроса в предложении OPTION  
 В приведенном ниже примере инструкция [!INCLUDE[ssDW](../../includes/ssdw-md.md)] SELECT содержит метку и несколько указаний запроса. При выполнении запроса в вычислительных узлах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет применять хэш-соединение или соединение слиянием в зависимости от стратегии, которую [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] определит как оптимальную.  
  
```sql
-- Uses AdventureWorks  
  
SELECT COUNT (*) FROM dbo.DimCustomer a  
INNER JOIN dbo.FactInternetSales b   
ON (a.CustomerKey = b.CustomerKey)  
OPTION ( Label = 'CustJoin', HASH JOIN, MERGE JOIN);  
```  
  
### <a name="e-using-a-query-hint-when-querying-a-view"></a>Д. Использование указания запроса при запросе представления  
 В приведенном ниже примере создается представление с именем CustomerView, а затем указание запроса HASH JOIN используется в запросе, который ссылается на представление и таблицу.  
  
```sql
-- Uses AdventureWorks  
  
CREATE VIEW CustomerView  
AS  
SELECT CustomerKey, FirstName, LastName FROM ssawPDW..DimCustomer;  
  
SELECT COUNT (*) FROM dbo.CustomerView a  
INNER JOIN dbo.FactInternetSales b  
ON (a.CustomerKey = b.CustomerKey)  
OPTION (HASH JOIN);  
  
DROP VIEW CustomerView;
```  
  
### <a name="f-query-with-a-subselect-and-a-query-hint"></a>Е. Запрос с подвыборкой и указанием запроса  
 В приведенном ниже примере показан запрос, который содержит как подвыборку, так и указание запроса. Указание запроса применяется глобально. Указания запроса нельзя добавлять к инструкции подвыборки.  
  
```sql
-- Uses AdventureWorks  
  
CREATE VIEW CustomerView AS  
SELECT CustomerKey, FirstName, LastName FROM ssawPDW..DimCustomer;  
  
SELECT * FROM (  
SELECT COUNT (*) AS a FROM dbo.CustomerView a  
INNER JOIN dbo.FactInternetSales b  
ON ( a.CustomerKey = b.CustomerKey )) AS t  
OPTION (HASH JOIN);  
```  
  
### <a name="g-force-the-join-order-to-match-the-order-in-the-query"></a>Ж. Принудительное соответствие порядка соединения порядку в запросе  
 В приведенном ниже примере с помощью указания FORCE ORDER настраивается принудительное использование планом запроса порядка соединения, указанного в запросе. Это повышает производительность некоторых, но не всех запросов.  
  
```sql
-- Uses AdventureWorks  
  
-- Obtain partition numbers, boundary values, boundary value types, and rows per boundary  
-- for the partitions in the ProspectiveBuyer table of the ssawPDW database.  
SELECT sp.partition_number, prv.value AS boundary_value, lower(sty.name) AS boundary_value_type, sp.rows   
FROM sys.tables st JOIN sys.indexes si ON st.object_id = si.object_id AND si.index_id <2  
JOIN sys.partitions sp ON sp.object_id = st.object_id AND sp.index_id = si.index_id  
JOIN sys.partition_schemes ps ON ps.data_space_id = si.data_space_id   
JOIN sys.partition_range_values prv ON prv.function_id = ps.function_id   
JOIN sys.partition_parameters pp ON pp.function_id = ps.function_id   
JOIN sys.types sty ON sty.user_type_id = pp.user_type_id AND prv.boundary_id = sp.partition_number   
WHERE st.object_id = (SELECT object_id FROM sys.objects WHERE name = 'FactResellerSales')   
ORDER BY sp.partition_number  
OPTION ( FORCE ORDER )  
;  
```  
  
### <a name="h-using-externalpushdown"></a>З. Использование EXTERNALPUSHDOWN  
 В приведенном ниже примере предложение WHERE принудительно передается в задание MapReduce во внешней таблице Hadoop.  
  
```sql
SELECT ID FROM External_Table_AS A   
WHERE ID < 1000000  
OPTION (FORCE EXTERNALPUSHDOWN);  
```  
  
 В приведенном ниже примере запрещается передача предложения WHERE в задание MapReduce во внешней таблице Hadoop. Все строки будут возвращены В PDW, где применяется предложение WHERE.  
  
```sql
SELECT ID FROM External_Table_AS A   
WHERE ID < 10  
OPTION (DISABLE EXTERNALPUSHDOWN);  
```  
  
## <a name="see-also"></a>См. также:  
 [Указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)   
 [MERGE (Transact-SQL)](../../t-sql/statements/merge-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)  
  
  


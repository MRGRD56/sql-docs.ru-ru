---
description: LEN (Transact-SQL)
title: LEN (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/03/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LEN
- LEN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- LEN function
- characters [SQL Server], number of
- number of characters
ms.assetid: fa20fee4-884d-4301-891a-c03e901345ae
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 442ff6051c99e907412498efab3e84ea73e03cef
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749274"
---
# <a name="len-transact-sql"></a>LEN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Возвращает количество символов указанного строкового выражения, исключая конечные пробелы.  
  
> [!NOTE]  
> Получить число байтов, используемых для представления выражения, можно с помощью функции [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
LEN ( string_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *string_expression*  
 Оцениваемое строковое [выражение](../../t-sql/language-elements/expressions-transact-sql.md). Аргумент *string_expression* может быть константой, переменной или столбцом символьных или двоичных данных.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **bigint**, если *expression* имеет тип данных **varchar(max)**, **nvarchar(max)** или **varbinary(max)**; в противном случае **int**.  
  
 Если используются параметры сортировки SC, то возвращаемое целое значение рассматривает суррогатные пары Юникода UTF-16 как один символ. Дополнительные сведения см. в статье [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="remarks"></a>Комментарии  
Функция LEN исключает конечные пробелы. Если это может создать проблемы, рекомендуется использовать функцию [DATALENGTH (Transact-SQL)](../../t-sql/functions/datalength-transact-sql.md), которая не усекает строку. При обработке строки Юникода DATALENGTH возвращает число, которое, возможно, не будет равно количеству символов. В приведенном ниже примере демонстрируется работа функций LEN и DATALENGTH с конечным пробелом.  
  
```sql  
  DECLARE @v1 VARCHAR(40),  
    @v2 NVARCHAR(40);  
SELECT   
@v1 = 'Test of 22 characters ',   
@v2 = 'Test of 22 characters ';  
SELECT LEN(@v1) AS [VARCHAR LEN] , DATALENGTH(@v1) AS [VARCHAR DATALENGTH];  
SELECT LEN(@v2) AS [NVARCHAR LEN], DATALENGTH(@v2) AS [NVARCHAR DATALENGTH];  
```  

> [!NOTE]
> Функция [LEN](../../t-sql/functions/len-transact-sql.md) возвращает количество символов, закодированных в определенное строковое выражение, а функция [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) — размер данных в байтах для определенного строкового выражения. Эти выходные данные могут быть разными в зависимости от типа данных и типа кодировки, используемой в столбце. Дополнительные сведения об отличиях типов кодировок, используемых для хранения данных, см. в статье [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md) (Поддержка параметров сортировки и Юникода).

## <a name="examples"></a>Примеры  
 Следующий пример выбирает число символов и данные по имени людей `FirstName`, живущих в `Australia`. В примере используется база данных AdventureWorks.  
  
```sql  
SELECT LEN(FirstName) AS Length, FirstName, LastName   
FROM Sales.vIndividualCustomer  
WHERE CountryRegionName = 'Australia';  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 В приведенном ниже примере возвращается число символов в столбце `FirstName`, а также первое и последнее имена сотрудников в `Australia`.  
  
```sql  
USE AdventureWorks2016  
GO  
SELECT DISTINCT LEN(FirstName) AS FNameLength, FirstName, LastName   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.DimGeography AS g   
    ON e.SalesTerritoryKey = g.SalesTerritoryKey   
WHERE EnglishCountryRegionName = 'Australia';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
FNameLength  FirstName  LastName  
-----------  ---------  ---------------  
4            Lynn       Tsoflias
```  
  
## <a name="see-also"></a>См. также  
 [DATALENGTH (Transact-SQL)](../../t-sql/functions/datalength-transact-sql.md)   
 [CHARINDEX (Transact-SQL)](../../t-sql/functions/charindex-transact-sql.md)  
 [PATINDEX (Transact-SQL)](../../t-sql/functions/patindex-transact-sql.md)  
 [LEFT (Transact-SQL)](../../t-sql/functions/left-transact-sql.md)   
 [RIGHT (Transact-SQL)](../../t-sql/functions/right-transact-sql.md)  
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Строковые функции (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)   
  
  

---
description: Логические функции — GREATEST (Transact-SQL)
title: GREATEST (Transact-SQL)
ms.custom: ''
ms.date: 04/14/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GREATEST
- GREATEST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GREATEST function
author: jmsteen
ms.author: josteen
ms.reviewer: wiassaf
ms.openlocfilehash: 8913fc9447049fb02b0ee6c04cee3f3715a37fb6
ms.sourcegitcommit: 233be9adaee3d19b946ce15cfcb2323e6e178170
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2021
ms.locfileid: "107560828"
---
# <a name="logical-functions---greatest-transact-sql"></a>Логические функции — GREATEST (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-svrless-poolonly](../../includes/applies-to-version/sql-asdb-asdbmi-asa-svrless-poolonly.md)]


 Эта функция возвращает максимальное значение из списка, содержащего одно или несколько выражений. 
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
GREATEST ( expression1 [ ,...expressionN ] ) 
```  

## <a name="arguments"></a>Аргументы
 *expression1, expressionN*  
 Список выражений любого сопоставимого типа данных, разделенный запятыми. Функция `GREATEST` требует по крайней мере один аргумент. Максимальное поддерживаемое число аргументов — 254.  
 
 Каждое выражение может быть константой, переменной, именем столбца или функцией, а также любым сочетанием арифметических, битовых и строковых операторов. Допускаются агрегатные функции и скалярные вложенные запросы.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Возвращает тип данных с наивысшим приоритетом из переданного функции набора типов. Дополнительные сведения см. в разделе [Приоритет типов данных (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md).  

 Если все аргументы имеют одинаковый тип данных и он поддерживается для сравнения, то  `GREATEST` возвращает значение этого типа. 

 В противном случае функция перед сравнением неявно приводит все аргументы к типу данных с наивысшим приоритетом и использует его как тип возвращаемого значения. 

 Для числовых типов масштаб типа возвращаемого значения будет соответствовать масштабу аргумента, имеющего тип данных с наивысшим приоритетом, либо наибольшему масштабу, если таких аргументов несколько.
  
## <a name="remarks"></a>Комментарии  
 Все выражения в списке аргументов должны иметь сопоставимый тип данных, который может быть неявно преобразован в тип аргумента с наивысшим приоритетом. 

 Перед сравнением выполняется неявное приведение всех аргументов к типу данных с наивысшим приоритетом. 

 Если неявное преобразование типа между аргументами не поддерживается, функция завершится сбоем и возвратит ошибку. 

 Дополнительные сведения о явном и неявном преобразовании см. в статье [Преобразование типов данных &#40;ядро СУБД&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md). 

 При наличии аргументов, не равных `NULL`, аргументы `NULL` при сравнении пропускаются. Если все аргументы имеют значение `NULL`, то `GREATEST` возвратит значение `NULL`. 

 Сравнение символьных аргументов осуществляется в соответствии с правилами, приведенными в разделе [Очередность параметров сортировки &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md). 

 Сравнение **не поддерживается** в `GREATEST` для следующих типов: **varchar(max), varbinary(max) или nvarchar(max) с размером, превышающим 8000 байт, а также cursor, geometry, geography, image, пользовательских типов без побайтного упорядочивания, ntext, table, text** и **xml**. 

 Типы данных varchar(max), varbinary(max) и nvarchar(max) поддерживаются для аргументов размером до 8000 байт и перед сравнением будут неявно преобразованы в типы varchar(n), varbinary(n) и nvarchar(n) соответственно. 

 Например, тип varchar(max) поддерживает до 8000 символов, если используется однобайтовая кодировка, а тип nvarchar(max) — до 4000 байтовых пар, если используется кодировка UTF-16. 
  
## <a name="examples"></a>Примеры  

### <a name="a-simple-example"></a>A. Простой пример

 В следующем примере возвращается максимальное значение из указанного списка констант. 

 Масштаб типа возвращаемого значения определяется масштабом аргумента с типом данных, имеющим наивысший приоритет. 
 
```sql 
SELECT GREATEST ( '6.62', 3.1415, N'7' ) AS GreatestVal; 
GO 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
GreatestVal 
-------- 
  7.0000 

(1 rows affected)  
```  

### <a name="b-simple-example-with-character-types"></a>Б. Простой пример с символьными типами

 В следующем примере возвращается максимальное значение из указанного списка символьных констант.  
  
```sql  
SELECT GREATEST ('Glacier', N'Joshua Tree', 'Mount Rainier') AS GreatestString;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
GreatestString 
------------- 
Mount Rainier 

(1 rows affected)  
```  

### <a name="c-simple-example-with-table"></a>В. Простой пример с таблицей
  
 Этот пример возвращает максимальное значение из списка аргументов столбца и при сравнении игнорирует значения `NULL`. 
  
```sql  
USE AdventureWorks2019; 
GO 

SELECT sp.SalesQuota, sp.SalesYTD, sp.SalesLastYear 
      , GREATEST(sp.SalesQuota, sp.SalesYTD, sp.SalesLastYear) AS Sales 
FROM Sales.SalesPerson AS sp 
WHERE sp.SalesYTD < 3000000; 
GO  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
SalesQuota            SalesYTD              SalesLastYear         Sales 

--------------------- --------------------- --------------------- --------------------- 
                 NULL           559697.5639                 .0000           559697.5639 
          250000.0000          1453719.4653          1620276.8966          1620276.8966 
          300000.0000          2315185.6110          1849640.9418          2315185.6110 
          250000.0000          1352577.1325          1927059.1780          1927059.1780 
          250000.0000          2458535.6169          2073505.9999          2458535.6169 
          250000.0000          2604540.7172          2038234.6549          2604540.7172 
          250000.0000          1573012.9383          1371635.3158          1573012.9383 
          300000.0000          1576562.1966                 .0000          1576562.1966 
                 NULL           172524.4512                 .0000           172524.4512 
          250000.0000          1421810.9242          2278548.9776          2278548.9776 
                 NULL           519905.9320                 .0000           519905.9320 
          250000.0000          1827066.7118          1307949.7917          1827066.7118 

(12 rows affected)
  
```  
### <a name="d-using-greatest-with-local-variables"></a>Г. Использование `GREATEST` с локальными переменными

 В этом примере с помощью `GREATEST` определяется максимальное значение из списка локальных переменных в предикате предложения `WHERE`. 
  
```sql  
CREATE TABLE dbo.studies (    
    VarX varchar(10) NOT NULL,    
    Correlation decimal(4, 3) NULL 
); 

INSERT INTO dbo.studies VALUES ('Var1', 0.2), ('Var2', 0.825), ('Var3', 0.61); 
GO 

DECLARE @PredictionA DECIMAL(2,1) = 0.7;  
DECLARE @PredictionB DECIMAL(3,1) = 0.65;  

SELECT VarX, Correlation  
FROM dbo.studies 
WHERE Correlation > GREATEST(@PredictionA, @PredictionB); 
GO 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
VarX   Correlation 
---------- ----------- 
Var2              .825 

(1 rows affected)  
```  

### <a name="e-using-greatest-with-columns-constants-and-variables"></a>Д. Использование `GREATEST` со столбцами, константами и переменными

 В этом примере с помощью `GREATEST` определяется максимальное значение из списка, содержащего столбцы, константы и переменные. 
  
```sql  
CREATE TABLE dbo.products (    
    prod_id int IDENTITY(1,1),    
    listprice smallmoney NULL 
); 

INSERT INTO dbo.products VALUES (14.99), (49.99), (24.99); 
GO 

DECLARE @PriceX smallmoney = 19.99;  

SELECT GREATEST(listprice, 0, @PriceX) as GreatestPrice  
FROM dbo.products;
GO 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
GreatestPrice
-------------
      19.9900
      49.9900
      24.9900

(3 rows affected)  
```  

  
## <a name="see-also"></a>См. также раздел  
 [LEAST &#40;Transact-SQL&#41;](../../t-sql/functions/logical-functions-least-transact-sql.md)  
 [MAX (Transact-SQL)](../../t-sql/functions/max-transact-sql.md)  
 [MIN &#40;Transact-SQL&#41;](../../t-sql/functions/min-transact-sql.md)  
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)  
 [CHOOSE (Transact-SQL)](../../t-sql/functions/logical-functions-choose-transact-sql.md)  
  
  

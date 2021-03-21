---
description: Скалярные функции ODBC (Transact-SQL)
title: Скалярные функции ODBC (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CURDATE ODBC function
- DAYOFMONTH ODBC function
- WEEK ODBC function
- functions, ODBC CONCAT
- SECOND ODBC function
- functions, ODBC CURRENT_TIME
- functions, ODBC HOUR
- functions, ODBC QUARTER
- TRUNCATE ODBC function
- functions, ODBC SECOND
- DAYOFWEEK ODBC function
- CURRENT_DATE ODBC function
- DAYNAME ODBC function
- CURTIME ODBC function
- functions, ODBC CURRENT_DATE
- MINUTE ODBC function
- functions, ODBC BIT_LENGTH
- functions, ODBC OCTET_LENGTH
- CONCAT ODBC function
- MONTHNAME ODBC function
- functions, ODBC DAYOFYEAR
- OCTET_LENGTH ODBC function
- functions [SQL Server], ODBC scalar functions
- BIT_LENGTH ODBC function
- functions, ODBC DAYOFMONTH
- CURRENT_TIME ODBC function
- functions, ODBC CURDATE
- functions, ODBC CURTIME
- functions, ODBC TRUNCATE
- functions, ODBC MONTHNAME
- functions, ODBC DAYNAME
- ODBC scalar functions
- functions, ODBC MINUTE
- functions, ODBC DAYOFWEEK
- QUARTER ODBC function
- DAYOFYEAR ODBC function
- functions, ODBC WEEK
- HOUR ODBC function
ms.assetid: a0df1ac2-6699-4ac0-8f79-f362f23496f1
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0f40b647d524aedb70c80bc54a22da0ffee97e21
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754384"
---
# <a name="odbc-scalar-functions-transact-sql"></a>Скалярные функции ODBC (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)] можно использовать [скалярные функции ODBC](../../odbc/reference/appendixes/appendix-e-scalar-functions.md). Эти инструкции интерпретируются средой [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Они могут использоваться в хранимых процедурах и других определяемых пользователем функциях. Они включают строковые, числовые и системные функции, а также функции даты, времени и интервалов.  
  
## <a name="usage"></a>Использование  
 ```syntaxsql
 SELECT {fn <function_name> [ (<argument>,....n) ] }
 ```
  
## <a name="functions"></a>Функции  
 В следующих таблицах приводится список скалярных функций ODBC, не дублируемых в [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
### <a name="string-functions"></a>Строковые функции  
  
|Функция|Описание|  
|--------------|-----------------|  
|BIT_LENGTH( строковое_выражение ) (ODBC 3.0)|Возвращает длину строкового выражения в битах.<br /><br /> Возвращает внутренний размер заданного типа данных без преобразования "строкового_выражения" в строку.|  
|CONCAT( строковое_выражение1,строковое_выражение2) (ODBC 1.0)|Возвращает символьную строку, являющуюся результатом сцепления строк строковое_выражение2 и строковое_выражение1. Полученная в результате строка зависит от СУБД. Например, если столбец, представленный строкой строковое_выражение1, объединяется со значением NULL, DB2 возвратит NULL, а [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвратит строку со значением, отличным от NULL.|  
|OCTET_LENGTH( строковое_выражение ) (ODBC 3.0)|Возвращает длину строкового выражения в байтах. Результатом является наименьшее целочисленное значение, не меньшее, чем число битов, разделенное на 8.<br /><br /> Возвращает внутренний размер заданного типа данных без преобразования "строкового_выражения" в строку.|  
  
### <a name="numeric-function"></a>Числовые функции  
  
|Функция|Описание|  
|--------------|-----------------|  
|TRUNCATE( числовое_выражение, целое_выражение) (ODBC 2.0)|Возвращает выражение «числовое_выражение», усеченное до целого числа позиций «целое_выражение» справа от десятичной запятой. Если значение "целое_выражение" отрицательное, выражение "числовое_выражение" усекается до &#124;целое_выражение&#124; позиций слева от десятичной запятой.|  
  
### <a name="time-date-and-interval-functions"></a>Функции даты, времени и интервалов  
  
|Функция|Описание|  
|--------------|-----------------|  
|CURRENT_DATE( ) (ODBC 3.0)|Возвращает текущую дату.|  
|CURDATE( ) (ODBC 3.0)|Возвращает текущую дату.|  
|CURRENT_TIME`[( time-precision )]` (ODBC 3.0)|Возвращает текущее время. Аргумент «точность_в_секундах» определяет точность времени возвращаемого значения в секундах|  
|CURTIME() (ODBC 3.0)|Возвращает текущее время.|  
|DAYNAME( выражение_даты ) (ODBC 2.0)|Возвращает символьную строку, содержащую зависящий от источника данных день недели для элемента дня в "выражении_даты". Например, от Sunday до Saturday или от Sun. до Sat., для источника данных, где используется английский язык. Или от Sonntag до Samstag для источника данных, где используется немецкий язык.|
|DAYOFMONTH( выражение_даты ) (ODBC 1.0)|Возвращает день месяца из поля месяца в "выражении_даты" в виде целочисленного значения. Значение находится в диапазоне 1–31.|  
|DAYOFWEEK( выражение_даты ) (ODBC 1.0)|Возвращает день недели из поля недели в "выражении_даты" в виде целочисленного значения. Возвращаемое значение будет в диапазоне 1–7, где 1 соответствует Sunday (воскресенью).|  
|HOUR( выражение_времени ) (ODBC 1.0)|Возвращает час из поля часа в "выражении_времени" в виде целочисленного значения в диапазоне 0–23.|  
|MINUTE( выражение_времени ) (ODBC 1.0)|Возвращает минуту из поля минуты в "выражении_времени" в виде целочисленного значения в диапазоне 0–59.|  
|SECOND( выражение_времени ) (ODBC 1.0)|Возвращает секунду из поля секунды в "выражении_времени" в виде целочисленного значения в диапазоне 0–59.|  
|MONTHNAME( выражение_даты ) (ODBC 2.0)|Возвращает символьную строку, содержащую зависящий от источника данных месяц для элемента месяца в "выражении_даты". Например, от January до December или от Jan. до Dec. для источника данных, где используется английский язык. Или от Januar до Dezember для источника данных, где используется немецкий язык.|  
|QUARTER( выражение_даты ) (ODBC 1.0)|Возвращает квартал в выражении "выражение_даты" в виде целочисленного значения в диапазоне 1–4, где 1 означает период с 1 января по 31 марта.|  
|WEEK( выражение_даты ) (ODBC 1.0)|Возвращает номер недели года из поля недели в "выражении_даты" в виде целочисленного значения в диапазоне 1–53.|  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-an-odbc-function-in-a-stored-procedure"></a>A. Использование функции ODBC в хранимой процедуре  
 В следующем примере функция ODBC используется в хранимой процедуре.  
  

```sql 
CREATE PROCEDURE dbo.ODBCprocedure  
(  
    @string_exp NVARCHAR(4000)  
)  
AS  
SELECT {fn OCTET_LENGTH( @string_exp )};  
```  
  
### <a name="b-using-an-odbc-function-in-a-user-defined-function"></a>Б. Использование функции ODBC в определяемой пользователем функции  
 В следующем примере функция ODBC используется в определяемой пользователем хранимой процедуре.  
  
```sql  
CREATE FUNCTION dbo.ODBCudf  
(  
    @string_exp NVARCHAR(4000)  
)  
RETURNS INT  
AS  
BEGIN  
DECLARE @len INT  
SET @len = (SELECT {fn OCTET_LENGTH( @string_exp )})  
RETURN(@len)  
END ;  
  
SELECT dbo.ODBCudf('Returns the length.');  
--Returns 38  
```  
  
### <a name="c-using-an-odbc-functions-in-select-statements"></a>В. Использование функций ODBC в инструкциях SELECT  
 В следующих инструкциях SELECT используются функции ODBC.  
  
```sql 
DECLARE @string_exp NVARCHAR(4000) = 'Returns the length.';  
SELECT {fn BIT_LENGTH( @string_exp )};  
-- Returns 304  
SELECT {fn OCTET_LENGTH( @string_exp )};  
-- Returns 38  
  
SELECT {fn CONCAT( 'CONCAT ','returns a character string')};  
-- Returns CONCAT returns a character string  
SELECT {fn TRUNCATE( 100.123456, 4)};  
-- Returns 100.123400  
SELECT {fn CURRENT_DATE( )};  
-- Returns 2007-04-20  
SELECT {fn CURRENT_TIME(6)};  
-- Returns 10:27:11.973000  
  
DECLARE @date_exp NVARCHAR(30) = '2007-04-21 01:01:01.1234567';  
SELECT {fn DAYNAME( @date_exp )};  
-- Returns Saturday  
SELECT {fn DAYOFMONTH( @date_exp )};  
-- Returns 21  
SELECT {fn DAYOFWEEK( @date_exp )};  
-- Returns 7  
SELECT {fn HOUR( @date_exp)};  
-- Returns 1   
SELECT {fn MINUTE( @date_exp )};  
-- Returns 1  
SELECT {fn SECOND( @date_exp )};  
-- Returns 1  
SELECT {fn MONTHNAME( @date_exp )};  
-- Returns April  
SELECT {fn QUARTER( @date_exp )};  
-- Returns 2  
SELECT {fn WEEK( @date_exp )};  
-- Returns 16  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-an-odbc-function-in-a-stored-procedure"></a>Г. Использование функции ODBC в хранимой процедуре  
 В следующем примере функция ODBC используется в хранимой процедуре.  
  
```sql  
CREATE PROCEDURE dbo.ODBCprocedure  
(  
    @string_exp NVARCHAR(4000)  
)  
AS  
SELECT {fn BIT_LENGTH( @string_exp )};  
```  
  
### <a name="e-using-an-odbc-function-in-a-user-defined-function"></a>Д. Использование функции ODBC в определяемой пользователем функции  
 В следующем примере функция ODBC используется в определяемой пользователем хранимой процедуре.  
  
```sql  
CREATE FUNCTION dbo.ODBCudf  
(  
    @string_exp NVARCHAR(4000)  
)  
RETURNS INT  
AS  
BEGIN  
DECLARE @len INT  
SET @len = (SELECT {fn BIT_LENGTH( @string_exp )})  
RETURN(@len)  
END ;  
  
SELECT dbo.ODBCudf('Returns the length in bits.');  
--Returns 432  
```  
  
### <a name="f-using-an-odbc-functions-in-select-statements"></a>Е. Использование функций ODBC в инструкциях SELECT  
 В следующих инструкциях SELECT используются функции ODBC.  
  
```sql  
DECLARE @string_exp NVARCHAR(4000) = 'Returns the length.';  
SELECT {fn BIT_LENGTH( @string_exp )};  
-- Returns 304  
  
SELECT {fn CONCAT( 'CONCAT ','returns a character string')};  
-- Returns CONCAT returns a character string  
SELECT {fn CURRENT_DATE( )};  
-- Returns today's date  
SELECT {fn CURRENT_TIME(6)};  
-- Returns the time  
  
DECLARE @date_exp NVARCHAR(30) = '2007-04-21 01:01:01.1234567';  
SELECT {fn DAYNAME( @date_exp )};  
-- Returns Saturday  
SELECT {fn DAYOFMONTH( @date_exp )};  
-- Returns 21  
SELECT {fn DAYOFWEEK( @date_exp )};  
-- Returns 7  
SELECT {fn HOUR( @date_exp)};  
-- Returns 1   
SELECT {fn MINUTE( @date_exp )};  
-- Returns 1  
SELECT {fn SECOND( @date_exp )};  
-- Returns 1  
SELECT {fn MONTHNAME( @date_exp )};  
-- Returns April  
SELECT {fn QUARTER( @date_exp )};  
-- Returns 2  
SELECT {fn WEEK( @date_exp )};  
-- Returns 16  
```  
  
## <a name="see-also"></a>См. также:  
 [Встроенные функции (Transact-SQL)](~/t-sql/functions/functions.md)
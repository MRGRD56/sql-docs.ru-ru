---
title: SUBSTRING (Transact-SQL) | Документы Майкрософт
description: Справочник по Transact-SQL для функции SUBSTRING. Эта функция возвращает часть указанного символьного, двоичного, текстового или графического выражения.
ms.date: 10/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SUBSTRING
- SUBSTRING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- portion of expression returned [SQL Server]
- part of expression returned [SQL Server]
- SUBSTRING function
- offsets [SQL Server]
- binary [SQL Server], returning part of
- expressions [SQL Server], part returned
- characters [SQL Server], returning part of
ms.assetid: a19c808f-aaf9-4a69-af59-b1a5fc3e5c4c
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6cfd3ffa82b9aaf333df0512f76860c726b1fd2f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753434"
---
# <a name="substring-transact-sql"></a>SUBSTRING (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Возвращает часть символьного, двоичного, текстового или графического выражения в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
SUBSTRING ( expression ,start , length )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *expression*  
 [Выражение](../../t-sql/language-elements/expressions-transact-sql.md) типа **character**, **binary**, **text**, **ntext** или **image**.  
  
 *start*  
 Целое число или выражение типа **bigint**, указывающее начальную позицию возвращаемых символов. (Нумерация начинается с 1, то есть первый символ в выражении имеет позицию 1.) Если аргумент *start* имеет значение меньше 1, то возвращаемое выражение начинается с первого символа, который указан в аргументе *expression*. В этом случае количество возвращаемых символов является наибольшим значением либо суммы *start* + *length*– 1, либо 0. Если значение *start* больше количества символов в выражении значения, возвращается выражение нулевой длины.  
  
 *length*  
 Положительное целое число или выражение типа **bigint**, указывающее количество символов выражения *expression*, которое будет возвращено. Если значение *length* отрицательно, возникает ошибка и выполнение инструкции прерывается. Если сумма *start* и *length* больше количества символов в *expression*, то возвращается целочисленное выражение значения, начинающееся со значения *start*.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Возвращает символьные данные, если *expression* имеет один из поддерживаемых символьных типов данных. Возвращает двоичные данные, если аргумент *expression* имеет один из поддерживаемых **двоичных** типов данных. Возвращенная строка имеет тот же самый тип, как и заданное выражение. Исключения указаны в таблице.  
  
|Заданное выражение|Возвращаемый тип|  
|--------------------------|-----------------|  
|**char**/**varchar**/**text**|**varchar**|  
|**nchar**/**nvarchar**/**ntext**|**nvarchar**|  
|**binary**/**varbinary**/**image**|**varbinary**|  
  
## <a name="remarks"></a>Remarks  
 Значения *start* и *length* должны быть указаны в виде количества символов для типов данных **ntext**, **char** или **varchar** и байтов для типов данных **text**, **image**, **binary** или **varbinary**.  
  
 Аргумент *expression* должен иметь тип **varchar(max)** или **varbinary(max)** , если аргумент *start* или *length* содержит значение, превышающее 2 147 483 647.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Дополнительные символы (суррогатные пары)  
 При использовании параметров сортировки дополнительных символов (SC) и *start*, и *length* обрабатывают каждую суррогатную пару в *expression* как один символ. Дополнительные сведения см. в статье [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-substring-with-a-character-string"></a>A. Использование SUBSTRING с символьной строкой  
 Следующий пример показывает, как получить часть символьной строки. Из таблицы `sys.databases` этот запрос возвращает имена системных баз данных в первом столбце, первую букву имени базы данных во втором столбце и третий и четвертый символы в последнем столбце.  
  
```sql
SELECT name, SUBSTRING(name, 1, 1) AS Initial ,
SUBSTRING(name, 3, 2) AS ThirdAndFourthCharacters
FROM sys.databases  
WHERE database_id < 5;   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

|name |Initial |ThirdAndFourthCharacters|
|---|--|--|
|master    |m    |st |
|tempdb    |t    |mp |
|model    |m    |de |
|msdb    |m    |db |


  
 Далее показано, как можно вывести второй, третий и четвертый символ строковой константы `abcdef`.  
  
```sql
SELECT x = SUBSTRING('abcdef', 2, 3);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
x  
----------  
bcd  
  
(1 row(s) affected)
```  
  
### <a name="b-using-substring-with-text-ntext-and-image-data"></a>Б. Использование SUBSTRING с данными типа text, ntext или image  
  
> [!NOTE]  
>  Для выполнения приведенных ниже примеров необходимо установить базу данных **pubs**.  
  
 В приведенном ниже примере показано, как вернуть первые 10 символов из каждого столбца данных **text** и **image** в таблице `pub_info` базы данных `pubs`. Данные **text** возвращаются как **varchar**, а данные **image** — как **varbinary**.  
  
```sql
USE pubs;  
SELECT pub_id, SUBSTRING(logo, 1, 10) AS logo,   
   SUBSTRING(pr_info, 1, 10) AS pr_info  
FROM pub_info  
WHERE pub_id = '1756';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 pub_id logo    pr_info
------ ---------------------- ----------
1756   0x474946383961E3002500 This is sa

(1 row(s) affected)
```  
  
 В приведенном ниже примере показано влияние функции SUBSTRING на данные типов **text** и **ntext**. Во-первых, пример создает новую таблицу в базе данных `pubs` под именем `npub_info`. Во-вторых, пример создает столбец `pr_info` в таблице `npub_info` из первых 80 символов столбца `pub_info.pr_info` и добавляет `ü` в качестве первого символа. Наконец, с помощью предложения `INNER JOIN` извлекаются все идентификационные номера издателей, а также обработанные функцией `SUBSTRING` значения столбцов типа **text** и **ntext** со сведениями об издателях.  
  
```sql
IF EXISTS (SELECT table_name FROM INFORMATION_SCHEMA.TABLES   
      WHERE table_name = 'npub_info')  
   DROP TABLE npub_info;  
GO  
-- Create npub_info table in pubs database. Borrowed from instpubs.sql.  
USE pubs;  
GO  
CREATE TABLE npub_info  
(  
 pub_id CHAR(4) NOT NULL  
    REFERENCES publishers(pub_id)  
    CONSTRAINT UPKCL_npubinfo PRIMARY KEY CLUSTERED,  
pr_info ntext NULL  
);  
  
GO  
  
-- Fill the pr_info column in npub_info with international data.  
RAISERROR('Now at the inserts to pub_info...',0,1);  
  
GO  
  
INSERT npub_info VALUES('0736', N'üThis is sample text data for New Moon Books, publisher 0736 in the pubs database')  
,('0877', N'üThis is sample text data for Binnet & Hardley, publisher 0877 in the pubs databa')  
,('1389', N'üThis is sample text data for Algodata Infosystems, publisher 1389 in the pubs da')  
,('9952', N'üThis is sample text data for Scootney Books, publisher 9952 in the pubs database')  
,('1622', N'üThis is sample text data for Five Lakes Publishing, publisher 1622 in the pubs d')  
,('1756', N'üThis is sample text data for Ramona Publishers, publisher 1756 in the pubs datab')  
,('9901', N'üThis is sample text data for GGG&G, publisher 9901 in the pubs database. GGG&G i')  
,('9999', N'üThis is sample text data for Lucerne Publishing, publisher 9999 in the pubs data');  
GO  
-- Join between npub_info and pub_info on pub_id.  
SELECT pr.pub_id, SUBSTRING(pr.pr_info, 1, 35) AS pr_info,  
   SUBSTRING(npr.pr_info, 1, 35) AS npr_info  
FROM pub_info pr INNER JOIN npub_info npr  
   ON pr.pub_id = npr.pub_id  
ORDER BY pr.pub_id ASC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-substring-with-a-character-string"></a>В. Использование SUBSTRING с символьной строкой  
 Следующий пример показывает, как получить часть символьной строки. Из таблицы `dbo.DimEmployee` данный запрос возвращает фамилию в одном столбце и первую букву имени в другом.  
  
```sql
-- Uses AdventureWorks  
  
SELECT LastName, SUBSTRING(FirstName, 1, 1) AS Initial  
FROM dbo.DimEmployee  
WHERE LastName LIKE 'Bar%'  
ORDER BY LastName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName             Initial
-------------------- -------
Barbariol            A
Barber               D
Barreto de Mattos    P
```  
  
 В приведенном ниже примере показано, как получить второй, третий и четвертый символы строковой константы `abcdef`.  
  
```sql
USE ssawPDW;  
  
SELECT TOP 1 SUBSTRING('abcdef', 2, 3) AS x FROM dbo.DimCustomer;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
x
-----
bcd
```  
  
## <a name="see-also"></a>См. также:  
 [LEFT (Transact-SQL)](../../t-sql/functions/left-transact-sql.md)  
 [LTRIM (Transact-SQL)](../../t-sql/functions/ltrim-transact-sql.md)  
 [RIGHT (Transact-SQL)](../../t-sql/functions/right-transact-sql.md)  
 [RTRIM (Transact-SQL)](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT (Transact-SQL)](../../t-sql/functions/string-split-transact-sql.md)  
 [TRIM (Transact-SQL)](../../t-sql/functions/trim-transact-sql.md)  
 [Строковые функции (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
  
  



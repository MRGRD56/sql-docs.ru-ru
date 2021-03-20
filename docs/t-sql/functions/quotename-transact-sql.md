---
description: QUOTENAME (Transact-SQL)
title: QUOTENAME (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- QUOTENAME_TSQL
- QUOTENAME
dev_langs:
- TSQL
helpviewer_keywords:
- delimited identifiers [SQL Server]
- input strings [SQL Server]
- Unicode [SQL Server], delimited identifiers
- QUOTENAME function
- valid identifiers [SQL Server]
ms.assetid: 34d47f1e-2ac7-4890-8c9c-5f60f115e076
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b574a702912d624c1c41316b7d74ff3b744b138e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753834"
---
# <a name="quotename-transact-sql"></a>QUOTENAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает Юникод-строку с разделителями, образуя из строки ввода правильный идентификатор с разделителем [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
QUOTENAME ( 'character_string' [ , 'quote_character' ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 '*character_string*'  
 Строка символьных данных в Юникоде. Аргумент *character_string* имеет тип **sysname**, а его длина ограничена 128 символами. Если ввести более 128 символов, будет возвращено значение NULL.  
  
 '*quote_character*'  
 Односимвольная строка, используемая в качестве разделителя. Может быть одинарной кавычкой ( **'** ), левой или правой квадратной скобкой ( **[]** ), двойной кавычкой ( **"** ), левой или правой круглой скобкой ( **()** ), знаком больше или меньше ( **><** ), левой или правой фигурной скобкой ( **{}** ) либо обратным апострофом ( **\`** ). Если указан недопустимый символ, возвращается значение NULL. Если значение аргумента *quote_character* не задано, то используются скобки.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **nvarchar(258)**  
  
## <a name="examples"></a>Примеры  
 В следующем примере из строки `abc[]def` и символов `[` и `]` создается правильный идентификатор с разделителем [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql
SELECT QUOTENAME('abc[]def');
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
[abc[]]def]
  
(1 row(s) affected)  
```  
  
 Обратите внимание, что закрывающая квадратная скобка в строке `abc[]def` удвоена, чтобы указать на escape-символ.  
 
 В следующем примере строка, заключенная в кавычки, подготавливается к использованию при именовании столбца.  
  
```sql
DECLARE @columnName NVARCHAR(255)='user''s "custom" name'
DECLARE @sql NVARCHAR(MAX) = 'SELECT FirstName AS ' + QUOTENAME(@columnName) + ' FROM dbo.DimCustomer'

EXEC sp_executesql @sql
```
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 В следующем примере из строки `abc def` и символов `[` и `]` создается правильный идентификатор с разделителем [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql
SELECT QUOTENAME('abc def');   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
[abc def]  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также:  
 [PARSENAME (Transact-SQL)](../../t-sql/functions/parsename-transact-sql.md)  
 [CONCAT (Transact-SQL)](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS (Transact-SQL)](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE (Transact-SQL)](../../t-sql/functions/formatmessage-transact-sql.md)  
 [REPLACE (Transact-SQL)](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE (Transact-SQL)](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG (Transact-SQL)](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE (Transact-SQL)](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF (Transact-SQL)](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE (Transact-SQL)](../../t-sql/functions/translate-transact-sql.md)  
 [Строковые функции (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  

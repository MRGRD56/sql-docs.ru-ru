---
description: NCHAR (Transact-SQL)
title: NCHAR (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- nchar
- nchar_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- NCHAR function
- Unicode [SQL Server], NCHAR function
ms.assetid: 68cefc68-7c4f-4326-80c1-300f90cf19db
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b9e6edb70a872e69e33c01553ca86d32b43ad34f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756414"
---
# <a name="nchar-transact-sql"></a>NCHAR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает символ Юникода с указанным целочисленным кодом, определенным в стандарте Юникода.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
NCHAR ( integer_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *integer_expression*  
 Если параметры сортировки базы данных не содержат флаг [дополнительных символов](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters), используется положительное целое число от 0 до 65 535 (от 0 до 0xFFFF). При указании значения вне этого диапазона возвращается значение NULL. Дополнительные сведения о дополнительных символах см. в статье [Поддержка параметров сортировки и Юникода](../../relational-databases/collations/collation-and-unicode-support.md).  
  
 Если параметры сортировки базы данных поддерживают флаг дополнительных символов, используется положительное целое число от 0 до 1 114 111 (от 0 до 0x10FFFF). При указании значения вне этого диапазона возвращается значение NULL.  
  
## <a name="return-types"></a>Типы возвращаемых значений  
 **nchar(1)**, когда параметры сортировки базы данных по умолчанию не поддерживают дополнительные символы.  
  
 **nvarchar(2)**, когда параметры сортировки базы данных по умолчанию поддерживают дополнительные символы.  
  
 Если параметр *integer_expression* лежит в диапазоне 0–0xFFFF, то возвращается только один символ. Для больших значений NCHAR возвращает соответствующую суррогатную пару. Не создавайте суррогатные пары с применением `NCHAR(<High surrogate>) + NCHAR(\<Low Surrogate>)`. Вместо этого используйте параметры сортировки базы данных, которые поддерживают дополнительные символы, с указанием кодовой точки в Юникоде для суррогатной пары. В следующем примере рассматривается как старый метод конструирования суррогатной пары, так и предпочтительный метод с указанием кодовой точки в Юникоде.  
  
```sql  
CREATE DATABASE test COLLATE Finnish_Swedish_100_CS_AS_SC;  
DECLARE @d NVARCHAR(10) = N'𣅿';
-- Old style method.  
SELECT NCHAR(0xD84C) + NCHAR(0xDD7F);   
  
-- Preferred method.   
SELECT NCHAR(143743);   
  
-- Alternative preferred method.  
SELECT NCHAR(UNICODE(@d));    
```  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-nchar-and-unicode"></a>A. Использование функций NCHAR и UNICODE  
 В следующем примере функции `UNICODE` и `NCHAR` используются для вывода значения `UNICODE` и `NCHAR` (символ Юникода) второго символа строки `København` и вывода второго фактического символа `ø`.  
  
```sql  
DECLARE @nstring NCHAR(8);  
SET @nstring = N'København';  
SELECT UNICODE(SUBSTRING(@nstring, 2, 1)),   
   NCHAR(UNICODE(SUBSTRING(@nstring, 2, 1)));  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------- -   
248         ø  
(1 row(s) affected)  
```  
  
### <a name="b-using-substring-unicode-convert-and-nchar"></a>Б. Использование функций SUBSTRING, UNICODE, CONVERT и NCHAR  
 В следующем пример функции `SUBSTRING`, `UNICODE`, `CONVERT` и `NCHAR` используются для печати номера символа, символа Юникода и значения Юникода каждого символа в строке `København`.  
  
```sql  
-- The @position variable holds the position of the character currently  
-- being processed. The @nstring variable is the Unicode character   
-- string to process.  
DECLARE @position INT, @nstring NCHAR(9);  
-- Initialize the current position variable to the first character in   
-- the string.  
SET @position = 1;  
-- Initialize the character string variable to the string to process.  
-- Notice that there is an N before the start of the string. This   
-- indicates that the data following the N is Unicode data.  
SET @nstring = N'København';  
-- Print the character number of the position of the string you are at,   
-- the actual Unicode character you are processing, and the UNICODE   
-- value for this particular character.  
PRINT 'Character #' + ' ' + 'Unicode Character' + ' ' + 'UNICODE Value';  
WHILE @position <= DATALENGTH(@nstring)  
   BEGIN  
   SELECT @position,   
      NCHAR(UNICODE(SUBSTRING(@nstring, @position, 1))),  
      CONVERT(NCHAR(17), SUBSTRING(@nstring, @position, 1)),  
      UNICODE(SUBSTRING(@nstring, @position, 1))  
   SELECT @position = @position + 1  
   END;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Character # Unicode Character UNICODE Value  
  
----------- ---- ----------------- -----------   
1           K    K                 75  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
2           ø    ø                 248  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
3           b    b                 98  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
4           e    e                 101  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
5           n    n                 110  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
6           h    h                 104  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
7           a    a                 97  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
8           v    v                 118  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
9           n    n                 110  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
10          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
11          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
12          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
13          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
14          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
15          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
16          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
17          NULL                   NULL  
  
(1 row(s) affected)  
  
----------- ---- ----------------- -----------   
18          NULL                   NULL  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также  
 [ASCII (Transact-SQL)](../../t-sql/functions/ascii-transact-sql.md)  
 [CHAR (Transact-SQL)](../../t-sql/functions/char-transact-sql.md)  
 [UNICODE (Transact-SQL)](../../t-sql/functions/unicode-transact-sql.md)  
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Строковые функции (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)   
  
  


---
description: PATINDEX (Transact-SQL)
title: PATINDEX (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PATINDEX
- PATINDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- first occurrence of pattern [SQL Server]
- searches [SQL Server], pattern starting position
- starting position of patten search
- pattern searching [SQL Server]
- PATINDEX function
ms.assetid: c0dfb17f-2230-4e36-98da-a9b630bab656
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 10610617de5844e637a264335cd96889aae53f03
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749264"
---
# <a name="patindex-transact-sql"></a>PATINDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Для любого допустимого символьного или текстового типа данных возвращает начальную позицию первого вхождения шаблона в указанном выражении или нули, если шаблон не найден.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
PATINDEX ( '%pattern%' , expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *pattern*  
 Символьное выражение, содержащее последовательность символов, которую надо найти. Можно использовать подстановочные знаки. При этом символ "%" должен указываться до и после аргумента *pattern* (за исключением случаев, когда производится поиск первых или последних символов). *pattern* представляет собой выражение из категории типа данных "символьная строка". Максимальная длина *pattern* — 8000 символов.

 > [!NOTE]
 > Хотя традиционные регулярные выражения изначально не поддерживаются в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], аналогичное по сложности сопоставление шаблонов можно реализовать с помощью различных подстановочных выражений. Дополнительные сведения о синтаксисе с подстановочными знаками см. в разделе документации [Строковые операторы](../../t-sql/language-elements/string-operators-transact-sql.md).
  
 *expression*  
 [Выражение](../../t-sql/language-elements/expressions-transact-sql.md), обычно столбец, в котором производится поиск по указанному шаблону. *expression* представляет собой выражение из категории типа данных "символьная строка".  
  
## <a name="return-types"></a>Типы возвращаемых данных  
**bigint**, если *expression* имеет тип данных **varchar(max)** или **nvarchar(max)**; в противном случае **int**.  
  
## <a name="remarks"></a>Комментарии  
Если аргумент *pattern* или *expression* имеет значение NULL, функция PATINDEX возвращает значение NULL.  
 
Начальная позиция PATINDEX — это 1.
 
Функция PATINDEX выполняет сравнение с учетом параметров сортировки входных значений. Для выполнения сравнения в указанных параметрах сортировки можно воспользоваться функцией COLLATE, чтобы явно указать параметры сортировки для входных данных.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Дополнительные символы (суррогатные пары)  
При использовании параметров сортировки SC возвращаемое значение рассматривает любые суррогатные пары UTF-16 в параметре *expression* как один символ. Дополнительные сведения см. в статье [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md).  
  
Символ 0x0000 (**char(0)**) не определен в параметрах сортировки Windows, и его нельзя включать в PATINDEX.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-simple-patindex-example"></a>A. Простой пример использования функции PATINDEX  
 В приведенном ниже примере в короткой строке символов (`interesting data`) проверяется начальная позиция символов `ter`.  
  
```sql  
SELECT position = PATINDEX('%ter%', 'interesting data');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
position
--------
3
```
  
### <a name="b-using-a-pattern-with-patindex"></a>Б. Использование шаблона в функции PATINDEX  
В следующем примере производится поиск позиции, с которой начинается шаблон `ensure` в указанной строке столбца `DocumentSummary` в таблице `Document` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT position = PATINDEX('%ensure%',DocumentSummary)  
FROM Production.Document  
WHERE DocumentNode = 0x7B40;  
GO   
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
position
--------  
64  
```  
  
Если не ограничить строки для поиска предложением `WHERE`, запрос возвращает все строки, содержащиеся в таблице, и выдает ненулевые значения для тех строк, в которых найден шаблон, либо нулевые для тех, где он не найден.  
  
### <a name="c-using-wildcard-characters-with-patindex"></a>В. Использование символов-шаблонов в функции PATINDEX  
 В следующих примерах символы-шаблоны % и _ используются для поиска позиции, где в указанной строке (индекс начинается с позиции 1) начинается шаблон `'en'`, за которым следует один любой символ и `'ure'`:  
  
```sql  
SELECT position = PATINDEX('%en_ure%', 'Please ensure the door is locked!');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
position
--------  
8  
```  
  
`PATINDEX` работает аналогично `LIKE`, то есть можно можно использовать любой из этих шаблонов. Нет необходимости заключать шаблон в символы процентов (%). `PATINDEX('a%', 'abc')` возвращает 1 и `PATINDEX('%a', 'cba')` возвращает 3.  
  
 В отличие от `LIKE`, `PATINDEX` возвращает позицию, аналогично `CHARINDEX`.  

### <a name="d-using-complex-wildcard-expressions-with-patindex"></a>Г. Использование сложных выражений с подстановочными знаками с PATINDEX 
В следующем примере оператор  [string](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)`[^]` используется для поиска позиции символа, который не является числом, буквой или пробелом.

```sql
SELECT position = PATINDEX('%[^ 0-9A-z]%', 'Please ensure the door is locked!'); 
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
position
--------
33
```

### <a name="e-using-collate-with-patindex"></a>Д. Использование предложения COLLATE в функции PATINDEX  
 Следующий пример показывает, как функция `COLLATE` явно определяет параметры сортировки при поиске в выражении.  
  
```sql  
USE tempdb;  
GO  
SELECT PATINDEX ( '%ein%', 'Das ist ein Test'  COLLATE Latin1_General_BIN) ;  
GO  
```  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
position
--------
9
```

### <a name="f-using-a-variable-to-specify-the-pattern"></a>Е. Использование переменной для указания шаблона  
В приведенном ниже примере значение передается в параметр *pattern* с помощью переменной. В этом примере используется база данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
DECLARE @MyValue VARCHAR(10) = 'safety';   
SELECT position = PATINDEX('%' + @MyValue + '%', DocumentSummary)   
FROM Production.Document  
WHERE DocumentNode = 0x7B40;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
position
--------  
22
```  
  
## <a name="see-also"></a>См. также  
 [LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)   
 [CHARINDEX (Transact-SQL)](../../t-sql/functions/charindex-transact-sql.md)  
 [LEN (Transact-SQL)](../../t-sql/functions/len-transact-sql.md)  
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Строковые функции (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)   
 [(символ-шаблон для сопоставления) (Transact-SQL)](../../t-sql/language-elements/wildcard-character-s-to-match-transact-sql.md)   
 [(символ-шаблон — совпадение не найдено) (Transact-SQL)](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)   
 [_ (символ-шаблон — совпадение одного символа) (Transact-SQL)](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)   
 [Символ процента (символ-шаблон для сопоставления) (Transact-SQL)](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)  
  
  



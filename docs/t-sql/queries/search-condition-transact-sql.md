---
description: Условие поиска (Transact-SQL)
title: Условие поиска (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 01/15/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- search
- Search Condition
- condition
dev_langs:
- TSQL
helpviewer_keywords:
- OR operator [Transact-SQL]
- CONTAINS predicate (Transact-SQL)
- ESCAPE keyword
- operators [Transact-SQL], search condition
- search conditions [SQL Server]
- WHERE clause, search conditions
- ALL keyword
- FREETEXT predicate (Transact-SQL)
- EXISTS keyword
- search conditions [SQL Server], about search conditions
- NOT operator [Transact-SQL]
- BETWEEN operator
- SOME | ANY keyword
- predicates [full-text search]
- AND, about search conditions
- logical operators [SQL Server], about search conditions
- precedence [SQL Server], logical operators
- logical operators [SQL Server], precedence
- LIKE comparisons
ms.assetid: 09974469-c5d2-4be8-bc5a-78e404660b2c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b19596b68a6ec8a4dac1eef04b14d186d60318ba
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754444"
---
# <a name="search-condition-transact-sql"></a>Условие поиска (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Сочетание одного или нескольких предикатов, в котором используются логические операторы AND, OR и NOT.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
<search_condition> ::=  
    MATCH(<graph_search_pattern>) | <search_condition_without_match> | <search_condition> AND <search_condition>

<search_condition_without_match> ::= 
    { [ NOT ] <predicate> | ( <search_condition_without_match> ) }   
    [ { AND | OR } [ NOT ] { <predicate> | ( <search_condition_without_match> ) } ]   
[ ...n ]   
  
<predicate> ::=   
    { expression { = | < > | ! = | > | > = | ! > | < | < = | ! < } expression   
    | string_expression [ NOT ] LIKE string_expression   
  [ ESCAPE 'escape_character' ]   
    | expression [ NOT ] BETWEEN expression AND expression   
    | expression IS [ NOT ] NULL   
    | CONTAINS   
  ( { column | * } , '<contains_search_condition>' )   
    | FREETEXT ( { column | * } , 'freetext_string' )   
    | expression [ NOT ] IN ( subquery | expression [ ,...n ] )   
    | expression { = | < > | ! = | > | > = | ! > | < | < = | ! < }   
  { ALL | SOME | ANY} ( subquery )   
    | EXISTS ( subquery )     }   
    
<graph_search_pattern> ::=
    { <node_alias> { 
                      { <-( <edge_alias> )- } 
                    | { -( <edge_alias> )-> }
                    <node_alias> 
                   } 
    }
  
<node_alias> ::=
    node_table_name | node_table_alias 

<edge_alias> ::=
    edge_table_name | edge_table_alias
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
< search_condition > ::=   
    { [ NOT ] <predicate> | ( <search_condition> ) }   
    [ { AND | OR } [ NOT ] { <predicate> | ( <search_condition> ) } ]   
[ ...n ]   
  
<predicate> ::=   
    { expression { = | < > | ! = | > | > = | < | < = } expression   
    | string_expression [ NOT ] LIKE string_expression   
    | expression [ NOT ] BETWEEN expression AND expression   
    | expression IS [ NOT ] NULL   
    | expression [ NOT ] IN (subquery | expression [ ,...n ] )   
    | expression [ NOT ] EXISTS (subquery)     }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 \<search_condition>  
 Задает условия для строк, возвращаемых в результирующем наборе инструкции SELECT, выражения запроса или вложенного запроса. Задает обновляемые строки для инструкции UPDATE. Задает удаляемые строки для инструкции DELETE. Количество предикатов, которое может содержаться в условии поиска для инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)], не ограничено.  
  
 \<graph_search_pattern>  
 Определяет шаблон сопоставления графов. Дополнительные сведения об аргументах этого предложения см. в статье [MATCH &#40;Transact-SQL&#41;](../../t-sql/queries/match-sql-graph.md).
 
 NOT  
 Инвертирует логическое выражение, задаваемое предикатом. Дополнительные сведения см. в разделе [NOT (Transact-SQL)](../../t-sql/language-elements/not-transact-sql.md).  
  
 AND  
 Объединяет два условия и выдает значение TRUE, если оба условия имеют значение TRUE. Дополнительные сведения см. в разделе [AND (Transact-SQL)](../../t-sql/language-elements/and-transact-sql.md).  
  
 OR  
 Объединяет два условия и выдает значение TRUE, если хотя бы одно условие имеет значение TRUE. Дополнительные сведения см. в разделе [OR (Transact-SQL)](../../t-sql/language-elements/or-transact-sql.md).  
  
 \< predicate >  
 Выражение, возвращающее значения TRUE, FALSE или UNKNOWN.  
  
 *expression*  
 Может являться именем столбца, константой, функцией, переменной, скалярным вложенным запросом или любым сочетанием имен столбцов, констант и функций, связанных операторами или вложенным запросом. Также может содержать выражение CASE.  
  
> [!NOTE]  
>  Строковые константы и переменные не в Юникоде используют кодовую страницу, соответствующую параметрам сортировки базы данных, действующим по умолчанию. Преобразования кодовых страниц могут происходить при работе только с символьными данными не в Юникоде и упоминании символьных типов данных **char**, **varchar** и **text** не в Юникоде. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] преобразует строковые константы и переменные не в Юникоде в кодовую страницу, соответствующую параметрам сортировки упоминаемого столбца или столбца, указанного с помощью предложения COLLATE, если эта кодовая страница отличается от кодовой страницы, соответствующей параметрам сортировки по умолчанию базы данных. Символы, не найденные на новой кодовой странице, преобразуются в похожий символ при обнаружении [наилучшего соответствия](https://www.unicode.org/Public/MAPPINGS/VENDORS/MICSFT/WindowsBestFit/) либо, в противном случае, преобразуются в символ замены по умолчанию "?".  
>  
> Чтобы при работе с несколькими кодовыми страницами избежать их преобразования, можно использовать переменные Юникода, а символьные константы могут иметь префикс в виде прописной буквы N.  
  
 =  
 Оператор, используемый для проверки равенства двух выражений.  
  
 <>  
 Оператор, используемый для проверки условий неравенства условий двух выражений.  
  
 !=  
 Оператор, используемый для проверки условий неравенства условий двух выражений.  
  
 \>  
 Оператор, используемый для проверки превышения одного выражения над условием другого.  
  
 \>=  
 Оператор, используемый для проверки превышения либо равенства двух выражений.  
  
 !>  
 Оператор, используемый для проверки того, что одно выражение не превышает другое выражение.  
  
 <  
 Оператор, используемый для проверки того, что одно выражение меньше другого.  
  
 <=  
 Оператор, используемый для проверки того, что одно выражение меньше или равно другому.  
  
 !<  
 Оператор, используемый для проверки того, что одно выражение не меньше другого.  
  
 *string_expression*  
 Строка обычных символов и символов-шаблонов.  
  
 [ NOT ] LIKE  
 Указывает, что последующая строка символов будет использоваться с сопоставлением шаблонов. Дополнительные сведения см. в разделе [LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md).  
  
 ESCAPE **'** _escape_ character_ *_'_*  
 Позволяет найти сам символ-шаблон в строке (вместо того чтобы использовать его как шаблон). *escape_character* — это символ, который нужно поместить перед символом-шаблоном, чтобы указать данное специальное использование.  
  
 [ NOT ] BETWEEN  
 Задает включающий диапазон значений. Используйте оператор AND для разделения начальных и конечных значений. Дополнительные сведения см. в статье [BETWEEN (Transact-SQL)](../../t-sql/language-elements/between-transact-sql.md).  
  
 IS [ NOT ] NULL  
 Задает поиск значений NULL или значений, не являющихся значениями NULL, в зависимости от используемых ключевых слов. При обращении одного из операндов выражения с битовыми или арифметическими операторами в значение NULL указанное выражение также обращается в значение NULL.  
  
 CONTAINS  
 Осуществляет поиск столбцов, содержащих символьные данные с заданной точностью (*fuzzy*), соответствующие заданным отдельным словам и фразам на основе похожести словам и точному расстоянию между словами, взвешенному совпадению. Этот параметр может быть использован только в инструкции SELECT. Дополнительные сведения см. в статье [CONTAINS (Transact-SQL)](../../t-sql/queries/contains-transact-sql.md).  
  
 FREETEXT  
 Предоставляет простую форму естественного языка ввода запросов на осуществление поиска столбцов, содержащих символьные данные, совпадающие с содержанием предиката не точно, а по смыслу. Этот параметр может быть использован только в инструкции SELECT. Дополнительные сведения см. в статье [FREETEXT (Transact-SQL)](../../t-sql/queries/freetext-transact-sql.md).  
  
 [ NOT ] IN  
 Задает поиск выражения, основанного на выражении, включенного или исключенного из списка. Выражение поиска может быть константой или именем столбца, а списком может быть набор констант или, что чаще, вложенный запрос. Список значений необходимо заключать в скобки. Дополнительные сведения см. в статье [IN (Transact-SQL)](../../t-sql/language-elements/in-transact-sql.md).  
  
 *subquery*  
 Может рассматриваться как ограниченная инструкция SELECT, так и являющаяся подобной \<query_expression> в инструкции SELECT. Использование предложения ORDER BY и ключевого слова INTO не допускается. Дополнительные сведения см. в статье [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md).  
  
 ALL  
 Используется с оператором сравнения и вложенным запросом. Возвращает для \<predicate> значение TRUE, если все получаемые для вложенного запроса значения удовлетворяют условию, и значение FALSE, если не все значения удовлетворяют условию или в случае, когда в результате выполнения вложенного запроса внешней инструкции не выдается ни одной строки. Дополнительные сведения см. в статье [ALL (Transact-SQL)](../../t-sql/language-elements/all-transact-sql.md).  
  
 { SOME | ANY }  
 Используется с оператором сравнения и вложенным запросом. Возвращает для \<predicate> значение TRUE, если хотя бы одно получаемое для вложенного запроса значение удовлетворяет условию, и значение FALSE, если ни одно из значений не удовлетворяет условию или в случае, когда в результате выполнения вложенного запроса внешней инструкции не выдается ни одной строки. В противном случае результатом выражения является значение UNKNOWN. Дополнительные сведения см. в статье [SOME &#124; ANY (Transact-SQL)](../../t-sql/language-elements/some-any-transact-sql.md).  
  
 EXISTS  
 Используется во вложенном запросе для проверки существования строк, возвращенных вложенным запросом. Дополнительные сведения см. в статье [EXISTS (Transact-SQL)](../../t-sql/language-elements/exists-transact-sql.md).  
  
## <a name="remarks"></a>Remarks  
 Приоритеты выполнения логических операторов распределяются следующим образом: NOT (наивысший приоритет), AND, OR (низший приоритет). Для перераспределения указанных приоритетов в условии поиска используются скобки. Порядок выполнения логических операторов может меняться в зависимости от настроек оптимизатора запросов. Дополнительные сведения о работе логических операторов с логическими значениями см. в статьях [AND (Transact-SQL)](../../t-sql/language-elements/and-transact-sql.md), [OR (Transact-SQL)](../../t-sql/language-elements/or-transact-sql.md) и [NOT (Transact-SQL)](../../t-sql/language-elements/not-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-where-with-like-and-escape-syntax"></a>A. Использование предложения WHERE с синтаксисом LIKE и ESCAPE  
 В ходе выполнения следующего примера производится поиск строк, для которых элементы столбца `LargePhotoFileName` содержат символы `green_`, а также для символа-шаблона _ используется параметр `ESCAPE`. Без указания параметра `ESCAPE` в ходе выполнения запроса будет производиться поиск всех описанных значений, содержащих слово `green`, за которым следует любой единичный символ, отличный от _.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
SELECT *   
FROM Production.ProductPhoto  
WHERE LargePhotoFileName LIKE '%greena_%' ESCAPE 'a' ;  
```  
  
### <a name="b-using-where-and-like-syntax-with-unicode-data"></a>Б. Использование предложения WHERE и синтаксиса LIKE с данными в Юникоде  
 В следующем примере используется предложение `WHERE` для отображения всех адресов офисов компаний, находящихся за пределами США (`US`) и в городах, названия которых начинаются с `Pa`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
SELECT AddressLine1, AddressLine2, City, PostalCode, CountryRegionCode    
FROM Person.Address AS a  
JOIN Person.StateProvince AS s ON a.StateProvinceID = s.StateProvinceID  
WHERE CountryRegionCode NOT IN ('US')  
AND City LIKE N'Pa%' ;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-where-with-like"></a>В. Использование предложения WHERE с LIKE  
 В следующем примере производится поиск строк, в которых столбец `LastName` содержат символы `and`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EmployeeKey, LastName  
FROM DimEmployee  
WHERE LastName LIKE '%and%';  
```  
  
### <a name="d-using-where-and-like-syntax-with-unicode-data"></a>Г. Использование предложения WHERE и синтаксиса LIKE с данными в Юникоде  
 В следующем примере используется предложение `WHERE` для выполнения поиска в формате Юникод в столбце `LastName`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EmployeeKey, LastName  
FROM DimEmployee  
WHERE LastName LIKE N'%and%';  
```  
  
## <a name="see-also"></a>См. также:  
 [Агрегатные функции (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [CONTAINSTABLE (Transact-SQL)](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [Курсоры (Transact-SQL)](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)   
 [Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)  
  
  


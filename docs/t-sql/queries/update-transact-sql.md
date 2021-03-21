---
description: UPDATE (Transact-SQL)
title: UPDATE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UPDATE_TSQL
- UPDATE
dev_langs:
- TSQL
helpviewer_keywords:
- DML [SQL Server], UPDATE statement
- data updates [SQL Server], UPDATE statement
- updating data [SQL Server]
- TOP clause, UPDATE
- OUTPUT clause
- large value data types
- data manipulation language [SQL Server], UPDATE statement
- user-defined types [SQL Server], modifying
- INSTEAD OF triggers
- modifying data [SQL Server], UPDATE statement
- data modifications [SQL Server], UPDATE statement
- large data, modifying
- .WRITE clause
- table modifications [SQL Server], UPDATE statement
- UDTs [SQL Server], modifying
- UPDATE statement [SQL Server], about UPDATE statement
- LOB data [SQL Server], modifying
- modifying LOB data
- UPDATE statement [SQL Server]
- FROM clause, UPDATE statement
- WHERE clause, UPDATE statement
ms.assetid: 40e63302-0c68-4593-af3e-6d190181fee7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 55d98ecc2024d08cd745828b47ebbd04e82b47fd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751514"
---
# <a name="update-transact-sql"></a>UPDATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Изменяет существующие данные в таблице или представлении в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Примеры см. в разделе [Примеры](#UpdateExamples).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database  

[ WITH <common_table_expression> [...n] ]  
UPDATE   
    [ TOP ( expression ) [ PERCENT ] ]   
    { { table_alias | <object> | rowset_function_limited   
         [ WITH ( <Table_Hint_Limited> [ ...n ] ) ]  
      }  
      | @table_variable      
    }  
    SET  
        { column_name = { expression | DEFAULT | NULL }  
          | { udt_column_name.{ { property_name = expression  
                                | field_name = expression }  
                                | method_name ( argument [ ,...n ] )  
                              }  
          }  
          | column_name { .WRITE ( expression , @Offset , @Length ) }  
          | @variable = expression  
          | @variable = column = expression  
          | column_name { += | -= | *= | /= | %= | &= | ^= | |= } expression  
          | @variable { += | -= | *= | /= | %= | &= | ^= | |= } expression  
          | @variable = column { += | -= | *= | /= | %= | &= | ^= | |= } expression  
        } [ ,...n ]   
  
    [ <OUTPUT Clause> ]  
    [ FROM{ <table_source> } [ ,...n ] ]   
    [ WHERE { <search_condition>   
            | { [ CURRENT OF   
                  { { [ GLOBAL ] cursor_name }   
                      | cursor_variable_name   
                  }   
                ]  
              }  
            }   
    ]   
    [ OPTION ( <query_hint> [ ,...n ] ) ]  
[ ; ]  
  
<object> ::=  
{   
    [ server_name . database_name . schema_name .   
    | database_name .[ schema_name ] .   
    | schema_name .  
    ]  
    table_or_view_name}  
```  
  
```syntaxsql 
-- Syntax for Azure Synapse Analysis

[ WITH <common_table_expression> [ ,...n ] ]
UPDATE [ database_name . [ schema_name ] . | schema_name . ] table_name
SET { column_name = { expression | NULL } } [ ,...n ]  
FROM [ database_name . [ schema_name ] . | schema_name . ] table_name   
JOIN {<join_table_source>}[ ,...n ] 
ON <join_condition>
[ WHERE <search_condition> ]   
[ OPTION ( LABEL = label_name ) ]  
[;]  

<join_table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [ <tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
}  
```

```syntaxsql
-- Syntax for Parallel Data Warehouse

UPDATE [ database_name . [ schema_name ] . | schema_name . ] table_name   
SET { column_name = { expression | NULL } } [ ,...n ]  
[ FROM from_clause ]  
[ WHERE <search_condition> ]   
[ OPTION ( LABEL = label_name ) ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 WITH \<common_table_expression>  
 Задает временный именованный результирующий набор или представление, которые называются обобщенным табличным выражением (CTE), определяемым в пределах области действия инструкции UPDATE. Результирующий набор CTE, на который ссылается инструкция UPDATE, является производным простого запроса.  
  
 Обобщенные табличные выражения могут также использоваться с инструкциями SELECT, INSERT, DELETE и CREATE VIEW. Дополнительные сведения см. в разделе [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 TOP **(** _expression_ **)** [ PERCENT ]  
 Задает число или процент обновляемых строк. *expression* может быть либо числом, либо процентом от числа строк.  
  
 Строки, на которые ссылается выражение TOP, используемое с инструкциями INSERT, UPDATE и DELETE, не упорядочиваются.  
  
 Инструкции INSERT, UPDATE и DELETE требуют заключения *expression* в круглые скобки в TOP. Дополнительные сведения см. в разделе [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md).  
  
 *table_alias*  
 Псевдоним, заданный в предложении FROM и представляющий таблицу или представление, строки которых будут обновлены.  
  
 *server_name*  
 Имя сервера (с использованием имени связанного сервера или функции [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) в качестве имени сервера), на котором расположена таблица или представление. Если указано *server_name*, необходимо указать *database_name* и *schema_name*.  
  
 *database_name*  
 Имя базы данных.  
  
 *schema_name*  
 Имя схемы, которой принадлежит таблица или представление.  
  
 *table_or_view_name*  
 Имя таблицы или представления, из которых должны обновляться строки. Представление, на которое ссылается аргумент *table_or_view_name*, должно быть обновляемым и ссылаться только на одну базовую таблицу в предложении FROM в представлении. Дополнительные сведения об обновляемых представлениях см. в разделе [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md).  
  
 *rowset_function_limited*  
 Функция [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) или [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md), в зависимости от возможностей поставщика.  
  
 WITH **(** \<Table_Hint_Limited> **)**  
 Задает одно или несколько табличных указаний, разрешенных для целевой таблицы. Ключевое слово WITH и круглые скобки обязательны. Использование NOLOCK, READUNCOMMITTED и NOEXPAND запрещено. Сведения о табличных указаниях см. в разделе [Табличные указания (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md).  
  
 @*table_variable*  
 Задает переменную [table](../../t-sql/data-types/table-transact-sql.md) в качестве источника таблицы.  
  
 SET  
 Задает список обновляемых имен столбцов или переменных.  
  
 *column_name*  
 Столбец, содержащий изменяемые данные. Аргумент *column_name* должен существовать в *table_or view_name*. Столбцы идентификаторов не могут быть обновлены.  
  
 *expression*  
 Переменная, литеральное значение, выражение или инструкция подзапроса выборки (заключенная в скобки), которые возвращают единственное значение. Значение, возвращаемое *expression*, заменяет существующее значение в *column_name* или @*variable*.  
  
> [!NOTE]  
>  При ссылке на типы данных символов Юникода **nchar**, **nvarchar** и **ntext** выражение 'expression' должно начинаться с заглавной буквы 'N'. Если префикс «N» не указан, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполнит преобразование строки в кодовую страницу, соответствующую параметрам сортировки базы данных или столбца, действующим по умолчанию. Любые символы, не входящие в эту кодовую страницу, будут утрачены.  
  
 DEFAULT  
 Указывает, что существующее в столбце значение будет заменено значением по умолчанию, определенным для данного столбца. Также может использоваться для присвоения значения NULL, если столбец не имеет значений по умолчанию и может принимать значения NULL.  
  
 { **+=** \| **-=** \| **\*=** \| **/=** \| **%=** \| **&=** \| **^=** \| **|=** }  
 Составной оператор присваивания:  
 +=                       Сложение и присваивание  
 –=                        Вычитание и присваивание  
 *=                        Умножение и присваивание  
 /=                         Деление и присваивание  
 %=                       Остаток от деления и присваивание  
 &=                        Выполнение побитовой операции AND и присваивание  
 ^=                        Выполнение побитовой операции XOR и присваивание  
 |=                         Выполнение побитовой операции OR и присваивание  
  
 *udt_column_name*  
 Столбец определяемого пользователем типа.  
  
 *property_name* | *field_name*  
 Общедоступное свойство или общедоступный элемент данных определяемого пользоватлем типа.  
  
 *method_name* **(** *argument* [ **,** ... *n*] **)**  
 Не статичный метод общего мутатора *udt_column_name*, принимающий один или несколько аргументов.  
  
 **.** WRITE **(** _expression_ **,** @_Offset_ **,** @_Length_ **)**  
 Указывает, что часть значения *column_name* будет изменена. *expression* заменяет единицы @*Length* начиная с @*Offset* в *column_name*. В этом предложении можно указать только столбцы типа **varchar(max)** , **nvarchar(max)** или **varbinary(max)** . Аргумент *column_name* не может иметь значение NULL и не может быть задан именем или псевдонимом таблицы.  
  
 *expression* является значением, которое копируется в *column_name*. Аргумент *expression* должен преобразовываться или поддерживать неявное преобразование к типу *column_name*. Если для *expression* установлено значение NULL, @*Length* не учитывается, а значение в *column_name* усекается с позиции, на которую указывает аргумент @*Offset*.  
  
 Аргумент @*Offset* — это начальная точка в значении, хранимом в *column_name*, начиная с которой записывается *expression*.Аргумент  @*Offset* задает последовательную позицию разряда, начиная с 0, имеет тип **bigint** и не может быть отрицательным. Если аргумент @*Offset* имеет значение NULL, операция обновления добавляет значение *expression* в конце существующего значения аргумента *column_name*, а аргумент @*Length* пропускается. Если значение аргумента @*Offset* больше, чем байтовая длина значения аргумента *column_name*, компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] возвращает ошибку. Если сумма значений @*Offset* и @*Length* превышает длину базового значения столбца, удаление выполняется до последнего символа этого значения.  
  
 Аргумент @*Length* задает длину части значения столбца начиная с @*Offset*, которая заменяется на выражение *expression*.Аргумент  @*Length* имеет тип **bigint** и не может быть отрицательным. Если аргумент @*Length* имеет значение NULL, операция обновления удаляет все данные, начиная со значения @*Offset* до конца значения *column_name*.  
  
 Дополнительные сведения см. ниже в разделе [Обновление типов данных большого объема](#updating-lobs).  
  
 **@** *variable*  
 Объявленная переменная, которой присваивается значение, возвращенное *expression*.  
  
 Предложение SET **@** _variable_ = *column* = *expression* присваивает переменной то же значение, что и столбцу. Это отличается от предложения SET **@** _variable_ = _column_, _column_ = _expression_, присваивающего переменной значение столбца до обновления.  
  
 \<OUTPUT_Clause>  
 Возвращает обновленные данные или основанные на них выражения в рамках выполнения операции UPDATE. Предложение OUTPUT не поддерживается ни в одной инструкции DML, целью которой являются удаленные таблицы или представления. Дополнительные сведения об аргументах и поведении этого предложения см. в статье [Предложение OUTPUT (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md).  
  
 FROM \<table_source>  
 Определяет, что для определения критериев операции обновления используется таблица, представление или производная таблица. Дополнительные сведения см. в разделе [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md).  
  
 Если обновляемый объект совпадает с объектом в предложении FROM, а в предложении FROM имеется только одна ссылка на этот объект, псевдоним объекта указывать не обязательно. Если обновляемый объект встречается в предложении FROM несколько раз, одна и только одна ссылка на этот объект не должна указывать псевдоним таблицы. Все остальные ссылки на объект в предложении FROM должны включать псевдоним объекта.  
  
 Представление с триггером INSTEAD OF UPDATE не может быть целью инструкции UPDATE с предложением FROM.  
  
> [!NOTE]  
>  Любой вызов функции OPENDATASOURCE, OPENQUERY или OPENROWSET в предложении FROM вычисляется отдельно и независимо от любого вызова этих функций, используемого как назначение при обновлении, даже если в двух таких вызовах будут заданы идентичные аргументы. В частности, условия фильтра или соединения, применяемые к результатам одного из таких вызовов, никак не влияют на результаты другого.  
  
 WHERE  
 Задает условия, ограничивающие обновляемые строки. Существует два вида обновлений в зависимости от используемой формы предложения WHERE.  
  
-   В поисковых обновлениях задается условие поиска строк, предназначенных к удалению.  
  
-   В позиционных обновлениях используется предложение CURRENT OF для указания курсора. Операция обновления выполняется в текущем положении курсора.  
  
\<search_condition>  
 Задает условие, которому должны удовлетворять обновляемые строки. Условие поиска может также представлять собой условие, на котором основано соединение. Количество предикатов, которое может содержать условие поиска, не ограничено. Дополнительные сведения об условиях поиска и предикатах см. в статье [Условие поиска (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md).  
  
CURRENT OF  
 Определяет, что обновление выполняется в текущей позиции указанного курсора.  
  
 Позиционированное обновление с использованием предложения WHERE CURRENT OF обновляет единственную строку в текущем положении курсора. Такое обновление может быть более точным, чем поисковое обновление, в котором для выбора строк используется предложение WHERE \<search_condition>. Если условие поиска не определяет однозначно единственную строку, поисковое обновление изменяет несколько строк.  
  
GLOBAL  
 Указывает, что аргумент *cursor_name* ссылается на глобальный курсор.  
  
*cursor_name*  
 Имя открытого курсора, из которого должна быть произведена выборка. Если существует как глобальный, так и локальный курсор с именем *cursor_name*, этот аргумент ссылается на глобальный курсор, если указан аргумент GLOBAL, в противном случае он ссылается на локальный курсор. Курсор должен позволять производить обновления.  
  
*cursor_variable_name*  
 Имя переменной курсора. Аргумент *cursor_variable_name* должен содержать ссылку на курсор, обновления которого разрешены.  
  
OPTION **(** \<query_hint> [ **,** ... *n* ] **)**  
 Определяет, что для настройки способа, которым компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] обрабатывает инструкцию, используются подсказки оптимизатора. Дополнительные сведения см. в разделе [Указания запросов (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="best-practices"></a>Рекомендации  
 Для возврата в клиентское приложение количества вставленных строк используйте функцию `@@ROWCOUNT`. Дополнительные сведения см. в статье [@@ROWCOUNT (Transact-SQL)](../../t-sql/functions/rowcount-transact-sql.md).  
  
 В инструкции UPDATE можно использовать имена переменных для показа старых и новых значений, но только в том случае, если инструкция UPDATE обрабатывает одну запись. Если инструкция UPDATE затрагивает несколько записей, для возвращения старых и новых значений каждой записи используйте [предложение OUTPUT](../../t-sql/queries/output-clause-transact-sql.md).  
  
 Проявляйте осторожность, указывая предложение FROM при задании критериев для операции обновления. Результаты инструкции UPDATE не определены, если инструкция включает предложение FROM, в котором для каждого вхождения обновляемого столбца не задано единственное значение, то есть если инструкция UPDATE не является детерминированной. Например, в инструкции UPDATE следующего скрипта обе строки в `Table1` удовлетворяют условиям предложения FROM в инструкции UPDATE, но не определено, какая строка из `Table1` используется для обновления строки в `Table2.`  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ('dbo.Table1', 'U') IS NOT NULL  
    DROP TABLE dbo.Table1;  
GO  
IF OBJECT_ID ('dbo.Table2', 'U') IS NOT NULL  
    DROP TABLE dbo.Table2;  
GO  
CREATE TABLE dbo.Table1   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
CREATE TABLE dbo.Table2   
    (ColA INT PRIMARY KEY NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0), (1, 20.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  
UPDATE dbo.Table2   
SET dbo.Table2.ColB = dbo.Table2.ColB + dbo.Table1.ColB  
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA);  
GO  
SELECT ColA, ColB   
FROM dbo.Table2;  
```  
  
 Та же проблема может возникнуть при объединении предложений `FROM` и `WHERE CURRENT OF`. В следующем примере обе строки в `Table2` удовлетворяют условиям предложения `FROM` в инструкции `UPDATE`. Не определено, какая строка из `Table2` должна использоваться для обновления строки в `Table1`.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ('dbo.Table1', 'U') IS NOT NULL  
    DROP TABLE dbo.Table1;  
GO  
IF OBJECT_ID ('dbo.Table2', 'U') IS NOT NULL  
    DROP TABLE dbo.Table2;  
GO  
CREATE TABLE dbo.Table1  
    (c1 INT PRIMARY KEY NOT NULL, c2 INT NOT NULL);  
GO  
CREATE TABLE dbo.Table2  
    (d1 INT PRIMARY KEY NOT NULL, d2 INT NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES (1, 10);  
INSERT INTO dbo.Table2 VALUES (1, 20), (2, 30);  
GO  
DECLARE abc CURSOR LOCAL FOR  
    SELECT c1, c2   
    FROM dbo.Table1;  
OPEN abc;  
FETCH abc;  
UPDATE dbo.Table1   
SET c2 = c2 + d2   
FROM dbo.Table2   
WHERE CURRENT OF abc;  
GO  
SELECT c1, c2 FROM dbo.Table1;  
GO  
```  
  
## <a name="compatibility-support"></a>Поддержка совместимости  
 Поддержка использования подсказок READUNCOMMITTED и NOLOCK в предложении FROM, применяемом к целевой таблице инструкции UPDATE или DELETE, будет удалена в следующей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Следует избегать использования этих указаний в таком контексте в новой разработке и запланировать изменение приложений, использующих их в настоящий момент.  
  
## <a name="data-types"></a>Типы данных  
 Все столбцы, имеющие тип данных **char** и **nchar**, дополняются справа до заданной длины.  
  
 Если параметр ANSI_PADDING имеет значение OFF, все конечные пробелы удаляются из данных, вставленных в столбцы **varchar** и **nchar**, за исключением строк, содержащих только пробелы. Эти строки усекаются до пустых строк. Если ANSI_PADDING имеет значение ON, вставляются конечные пробелы. Драйвер ODBC для Microsoft SQL Server и поставщик OLE DB для SQL Server автоматически устанавливают ANSI_PADDING ON для каждого соединения. Этот параметр можно настроить в источниках данных ODBC или устанавливая атрибуты или свойства соединений. Дополнительные сведения см. в разделе [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
### <a name="updating-text-ntext-and-image-columns"></a>Обновление столбцов типа text, ntext и image  
 Изменение столбцов типа **text**, **ntext** или **image** с помощью инструкции UPDATE инициализирует столбец, присваивает ему допустимый текстовый указатель и выделяет по крайней мере одну страницу данных, если столбец не обновляется значением NULL.  
  
 Чтобы заменить или изменить большие блоки данных типа **text**, **ntext** или **image**, вместо UPDATE используется инструкция [WRITETEXT](../../t-sql/queries/writetext-transact-sql.md) или [UPDATETEXT](../../t-sql/queries/updatetext-transact-sql.md).  
  
 Если инструкция UPDATE могла обновить несколько строк при обновлении как ключа кластеризации, так и одного или нескольких столбцов типа **text**, **ntext** или **image**, то частичное обновление этих столбцов выполняется как полная замена значений.  
  
> [!IMPORTANT]
>  Типы данных **ntext**, **text** и **image** будут исключены в следующей версии [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Следует избегать использования этих типов данных при новой разработке и запланировать изменение приложений, использующих их в настоящий момент. Вместо них следует использовать типы данных [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)и [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) .  
  
### <a name="updating-large-value-data-types"></a><a name="updating-lobs"></a> Обновление типов данных большого объема  
 Используйте предложение **.** WRITE **(** _expression_ **,** @_Offset_ **,** @_Length_ **)** для выполнения частичного или полного обновления типов данных **varchar(max)** , **nvarchar(max)** и **varbinary(max)** . 
 
 Например, частичное обновление столбца с типом **varchar(max)** может удалить или изменить только первые 200 байтов в столбце (200 символов при использовании символов ASCII), тогда как полное обновление удалит или изменит все данные в столбце. Обновления **.WRITE**, вставляющие или добавляющие новые данные, имеют минимальное протоколирование, если установлена простая модель восстановления базы данных или модель восстановления с неполным протоколированием. Если обновляются существующие значения, ведение журнала не сокращается до минимума. Дополнительные сведения см. в статье [Журнал транзакций (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
 Компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] преобразует частичное обновление в полное, если инструкция UPDATE приводит к одному из следующих действий.  
-   Изменения ключевого столбца секционированного представления или таблицы.  
-   Изменение более одной строки, а также обновление ключа неуникального кластеризованного индекса на непостоянное значение.  
  
Нельзя использовать предложение **.WRITE** для обновления столбца NULL или присваивания аргументу *column_name* значения NULL.  
  
Параметры @*Offset* и @*Length* указываются в байтах для типов данных **varbinary** и **varchar** и в байтовых парах для типа данных **nvarchar**. Дополнительные сведения о длине строковых типов данных см. в разделах [char и varchar (Transact-SQL)](../../t-sql/data-types/char-and-varchar-transact-sql.md) и [nchar и nvarchar (Transact-SQL)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md).
  
В целях увеличения производительности рекомендуется вставлять или обновлять данные фрагментами, кратными 8040 байтам.  
  
Если на столбец, измененный предложением **\.WRITE**, ссылается предложение OUTPUT, полное значение столбца либо исходный образ в **deleted.** _column\_name_ или преобразованный образ в **inserted.** _column\_name_ возвращается определенному столбцу в табличной переменной. См. пример Т ниже.  
  
Чтобы добиться функциональности предложения **\.WRITE** при обработке других символьных или двоичных типов данных, используется [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md).  
  
### <a name="updating-user-defined-type-columns"></a>Обновление столбцов определяемого пользователем типа  
 Обновление столбцов определяемого пользователем типа можно выполнить одним из следующих способов.  
  
-   Предоставление значения типа системных данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] происходит, если определяемый пользователем тип поддерживает явное или неявное преобразование из этого типа. Следующий пример демонстрирует, как обновить значение в столбце определяемого пользователем типа `Point` путем явного преобразования строки.  
  
    ```sql  
    UPDATE Cities  
    SET Location = CONVERT(Point, '12.3:46.2')  
    WHERE Name = 'Anchorage';  
    ```  
  
-   Вызов метода, помеченного в качестве мутатора определенного пользователем типа для осуществления обновления. Следующий пример вызывает метод мутатора типа `Point` с именем `SetXY`. Это обновляет состояние экземпляра типа.  
  
    ```sql  
    UPDATE Cities  
    SET Location.SetXY(23.5, 23.5)  
    WHERE Name = 'Anchorage';  
    ```  
  
    > [!NOTE]  
    > [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает ошибку, если метод мутатора возвращает значение NULL языка [!INCLUDE[tsql](../../includes/tsql-md.md)] либо если новое значение, порожденное методом мутатора, соответствует значению NULL.  
  
-   Изменение значения зарегистрированного свойства или общедоступного элемента данных определяемого пользователем типа. Выражение, предоставляющее значение, должно допускать неявное преобразование в тип свойства. В следующем примере изменяется значение свойства `X` определяемого пользователем типа `Point`.  
  
    ```sql  
    UPDATE Cities  
    SET Location.X = 23.5  
    WHERE Name = 'Anchorage';  
    ```  
  
     Для изменения различных свойств одного и того же столбца определяемого пользователем типа нужно выполнить несколько инструкций UPDATE или использовать метод мутатора соответствующего типа.  
  
### <a name="updating-filestream-data"></a>Обновление данных FILESTREAM  
 Инструкция UPDATE позволяет обновить поля FILESTREAM значением NULL, пустым значением или встроенными данными относительно небольшого размера. Однако при работе с большими объемами данных более эффективно передавать поток в файл с использованием интерфейсов Win32. При обновлении поля FILESTREAM происходит изменение базовых данных BLOB в файловой системе. Если в поле FILESTREAM содержится значение NULL, данные BLOB, связанные с этим полем, удаляются. Для частичного обновления данных потока FILESTREAM недопустимо использовать метод .WRITE(). Дополнительные сведения см. в разделе [FILESTREAM (SQL Server)](../../relational-databases/blob/filestream-sql-server.md).  
  
## <a name="error-handling"></a>Обработка ошибок  
 Если обновление строки нарушает ограничение, правило или установку NULL для столбца либо новое значение имеет несовместимый тип данных, то инструкция отменяется, возвращается ошибка и никакие записи не обновляются.  
  
 Если инструкция UPDATE при оценке выражения встречает арифметическую ошибку (переполнение, деление на ноль или ошибку домена), обновление не выполняется. Остальная часть пакета не выполняется и возвращается сообщение об ошибке.  
  
 Если обновление столбца или столбцов, участвующих в кластеризованном индексе, приводит к тому, что размер кластеризованного индекса и строки превышает 8 060 байт, обновление заканчивается неудачей и возвращается сообщение об ошибке.  
  
## <a name="interoperability"></a>Совместимость  
 Инструкции UPDATE разрешается использовать в теле определяемых пользователем функций только в том случае, если изменяемая таблица является табличной переменной.  
  
 Если для операций UPDATE по отношению к таблице определен триггер `INSTEAD OF`, вместо инструкции UPDATE запускается этот триггер. Ранние версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживали для UPDATE и других инструкций изменения данных только определение триггеров AFTER. В инструкции UPDATE, которая прямо или косвенно ссылается на представление с определенным для него триггером `INSTEAD OF`, не может быть указано предложение FROM. Дополнительные сведения о триггерах INSTEAD OF см. в разделе [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md).  
  
## <a name="limitations-and-restrictions"></a>ограничения  
 В инструкции UPDATE, которая прямо или косвенно ссылается на представление с определенным для него триггером `INSTEAD OF`, не может быть указано предложение FROM. Дополнительные сведения о триггерах `INSTEAD OF` см. в разделе [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md).  
  
 Если обобщенное табличное выражение указывается в качестве цели инструкции UPDATE, должны совпадать все ссылки на это выражение в инструкции. Например, если для обобщенного табличного выражения в предложении FROM назначается псевдоним, то этот псевдоним должен использоваться для всех остальных ссылок на обобщенное табличное выражение. Требуются однозначные ссылки на обобщенное табличное выражение, так как обобщенное табличное выражение не имеет идентификатор объекта, который [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует для распознавания неявной связи между объектом и его псевдонимом. В отсутствие такой связи план запроса может непредвиденным образом построить работу с соединениями, что приведет к нежелательным результатам запроса. В следующих примерах показаны правильные и неправильные методы задания обобщенного табличного выражения, когда оно является целевым объектом операции обновления.  
  
```sql  
USE tempdb;  
GO  
-- UPDATE statement with CTE references that are correctly matched.  
DECLARE @x TABLE (ID INT, Value INT);  
DECLARE @y TABLE (ID INT, Value INT);  
INSERT @x VALUES (1, 10), (2, 20);  
INSERT @y VALUES (1, 100),(2, 200);  
  
WITH cte AS (SELECT * FROM @x)  
UPDATE x -- cte is referenced by the alias.  
SET Value = y.Value  
FROM cte AS x  -- cte is assigned an alias.  
INNER JOIN @y AS y ON y.ID = x.ID;  
SELECT * FROM @x;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 ID     Value  
 ------ -----  
 1      100  
 2      200  
 (2 row(s) affected)  
```  

Инструкция UPDATE с неправильно подобранными ссылками на обобщенное табличное выражение.  

```sql  
USE tempdb;  
GO  
DECLARE @x TABLE (ID INT, Value INT);  
DECLARE @y TABLE (ID INT, Value INT);  
INSERT @x VALUES (1, 10), (2, 20);  
INSERT @y VALUES (1, 100),(2, 200);  
  
WITH cte AS (SELECT * FROM @x)  
UPDATE cte   -- cte is not referenced by the alias.  
SET Value = y.Value  
FROM cte AS x  -- cte is assigned an alias.  
INNER JOIN @y AS y ON y.ID = x.ID;   
SELECT * FROM @x;   
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
ID     Value  
------ -----  
1      100  
2      100  
(2 row(s) affected)  
```  

## <a name="locking-behavior"></a>Режим блокировки  
 Инструкция UPDATE получает монопольную блокировку (X) на строки, которые она изменяет, и держит блокировку до тех пор, пока транзакция не завершится. В зависимости от плана запроса для инструкции UPDATE, количества изменяемых строк и уровня изоляции транзакции, блокировки могут быть получены на уровне страницы или на уровне таблицы, а не на уровне строк. Чтобы избежать этих блокировок более высокого уровня, можно разделить инструкцию UPDATE, которая распространяется на тысячи или более строк, на пакеты и убедиться, что все условия объединения и фильтрации поддерживаются индексами. Дополнительные сведения о блокировке механизмов в SQL Server см. в статье [Блокировка в ядре СУБД](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine).  
  
## <a name="logging-behavior"></a>Режим ведения журнала  
 Инструкция UPDATE записывается в журнал, однако частичные обновления типов данных с большими значениями с использованием предложения **\.WRITE** регистрируются на минимальном уровне. Дополнительные сведения см. ниже в подразделе "Обновление типов данных большого объема" приведенного ранее раздела "Типы данных".  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Разрешения `UPDATE` необходимы для целевой таблицы. Кроме того, требуются разрешения на выполнение `SELECT` для обновляемой таблицы, если инструкция UPDATE содержит предложение WHERE или если аргумент *expression* в предложении SET использует столбец в этой таблице.  
  
 Разрешения UPDATE по умолчанию предоставляются членам предопределенной роли сервера `sysadmin`, членам предопределенных ролей баз данных `db_owner` и `db_datawriter`, а также владельцу таблицы. Члены ролей `sysadmin`, `db_owner` и `db_securityadmin`, а также владелец таблицы могут передавать разрешения другим пользователям.  
  
##  <a name="examples"></a><a name="UpdateExamples"></a> Примеры  
  
|Категория|Используемые элементы синтаксиса|  
|--------------|------------------------------|  
|[Базовый синтаксис](#BasicSyntax)|UPDATE|  
|[Ограничение обновляемых строк](#LimitingValues)|WHERE • TOP • WITH обобщенное табличное выражение • WHERE CURRENT OF|  
|[Установка значений столбца](#ColumnValues)|вычисляемые значения • составные операторы • значения по умолчанию • вложенные запросы|  
|[Указание целевых объектов, отличных от стандартных таблиц](#TargetObjects)|представления • табличные переменные • псевдонимы таблицы|  
|[Обновление данных на основе данных из других таблиц](#OtherTables)|FROM|  
|[Обновление строк в удаленной таблице](#RemoteTables)|связанный сервер • OPENQUERY • OPENDATASOURCE|  
|[Обновление типов данных больших объектов](#LOBValues)|.WRITE • OPENROWSET|  
|[Обновление определяемых пользователем типов данных](#UDTs)|определяемые пользователем типы|  
|[Переопределение поведения по умолчанию для оптимизатора запросов с помощью указаний](#TableHints)|табличные подсказки • подсказки в запросах|  
|[Сбор результатов выполнения инструкции UPDATE](#CaptureResults)|OUTPUT, предложение|  
|[Использование инструкции UPDATE в других инструкциях](#Other)|Хранимые процедуры • TRY...CATCH|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> Основной синтаксис  
 В примерах в этом разделе описывается базовая функциональность инструкции UPDATE с помощью минимального необходимого синтаксиса.  
  
#### <a name="a-using-a-simple-update-statement"></a>A. Использование простой инструкции UPDATE  
 В следующем примере обновляется один столбец для всех строк в таблице `Person.Address`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Person.Address  
SET ModifiedDate = GETDATE();  
```  
  
#### <a name="b-updating-multiple-columns"></a>Б. Обновление нескольких столбцов  
 В следующем примере выполняется обновление значений в столбцах `Bonus`, `CommissionPct` и `SalesQuota` для всех строк в таблице `SalesPerson`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET Bonus = 6000, CommissionPct = .10, SalesQuota = NULL;  
GO  
```  
  
###  <a name="limiting-the-rows-that-are-updated"></a><a name="LimitingValues"></a> Ограничение обновляемых строк  
 В примерах в этом разделе описываются способы ограничения количества строк, на которые влияет инструкция UPDATE.  
  
#### <a name="c-using-the-where-clause"></a>В. Применение предложения WHERE  
 В следующем примере предложение WHERE используется для указания строк, которые необходимо обновить. Инструкция обновляет значение в столбце `Color` таблицы `Production.Product` для всех строк, в которых имеется существующее значение Red в столбце `Color` и имеется значение в столбце `Name`, который начинается с Road-250.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.Product  
SET Color = N'Metallic Red'  
WHERE Name LIKE N'Road-250%' AND Color = N'Red';  
GO  
```  
  
#### <a name="d-using-the-top-clause"></a>Г. Использование предложения TOP  
 В следующем примере предложение TOP используется для ограничения числа строк, изменяемых в процессе выполнения инструкции UPDATE. Если в инструкции UPDATE указано предложение TOP (*n*), то операция обновления выполняется для произвольного подмножества в *n* строк. В следующем примере в столбце `VacationHours` для случайных 10 строк таблицы `Employee` значение меняется на 25 %.  
  
```sql  
USE AdventureWorks2012;
GO
UPDATE TOP (10) HumanResources.Employee
SET VacationHours = VacationHours * 1.25 ;
GO  
```  
  
 Если нужно применить изменения с предложением TOP в определенной последовательности, укажите во вложенной инструкции выборки инструкцию ORDER BY. В следующем примере изменяется длительность отпуска для 10 сотрудников, имеющих наибольший стаж работы.  
  
```sql  
UPDATE HumanResources.Employee  
SET VacationHours = VacationHours + 8  
FROM (SELECT TOP 10 BusinessEntityID FROM HumanResources.Employee  
     ORDER BY HireDate ASC) AS th  
WHERE HumanResources.Employee.BusinessEntityID = th.BusinessEntityID;  
GO  
```  
  
#### <a name="e-using-the-with-common_table_expression-clause"></a>Д. Использование предложения WITH обобщенное_табличное_выражение  
 В следующем примере обновляется значение `PerAssemblyQty` для всех частей и компонентов, прямо или косвенно используемых для создания `ProductAssemblyID 800`. Обобщенное табличное выражение возвращает иерархический список частей, которые непосредственно используются для сборки `ProductAssemblyID 800`, и частей, которые используются для сборки этих компонентов, и т. д. Изменяются только строки, возвращенные обобщенным табличным выражением.  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH Parts(AssemblyID, ComponentID, PerAssemblyQty, EndDate, ComponentLevel) AS  
(  
    SELECT b.ProductAssemblyID, b.ComponentID, b.PerAssemblyQty,  
        b.EndDate, 0 AS ComponentLevel  
    FROM Production.BillOfMaterials AS b  
    WHERE b.ProductAssemblyID = 800  
          AND b.EndDate IS NULL  
    UNION ALL  
    SELECT bom.ProductAssemblyID, bom.ComponentID, p.PerAssemblyQty,  
        bom.EndDate, ComponentLevel + 1  
    FROM Production.BillOfMaterials AS bom   
        INNER JOIN Parts AS p  
        ON bom.ProductAssemblyID = p.ComponentID  
        AND bom.EndDate IS NULL  
)  
UPDATE Production.BillOfMaterials  
SET PerAssemblyQty = c.PerAssemblyQty * 2  
FROM Production.BillOfMaterials AS c  
JOIN Parts AS d ON c.ProductAssemblyID = d.AssemblyID  
WHERE d.ComponentLevel = 0;  
```  
  
#### <a name="f-using-the-where-current-of-clause"></a>Е. Использование предложения WHERE CURRENT OF  
 В следующем примере предложение WHERE CURRENT OF используется только для обновления строк, на которых установлен курсор. Если курсор основан на соединении, изменяется только таблица `table_name`, указанная в инструкции UPDATE. Другие таблицы, участвующие в курсоре, не затрагиваются.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE complex_cursor CURSOR FOR  
    SELECT a.BusinessEntityID  
    FROM HumanResources.EmployeePayHistory AS a  
    WHERE RateChangeDate <>   
         (SELECT MAX(RateChangeDate)  
          FROM HumanResources.EmployeePayHistory AS b  
          WHERE a.BusinessEntityID = b.BusinessEntityID) ;  
OPEN complex_cursor;  
FETCH FROM complex_cursor;  
UPDATE HumanResources.EmployeePayHistory  
SET PayFrequency = 2   
WHERE CURRENT OF complex_cursor;  
CLOSE complex_cursor;  
DEALLOCATE complex_cursor;  
GO  
```  
  
###  <a name="setting-column-values"></a><a name="ColumnValues"></a> Установка значений столбца  
 В примерах этого раздела описывается обновление столбцов с помощью вычисляемых значений, вложенных запросов и значений DEFAULT.  
  
#### <a name="g-specifying-a-computed-value"></a>Ж. Указание вычисляемого значения  
 В следующих примерах используются вычисляемые значения в инструкции UPDATE. В примере удваивается значение столбца `ListPrice` для всех строк в таблице `Product`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
UPDATE Production.Product  
SET ListPrice = ListPrice * 2;  
GO  
```  
  
#### <a name="h-specifying-a-compound-operator"></a>З. Задание составного оператора  
 В следующем примере демонстрируется использование переменной `@NewPrice` для увеличения цены красных велосипедов прибавлением 10 к текущей цене.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @NewPrice INT = 10;  
UPDATE Production.Product  
SET ListPrice += @NewPrice  
WHERE Color = N'Red';  
GO  
```  
  
 В следующем примере используется составной оператор += для добавления данных `' - tool malfunction'` к существующему значению в столбце `Name` для строк, имеющих значение `ScrapReasonID` от 10 до 12.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.ScrapReason   
SET Name += ' - tool malfunction'  
WHERE ScrapReasonID BETWEEN 10 and 12;  
```  
  
#### <a name="i-specifying-a-subquery-in-the-set-clause"></a>И. Задание вложенного запроса в предложении SET  
 В следующем примере используется вложенный запрос в предложении SET для определения значения, которое используется для обновления столбца. Вложенный запрос должен возвращать только скалярное значение (то есть одно значение для каждой строки). В примере изменяется столбец `SalesYTD` в таблице `SalesPerson` для отображения самой последней информации о продажах, зафиксированной в таблице `SalesOrderHeader`. Вложенный запрос проводит статистическую обработку сведений о продажах по всем продавцам в инструкции `UPDATE`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET SalesYTD = SalesYTD +   
    (SELECT SUM(so.SubTotal)   
     FROM Sales.SalesOrderHeader AS so  
     WHERE so.OrderDate = (SELECT MAX(OrderDate)  
                           FROM Sales.SalesOrderHeader AS so2  
                           WHERE so2.SalesPersonID = so.SalesPersonID)  
     AND Sales.SalesPerson.BusinessEntityID = so.SalesPersonID  
     GROUP BY so.SalesPersonID);  
GO  
```  
  
#### <a name="j-updating-rows-using-default-values"></a>К. Обновление строк с использованием значений DEFAULT  
 В следующем примере для столбца `CostRate` задается значение по умолчанию (0,00) для всех строк, значение `CostRate` которых больше `20.00`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.Location  
SET CostRate = DEFAULT  
WHERE CostRate > 20.00;  
```  
  
###  <a name="specifying-target-objects-other-than-standard-tables"></a><a name="TargetObjects"></a> Указание целевых объектов, отличных от стандартных таблиц  
 В примерах этого раздела описаны методы обновления строк с указанием представления, псевдонима таблицы или табличной переменной.  
  
#### <a name="k-specifying-a-view-as-the-target-object"></a>Л. Указание представления в качестве целевого объекта  
 В следующем примере выполняется обновление строк таблицы путем указания представления в качестве целевого объекта. Определение представления ссылается на несколько таблиц, однако инструкция UPDATE была успешно выполнена, поскольку она ссылается на столбцы только одной из базовых таблиц. При выполнении инструкции UPDATE произойдет сбой, если были указаны столбцы из обеих таблиц. Дополнительные сведения см. в разделе [Изменение данных через представление](../../relational-databases/views/modify-data-through-a-view.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Person.vStateProvinceCountryRegion  
SET CountryRegionName = 'United States of America'  
WHERE CountryRegionName = 'United States';  
```  
  
#### <a name="l-specifying-a-table-alias-as-the-target-object"></a>М. Задание псевдонима таблицы в качестве целевого объекта  
 В следующем примере производится обновление строк в таблице `Production.ScrapReason`. Псевдоним таблицы, заданный для `ScrapReason` в предложении FROM, указывается как целевой объект в предложении UPDATE.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE sr  
SET sr.Name += ' - tool malfunction'  
FROM Production.ScrapReason AS sr  
JOIN Production.WorkOrder AS wo   
     ON sr.ScrapReasonID = wo.ScrapReasonID  
     AND wo.ScrappedQty > 300;  
```  
  
#### <a name="m-specifying-a-table-variable-as-the-target-object"></a>Н. Задание табличной переменной в качестве целевого объекта  
 В следующем примере производится обновление строк в табличной переменной.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create the table variable.  
DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    NewVacationHours INT,  
    ModifiedDate DATETIME);  
  
-- Populate the table variable with employee ID values from HumanResources.Employee.  
INSERT INTO @MyTableVar (EmpID)  
    SELECT BusinessEntityID FROM HumanResources.Employee;  
  
-- Update columns in the table variable.  
UPDATE @MyTableVar  
SET NewVacationHours = e.VacationHours + 20,  
    ModifiedDate = GETDATE()  
FROM HumanResources.Employee AS e   
WHERE e.BusinessEntityID = EmpID;  
  
-- Display the results of the UPDATE statement.  
SELECT EmpID, NewVacationHours, ModifiedDate FROM @MyTableVar  
ORDER BY EmpID;  
GO  
```  
  
###  <a name="updating-data-based-on-data-from-other-tables"></a><a name="OtherTables"></a> Обновление данных на основе данных из других таблиц  
 В примерах этого раздела описаны методы обновления строк одной таблицы на основе данных в другой таблице.  
  
#### <a name="n-using-the-update-statement-with-information-from-another-table"></a>О. Использование инструкции UPDATE с данными из другой таблицы  
 В следующем примере изменяется столбец `SalesYTD` в таблице `SalesPerson` для отображения самой последней информации о продажах, зафиксированной в таблице `SalesOrderHeader`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET SalesYTD = SalesYTD + SubTotal  
FROM Sales.SalesPerson AS sp  
JOIN Sales.SalesOrderHeader AS so  
    ON sp.BusinessEntityID = so.SalesPersonID  
    AND so.OrderDate = (SELECT MAX(OrderDate)  
                        FROM Sales.SalesOrderHeader  
                        WHERE SalesPersonID = sp.BusinessEntityID);  
GO  
```  
  
 В предыдущем примере подразумевается, что на конкретного менеджера по продажам на каждую конкретную дату записана только одна продажа и выполнены все текущие обновления. Если в один и тот же день для указанного менеджера может иметься более одной продажи, приведенный пример будет работать неправильно. Пример выполняется без ошибок, но каждое из значений `SalesYTD` обновляется только для одной из продаж, вне зависимости от действительного количества продаж в этот день. Это происходит потому, что одиночная инструкция UPDATE никогда не обновляет одну и ту же строку дважды.  
  
 В ситуации, когда у данного менеджера по продажам имеется несколько продаж в день, все продажи для каждого из менеджеров по продажам должны быть собраны вместе инструкцией `UPDATE`, как показано в следующем примере.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET SalesYTD = SalesYTD +   
    (SELECT SUM(so.SubTotal)   
     FROM Sales.SalesOrderHeader AS so  
     WHERE so.OrderDate = (SELECT MAX(OrderDate)  
                           FROM Sales.SalesOrderHeader AS so2  
                           WHERE so2.SalesPersonID = so.SalesPersonID)  
     AND Sales.SalesPerson.BusinessEntityID = so.SalesPersonID  
     GROUP BY so.SalesPersonID);  
GO  
```  
  
###  <a name="updating-rows-in-a-remote-table"></a><a name="RemoteTables"></a> Обновление строк в удаленной таблице  
 В примерах в этом разделе описаны способы обновления строк в удаленной целевой таблице с использованием в качестве ссылки на удаленную таблицу [связанного сервера](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) или [функции, возвращающей набор строк](../functions/opendatasource-transact-sql.md).  
  
#### <a name="o-updating-data-in-a-remote-table-by-using-a-linked-server"></a>П. Обновление данных в удаленной таблице с использованием связанного сервера  
 В следующем примере обновляется таблица на удаленном сервере. Этот пример начинается с создания ссылки на удаленный источник данных с помощью хранимой процедуры [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md). Затем имя связанного сервера `MyLinkedServer` указывается в качестве одного из четырех компонентов имени объекта в формате сервер.каталог.схема.объект. Обратите внимание, что необходимо указать действительное имя сервера для `@datasrc`.  
  
```sql  
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' or 'server_nameinstance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkedServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI10',   
    @datasrc = N'<server name>',  
    @catalog = N'AdventureWorks2012';  
GO  
USE AdventureWorks2012;  
GO  
-- Specify the remote data source using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
UPDATE MyLinkedServer.AdventureWorks2012.HumanResources.Department  
SET GroupName = N'Public Relations'  
WHERE DepartmentID = 4;  
```  
  
#### <a name="p-updating-data-in-a-remote-table-by-using-the-openquery-function"></a>Т. Обновление данных в удаленной таблице с помощью функции OPENQUERY  
 В следующем примере выполняется обновление строки в удаленной таблице с помощью вызова функции [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md), возвращающей набор строк. В этом примере используется имя связанного сервера, созданного в предыдущем примере.  
  
```sql  
UPDATE OPENQUERY (MyLinkedServer, 'SELECT GroupName FROM HumanResources.Department WHERE DepartmentID = 4')   
SET GroupName = 'Sales and Marketing';  
```  
  
#### <a name="q-updating-data-in-a-remote-table-by-using-the-opendatasource-function"></a>У. Обновление данных в удаленной таблице с помощью функции OPENDATASOURCE  
 В следующем примере выполняется обновление строки в удаленной таблице с помощью вызова функции [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md), возвращающей набор строк. Определите допустимое имя сервера для источника данных, используя формат *server_name* или *server_name\instance_name*. Возможно, потребуется настроить у экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] параметр Ad Hoc Distributed Queries. Дополнительные сведения см. в статье [Параметр конфигурации сервера "ad hoc distributed queries"](../../database-engine/configure-windows/ad-hoc-distributed-queries-server-configuration-option.md).  

```sql
UPDATE OPENDATASOURCE('SQLNCLI', 'Data Source=<server name>;Integrated Security=SSPI').AdventureWorks2012.HumanResources.Department
SET GroupName = 'Sales and Marketing' WHERE DepartmentID = 4;  
```

###  <a name="updating-large-object-data-types"></a><a name="LOBValues"></a> Обновление типов данных больших объектов  
 В примерах в этом разделе описываются методы обновления значений в столбцах, определенных с типами данных больших объектов (LOB).  
  
#### <a name="r-using-update-with-write-to-modify-data-in-an-nvarcharmax-column"></a>Ф. Использование инструкции UPDATE с предложением .WRITE для изменения данных в столбце nvarchar(max)  
 В следующем примере предложение .WRITE используется для обновления частичного значения столбца `DocumentSummary` типа **nvarchar(max)** в таблице `Production.Document`. Слово `components` заменяется словом `features` путем указания слова для замены, начального положения (смещения) заменяемого слова в существующих данных и количества заменяемых символов (длины). В этом примере также используется предложение OUTPUT для возврата исходного и преобразованного образов столбца `DocumentSummary` в табличную переменную `@MyTableVar`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar TABLE (  
    SummaryBefore NVARCHAR(max),  
    SummaryAfter NVARCHAR(max));  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N'features',28,10)  
OUTPUT deleted.DocumentSummary,   
       inserted.DocumentSummary   
    INTO @MyTableVar  
WHERE Title = N'Front Reflector Bracket Installation';  
SELECT SummaryBefore, SummaryAfter   
FROM @MyTableVar;  
GO  
```  
  
#### <a name="s-using-update-with-write-to-add-and-remove-data-in-an-nvarcharmax-column"></a>Х. Использование UPDATE с предложением .WRITE для добавления и удаления данных в столбце типа nvarchar(max)  
 В следующем примере данные добавляются в столбец типа **nvarchar(max)** , имеющий текущее значение NULL, и удаляются из него. Поскольку предложение .WRITE не может использоваться для изменения столбца со значением NULL, этот столбец сначала заполняется временными данными. Затем они заменяются правильными данными с помощью предложения .WRITE. В дополнительных примерах данные добавляются в конец значения столбца, удаляются из столбца (усекаются) и, наконец, удаляются частичные данные из столбца. Инструкции SELECT выводят на экран изменения данных, создаваемые каждой из инструкций UPDATE.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Replacing NULL value with temporary data.  
UPDATE Production.Document  
SET DocumentSummary = N'Replacing NULL value'  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Replacing temporary data with the correct data. Setting @Length to NULL   
-- truncates all existing data from the @Offset position.  
UPDATE Production.Document  
SET DocumentSummary .WRITE(N'Carefully inspect and maintain the tires and crank arms.',0,NULL)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Appending additional data to the end of the column by setting   
-- @Offset to NULL.  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N' Appending data to the end of the column.', NULL, 0)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Removing all data from @Offset to the end of the existing value by   
-- setting expression to NULL.   
UPDATE Production.Document  
SET DocumentSummary .WRITE (NULL, 56, 0)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Removing partial data beginning at position 9 and ending at   
-- position 21.  
UPDATE Production.Document  
SET DocumentSummary .WRITE ('',9, 12)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
```  
  
#### <a name="t-using-update-with-openrowset-to-modify-a-varbinarymax-column"></a>T. Использование инструкции UPDATE с функцией OPENROWSET для изменения столбца типа varbinary(max)  
 В следующем примере существующий образ в столбце типа **varbinary(max)** заменяется новым образом. Для загрузки образа в столбец используется функция [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) с параметром BULK. В этом примере предполагается, что по заданному пути существует файл с именем `Tires.jpg`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.ProductPhoto  
SET ThumbNailPhoto = (  
    SELECT *  
    FROM OPENROWSET(BULK 'c:Tires.jpg', SINGLE_BLOB) AS x )  
WHERE ProductPhotoID = 1;  
GO  
```  
  
#### <a name="u-using-update-to-modify-filestream-data"></a>Ф. Использование UPDATE для изменения данных FILESTREAM  
 В следующем примере инструкция UPDATE используется для изменения данных в файлах файловой системы. Не рекомендуется использовать этот метод для потоковой передачи больших объемов данных в файл. Используйте соответствующие интерфейсы Win32. В следующем примере любой текст в записи файла заменяется текстом `Xray 1`. Дополнительные сведения см. в разделе [FILESTREAM (SQL Server)](../../relational-databases/blob/filestream-sql-server.md).  
  
```sql  
UPDATE Archive.dbo.Records  
SET [Chart] = CAST('Xray 1' as VARBINARY(max))  
WHERE [SerialNumber] = 2;  
```  
  
###  <a name="updating-user-defined-types"></a><a name="UDTs"></a> Обновление определяемых пользователем типов данных  
 В следующих примерах изменяются значения в столбцах определяемых пользователем типов данных CLR. Описываются три метода. Дополнительные сведения об определяемых пользователем столбцах см. в разделе [Определяемые пользователем типы данных CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md).  
  
#### <a name="v-using-a-system-data-type"></a>V. Использование системных типов данных  
 Определяемый пользователем тип можно обновить путем предоставления значения в типе системных данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при поддержке определяемым пользователем типа явного или неявного преобразования из этого типа. Следующий пример демонстрирует, как обновить значение в столбце определяемого пользователем типа `Point` путем явного преобразования строки.  
  
```sql  
UPDATE dbo.Cities  
SET Location = CONVERT(Point, '12.3:46.2')  
WHERE Name = 'Anchorage';  
```  
  
#### <a name="w-invoking-a-method"></a>Ц. Вызов метода  
 Определяемый пользователем тип можно обновить путем вызова метода, отмеченного в качестве мутатора определяемого пользователем типа, для выполнения обновления. Следующий пример вызывает метод мутатора типа `Point` с именем `SetXY`. Это обновляет состояние экземпляра типа.  
  
```sql  
UPDATE dbo.Cities  
SET Location.SetXY(23.5, 23.5)  
WHERE Name = 'Anchorage';  
```  
  
#### <a name="x-modifying-the-value-of-a-property-or-data-member"></a>X. Изменение значения свойства или элемента данных  
 Определяемый пользователем тип можно обновить путем изменения значения зарегистрированного свойства или общедоступного элемента данных определяемого пользователем типа. Выражение, предоставляющее значение, должно допускать неявное преобразование в тип свойства. В следующем примере изменяется значение свойства `X` определяемого пользователем типа `Point`.  
  
```sql  
UPDATE dbo.Cities  
SET Location.X = 23.5  
WHERE Name = 'Anchorage';  
```  
  
###  <a name="overriding-the-default-behavior-of-the-query-optimizer-by-using-hints"></a><a name="TableHints"></a> Переопределение поведения по умолчанию для оптимизатора запросов с помощью указаний  
 Примеры в этом разделе описывают использование табличных подсказок и подсказок в запросах для временного переопределения поведения оптимизатора запросов при обработке инструкции UPDATE.  
  
> [!CAUTION]  
>  Поскольку оптимизатор запросов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обычно выбирает наилучший план выполнения запроса, подсказки рекомендуется использовать только опытным разработчикам и администраторам баз данных в качестве последнего средства.  
  
#### <a name="y-specifying-a-table-hint"></a>Ш. Задание табличной подсказки  
 В следующем примере задается [табличное указание](../../t-sql/queries/hints-transact-sql-table.md) TABLOCK. Эта подсказка указывает, что на таблицу `Production.Product` накладывается совмещаемая блокировка, удерживаемая до завершения инструкции UPDATE.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.Product  
WITH (TABLOCK)  
SET ListPrice = ListPrice * 1.10  
WHERE ProductNumber LIKE 'BK-%';  
GO  
```  
  
#### <a name="z-specifying-a-query-hint"></a>Z. Задание подсказки в запросе  
 В следующем примере задается [указание запроса](../../t-sql/queries/hints-transact-sql-query.md)`OPTIMIZE FOR (@variable)` в инструкции UPDATE. Эта подсказка указывает на необходимость использования оптимизатором запросов при компиляции и оптимизации запросов конкретного значения локальной переменной. Значение используется только в процессе оптимизации запроса, но не в процессе выполнения.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PROCEDURE Production.uspProductUpdate  
@Product NVARCHAR(25)  
AS  
SET NOCOUNT ON;  
UPDATE Production.Product  
SET ListPrice = ListPrice * 1.10  
WHERE ProductNumber LIKE @Product  
OPTION (OPTIMIZE FOR (@Product = 'BK-%') );  
GO  
-- Execute the stored procedure   
EXEC Production.uspProductUpdate 'BK-%';  
```  
  
###  <a name="capturing-the-results-of-the-update-statement"></a><a name="CaptureResults"></a> Сбор результатов выполнения инструкции UPDATE  
 Примеры в этом разделе описывают использование [предложения OUTPUT](../../t-sql/queries/output-clause-transact-sql.md) для возврата данных для всех строк, изменившихся в результате выполнения инструкции UPDATE, либо выражений на основе этих данных. Эти результаты могут быть возвращены приложению, например для вывода подтверждающих сообщений, архивирования и т. п.  
  
#### <a name="aa-using-update-with-the-output-clause"></a>AA. Использование инструкции UPDATE с предложением OUTPUT  
 В следующем примере значения в столбце `VacationHours` в первых 10 строках таблицы `Employee` уменьшаются до 25 % от исходных значений, а в столбец `ModifiedDate` заносится текущая дата. Предложение `OUTPUT` возвращает значение `VacationHours`, существующее до применения инструкции `UPDATE` в столбце `deleted.VacationHours`, и обновленное значение в столбце `inserted.VacationHours` к табличной переменной `@MyTableVar`.  
  
 Две следующие инструкции `SELECT` возвращают значения в табличную переменную `@MyTableVar`, а результаты операции обновления — в таблицу `Employee`. Дополнительные примеры использования предложения OUTPUT см. в статье [Предложение OUTPUT (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md).  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate datetime);  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25,  
    ModifiedDate = GETDATE()   
OUTPUT inserted.BusinessEntityID,  
       deleted.VacationHours,  
       inserted.VacationHours,  
       inserted.ModifiedDate  
INTO @MyTableVar;  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```  
  
###  <a name="using-update-in-other-statements"></a><a name="Other"></a> Использование инструкции UPDATE в других инструкциях  
 В примерах в этом разделе описывается использование UPDATE в других инструкциях.  
  
#### <a name="ab-using-update-in-a-stored-procedure"></a>АБ. Использование UPDATE в хранимой процедуре  
 В следующем примере инструкция UPDATE используется в хранимой процедуре. Процедура принимает один входной параметр `@NewHours` и один выходной параметр `@RowCount`. Значение параметра `@NewHours` используется в инструкции UPDATE для обновления столбца `VacationHours` в таблице `HumanResources.Employee`. Выходной параметр `@RowCount` используется для возврата значения числа задействованных строк в локальную переменную. Выражение CASE используется в предложении SET для условного определения значения, которое задано для столбца `VacationHours`. Если для сотрудника применяется почасовая ставка оплаты (`SalariedFlag` = 0), то в столбце `VacationHours` устанавливается текущее количество часов плюс значение, заданное в `@NewHours`. В противном случае в столбце `VacationHours` указывается значение, заданное в `@NewHours`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PROCEDURE HumanResources.Update_VacationHours  
@NewHours SMALLINT  
AS   
SET NOCOUNT ON;  
UPDATE HumanResources.Employee  
SET VacationHours =   
    ( CASE  
         WHEN SalariedFlag = 0 THEN VacationHours + @NewHours  
         ELSE @NewHours  
       END  
    )  
WHERE CurrentFlag = 1;  
GO  
  
EXEC HumanResources.Update_VacationHours 40;  
```  
  
#### <a name="ac-using-update-in-a-trycatch-block"></a>АВ. Использование UPDATE в блоке TRY...CATCH  
 В следующем примере инструкция UPDATE используется в блоке TRY...CATCH для обработки ошибок выполнения, которые могут возникнуть во время операции обновления.  
  
```sql  
USE AdventureWorks2012;  
GO  
BEGIN TRANSACTION;  
  
BEGIN TRY  
    -- Intentionally generate a constraint violation error.  
    UPDATE HumanResources.Department  
    SET Name = N'MyNewName'  
    WHERE DepartmentID BETWEEN 1 AND 2;  
END TRY  
BEGIN CATCH  
    SELECT   
         ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
  
    IF @@TRANCOUNT > 0  
        ROLLBACK TRANSACTION;  
END CATCH;  
  
IF @@TRANCOUNT > 0  
    COMMIT TRANSACTION;  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Примеры: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="ad-using-a-simple-update-statement"></a>АГ. Использование простой инструкции UPDATE  
 В следующем примере показано, как могут быть обработаны все строки, если не использовать предложение WHERE для указания обновляемой строки или строк.  
  
 Происходит обновление значений в столбцах `EndDate` и `CurrentFlag` для всех строк в таблице `DimEmployee`.  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimEmployee  
SET EndDate = '2010-12-31', CurrentFlag='False';  
```  
  
 В инструкции UPDATE можно также использовать вычисляемые значения. В следующем примере удваивается значение столбца `ListPrice` для всех строк в таблице `Product`.  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimEmployee  
SET BaseRate = BaseRate * 2;  
```  
  
### <a name="ae-using-the-update-statement-with-a-where-clause"></a>АД. Использование инструкции UPDATE с предложением WHERE  
 В следующем примере предложение WHERE используется для указания строк, которые необходимо обновить.  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimEmployee  
SET FirstName = 'Gail'  
WHERE EmployeeKey = 500;  
```  
  
### <a name="af-using-the-update-statement-with-label"></a>АЕ. Использование инструкции UPDATE с меткой  
 В следующем примере показано использование LABEL с инструкцией UPDATE.  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimProduct  
SET ProductSubcategoryKey = 2   
WHERE ProductKey = 313  
OPTION (LABEL = N'label1');  
```  
  
### <a name="ag-using-the-update-statement-with-information-from-another-table"></a>АЖ. Использование инструкции UPDATE с данными из другой таблицы  
 В этом примере создается таблица для хранения итогов продаж по годам. Обновляются итоги продаж за 2004 год с помощью инструкции SELECT в таблице FactInternetSales.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE TABLE YearlyTotalSales (  
    YearlySalesAmount MONEY NOT NULL,  
    Year SMALLINT NOT NULL )  
WITH ( DISTRIBUTION = REPLICATE );  
  
INSERT INTO YearlyTotalSales VALUES (0, 2004);  
INSERT INTO YearlyTotalSales VALUES (0, 2005);  
INSERT INTO YearlyTotalSales VALUES (0, 2006);  
  
UPDATE YearlyTotalSales  
SET YearlySalesAmount=  
(SELECT SUM(SalesAmount) FROM FactInternetSales WHERE OrderDateKey >=20040000 AND OrderDateKey < 20050000)  
WHERE Year=2004;  
  
SELECT * FROM YearlyTotalSales;   
```  

### <a name="ah-ansi-join-for-update-statements"></a>АЗ. Соединение ANSI для инструкций обновления
В этом примере показано, как обновить данные на основе результатов соединения с другой таблицей.

```sql
CREATE TABLE dbo.Table1   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
CREATE TABLE dbo.Table2   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  
UPDATE dbo.Table2   
SET dbo.Table2.ColB = dbo.Table2.ColB + dbo.Table1.ColB  
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA);  
GO  
SELECT ColA, ColB   
FROM dbo.Table2;
GO
```

## <a name="see-also"></a>См. также:  
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [Курсоры (Transact-SQL)](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [Функции для работы с изображениями и текстом (Transact-SQL)](../functions/text-and-image-functions-textptr-transact-sql.md)   
 [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md)   
 [FILESTREAM (SQL Server)](../../relational-databases/blob/filestream-sql-server.md)  
 [Поддержка параметров сортировки и Юникода](../../relational-databases/collations/collation-and-unicode-support.md)    
 [Однобайтовые и многобайтовые кодировки](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)  

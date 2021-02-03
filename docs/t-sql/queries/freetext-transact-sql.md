---
description: FREETEXT (Transact-SQL)
title: FREETEXT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FREETEXT
- FREETEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], meaning matches
- meaning matches [full-text search]
- FREETEXT predicate (Transact-SQL)
- words in predicate [full-text search]
- column searches [full-text search]
ms.assetid: 2f199d3c-440e-4bcf-bdb5-82bb3994005d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 927f11ff77a50a3a7b7a04983fa197510e88232f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207746"
---
# <a name="freetext-transact-sql"></a>FREETEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Предикат, используемый в [!INCLUDE[tsql](../../includes/tsql-md.md)] в [предложении WHERE](../../t-sql/queries/where-transact-sql.md) инструкции SELECT [!INCLUDE[tsql](../../includes/tsql-md.md)] для выполнения полнотекстового поиска по столбцам полнотекстового индекса, содержащим символьные типы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Этот предикат выполняет поиск значений, которые соответствуют условию поиска по смыслу, а не написанию. Когда используется предикат FREETEXT, ядро полнотекстовых запросов автоматически выполняет описанные далее действия над строкой *freetext_string*, присваивает вес каждому терму, а затем ищет совпадения.  
  
-   Разбивает строку на отдельные слова согласно границам слов (пословное разбиение).  
  
-   Формирует словоформы (а также производит выделение основы слова).  
  
-   Определяет список расширений или замен для термов на основании совпадений в тезаурусе.  
  
> [!NOTE]  
>  Сведения о формах и полнотекстовом поиске, поддерживаемых в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в разделе [Запрос с полнотекстовым поиском](../../relational-databases/search/query-with-full-text-search.md).  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] до [текущей версии](/troubleshoot/sql/general/determine-version-edition-update-level)).
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
FREETEXT ( { column_name | (column_list) | * }   
          , 'freetext_string' [ , LANGUAGE language_term ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *column_name*  
 Имя одного или нескольких столбцов с полнотекстовой индексацией в таблице, указанной в предложении FROM. Столбцы должны иметь тип **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** или **varbinary(max)** .  
  
 *column_list*  
 Указывает, что можно задать несколько столбцов, разделенных запятыми. *column_list* должен быть заключен в скобки. Если задан аргумент *language_term*, то у всех столбцов в *column_list* должен быть одинаковый язык.  
  
 \*  
 Указывает, что все столбцы, которые были зарегистрированы для полнотекстового поиска, должны быть использованы для поиска строки, заданной аргументом *freetext_string*. Если в предложении FROM указано более одной таблицы, символ \*следует заменить на имя таблицы. Если не определен аргумент *language_term*, язык для всех столбцов таблицы должен быть одинаковым.  
  
 *freetext_string*  
 Искомый текст в столбце, определенном аргументом *column_name*. Допустимо вводить любой текст, включая слова, фразы или предложения. Соответствия формируются, если любой ключ или его формы найдены в полнотекстовом индексе.  
  
 В отличие от условий поиска CONTAINS и CONTAINSTABLE, где слово AND является ключевым, при использовании в параметре *freetext_string* слово "and" считается пропускаемым (или [стоп-словом](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)) и не обрабатывается.  
  
 Использование функций WEIGHT, FORMSOF, символов шаблонов, NEAR, а также других синтаксических элементов запрещено. *freetext_string* разбивается на слова, проходит процедуру вычленения корней и пропускается через тезаурус.  
  
 *freetext_string* имеет тип **nvarchar**. Если в качестве входных данных используется другой тип символьных данных, производится неявное преобразование. Типы данных больших значений nvarchar(max) и varchar(max) использовать нельзя. В следующем примере переменная `@SearchWord`, тип которой определен как `varchar(30)`, вызывает неявное преобразование в предикате `FREETEXT`.  
  
```sql
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord ='performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```  
  
 Так как пробное сохранение параметров не работает с преобразованием, для улучшения производительности следует использовать тип **nvarchar**. В данном примере следует объявить переменную `@SearchWord` с типом `nvarchar(30)`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```  
  
 Указанием запроса OPTIMIZE FOR можно также воспользоваться в случаях, когда формируется неоптимальный план.  
  
 LANGUAGE *language_term*  
 Язык, ресурсы которого будут использованы для разбиения по словам, выделения корня, проверки по тезаурусу и удаления стоп-слов в составе запроса. Этот аргумент не является обязательным и может быть строкой, целым числом или шестнадцатеричным значением, соответствующим идентификатору локали (LCID). Если аргумент *language_term* задан, то соответствующий язык будет применяться ко всем элементам условия поиска. Если значение не указано, то используется язык полнотекстового поиска, заданный для столбца.  
  
 Если в одном столбце хранятся документы на различных языках в виде больших двоичных объектов, то идентификатор локали заданного документа определяет, какой язык должен использоваться для индексирования его содержимого. Указание аргумента LANGUAGE *language_term* при запросе к такому столбцу может повысить вероятность хорошего соответствия.  
  
 Если аргумент *language_term* указан как строка, он соответствует значению столбца **alias** в представлении совместимости [sys.languages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) (Transact-SQ).  Строка должна быть заключена в одиночные кавычки: '*language_term*'. Если значением аргумента *language_term* является целое число, оно представляет собой действительный код языка. Если значение *language_term* задано в шестнадцатеричной форме, то после символов "0x" должна следовать шестнадцатеричная запись кода языка. Шестнадцатеричное значение не может иметь более восьми знаков, включая начальные нули.  
  
 Если значение указано в двухбайтовой кодировке (DBCS), [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] преобразует его в Юникод.  
  
 Если указанный язык является недопустимым или связанные с ним ресурсы не установлены, то [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает сообщение об ошибке. Для использования нейтральных языковых ресурсов следует указать 0x0 в качестве значения аргумента *language_term*.  
  
## <a name="general-remarks"></a>Общие замечания  
 Полнотекстовые предикаты и функции работают в одной таблице, что следует из наличия предиката FROM. Для поиска в нескольких таблицах используйте в предложении FROM соединенную таблицу, чтобы выполнять поиск в результирующем наборе, который получен в результате соединения нескольких таблиц.  
  
Полнотекстовые запросы с использованием FREETEXT являются менее точными, нежели полнотекстовые запросы с использованием CONTAINS. Средство [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] полнотекстового поиска идентифицирует важные слова и фразы. Ни одному из зарезервированных ключевых слов или символов-шаблонов не придается специального смысла, который они обычно имеют при использовании в параметре \<contains_search_condition> предиката CONTAINS.
  
 Полнотекстовые предикаты не допускаются в [предложении OUTPUT](../../t-sql/queries/output-clause-transact-sql.md), если уровень совместимости базы данных установлен в значение 100.  
  
> [!NOTE]  
>  Функция FREETEXTTABLE полезна для поиска совпадений того же типа, что и предикат FREETEXT. На эту функцию можно ссылаться как на обычное имя таблицы в [предложении FROM](../../t-sql/queries/from-transact-sql.md) инструкции SELECT. Дополнительные сведения см. в разделе [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql.md).  
  
## <a name="querying-remote-servers"></a>Запрос к удаленному серверу  
 Четырехкомпонентное имя может использоваться в предикате [CONTAINS](../../t-sql/queries/contains-transact-sql.md) или FREETEXT для запроса по столбцам полнотекстового индекса целевых таблиц на связанном сервере. Чтобы подготовить удаленный сервер к приему полнотекстовых запросов, сначала необходимо создать полнотекстовые индексы для целевых таблиц и столбцов на удаленном сервере, а затем добавить удаленный сервер в качестве связанного сервера.  
  
## <a name="comparison-of-like-to-full-text-search"></a>Сравнение предиката LIKE с полнотекстовым поиском  
 В отличие от полнотекстового поиска предикат [LIKE](../../t-sql/language-elements/like-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] работает только с комбинациями символов. Кроме того, предикат LIKE нельзя использовать в запросах к форматированным двоичным данным. Более того, запрос с предикатом LIKE к большому количеству неструктурированных текстовых данных выполняется гораздо медленнее, чем эквивалентный полнотекстовый запрос к тем же данным. Выполнение запроса LIKE к нескольким миллионам строк текстовых данных может занять несколько минут, в то время как полнотекстовый запрос к тем же данным занимает всего несколько секунд или даже меньше, в зависимости от количества возвращаемых строк.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-freetext-to-search-for-words-containing-specified-character-values"></a>A. Использование инструкции FREETEXT для поиска слов, содержащих определенные значения символов  
 Следующий пример просматривает все документы, содержащие слова, которые связаны со словами «vital», «safety», «components».  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Title  
FROM Production.Document  
WHERE FREETEXT (Document, 'vital safety components' );  
GO  
```  
  
### <a name="b-using-freetext-with-variables"></a>Б. Использование FREETEXT с переменными  
 Нижеприведенные примеры используют переменную вместо конкретного термина для поиска.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30);  
SET @SearchWord = N'high-performance';  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Приступая к работе с компонентом Full-Text Search](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Создание полнотекстовых каталогов и управление ими](../../relational-databases/search/create-and-manage-full-text-catalogs.md)   
 [CREATE FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [Создание и управление полнотекстовыми индексами](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [Запрос с полнотекстовым поиском](../../relational-databases/search/query-with-full-text-search.md)   
 [Создание запросов полнотекстового поиска (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [CONTAINS (Transact-SQL)](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE (Transact-SQL)](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
  

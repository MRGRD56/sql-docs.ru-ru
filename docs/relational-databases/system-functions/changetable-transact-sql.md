---
description: CHANGETABLE (Transact-SQL)
title: CHANGETABLE (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 02/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- CHANGETABLE_TSQL
- CHANGETABLE
dev_langs:
- TSQL
helpviewer_keywords:
- CHANGETABLE
- change tracking [SQL Server], CHANGETABLE
ms.assetid: d405fb8d-3b02-4327-8d45-f643df7f501a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 42c0327cd409f4a9649f147e1be6536de85ecaa0
ms.sourcegitcommit: 8050df4db7a3a76e4fa03e5c79dcb49031defed7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2021
ms.locfileid: "107210944"
---
# <a name="changetable-transact-sql"></a>CHANGETABLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает информацию отслеживания изменений для таблицы. Можно использовать эту инструкцию для возврата всех изменений таблицы или информации отслеживания изменений для конкретной строки.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CHANGETABLE (  
    { CHANGES <table_name> , <last_sync_version> 
    | VERSION <table_name> , <primary_key_values> } 
    , [ FORCESEEK ] 
    )  
[AS] <table_alias> [ ( <column_alias> [ ,...n ] )  
  
<primary_key_values> ::=  
( <column_name> [ , ...n ] ) , ( <value> [ , ...n ] )  
```  
  
## <a name="arguments"></a>Аргументы  
 ИЗМЕНЕНИЯ *table_name* , *last_sync_version*  
 Возвращает сведения об отслеживании для всех изменений в таблице, произошедших с версии, указанной в *last_sync_version*.  
  
 *table_name*  
 Пользовательская таблица, в которой регистрируются отслеживаемые изменения. Отслеживание изменений необходимо включить в таблице. Может использоваться имя таблицы, состоящее из одной, двух, трех или четырех частей. Имя таблицы может быть синонимом таблицы.  
  
 *last_sync_version*  
 Скалярное значение типа **bigint** , допускающее значение null. [Выражение](../../t-sql/language-elements/expressions-transact-sql.md) приведет к синтаксической ошибке. При значении NULL возвращаются все отслеживаемые изменения.
При получении изменений вызывающее приложение должно указать точку, с которой необходимы эти изменения. *Last_sync_version* указывает эту точку. Функция возвращает данные обо всех строках, изменившихся начиная с этой версии. Приложение получает запросы на получение изменений с версией выше *last_sync_version*. Как правило, перед получением изменений приложение вызывает метод `CHANGE_TRACKING_CURRENT_VERSION()` , чтобы получить версию, которая будет использоваться в следующий раз, когда требуются изменения. Поэтому в приложении нет необходимости интерпретировать или разбирать фактическое значение. Поскольку *last_sync_version* получает вызывающее приложение, приложение должно сохранить значение. Если это значение в приложении будет утеряно, потребуется повторная инициализация данных. 
 *last_sync_version* следует проверить, чтобы убедиться, что он не устарел, так как некоторые или все сведения об изменениях могли быть очищены в соответствии с периодом хранения, настроенным для базы данных. Дополнительные сведения см. в статьях [CHANGE_TRACKING_MIN_VALID_VERSION &#40;Transact-sql&#41;](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md) и [Параметры ALTER DATABASE SET &#40;transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 ВЕРСИЯ *table_name*, { *primary_key_values* }  
 Возвращает информацию о последнем изменении указанной строки. Значения первичного ключа должны идентифицировать строку. *primary_key_values* идентифицирует столбцы первичного ключа и задает значения. Имена первичных ключевых столбцов могут быть указаны в любом порядке.  
  
 *table_name*  
 Пользовательская таблица для получения информации отслеживания изменений. Отслеживание изменений необходимо включить в таблице. Может использоваться имя таблицы, состоящее из одной, двух, трех или четырех частей. Имя таблицы может быть синонимом таблицы.  
  
 *column_name*  
 Указывает одно или несколько имен первичных ключевых столбцов. Несколько имен столбцов могут быть указаны в любом порядке.  
  
 *value*  
 Значение первичного ключа. При наличии нескольких столбцов первичного ключа значения должны быть указаны в том же порядке, в котором столбцы отображаются в списке *column_name* .  

 FORCESEEK   
 **Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Начиная с [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU16 и [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] CU9), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] и [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]    
 
 Необязательный параметр, который заставляет операцию поиска использоваться для доступа к *table_name*. В некоторых случаях, когда изменилось очень мало строк, операция просмотра может использоваться для доступа к *table_name*. Если операция просмотра создает ошибку производительности, используйте `FORCESEEK` параметр.

 ТЕХ *table_alias* [(*column_alias* [,... *n* ])]  
 Задает имена для результатов, возвращаемых функцией CHANGETABLE.  
  
 *table_alias*  
 Псевдоним таблицы, возвращаемый функцией CHANGETABLE. *table_alias* является обязательным и должен быть допустимым [идентификатором](../../relational-databases/databases/database-identifiers.md).  
  
 *column_alias*  
 Необязательный псевдоним столбца или список псевдонимов столбцов, возвращаемых функцией CHANGETABLE. Обеспечивает возможность настройки имен столбцов в случае, если в результатах присутствуют повторяющиеся имена.  

## <a name="return-types"></a>Типы возвращаемых данных  
 **table**  
  
## <a name="return-values"></a>Возвращаемые значения  
  
### <a name="changetable-changes"></a>CHANGETABLE CHANGES  
 При указании ключевого слова CHANGES возвращается ноль или несколько строк, содержащих следующие столбцы.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|SYS_CHANGE_VERSION|**bigint**|Значение версии, связанное с последним изменением в строке|  
|SYS_CHANGE_CREATION_VERSION|**bigint**|Значения версии, связанные с последней операцией вставки.|  
|SYS_CHANGE_OPERATION|**nchar (1)**|Задает тип изменения:<br /><br /> **U** = обновление<br /><br /> **I** = вставить<br /><br /> **D** = удалить|  
|SYS_CHANGE_COLUMNS|**varbinary (4100)**|Содержит список столбцов, измененных после last_sync_version (базовой версии). Обратите внимание, что вычисленные столбцы никогда не перечисляются как измененные.<br /><br /> Принимает значение NULL, если выполняется любое из следующих условий.<br /><br /> Отслеживание изменений столбцов не включено.<br /><br /> Операция представляет собой операцию вставки или удаления.<br /><br /> Все ключевые столбцы, не являющиеся первичными, были обновлены одной операцией. Это двоичное значение не следует интерпретировать непосредственно. Вместо этого, чтобы интерпретировать его, используйте [CHANGE_TRACKING_IS_COLUMN_IN_MASK ()](../../relational-databases/system-functions/change-tracking-is-column-in-mask-transact-sql.md).|  
|SYS_CHANGE_CONTEXT|**varbinary(128)**|Изменение контекстной информации, которую можно дополнительно указать с помощью предложения [with](../../relational-databases/system-functions/with-change-tracking-context-transact-sql.md) как части инструкции INSERT, UPDATE или DELETE.|  
|\<primary key column value>|Такие же, как столбцы таблицы пользователя|Значения первичного ключа для отслеживаемой таблицы. Эти значения уникально идентифицируют каждую строку в таблице пользователя.|  
  
### <a name="changetable-version"></a>CHANGETABLE VERSION  
 При указании значения VERSION возвращается одна строка, содержащая следующие столбцы.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|SYS_CHANGE_VERSION|**bigint**|Текущее значение версии изменений, связанное со строкой.<br /><br /> Принимает значение NULL, если изменение не производилось в течение периода времени, превышающего срок хранения данных отслеживания изменений, либо если строка не изменялась с момента включения отслеживания изменений.|  
|SYS_CHANGE_CONTEXT|**varbinary(128)**|Измените контекст, который указывается дополнительно с использованием предложения WITH как часть инструкции INSERT, UPDATE или DELETE.|  
|\<primary key column value>|Такие же, как столбцы таблицы пользователя|Значения первичного ключа для отслеживаемой таблицы. Эти значения уникально идентифицируют каждую строку в таблице пользователя.|  
  
## <a name="remarks"></a>Примечания  
 Функция CHANGETABLE обычно используется в предложении FROM запроса, как если бы она была таблицей.  
  
## <a name="changetablechanges"></a>CHANGETABLE(CHANGES...)  
 Чтобы получить данные для новых или измененных строк, соедините результирующий набор с пользовательской таблицей с помощью первичных ключевых столбцов. Для каждой строки в пользовательской таблице, которая была изменена, возвращается только одна строка, даже если с момента *last_sync_version* значения было внесено несколько изменений в одну строку.  
  
 Изменения первичного ключевого столбца никогда не помечаются как обновления. Если значение первичного ключа изменяется, это изменение рассматривается как удаление прежнего значения и вставка нового.  
  
 Если удалить, а затем вставить строку, содержащую тот же первичный ключ, такое изменение рассматривается как обновление для всех столбцов в строке.  
  
 Значения, возвращаемые для `SYS_CHANGE_OPERATION` столбцов и, `SYS_CHANGE_COLUMNS` относятся к указанному базовому значению (last_sync_version). Например, если операция вставки была выполнена в версии `10` и операции обновления в версии `15` , а базовая *last_sync_version* — `12` , будет сообщено об обновлении. Если *last_sync_version* значение равно `8` , будет выведено сообщение о вставке. `SYS_CHANGE_COLUMNS`Изменения в вычисляемых столбцах никогда не регистрируются в столбцах  как обновления.   
  
 В целом в пользовательских таблицах отслеживаются все операции вставки, обновления и удаления данных, включая инструкцию MERGE.  
  
 Не отслеживаются следующие операции, затрагивающие данные в пользовательских таблицах.  
  
-   Выполняя `UPDATETEXT` инструкцию. Эта инструкция устарела и в следующей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет удалена. Однако изменения, внесенные с помощью `.WRITE` предложения инструкции UPDATE, будут отслеживанием.  
  
-   Удаление строк с помощью `TRUNCATE TABLE` . При усечении таблицы данные отслеживания изменений, связанные с таблицей, будут сброшены, как будто отслеживание изменений для таблицы только что включено. Клиентское приложение должно обязательно произвести синхронизацию версий. Проверка окончится неуспешно, если таблица была усечена.  
  
## <a name="changetableversion"></a>CHANGETABLE(VERSION...)  
 Если указан несуществующий первичный ключ, возвращается пустой результирующий набор.  
  
 Значение `SYS_CHANGE_VERSION` может быть равно null, если изменение не было выполнено дольше периода хранения (например, очистка удаляет сведения об изменениях) или строка никогда не была изменена, так как для таблицы включено отслеживание изменений.  
  
## <a name="permissions"></a>Разрешения  
 `SELECT` `VIEW CHANGE TRACKING` Для получения сведений об отслеживании изменений требуется разрешение на столбцы первичного ключа и разрешение для таблицы, заданной параметром *<table_name>* .
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returning-rows-for-an-initial-synchronization-of-data"></a>A. Возврат строк для начальной синхронизации данных  
 В следующем примере показано, как получить данные для исходной синхронизации данных таблицы. Запрос возвращает все данные строк и их связанные версии. Можно затем вставить или добавить эти данные в систему, где будут содержаться синхронизированные данные.  
  
```sql  
-- Get all current rows with associated version  
SELECT e.[Emp ID], e.SSN, e.FirstName, e.LastName,  
    c.SYS_CHANGE_VERSION, c.SYS_CHANGE_CONTEXT  
FROM Employees AS e  
CROSS APPLY CHANGETABLE   
    (VERSION Employees, ([Emp ID], SSN), (e.[Emp ID], e.SSN)) AS c;  
```  
  
### <a name="b-listing-all-changes-that-were-made-since-a-specific-version"></a>Б. Список всех изменений, внесенных после определенной версии  
 В следующем примере показано, как получить список всех изменений, внесенных в таблицу после указанной версии (`@last_sync_version)`. [Emp ID] и SSN являются столбцами составного первичного ключа.  
  
```sql  
DECLARE @last_sync_version bigint;  
SET @last_sync_version = <value obtained from query>;  
SELECT [Emp ID], SSN,  
    SYS_CHANGE_VERSION, SYS_CHANGE_OPERATION,  
    SYS_CHANGE_COLUMNS, SYS_CHANGE_CONTEXT   
FROM CHANGETABLE (CHANGES Employees, @last_sync_version) AS C;  
```  
  
### <a name="c-obtaining-all-changed-data-for-a-synchronization"></a>В. Получение всех измененных данных для синхронизации  
 В следующем примере показано, как можно получить все измененные данные. Этим запросом данные отслеживания изменений объединяются с пользовательской таблицей таким образом, чтобы был выполнен возврат данных пользовательской таблицы. Ключевое слово `LEFT OUTER JOIN` используется для возврата строки для удаленных строк.  
  
```sql  
-- Get all changes (inserts, updates, deletes)  
DECLARE @last_sync_version bigint;  
SET @last_sync_version = <value obtained from query>;  
SELECT e.FirstName, e.LastName, c.[Emp ID], c.SSN,  
    c.SYS_CHANGE_VERSION, c.SYS_CHANGE_OPERATION,  
    c.SYS_CHANGE_COLUMNS, c.SYS_CHANGE_CONTEXT   
FROM CHANGETABLE (CHANGES Employees, @last_sync_version) AS c  
    LEFT OUTER JOIN Employees AS e  
        ON e.[Emp ID] = c.[Emp ID] AND e.SSN = c.SSN;  
```  
  
### <a name="d-detecting-conflicts-by-using-changetableversion"></a>Г. Выявление конфликтов с помощью инструкции CHANGETABLE(VERSION...)  
 В следующем примере показано, как выполнить обновление строки только в случае, если строка не изменялась после последней синхронизации. Номер версии конкретной строки можно получить с помощью функции `CHANGETABLE`. Если строка была обновлена, изменения не вносятся и запрос возвращает данные о самом последнем изменении, внесенном в строку.  
  
```sql  
-- @last_sync_version must be set to a valid value  
UPDATE  
    SalesLT.Product  
SET  
    ListPrice = @new_listprice  
FROM  
    SalesLT.Product AS P  
WHERE  
    ProductID = @product_id AND  
    @last_sync_version >= ISNULL (  
        (SELECT CT.SYS_CHANGE_VERSION FROM   
            CHANGETABLE(VERSION SalesLT.Product,  
            (ProductID), (P.ProductID)) AS CT),  
        0);  
```  
  
## <a name="see-also"></a>См. также  
 [Функции отслеживания изменений (Transact-SQL)](../../relational-databases/system-functions/change-tracking-functions-transact-sql.md)   
 [Отслеживание измененных данных (SQL Server)](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [CHANGE_TRACKING_IS_COLUMN_IN_MASK &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-tracking-is-column-in-mask-transact-sql.md)   
 [CHANGE_TRACKING_CURRENT_VERSION (Transact-SQL)](../../relational-databases/system-functions/change-tracking-current-version-transact-sql.md)   
 [CHANGE_TRACKING_MIN_VALID_VERSION (Transact-SQL)](../../relational-databases/system-functions/change-tracking-min-valid-version-transact-sql.md)  
  

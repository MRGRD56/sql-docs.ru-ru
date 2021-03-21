---
description: sp_tables (Transact-SQL)
title: sp_tables (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_tables
- sp_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_tables
ms.assetid: 787a2fa5-87a1-49bd-938b-6043c245f46b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fd4c53e4191fadbf95a4119875aa9d19fad4ebbc
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754644"
---
# <a name="sp_tables-transact-sql"></a>sp_tables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает список объектов, к которым можно выполнять запросы в текущем окружении. Это означает применение любой таблицы или представления, кроме объектов-синонимов.  
  
> [!NOTE]  
>  Чтобы определить имя базового объекта синонима, запросите представление каталога [sys. synonyms](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md) .  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_tables [ [ @table_name = ] 'name' ]   
     [ , [ @table_owner = ] 'owner' ]   
     [ , [ @table_qualifier = ] 'qualifier' ]   
     [ , [ @table_type = ] "type" ]   
     [ , [@fUsePattern = ] 'fUsePattern'];  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @table_name = ] 'name'` Таблица, используемая для возврата сведений о каталоге. *имя* имеет тип **nvarchar (384)** и значение по умолчанию NULL. Поиск совпадений по шаблону поддерживается.  
  
`[ @table_owner = ] 'owner'` Владелец таблицы таблицы, используемой для возврата сведений о каталоге. *owner* имеет тип **nvarchar (384)** и значение по умолчанию NULL. Поиск совпадений по шаблону поддерживается. Если владелец не указан, применяются правила видимости таблиц по умолчанию базовой СУБД.  
  
 В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если текущий пользователь является владельцем таблицы с указанным именем, возвращаются ее столбцы. Если пользователь не указан, а текущий пользователь не является владельцем таблицы с заданным именем, процедура ищет таблицу с заданным именем, которая принадлежит владельцу базы данных. Если такая таблица существует, возвращаются ее столбцы.  
  
`[ @table_qualifier = ] 'qualifier'` Имя квалификатора таблицы. *квалификатор* имеет тип **sysname** и значение по умолчанию NULL. Различные продукты СУБД поддерживают имена таблиц (_Квалификаторы_**,** состоящие из трех частей). _владелец_**.** _имя_). В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] этот столбец представляет имя базы данных. В некоторых СУБД он представляет имя сервера в среде базы данных, где находится таблица.  
  
``[ , [ @table_type = ] "'type', 'type'" ]`` Список значений, разделенных запятыми, который предоставляет сведения обо всех таблицах указанных табличных типов. К ним относятся **Таблица**, **SYSTEMTABLE** и **представление**. *Type имеет тип* **varchar (100)** и значение по умолчанию NULL.  
  
> [!NOTE]  
>  Каждый табличный тип должен быть заключен в одиночные кавычки, а весь параметр должен быть заключен в двойные кавычки. Типы таблиц должны указываться в верхнем регистре. Если настройка SET QUOTED_IDENTIFIER установлена в ON, то каждый тип таблицы должен быть заключен в двойные кавычки, а весь параметр должен быть заключен в одинарные кавычки.  
  
`[ @fUsePattern = ] 'fUsePattern'` Определяет, будут ли символы подчеркивания (_), процента (%) и квадратных скобок ([или]) интерпретироваться как подстановочные знаки. Допустимые значения: 0 (сопоставление с шаблоном отключено) и 1 (сопоставление с шаблоном включено). *фусепаттерн* имеет **бит** и значение по умолчанию 1.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 Нет  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Имя квалификатора таблицы. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] этот столбец представляет имя базы данных. Это поле может иметь значение NULL.|  
|**TABLE_OWNER**|**sysname**|Имя владельца таблицы. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] этот столбец представляет имя пользователя базы данных, создавшего таблицу. Это поле всегда возвращает значение.|  
|**TABLE_NAME**|**sysname**|Имя таблицы. Это поле всегда возвращает значение.|  
|**TABLE_TYPE**|**varchar(32)**|Таблица, системная таблица или представление.|  
|**ЗАМЕЧАНИЯ**|**varchar (254)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не возвращает значение для этого столбца.|  
  
## <a name="remarks"></a>Remarks  
 Для максимальной совместимости клиент шлюза должен принимать только сопоставление шаблонов стандарта SQL-92 (символы-шаблоны «%» и «_»).  
  
 Данные о правах доступа текущего пользователя на чтение и запись в конкретную таблицу не всегда проверяются. Поэтому доступ не гарантируется. Этот результирующий набор включает не только таблицы и представления, но и синонимы, и псевдонимы для шлюзов к СУБД, поддерживающим эти типы. Если атрибут сервера **ACCESSIBLE_TABLES** имеет значение Y в результирующем наборе для **sp_server_info**, возвращаются только таблицы, к которым текущий пользователь может получить доступ.  
  
 **sp_tables** эквивалентен **SQLTables** в ODBC. Возвращаемые результаты упорядочиваются по **TABLE_TYPE**, **TABLE_QUALIFIER**, **table_owner** и **table_name**.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение SELECT для схемы.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returning-a-list-of-objects-that-can-be-queried-in-the-current-environment"></a>A. Возврат списка объектов, к которым можно выполнять запросы в текущем окружении  
 Следующий пример возвращает список объектов, которые могут быть запросами в текущем окружении.  
  
```sql  
EXEC sp_tables ;  
```  
  
### <a name="b-returning-information-about-the-tables-in-a-specified-schema"></a>Б. Возврат сведений о таблицах в указанной схеме  
 В следующем примере возвращаются сведения о таблицах, принадлежащих схеме `Person` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_tables   
   @table_name = '%',  
   @table_owner = 'Person',  
   @table_qualifier = 'AdventureWorks2012';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-returning-a-list-of-objects-that-can-be-queried-in-the-current-environment"></a>В. Возврат списка объектов, к которым можно выполнять запросы в текущем окружении  
 Следующий пример возвращает список объектов, которые могут быть запросами в текущем окружении.  
  
```sql  
EXEC sp_tables ;  
```  
  
### <a name="d-returning-information-about-the-tables-in-a-specified-schema"></a>Г. Возврат сведений о таблицах в указанной схеме  
 В следующем примере возвращаются сведения о таблицах измерений в `AdventureWorksPDW201` базе данных.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_tables   
   @table_name = 'Dim%',  
   @table_owner = 'dbo',  
   @table_qualifier = 'AdventureWorksPDW2012';  
```  
  
## <a name="see-also"></a>См. также:  
 [sys. синонимы &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-synonyms-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  


---
description: sp_rename (Transact-SQL)
title: sp_rename (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 10/14/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_rename_TSQL
- sp_rename
dev_langs:
- TSQL
helpviewer_keywords:
- renaming indexes
- renaming columns
- sp_rename
- renaming tables
ms.assetid: bc3548f0-143f-404e-a2e9-0a15960fc8ed
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: 2e24d74b8c2d2d3e661ab5169ae800e99af42f68
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610866"
---
# <a name="sp_rename-transact-sql"></a>sp_rename (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Изменяет имя пользовательского объекта в текущей базе данных. Этот объект может быть таблицей, индексом, столбцом, псевдонимом типа данных или [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] определяемым пользователем типом среды CLR.  
  
> [!NOTE]
> В [!INCLUDE[ssazuresynapse](../../includes/ssazuresynapse_md.md)] sp_rename находится в **режиме предварительной версии** и может использоваться только для переименования столбца в объекте пользователя в схеме **dbo** .

> [!CAUTION]  
>  Изменение любой части имени объекта может разрушить скрипты и хранимые процедуры. Не рекомендуется использовать эту инструкцию для переименования хранимых процедур, триггеров, определяемых пользователем функций или представлений; следует удалить объект и создать его повторно с новым именем.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
-- Transact-SQL Syntax for sp_rename in SQL Server and Azure SQL Database
sp_rename [ @objname = ] 'object_name' , [ @newname = ] 'new_name'   
    [ , [ @objtype = ] 'object_type' ]   
```  

```sql  
-- Transact-SQL Syntax for sp_rename (preview) in Azure Synapse Analytics
sp_rename [ @objname = ] 'object_name' , [ @newname = ] 'new_name'   
    , [ @objtype = ] 'COLUMN'   
``` 
  
## <a name="arguments"></a>Аргументы  
 [ @objname =] "*object_name*"  
 Текущее полное или неполное имя пользовательского объекта или типа данных. Если переименованный объект является столбцом в таблице, *object_name* должен быть в формате *Table. Column* или *Schema. Table. Column*. Если объект для переименования является индексом, *object_name* должен быть указан в форме *Table. index* или *Schema. Table. index*. Если переименованный объект является ограничением, *object_name* должен иметь форму *Schema. Constraint*.  
  
 Кавычки необходимы, только если указан объект с полным именем. Если предоставлено полное имя таблицы, включая имя базы данных, в качестве последнего должно использоваться имя текущей базы данных. *object_name* имеет тип **nvarchar (776)** и не имеет значения по умолчанию.  
  
 [ @newname =] "*new_name*"  
 Новое имя для указанного объекта. *new_name* должно быть именем из одной части и должно соответствовать правилам для идентификаторов. Аргумент *newname* имеет тип **sysname** и не имеет значения по умолчанию.  
  
> [!NOTE]  
>  Имена триггеров не могут начинаться с символов # или ##.  
  
 [ @objtype =] "*object_type*"  
 Тип переименовываемого объекта. *object_type* имеет тип **varchar (13)**, значение по умолчанию NULL и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|COLUMN|Столбец, который будет переименован.|  
|DATABASE|Пользовательская база данных. Этот тип объекта необходим при переименовании базы данных.|  
|INDEX|Пользовательский индекс. При переименовании индекса со статистикой также автоматически переименовывается эта статистика.|  
|OBJECT|Элемент типа, записанный в [sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md). Например, значение OBJECT может быть использовано для переименования объектов с ограничениями (CHECK, FOREIGN KEY, PRIMARY/UNIQUE KEY) пользовательских таблиц и правил.|  
|STATISTICS|**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, а также [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].<br /><br /> Статистика создается явно пользователем или неявно с индексом. При переименовании статистики для индекса также автоматически переименовывается этот индекс.|  
|USERDATATYPE|[Определяемые пользователем типы данных CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) , добавленные с помощью команды [CREATE TYPE](../../t-sql/statements/create-type-transact-sql.md) или [sp_addtype](../../relational-databases/system-stored-procedures/sp-addtype-transact-sql.md).|  
  
[ @objtype =] "*Столбец*" **применяется к** Azure синапсе Analytics.  
В sp_rename (Предварительная версия) для [!INCLUDE[ssazuresynapse](../../includes/ssazuresynapse_md.md)] *столбец* является обязательным параметром, указывающим, что тип переименованного объекта является столбцом. Это **varchar (13)** без значения по умолчанию и всегда должен быть включен в инструкцию sp_rename (Предварительная версия). Столбец можно переименовать только в том случае, если он является столбцом, не являющимся распределением.

## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или ненулевое значение (неуспешное завершение)  
  
## <a name="remarks"></a>Комментарии  
**Применимо к** SQL Server (все поддерживаемые версии) и база данных SQL Azure  
 Процедура sp_rename автоматически переименовывает ассоциированный индекс каждый раз при переименовании ограничения PRIMARY KEY или UNIQUE. Если переименованный индекс привязан к ограничению PRIMARY KEY, то ограничение PRIMARY KEY также автоматически переименовывается хранимой процедурой sp_rename.  

**Применимо к** SQL Server (все поддерживаемые версии) и база данных SQL Azure  
 Процедура sp_rename может использоваться для переименования первичных и вторичных XML-индексов.  
  
**Применимо к** SQL Server (все поддерживаемые версии) и база данных SQL Azure  
 Переименование хранимой процедуры, функции, представления или триггера не приведет к изменению имени соответствующего объекта либо в столбце определения представления [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) каталога, либо при получении с помощью встроенной функции [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) . Поэтому не рекомендуется использовать процедуру sp_rename для переименования объектов этих типов. Лучше удалить и создать объект повторно с новым именем.  

**Применимо к** SQL Server (все поддерживаемые версии), база данных SQL Azure и аналитика Azure синапсе  
 Переименование такого объекта, как таблица или столбец не приводит к автоматическому переименованию ссылок на этот объект. Необходимо вручную изменить любые объекты, которые ссылаются на переименованный объект. Например, если переименован столбец таблицы и на этот столбец имеется ссылка в триггере, то необходимо изменить триггер, указав новое имя столбца. Используйте [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) , чтобы составить список зависимостей для объекта перед его переименованием.  

**Применимо к** SQL Server (все поддерживаемые версии), база данных SQL Azure и аналитика Azure синапсе  
 Изменить имя объекта или типа данных можно только в текущей базе данных. Имена большинства системных типов данных и системных объектов изменить нельзя.  
  
## <a name="permissions"></a>Разрешения  
 Для переименования объектов, столбцов и индексов требуется разрешение ALTER на объект. Для переименования определенных пользователем типов требуется разрешение CONTROL на тип. Для переименования базы данных требуется членство в предопределенных ролях сервера sysadmin или dbcreator.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-renaming-a-table"></a>A. Переименование таблицы  
 В следующем примере столбец `SalesTerritory` в таблице `SalesTerr` переименовывается в `Sales` .  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_rename 'Sales.SalesTerritory', 'SalesTerr';  
GO  
```  
  
### <a name="b-renaming-a-column"></a>Б. Переименование столбца  
 В следующем примере `TerritoryID` столбец в таблице переименовывается в `SalesTerritory` `TerrID` .  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_rename 'Sales.SalesTerritory.TerritoryID', 'TerrID', 'COLUMN';  
GO  
```  
  
### <a name="c-renaming-an-index"></a>В. Переименование индекса  
 В следующем примере индекс `IX_ProductVendor_VendorID` переименовывается в `IX_VendorID`.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_rename N'Purchasing.ProductVendor.IX_ProductVendor_VendorID', N'IX_VendorID', N'INDEX';  
GO  
```  
  
### <a name="d-renaming-an-alias-data-type"></a>Г. Переименование псевдонима типа данных  
 В следующем примере псевдоним типа данных `Phone` переименовывается в `Telephone`.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_rename N'Phone', N'Telephone', N'USERDATATYPE';  
GO  
```  
  
### <a name="e-renaming-constraints"></a>Д. Переименование ограничений  
 Следующие примеры иллюстрируют переименование ограничения PRIMARY KEY, ограничения CHECK и ограничения FOREIGN KEY. При переименовании ограничения должна быть указана схема, к которой принадлежит ограничение.  
  
```sql  
USE AdventureWorks2012;   
GO  
-- Return the current Primary Key, Foreign Key and Check constraints for the Employee table.  
SELECT name, SCHEMA_NAME(schema_id) AS schema_name, type_desc  
FROM sys.objects  
WHERE parent_object_id = (OBJECT_ID('HumanResources.Employee'))   
AND type IN ('C','F', 'PK');   
GO  
  
-- Rename the primary key constraint.  
sp_rename 'HumanResources.PK_Employee_BusinessEntityID', 'PK_EmployeeID';  
GO  
  
-- Rename a check constraint.  
sp_rename 'HumanResources.CK_Employee_BirthDate', 'CK_BirthDate';  
GO  
  
-- Rename a foreign key constraint.  
sp_rename 'HumanResources.FK_Employee_Person_BusinessEntityID', 'FK_EmployeeID';  
  
-- Return the current Primary Key, Foreign Key and Check constraints for the Employee table.  
SELECT name, SCHEMA_NAME(schema_id) AS schema_name, type_desc  
FROM sys.objects  
WHERE parent_object_id = (OBJECT_ID('HumanResources.Employee'))   
AND type IN ('C','F', 'PK');  
  
```  
  
```  
  
name                                  schema_name        type_desc  
------------------------------------- ------------------ ----------------------  
FK_Employee_Person_BusinessEntityID   HumanResources     FOREIGN_KEY_CONSTRAINT  
PK_Employee_BusinessEntityID          HumanResources     PRIMARY_KEY_CONSTRAINT  
CK_Employee_BirthDate                 HumanResources     CHECK_CONSTRAINT  
CK_Employee_MaritalStatus             HumanResources     CHECK_CONSTRAINT  
CK_Employee_HireDate                  HumanResources     CHECK_CONSTRAINT  
CK_Employee_Gender                    HumanResources     CHECK_CONSTRAINT  
CK_Employee_VacationHours             HumanResources     CHECK_CONSTRAINT  
CK_Employee_SickLeaveHours            HumanResources     CHECK_CONSTRAINT  
  
(7 row(s) affected)  
  
name                                  schema_name        type_desc  
------------------------------------- ------------------ ----------------------  
FK_Employee_ID                        HumanResources     FOREIGN_KEY_CONSTRAINT  
PK_Employee_ID                        HumanResources     PRIMARY_KEY_CONSTRAINT  
CK_BirthDate                          HumanResources     CHECK_CONSTRAINT  
CK_Employee_MaritalStatus             HumanResources     CHECK_CONSTRAINT  
CK_Employee_HireDate                  HumanResources     CHECK_CONSTRAINT  
CK_Employee_Gender                    HumanResources     CHECK_CONSTRAINT  
CK_Employee_VacationHours             HumanResources     CHECK_CONSTRAINT  
CK_Employee_SickLeaveHours            HumanResources     CHECK_CONSTRAINT  
  
(7 row(s) affected)  
  
```  
  
### <a name="f-renaming-statistics"></a>Е. Переименование статистики  
 В следующем примере создается объект статистики с именем contactMail1, а затем этот статистический параметр переименовывается в Невконтакт с помощью sp_rename. При переименовании объекта статистики его имя должно быть указано в формате schema.table.statistics_name.  
  
```sql  
CREATE STATISTICS ContactMail1  
    ON Person.Person (BusinessEntityID, EmailPromotion)  
    WITH SAMPLE 5 PERCENT;  
  
sp_rename 'Person.Person.ContactMail1', 'NewContact','Statistics';  
  
```  

## <a name="examples-sssdwfull"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)]
### <a name="g-renaming-a-column"></a>Ж. Переименование столбца  
 В следующем примере `c1` столбец в таблице переименовывается в `table1` `col1` . 

> [!NOTE]
> Эта [!INCLUDE[ssazuresynapse](../../includes/ssazuresynapse_md.md)] функция все еще находится в предварительной версии и в настоящее время доступна только для объектов в схеме **dbo** . 
  
```sql  
CREATE TABLE table1 (c1 INT, c2 INT);
EXEC sp_rename 'table1.c1', 'col1', 'COLUMN';
GO  
```  
  
## <a name="see-also"></a>См. также  
 [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Ядро СУБД хранимых процедур &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)  
  
  

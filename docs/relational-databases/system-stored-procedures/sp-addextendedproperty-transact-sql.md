---
description: sp_addextendedproperty (Transact-SQL)
title: sp_addextendedproperty (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addextendedproperty
- sp_addextendedproperty_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addextendedproperty
ms.assetid: 565483ea-875b-4133-b327-d0006d2d7b4c
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 448f2c67d7e7c09f0e2768cc0d6c359224d15b45
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182770"
---
# <a name="sp_addextendedproperty-transact-sql"></a>sp_addextendedproperty (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Добавляет к объекту базы данных новое расширенное свойство.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_addextendedproperty  
    [ @name = ] { 'property_name' }  
    [ , [ @value = ] { 'value' }   
        [ , [ @level0type = ] { 'level0_object_type' }   
          , [ @level0name = ] { 'level0_object_name' }   
                [ , [ @level1type = ] { 'level1_object_type' }   
                  , [ @level1name = ] { 'level1_object_name' }   
                        [ , [ @level2type = ] { 'level2_object_type' }   
                          , [ @level2name = ] { 'level2_object_name' }   
                        ]   
                ]  
        ]   
    ]   
[;]  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @name ] = {'*property_name*'}  
 Имя свойства, которое необходимо добавить. *property_name* имеет тип **sysname** и не может иметь значение null. Имена могут также включать пустые или не являющиеся алфавитно-цифровыми строки символов, а также двоичные значения.  
  
 [ @value =] {'*значение*'}  
 Значение, связываемое со свойством. *значение* равно **sql_variant** и значение по умолчанию NULL. Размер аргумента *value* не может превышать 7 500 байт.  
  
 [ @level0type =] {'*level0_object_type*'}  
 Тип объекта уровня 0. *level0_object_type* имеет тип **varchar (128)** и значение по умолчанию NULL.  
  
 Допустимые входные данные: ASSEMBLY, CONTRACT, EVENT NOTIFICATION, FILEGROUP, MESSAGE TYPE, PARTITION FUNCTION, PARTITION SCHEME, REMOTE SERVICE BINDING, ROUTE, SCHEMA, SERVICE, USER, TRIGGER, TYPE, PLAN GUIDE и NULL.  
  
> [!IMPORTANT]  
>  Возможность указать USER как тип уровня 0 в расширенном свойстве объекта типа уровня 1 будет исключена из будущей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Используйте вместо этого в качестве типа уровня 0 схему (SCHEMA). Например, при определении расширенного свойства таблицы задайте схему таблицы вместо имени пользователя. Возможность указывать TYPE как тип уровня 0 будет исключена в будущей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. В значении аргумента TYPE следует указывать тип SCHEMA в качестве типа уровня 0 и TYPE в качестве типа уровня 1.  
  
 [ @level0name =] {'*level0_object_name*'}  
 Имя указанного типа объекта уровня 0. Аргумент *level0_object_name* имеет тип **sysname** и значение по умолчанию NULL.  
  
 [ @level1type =] {'*level1_object_type*'}  
 Тип объекта уровня 1. *level1_object_type* имеет тип **varchar (128)** и значение по умолчанию NULL. Допустимыми входными значениями являются агрегат, значение по умолчанию, функция, логическое имя файла, процедура, очередь, правило, последовательность, синоним, таблица, TABLE_TYPE, тип, представление, коллекция схем XML и значение NULL.    
 [ @level1name =] {'*level1_object_name*'}  
 Имя указанного типа объекта уровня 1. Аргумент *level1_object_name* имеет тип **sysname** и значение по умолчанию NULL.  
  
 [ @level2type =] {'*level2_object_type*'}  
 Тип объекта уровня 2. *level2_object_type* имеет тип **varchar (128)** и значение по умолчанию NULL. Допустимые входные данные: COLUMN, CONSTRAINT, EVENT NOTIFICATION, INDEX, PARAMETER, TRIGGER и NULL.  
  
 [ @level2name =] {'*level2_object_name*'}  
 Имя указанного типа объекта уровня 2. Аргумент *level2_object_name* имеет тип **sysname** и значение по умолчанию NULL.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Замечания  
 Чтобы указывать расширенные свойства, объекты в базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] классифицируются по трем уровням: 0, 1 и 2. Уровень 0 является самым высоким и определен как «объекты, содержащиеся в области базы данных». Объекты уровня 1 содержатся в схеме и в пользовательской области, а объекты уровня 2 содержатся в объектах уровня 1. Расширенные свойства могут быть определены для объектов на любом из этих уровней.  
  
 Ссылки на объект определенного уровня должны быть уточнены именами объектов более высокого уровня, в которых они содержатся или которым они принадлежат. Например, при добавлении расширенного свойства к столбцу таблицы (уровень 2) необходимо также задать имя таблицы (уровень 1), содержащей этот столбец, а также схему (уровень 0), содержащую таблицу.  
  
 Если значения всех типов и имен объектов равны NULL, свойство считается принадлежащим собственно текущей базе данных.  
  
 Для системных объектов, объектов вне области пользовательской базы данных, а также для объектов, не перечисленных в качестве допустимых входных параметров в списке аргументов, расширенные свойства недоступны.  
  
 Расширенные свойства не разрешены в таблицах, оптимизированных для памяти.  
  
## <a name="replicating-extended-properties"></a>Репликация расширенных свойств  
 Расширенные свойства реплицируются только во время начальной синхронизации между издателем и подписчиком. При добавлении или изменении расширенного свойства после начальной синхронизации эти изменения не реплицируются. Дополнительные сведения об репликации объектов базы данных см. в разделе [Публикация данных и объектов базы данных](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
## <a name="schema-vs-user"></a>Схема и  Пользователь  
 Не рекомендуется указывать тип USER в качестве типа уровня 0 при применении расширенного свойства к объекту базы данных, так как это может вызвать неоднозначность при разрешении имен. Например, предположим, что пользователь Владимир владеет двумя схемами (Vladimir и MySchema) и в обеих этих схемах содержится таблица с именем MyTable. Если Мэри добавляет расширенное свойство в таблицу MyTable и указывает **@level0type = N'USER '**, **@level0name = Mary**, то неясно, к какой таблице применяется расширенное свойство. Чтобы обеспечить обратную совместимость, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] применит свойство к таблице, содержащейся в схеме Mary.  
  
## <a name="permissions"></a>Разрешения  
 Члены предопределенных ролей базы данных db_owner и db_ddladmin могут добавлять расширенные свойства в любой объект со следующим исключением: db_ddladmin не может добавлять свойства в саму базу данных, а также для пользователей или ролей.  
  
 Пользователи могут добавлять расширенные свойства к объектам, которыми они владеют или на которые у них есть разрешения ALTER или CONTROL.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-adding-an-extended-property-to-a-database"></a>A. Добавление расширенного свойства к базе данных  
 В следующем примере к образцу базы данных `'Caption'` добавляется свойство с именем `'AdventureWorks2012 Sample OLTP Database'` и значением `AdventureWorks2012` .  
  
```  
USE AdventureWorks2012;  
GO  
--Add a caption to the AdventureWorks2012 Database object itself.  
EXEC sp_addextendedproperty   
@name = N'Caption',   
@value = 'AdventureWorks2012 Sample OLTP Database';  
```  
  
### <a name="b-adding-an-extended-property-to-a-column-in-a-table"></a>Б. Добавление расширенного свойства к столбцу в таблице  
 В следующем примере свойство Caption добавляется к столбцу `PostalCode` в таблице `Address`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_addextendedproperty   
@name = N'Caption',   
@value = 'Postal code is a required column.',  
@level0type = N'Schema', @level0name = 'Person',  
@level1type = N'Table',  @level1name = 'Address',  
@level2type = N'Column', @level2name = 'PostalCode';  
GO  
```  
  
### <a name="c-adding-an-input-mask-property-to-a-column"></a>В. Добавление свойства маски ввода к столбцу  
 В следующем примере к столбцу`99999 or 99999-9999 or #### ###`в таблице `PostalCode` добавляется свойство маски ввода ' `Address`'.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_addextendedproperty   
@name = N'Input Mask ', @value = '99999 or 99999-9999 or #### ###',  
@level0type = N'Schema', @level0name = 'Person',  
@level1type = N'Table', @level1name = 'Address',   
@level2type = N'Column',@level2name = 'PostalCode';  
GO  
```  
  
### <a name="d-adding-an-extended-property-to-a-filegroup"></a>Г. Добавление расширенного свойства к файловой группе  
 В следующем примере расширенное свойство добавляется к файловой группе `PRIMARY` .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',   
@value = N'Primary filegroup for the AdventureWorks2012 sample database.',   
@level0type = N'FILEGROUP', @level0name = 'PRIMARY';  
GO  
```  
  
### <a name="e-adding-an-extended-property-to-a-schema"></a>Д. Добавление расширенного свойства к схеме  
 В следующем примере расширенное свойство добавляется к схеме `HumanResources` .  
  
```  
USE AdventureWorks2012;  
GO  
EXECUTE sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',  
@value = N'Contains objects related to employees and departments.',  
@level0type = N'SCHEMA',   
@level0name = 'HumanResources';  
```  
  
### <a name="f-adding-an-extended-property-to-a-table"></a>Е. Добавление расширенного свойства к таблице  
 В следующем примере расширенное свойство добавляется к таблице `Address` , относящейся к схеме `Person` .  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_DescriptionExample',   
@value = N'Street address information for customers, employees, and vendors.',   
@level0type = N'SCHEMA', @level0name = 'Person',  
@level1type = N'TABLE',  @level1name = 'Address';  
GO  
```  
  
### <a name="g-adding-an-extended-property-to-a-role"></a>Ж. Добавление расширенного свойства к роли  
 В следующем примере создается роль приложения и добавляется расширенное свойство к роли.  
  
```  
USE AdventureWorks2012;   
GO  
CREATE APPLICATION ROLE Buyers  
WITH Password = '987G^bv876sPY)Y5m23';   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'Application Role for the Purchasing Department.',  
@level0type = N'USER',  
@level0name = 'Buyers';  
```  
  
### <a name="h-adding-an-extended-property-to-a-type"></a>З. Добавление расширенного свойства к типу  
 В следующем примере расширенное свойство добавляется к типу.  
  
```  
USE AdventureWorks2012;   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'Data type (alias) to use for any column that represents an order number. For example a sales order number or purchase order number.',   
@level0type = N'SCHEMA',   
@level0name = N'dbo',   
@level1type = N'TYPE',   
@level1name = N'OrderNumber';  
```  
  
### <a name="i-adding-an-extended-property-to-a-user"></a>И. Добавление расширенного свойства к пользователю  
 В следующем примере создается пользователь и добавляется расширенное свойство к пользователю.  
  
```  
USE AdventureWorks2012;   
GO  
CREATE USER CustomApp WITHOUT LOGIN ;   
GO  
EXEC sys.sp_addextendedproperty   
@name = N'MS_Description',   
@value = N'User for an application.',   
@level0type = N'USER',   
@level0name = N'CustomApp';  
```  
  
## <a name="see-also"></a>См. также:  
 [Ядро СУБД хранимых процедур &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.fn_listextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-listextendedproperty-transact-sql.md)   
 [sp_dropextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproperty-transact-sql.md)   
 [sp_updateextendedproperty &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updateextendedproperty-transact-sql.md)  
  
  

---
title: CREATE DATABASE AUDIT SPECIFICATION
description: Создает объект спецификации аудита базы данных с помощью компонента аудита SQL Server.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 01/03/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE DATABASE AUDIT
- DATABASE_AUDIT_SPECIFICATION_TSQL
- DATABASE AUDIT SPECIFICATION
- CREATE_DATABASE_AUDIT_SPECIFICATION_TSQL
- CREATE_DATABASE_AUDIT_TSQL
- CREATE DATABASE AUDIT SPECIFICATION
dev_langs:
- TSQL
helpviewer_keywords:
- database audit specification
- CREATE DATABASE AUDIT SPECIFICATION statement
ms.assetid: 0544da48-0ca3-4a01-ba4c-940e23dc315b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: dfa2c988670df2c2b7b9176cbde4057c4b125c75
ms.sourcegitcommit: 2cc2e4e17ce88ef47cda32a60a02d929e617738e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/15/2021
ms.locfileid: "103473207"
---
# <a name="create-database-audit-specification-transact-sql"></a>CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Создает объект спецификации аудита базы данных с помощью компонента аудита [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в статье [Подсистема аудита SQL Server (ядро СУБД)](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
CREATE DATABASE AUDIT SPECIFICATION audit_specification_name  
{  
    FOR SERVER AUDIT audit_name   
        [ { ADD ( { <audit_action_specification> | audit_action_group_name } )   
      } [, ...n] ]  
    [ WITH ( STATE = { ON | OFF } ) ]  
}  
[ ; ]  
<audit_action_specification>::=  
{  
      action [ ,...n ]ON [ class :: ] securable BY principal [ ,...n ]  
}  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *audit_specification_name*  
 Имя спецификации аудита.  
  
 *audit_name*  
 Имя аудита, к которому применяется эта спецификация.  
  
 *audit_action_specification*  
 Спецификация действий, выполняемых участниками над защищаемыми объектами, которые должны быть зарегистрированы в ходе аудита.  
  
 *action*  
 Имя одного или нескольких действий уровня базы данных, подлежащих аудиту. Список действий аудита см. в разделе [Действия и группы действий подсистемы аудита SQL Server](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *audit_action_group_name*  
 Имя одной или нескольких групп действий уровня базы данных, подлежащих аудиту. Список групп действий аудита см. в разделе [Действия и группы действий подсистемы аудита SQL Server](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *class*  
 Имя класса защищаемого объекта (если применимо).  
  
 *securable*  
 Таблица, представление или другой защищаемый объект в базе данных, к которой применяется действие аудита или группа действий аудита. Дополнительные сведения см. в статье [Securables](../../relational-databases/security/securables.md).  
  
 *principal*  
 Имя участника базы данных, к которому применяется действие аудита или группа действий аудита. Для аудита всех субъектов базы данных используйте субъект `public`. Дополнительные сведения см. в разделе [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md).  
  
 WITH (STATE = {ON | OFF})  
 Включает или отключает сбор записей для этой спецификации аудита.  
  
## <a name="remarks"></a>Remarks  
 Спецификации аудита базы данных являются незащищаемыми объектами, которые находятся в определенной базе данных. После создания спецификация аудита базы данных находится в отключенном состоянии.  
  
## <a name="permissions"></a>Разрешения  
 Пользователи, имеющие разрешение `ALTER ANY DATABASE AUDIT`, могут создавать спецификации аудита базы данных и привязывать их к любому аудиту.  
  
 После создания спецификации аудита базы данных ее могут просматривать пользователи с разрешением `CONTROL SERVER` или с учетной записью `sysadmin`.  
  
## <a name="examples"></a>Примеры

### <a name="a-audit-select-and-insert-on-a-table-for-any-database-principal"></a>A. Аудит SELECT и INSERT в таблице для любого субъекта базы данных 
 В следующем примере создается аудит сервера с именем `Payrole_Security_Audit`, а затем — спецификация аудита базы данных с именем `Payrole_Security_Audit`, которая анализирует инструкции `SELECT` и `INSERT`, выполняемые любым участником с атрибутом `public` для роли базы данных в таблице `HumanResources.EmployeePayHistory` базы `AdventureWorks2012`. Это приводит к тому, что аудит выполняется для каждого пользователя, так как каждый пользователь всегда является участником с атрибутом `public` для роли.
  
```sql  
USE master ;  
GO  
-- Create the server audit.  
CREATE SERVER AUDIT Payrole_Security_Audit  
    TO FILE ( FILEPATH =   
'D:\SQLAudit\' ) ;  -- make sure this path exists
GO  
-- Enable the server audit.  
ALTER SERVER AUDIT Payrole_Security_Audit   
WITH (STATE = ON) ;  
GO  
-- Move to the target database.  
USE AdventureWorks2012 ;  
GO  
-- Create the database audit specification.  
CREATE DATABASE AUDIT SPECIFICATION Audit_Pay_Tables  
FOR SERVER AUDIT Payrole_Security_Audit  
ADD (SELECT , INSERT  
     ON HumanResources.EmployeePayHistory BY public )  
WITH (STATE = ON) ;  
GO  
``` 

### <a name="b-audit-any-dml-insert-update-or-delete-on-_all_-objects-in-the-_sales_-schema-for-a-specific-database-role"></a>Б. Аудит любой инструкции DML (INSERT, UPDATE или DELETE) для _всех_ объектов в схеме _sales_ для конкретной роли базы данных  
 В следующем примере создается аудит сервера с именем `DataModification_Security_Audit`, а затем — спецификация аудита базы данных с именем `Audit_Data_Modification_On_All_Sales_Tables`, которая анализирует инструкции `INSERT`, `UPDATE` и `DELETE`, выполняемые пользователями с новой ролью базы данных `SalesUK` для всех объектов схемы `Sales` в базе `AdventureWorks2012`.  
  
```sql  
USE master ;  
GO  
-- Create the server audit.
-- Change the path to a path that the SQLServer Service has access to. 
CREATE SERVER AUDIT DataModification_Security_Audit  
    TO FILE ( FILEPATH = 
'D:\SQLAudit\' ) ;  -- make sure this path exists
GO  
-- Enable the server audit.  
ALTER SERVER AUDIT DataModification_Security_Audit   
WITH (STATE = ON) ;  
GO  
-- Move to the target database.  
USE AdventureWorks2012 ;  
GO  
CREATE ROLE SalesUK
GO
-- Create the database audit specification.  
CREATE DATABASE AUDIT SPECIFICATION Audit_Data_Modification_On_All_Sales_Tables  
FOR SERVER AUDIT DataModification_Security_Audit  
ADD ( INSERT, UPDATE, DELETE  
     ON Schema::Sales BY SalesUK )  
WITH (STATE = ON) ;    
GO  
```  


## <a name="see-also"></a>См. также:  
 [CREATE SERVER AUDIT (Transact-SQL)](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT (Transact-SQL)](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT (Transact-SQL)](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [Создание аудита сервера и спецификации аудита сервера](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  

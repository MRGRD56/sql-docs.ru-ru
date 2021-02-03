---
description: ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)
title: ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_DATABASE_AUDIT_SPECIFICATION_TSQL
- ALTER DATABASE AUDIT SPECIFICATION
- ALTER_DATABASE_AUDIT_TSQL
- ALTER DATABASE AUDIT
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER DATABASE AUDIT SPECIFICATION statement
ms.assetid: 85f4e7e6-a330-4de0-9048-64f386ccc314
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 088c1503fd306386ca85f36be97b78dcf480e7ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195925"
---
# <a name="alter-database-audit-specification-transact-sql"></a>ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Изменяет объект спецификации аудита базы данных с помощью компонента аудита [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в статье [Подсистема аудита SQL Server (ядро СУБД)](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ALTER DATABASE AUDIT SPECIFICATION audit_specification_name  
{  
    [ FOR SERVER AUDIT audit_name ]  
    [ { { ADD | DROP } (   
           { <audit_action_specification> | audit_action_group_name }   
                )   
      } [, ...n] ]  
    [ WITH ( STATE = { ON | OFF } ) ]  
}  
[ ; ]  
<audit_action_specification>::=  
{  
      <action_specification>[ ,...n ] ON [ class :: ] securable   
     BY principal [ ,...n ]   
}  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *audit_specification_name*  
 Имя спецификации аудита.  
  
 *audit_name*  
 Имя аудита, к которому применяется эта спецификация.  
  
 *audit_action_specification*  
 Имя одного или нескольких действий уровня базы данных, доступных для аудита. Список групп действий аудита см. в разделе [Действия и группы действий подсистемы аудита SQL Server](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *audit_action_group_name*  
 Имя одной или нескольких групп действий уровня базы данных, доступных для аудита. Список групп действий аудита см. в разделе [Действия и группы действий подсистемы аудита SQL Server](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *class*  
 Имя класса защищаемого объекта (если применимо).  
  
 *securable*  
 Таблица, представление или другой защищаемый объект в базе данных, к которой применяется действие аудита или группа действий аудита. Дополнительные сведения см. в статье [Securables](../../relational-databases/security/securables.md).  
  
 *column*  
 Имя столбца защищаемого объекта (если применимо).  
  
 *principal*  
 Имя участника [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], к которому применяется действие аудита или группа действий аудита. Дополнительные сведения см. в разделе [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md).  
  
 WITH **(** STATE **=** { ON | OFF } **)**  
 Включает или отключает сбор записей для этой спецификации аудита. Изменения состояния спецификации аудита должны выполняться вне пользовательской транзакции и не могут иметь других изменений в той же инструкции, если выполняется переход от состояния ON к OFF.  
  
## <a name="remarks"></a>Remarks  
 Спецификации аудита базы данных являются незащищаемыми объектами, которые находятся в определенной базе данных. Чтобы внести изменения в спецификацию аудита базы данных, необходимо установить состояние аудита в режим OFF. Если инструкция ALTER SERVER AUDIT SPECIFICATION выполняется при включенном аудите с любым параметром (кроме STATE=OFF), будет получено сообщение об ошибке. Дополнительные сведения см. в статье [tempdb Database](../../relational-databases/databases/tempdb-database.md).  
  
## <a name="permissions"></a>Разрешения  
 Пользователи с разрешением ALTER ANY DATABASE AUDIT могут изменить спецификации аудита базы данных и привязать их к любому аудиту.  
  
 После ее создания спецификацию аудита базы данных могут просматривать участники с разрешениями CONTROL SERVER или ALTER ANY SERVER AUDIT, учетной записью sysadmin или участники, имеющие явный доступ к аудиту.  
  
## <a name="examples"></a>Примеры  
 В следующем примере изменяется спецификация аудита базы данных, называемая `HIPAA_Audit_DB_Specification`, которая выполняет аудит инструкций `SELECT` пользователя `dbo` для аудита [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], называемого `HIPAA_Audit`.  
  
```sql  
ALTER DATABASE AUDIT SPECIFICATION HIPAA_Audit_DB_Specification  
FOR SERVER AUDIT HIPAA_Audit  
    ADD (SELECT  
         ON OBJECT::dbo.Table1  
         BY dbo)  
    WITH (STATE = ON);  
GO  
```  
  
 Полный пример создания аудита см. в разделе [Аудит SQL Server (ядро СУБД)](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
## <a name="see-also"></a>См. также  
 [CREATE SERVER AUDIT (Transact-SQL)](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT (Transact-SQL)](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT (Transact-SQL)](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
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
  
  

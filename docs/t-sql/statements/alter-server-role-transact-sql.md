---
description: ALTER SERVER ROLE (Transact-SQL)
title: ALTER SERVER ROLE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_SERVER_ROLE_TSQL
- ALTER SERVER ROLE
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE, ALTER
- ALTER SERVER ROLE statement
ms.assetid: 7a4db7bb-c442-4e12-9a8a-114da5bc7710
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c79b8ad73bc939354d3b9078c9486531ec8b01ec
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126231"
---
# <a name="alter-server-role-transact-sql"></a>ALTER SERVER ROLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)]

Изменяет членство в роли сервера или изменяет имя определяемой пользователем роли сервера. Предопределенные роли сервера нельзя переименовывать.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server  
  
ALTER SERVER ROLE server_role_name   
{  
    [ ADD MEMBER server_principal ]  
  | [ DROP MEMBER server_principal ]  
  | [ WITH NAME = new_server_role_name ]  
} [ ; ]  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
ALTER SERVER ROLE  server_role_name  ADD MEMBER login;  
  
ALTER SERVER ROLE  server_role_name  DROP MEMBER login;  
```  
  
## <a name="arguments"></a>Аргументы  
*server_role_name*  
Имя роли сервера, подлежащей изменению.  
  
ADD MEMBER *server_principal*  
Добавляет указанный сервер-участник к роли сервера. Аргумент *server_principal* может быть именем входа или определяемой пользователем ролью сервера. Аргумент *server_principal* не может быть предопределенной ролью сервера, ролью базы данных или sa.  
  
DROP MEMBER *server_principal*  
Удаляет указанный сервер-участник из роли сервера. Аргумент *server_principal* может быть именем входа или определяемой пользователем ролью сервера. Аргумент *server_principal* не может быть предопределенной ролью сервера, ролью базы данных или sa.  
  
WITH NAME **=** _new_server_role_name_  
Задает новое имя определяемой пользователем роли сервера. Это имя не должно быть занято на сервере.  
  
## <a name="remarks"></a>Remarks  
Изменение имени определяемой пользователем роли сервера не изменяет идентификационный номер, владельца или разрешения роли.  
  
Для изменения членства в роли, `ALTER SERVER ROLE` replaces sp_addsrvrolemember и sp_dropsrvrolemember. Эти хранимые процедуры являются устаревшими.  
  
Для просмотра ролей сервера выполните запрос к представлениям каталога `sys.server_role_members` и `sys.server_principals`.  
  
Чтобы изменить владельца определяемой пользователем роли сервера, воспользуйтесь инструкцией [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md).  
  
## <a name="permissions"></a>Разрешения  
Требует разрешения `ALTER ANY SERVER ROLE` на сервере для изменения имени определяемой пользователем роли сервера.  
  
**Предопределенные роли сервера**  
  
Для добавления нового члена в предопределенную роль сервера пользователь должен быть членом этой предопределенной роли сервера или членом роли сервера `sysadmin`.  
  
> [!NOTE]  
>  Разрешения `CONTROL SERVER` и `ALTER ANY SERVER ROLE` недостаточны для выполнения инструкции `ALTER SERVER ROLE` с предопределенной ролью сервера, а разрешение `ALTER` не может быть предоставлено для предопределенной роли сервера.  
  
**Определяемые пользователем роли сервера**  
  
Для добавления члена в определяемую пользователем роль сервера пользователь должен быть членом предопределенной роли сервера `sysadmin` или иметь разрешение `CONTROL SERVER` или `ALTER ANY SERVER ROLE`. Либо иметь разрешение `ALTER` для этой роли.  
  
> [!NOTE]  
>  В отличие от предопределенных ролей сервера, члены определяемой пользователем роли по сути не имеют разрешения на добавление членов в эту роль.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-changing-the-name-of-a-server-role"></a>A. Изменение имени роли сервера  
В следующем примере создается роль сервера с именем `Product`, затем имя роли сервера изменяется на `Production`.  
  
```sql
CREATE SERVER ROLE Product ;  
ALTER SERVER ROLE Product WITH NAME = Production ;  
GO  
```  
  
### <a name="b-adding-a-domain-account-to-a-server-role"></a>Б. Добавление учетной записи домена к роли сервера  
В следующем примере к определяемой пользователем роли сервера с именем `Production` добавляется учетная запись домена с именем `adventure-works\roberto0`.  
  
```sql
ALTER SERVER ROLE Production ADD MEMBER [adventure-works\roberto0] ;  
```  
  
### <a name="c-adding-a-sql-server-login-to-a-server-role"></a>В. Добавление имени входа SQL Server к роли сервера  
В следующем примере к предопределенной роли сервера `diskadmin` добавляется имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`Ted`.  
  
```sql
ALTER SERVER ROLE diskadmin ADD MEMBER Ted ;  
GO  
```  
  
### <a name="d-removing-a-domain-account-from-a-server-role"></a>Г. Удаление учетной записи домена из роли сервера  
В следующем примере учетная запись домена с именем `adventure-works\roberto0` удаляется из определяемой пользователем роли сервера с именем `Production`.  
  
```sql
ALTER SERVER ROLE Production DROP MEMBER [adventure-works\roberto0] ;  
```  
  
### <a name="e-removing-a-sql-server-login-from-a-server-role"></a>Д. Удаление имени входа SQL Server из роли сервера  
В следующем примере имя входа `Ted`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] удаляется из предопределенной роли сервера `diskadmin`.  
  
```sql
ALTER SERVER ROLE Production DROP MEMBER Ted ;  
GO  
```  
  
### <a name="f-granting-a-login-the-permission-to-add-logins-to-a-user-defined-server-role"></a>Е. Предоставление имени входа разрешения на добавление имен входа к определяемой пользователем роли сервера  
В следующем примере пользователь `Ted` получает разрешение добавлять другие имена входа к определяемой пользователем роли сервера с именем `Production`.  
  
```sql
GRANT ALTER ON SERVER ROLE::Production TO Ted ;  
GO  
```  
  
### <a name="g-to-view-role-membership"></a>Ж. Просмотр членства в роли  
Чтобы просмотреть членство в роли, воспользуйтесь страницей **Роль сервера (Члены)** в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или выполните следующий запрос:  
  
```sql
SELECT SRM.role_principal_id, SP.name AS Role_Name,   
SRM.member_principal_id, SP2.name  AS Member_Name  
FROM sys.server_role_members AS SRM  
JOIN sys.server_principals AS SP  
    ON SRM.Role_principal_id = SP.principal_id  
JOIN sys.server_principals AS SP2   
    ON SRM.member_principal_id = SP2.principal_id  
ORDER BY  SP.name,  SP2.name  
```  
  
## <a name="examples-sspdw"></a>Примеры: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="h-basic-syntax"></a>З. Базовый синтаксис  
В следующем примере к роли сервера `LargeRC` добавляется имя входа `Anna`.  
  
```sql
ALTER SERVER ROLE LargeRC ADD MEMBER Anna;  
```  
  
### <a name="i-remove-a-login-from-a-resource-class"></a>И. Удаление имени входа из класса ресурсов.  
В следующем примере удаляется членство Анны из роли сервера `LargeRC`.  
  
```sql
ALTER SERVER ROLE LargeRC DROP MEMBER Anna;  
```  
  
## <a name="see-also"></a>См. также:  
[CREATE SERVER ROLE (Transact-SQL)](../../t-sql/statements/create-server-role-transact-sql.md)   
[DROP SERVER ROLE (Transact-SQL)](../../t-sql/statements/drop-server-role-transact-sql.md)   
[CREATE ROLE (Transact-SQL)](../../t-sql/statements/create-role-transact-sql.md)   
[ALTER ROLE (Transact-SQL)](../../t-sql/statements/alter-role-transact-sql.md)   
[DROP ROLE (Transact-SQL)](../../t-sql/statements/drop-role-transact-sql.md)   
[Хранимые процедуры безопасности (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
[Функции безопасности (Transact-SQL)](../../t-sql/functions/security-functions-transact-sql.md)   
[Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
[sys.server_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)   
[sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  

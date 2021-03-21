---
title: Субъекты (ядро СУБД) | Документация Майкрософт
description: Сведения о субъектах в ядре СУБД, которые являются сущностями, способными запрашивать ресурсы SQL Server. Существуют субъекты уровня SQL Server и уровня базы данных.
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.roleproperties.selectroll.f1
- sql13.swb.databaseuser.permissions.user.f1--May use common.permissions
helpviewer_keywords:
- certificates [SQL Server], principals
- roles [SQL Server], principals
- permissions [SQL Server], principals
- '##MS_SQLAuthenticatorCertificate##'
- principals [SQL Server]
- '##MS_SQLResourceSigningCertificate##'
- groups [SQL Server], principals
- '##MS_AgentSigningCertificate##'
- authentication [SQL Server], principals
- schemas [SQL Server], principals
- principals [SQL Server], about principals
- security [SQL Server], principals
- users [SQL Server], principals
- '##MS_SQLReplicationSigningCertificate##'
ms.assetid: 3f7adbf7-6e40-4396-a8ca-71cbb843b5c2
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9dc2478bee6ef6eafe5e0fe5d830325311bbdd6a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750414"
---
# <a name="principals-database-engine"></a>Субъекты (компонент Database Engine)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  *Субъекты* — это сущности, которые могут запрашивать ресурсы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Как и другие компоненты модели авторизации [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , участников можно иерархически упорядочить. Область влияния субъекта зависит от области его определения: Windows, сервер, база данных, — а также от того, неделимый это субъект или коллекция. Имя входа Windows является примером индивидуального (неделимого) субъекта, а группа Windows — коллективного. Каждый субъект имеет идентификатор безопасности (SID). Эта статья относится ко всем версиям SQL Server, но в Базе данных SQL или Azure Synapse Analytics существуют некоторые ограничения для субъектов серверного уровня. 
  
## <a name="sql-server-level-principals"></a>Субъекты уровня SQL Server:  
  
- имя входа для проверки подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)];   
- имя входа для проверки подлинности Windows для пользователя Windows;  
- имя входа для проверки подлинности Windows для группы Windows;   
- имя входа для проверки подлинности Azure Active Directory для пользователя AD;
- имя входа для проверки подлинности Azure Active Directory для группы AD.
- Роль сервера  
  
## <a name="database-level-principals"></a>Субъекты уровня базы данных
  
- Пользователь базы данных (существует 12 типов пользователей; сведения см. в статье об инструкции [CREATE USER (Transact-SQL)](../../../t-sql/statements/create-user-transact-sql.md));
- Роль базы данных
- Роль приложения
  
## <a name="sa-login"></a>Имя входа SA  
 Имя входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] `sa` является субъектом уровня сервера. По умолчанию оно создается при установке экземпляра. Начиная с версии [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]базой данных для имени входа sa по умолчанию является master. Это поведение было изменено по сравнению с предыдущими версиями [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Имя входа `sa` является участником предопределенной роли сервера `sysadmin`. `sa` имеет все разрешения на сервере и не может быть ограничено. Имя входа `sa` нельзя удалить, но его можно отключить, чтобы никто не смог его использовать.

## <a name="dbo-user-and-dbo-schema"></a>Пользователь и схема dbo

Пользователь `dbo` — это особый субъект-пользователя, содержащийся в любой базе данных. Все администраторы SQL Server, участники предопределенной роли сервера `sysadmin`, имя входа `sa` и владельцы баз данных подключаются к базам данных в качестве пользователя `dbo`. Пользователь `dbo` имеет все разрешения в базе данных и не может быть ограничен или удален. `dbo` означает владельца базы данных, но учетная запись пользователя `dbo` не совпадает с предопределенной ролью базы данных `db_owner`, а предопределенная роль базы данных `db_owner` не соответствует учетной записи пользователя, помеченной как владелец базы данных.     
Пользователь `dbo` является владельцем схемы `dbo`. Если не указана другая схема, то схема `dbo` является схемой по умолчанию для всех пользователей.  Схема `dbo` не может быть удалена.
  
## <a name="public-server-role-and-database-role"></a>Роль сервера public и роль базы данных  
Каждое имя входа принадлежит к предопределенной роли сервера `public`, а каждый пользователь базы данных является участником роли базы данных `public`. Если имени входа или пользователю не были предоставлены или запрещены особые разрешения на доступ к защищаемому объекту, то они наследуют для него разрешения роли public. Предопределенная роль сервера `public` и предопределенная роль базы данных `public` не могут быть удалены. Однако можно отменить разрешения для ролей `public`. Существует множество разрешений, назначенных ролям `public` по умолчанию. Большая часть этих разрешений необходимы для выполнения повседневных операций в базе данных (операции, которые должен выполнять каждый). Будьте внимательны при отмене разрешения для общедоступного имени входа или пользователя, так как это повлияет на все имена входа и на всех пользователей. Обычно не следует запрещать разрешения для общедоступной роли public, так как инструкция DENY переопределяет любые инструкции GRANT, которые можно выдать для пользователей. 
  
## <a name="information_schema-and-sys-users-and-schemas"></a>Пользователи и схемы INFORMATION_SCHEMA и sys 
 Каждая база данных включает в себя две сущности, которые отображены в качестве пользователей в представлениях каталога: `INFORMATION_SCHEMA` и `sys`. Они необходимы для внутреннего применения ядром СУБД. Их нельзя изменить или удалить.  
  
## <a name="certificate-based-sql-server-logins"></a>Имена входа SQL Server на основе сертификата  
 Субъекты уровня сервера, имеющие имена, заключенные в хэш-символы (##), — только для внутреннего системного пользования. Следующие участники создаются из сертификатов при установке [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и не должны удаляться.  
  
-   \##MS_SQLResourceSigningCertificate##    
-   \##MS_SQLReplicationSigningCertificate##    
-   \##MS_SQLAuthenticatorCertificate##    
-   \##MS_AgentSigningCertificate##   
-   \##MS_PolicyEventProcessingLogin##   
-   \##MS_PolicySigningCertificate##   
-   \##MS_PolicyTsqlExecutionLogin##   
 
 В учетных записях субъектов нет паролей, доступных для изменения администраторам, так как они основаны на сертификатах, выданных Майкрософт.
  
## <a name="the-guest-user"></a>Пользователь-гость  
 Каждая база данных включает в себя пользователя `guest`. Разрешения, предоставленные пользователю `guest` , наследуются пользователями, которые имеют доступ к базе данных, но не обладают учетной записью пользователя в ней. Пользователя `guest` нельзя удалить, но его можно отключить, если отменить его разрешение CONNECT. Разрешение CONNECT можно отменить, выполнив инструкцию `REVOKE CONNECT FROM GUEST;` в любой базе данных, кроме `master` или `tempdb`.  
  
  
## <a name="related-tasks"></a>Связанные задачи  
 Сведения о проектировании системы разрешений см. в статье [Getting Started with Database Engine Permissions](../../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md).  
  
 Данный раздел электронной документации по [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] содержит следующие подразделы.  
  
-   [Инструкции по управлению именами входа, пользователями и схемами](./create-a-login.md)  
  
-   [Роли уровня сервера](../../../relational-databases/security/authentication-access/server-level-roles.md)  
  
-   [Роли уровня базы данных](../../../relational-databases/security/authentication-access/database-level-roles.md)  
  
-   [Роли приложений](../../../relational-databases/security/authentication-access/application-roles.md)  
  
## <a name="see-also"></a>См. также:  
 [Обеспечение безопасности SQL Server](../../../relational-databases/security/securing-sql-server.md)   
 [sys.database_principals (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.sql_logins (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)   
 [sys.database_role_members (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [Роли уровня сервера](../../../relational-databases/security/authentication-access/server-level-roles.md)   
 [Роли уровня базы данных](../../../relational-databases/security/authentication-access/database-level-roles.md)  
  

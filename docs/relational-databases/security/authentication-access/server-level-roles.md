---
title: Роли уровня сервера | Документация Майкрософт
description: SQL Server предоставляет роли уровня сервера. Эти субъекты безопасности группируют другие субъекты для управления разрешениями на уровне сервера.
ms.custom: ''
ms.date: 05/24/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.Security.NT_AUTHORITY.SYSTEM
- sql13.Security.BUILTIN.administrators
helpviewer_keywords:
- roles [SQL Server], server-level
- principals [SQL Server], server-level
- CONTROL SERVER permission
- fixed server roles [SQL Server]
- credentials [SQL Server], roles
- sysadmin fixed server role
- server-level roles [SQL Server]
- authentication [SQL Server], roles
ms.assetid: 7adf2ad7-015d-4cbe-9e29-abaefd779008
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6b7c7e5b915e6c7ad138154e1615decead642699
ms.sourcegitcommit: 17f05be5c08cf9a503a72b739da5ad8be15baea5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105103731"
---
# <a name="server-level-roles"></a>Роли уровня сервера
[!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md](../../../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] предоставляет роли уровня сервера и помогает управлять разрешениями на сервере. Эти роли являются субъектами безопасности, группирующими других участников. Разрешения ролей уровня сервера распространяются на весь сервер. (*Роли* похожи на *группы* в операционной системе Windows.)  
  
 Предопределенные роли сервера предусмотрены для удобства и обратной совместимости. Задавайте больше специфических прав каждый раз, когда это возможно.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] предоставляет девять предопределенных ролей сервера. Разрешения, назначенные предопределенным ролям сервера (кроме роли **public**), не могут быть изменены. Начиная с [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], можно создавать пользовательские роли сервера и добавлять разрешения на уровне сервера таким пользовательским ролям.  
  
 В роли уровня сервера можно добавлять субъекты уровня сервера (имена входа, учетные записи Windows и группы Windows[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ). Каждый член предопределенной роли сервера может добавлять другие имена входа к той же роли. Члены пользовательских ролей сервера не могут добавлять других участников на уровне сервера в роль.  
> [!NOTE]
>  Разрешения уровня сервера недоступны в Базе данных SQL или Azure Synapse Analytics. Дополнительные сведения о базе данных SQL см. в статье [Предоставление доступа к базе данных и управление им](/azure/sql-database/sql-database-manage-logins).
  
## <a name="fixed-server-level-roles"></a>Предопределенные роли уровня сервера  
 В следующей таблице представлены предопределенные роли уровня сервера и их возможности.  
  
|Предопределенная роль уровня сервера|Описание|  
|------------------------------|-----------------|  
|**sysadmin**|Члены предопределенной роли сервера **sysadmin** могут выполнять любые действия на сервере.|  
|**serveradmin**|Элементы предопределенной роли сервера **serveradmin** могут изменять параметры конфигурации на уровне сервера, а также выключать сервер.|  
|**securityadmin**|Элементы предопределенной роли сервера **securityadmin** управляют именами входа и их свойствами. Это могут быть разрешения на уровне сервера `GRANT`, `DENY` и `REVOKE`. Они также могут предоставлять, запрещать и отменять разрешения на уровне базы данных (инструкции `GRANT`, `DENY` и `REVOKE`) при наличии доступа к базе данных. Кроме того, они могут сбрасывать пароли для имен входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .<br /><br /> **Важно!** Возможность предоставления доступа к компоненту [!INCLUDE[ssDE](../../../includes/ssde-md.md)] и настройки разрешений пользователей позволяет администратору безопасности назначать большинство разрешений сервера. Роль **securityadmin** должна считаться эквивалентной роли **sysadmin** .|  
|**processadmin**|Члены предопределенной роли сервера **processadmin** могут завершать процессы, работающие на экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|**setupadmin**|Члены предопределенной роли сервера **setupadmin** могут добавлять или удалять связанные серверы с помощью инструкций [!INCLUDE[tsql](../../../includes/tsql-md.md)]. (Членство в роли **sysadmin** необходимо при использовании службы [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].)|  
|**bulkadmin**|Элементы предопределенной роли сервера **bulkadmin** могут выполнять инструкцию `BULK INSERT`.|  
|**diskadmin**|Предопределенная роль сервера **diskadmin** используется для управления файлами на диске.|  
|**dbcreator**|Члены предопределенной роли сервера **dbcreator** могут создавать, изменять, удалять и восстанавливать любые базы данных.|  
|**public**|Каждое имя для входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] принадлежит к роли сервера **public**. Если для участника на уровне сервера не были предоставлены или запрещены конкретные разрешения на защищаемый объект, он наследует разрешения роли public на этот объект. Разрешения роли public следует назначать только тому объекту, который будет доступен всем пользователям. Нельзя изменить членство в роли public.<br /><br /> **Примечание**. Роль **public** реализуется не так, как другие роли. В разрешениях может быть отказано, они могут предоставляться либо отменяться для предопределенных ролей public.|  
  
> [!IMPORTANT] 
> Большинство разрешений, предоставляемых следующими ролями сервера, не применимы к Synapse SQL — **processadmin**, **serveradmin**, **setupadmin** и **diskadmin**.
  
## <a name="permissions-of-fixed-server-roles"></a>Разрешения Предопределенных Ролей Сервера  
 Каждая предопределенная роль сервера обладает определенными разрешениями, назначенными ей. На следующем рисунке показаны разрешения, назначенные ролям сервера.   
![fixed_server_role_permissions](../../../relational-databases/security/authentication-access/media/permissions-of-server-roles.png)   
  
> [!IMPORTANT]  
>  Разрешение **CONTROL SERVER** похоже на, но не идентично **sysadmin** предопределенной роли сервера. Разрешения не влекут за собой членства роли, а членства роли не предоставляют разрешений. (В частности, **CONTROL SERVER** не подразумевает членство в предопределенной роли сервера **sysadmin**.) Однако иногда возможно олицетворять между ролями и эквивалентными разрешениями. Большинство команд **DBCC** и многие системные процедуры требуют членство в **sysadmin** предопределенной роли сервера. Список из 171 системной хранимой процедуры, которым требуется членство в роли **sysadmin** , содержится в следующей записи блога Андреаса Волтера (Andreas Wolter) [Сравнение CONTROL SERVER и sysadmin/sa: разрешения, системные процедуры, DBCC, автоматическое создание схем и расширение привилегий — разъяснения](http://andreas-wolter.com/en/control-server-vs-sysadmin-sa/).  
  
## <a name="server-level-permissions"></a>Разрешение на уровне сервера  
 В пользовательские роли сервера можно добавить только разрешения уровня сервера. Для составления списка разрешений уровня сервера, выполните следующее выражение. Разрешениями уровня сервера являются:  
  
```  
SELECT * FROM sys.fn_builtin_permissions('SERVER') ORDER BY permission_name;  
```  
  
 Дополнительные сведения о разрешениях см. в разделах [Разрешения (ядро СУБД)](../../../relational-databases/security/permissions-database-engine.md) и [sys.fn_builtin_permissions (Transact-SQL)](../../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md).  
  
## <a name="working-with-server-level-roles"></a>Работа с ролями уровня сервера  
 В следующей таблице описаны команды, представления и функции, предназначенные для работы с ролями уровня сервера.  
  
|Компонент|Тип|Описание|  
|-------------|----------|-----------------|  
|[sp_helpsrvrole (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-helpsrvrole-transact-sql.md)|Метаданные|Возвращает список ролей уровня сервера.|  
|[sp_helpsrvrolemember (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-helpsrvrolemember-transact-sql.md)|Метаданные|Возвращает сведения о членах роли уровня сервера.|  
|[sp_srvrolepermission (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-srvrolepermission-transact-sql.md)|Метаданные|Отображает разрешения роли уровня сервера.|  
|[Функция IS_SRVROLEMEMBER (Transact-SQL)](../../../t-sql/functions/is-srvrolemember-transact-sql.md)|Метаданные|Указывает, является ли имя входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] членом указанной роли уровня сервера.|  
|[sys.server_role_members (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)|Метаданные|Возвращает одну строку для каждого члена каждой роли уровня сервера.|  
|[sp_addsrvrolemember (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)|Get-Help|Добавляет имя входа в качестве члена роли уровня сервера. Не рекомендуется. Используйте вместо этого [ALTER SERVER ROLE](../../../t-sql/statements/alter-server-role-transact-sql.md) .|  
|[sp_dropsrvrolemember (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-dropsrvrolemember-transact-sql.md)|Get-Help|Удаляет из роли уровня сервера имя входа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] либо пользователя или группу Windows. Не рекомендуется. Используйте вместо этого [ALTER SERVER ROLE](../../../t-sql/statements/alter-server-role-transact-sql.md) .|  
|[CREATE SERVER ROLE (Transact-SQL)](../../../t-sql/statements/create-server-role-transact-sql.md)|Get-Help|Создает определяемую пользователем роль сервера.|  
|[ALTER SERVER ROLE (Transact-SQL)](../../../t-sql/statements/alter-server-role-transact-sql.md)|Get-Help|Изменяет членство в роли сервера или изменяет имя определяемой пользователем роли сервера.|  
|[DROP SERVER ROLE (Transact-SQL)](../../../t-sql/statements/drop-server-role-transact-sql.md)|Get-Help|Удаляет определяемую пользователем роль сервера.|  
|[Функция IS_SRVROLEMEMBER (Transact-SQL)](../../../t-sql/functions/is-srvrolemember-transact-sql.md)|Компонент|Определяет членство роли сервера.|  
  
## <a name="see-also"></a>См. также:  
 [Роли уровня базы данных](../../../relational-databases/security/authentication-access/database-level-roles.md)   
 [Представления каталога безопасности (Transact-SQL)](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Функции безопасности (Transact-SQL)](../../../t-sql/functions/security-functions-transact-sql.md)   
 [Обеспечение безопасности SQL Server](../../../relational-databases/security/securing-sql-server.md)   
 [GRANT, предоставление разрешений участникам на уровне сервера (Transact-SQL)](../../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)   
 [REVOKE, отмена разрешений участника на уровне сервера (Transact-SQL)](../../../t-sql/statements/revoke-server-principal-permissions-transact-sql.md)   
 [DENY, запрет разрешения участника на уровне сервера (Transact-SQL)](../../../t-sql/statements/deny-server-principal-permissions-transact-sql.md)   
 [Создание роли сервера](../../../relational-databases/security/authentication-access/create-a-server-role.md)  
  

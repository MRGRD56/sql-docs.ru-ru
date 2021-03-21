---
description: sys.database_role_members (Transact-SQL)
title: sys.database_role_members (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 01/31/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.database_role_members_TSQL
- sys.database_role_members
- database_role_members_TSQL
- database_role_members
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_role_members catalog view
ms.assetid: ed1b019d-ca48-4db3-85df-cf6d2db591cf
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 67ad5bc894d0a9bb9b212a427503298fd29c6eaf
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752224"
---
# <a name="sysdatabase_role_members-transact-sql"></a>sys.database_role_members (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает одну строку для каждого члена каждой роли базы данных.  Пользователи базы данных, роли приложений и другие роли базы данных могут быть членами роли базы данных. Чтобы добавить членов в роль, используйте инструкцию [ALTER ROLE](../../t-sql/statements/alter-role-transact-sql.md) с `ADD MEMBER` параметром. Присоединитесь к [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) , чтобы вернуть имена `principal_id` значений.
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**role_principal_id**|**int**|Идентификатор участника базы данных для роли.|  
|**member_principal_id**|**int**|Идентификатор участника базы данных элемента.|  
  
## <a name="permissions"></a>Разрешения  
 Любой пользователь может просматривать данные о своем членстве в роли. Для просмотра членства в других ролях требуется членство в `db_securityadmin` предопределенной роли базы данных или `VIEW DEFINITION` в базе данных.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="example"></a>Пример  
 Следующий запрос возвращает элементы ролей базы данных.  
  
```  
SELECT DP1.name AS DatabaseRoleName,   
   isnull (DP2.name, 'No members') AS DatabaseUserName   
 FROM sys.database_role_members AS DRM  
 RIGHT OUTER JOIN sys.database_principals AS DP1  
   ON DRM.role_principal_id = DP1.principal_id  
 LEFT OUTER JOIN sys.database_principals AS DP2  
   ON DRM.member_principal_id = DP2.principal_id  
WHERE DP1.type = 'R'
ORDER BY DP1.name;  
```  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога безопасности (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
[ALTER ROLE (Transact-СКЛЛ)](../../t-sql/statements/alter-role-transact-sql.md)      
[sys.server_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)   
  



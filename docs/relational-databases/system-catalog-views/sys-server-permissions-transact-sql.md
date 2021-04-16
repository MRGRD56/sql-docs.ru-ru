---
description: sys.server_permissions (Transact-SQL)
title: sys.server_permissions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 09/20/2019
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.server_permissions_TSQL
- sys.server_permissions
- server_permissions
- server_permissions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_permissions catalog view
ms.assetid: 7d78bf17-6c64-4166-bd0b-9e9e20992136
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e9ef1a38eded0512041b3e7e1da81047518fd052
ms.sourcegitcommit: 233be9adaee3d19b946ce15cfcb2323e6e178170
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2021
ms.locfileid: "107561089"
---
# <a name="sysserver_permissions-transact-sql"></a>sys.server_permissions (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Возвращает по одной строке на каждое разрешение на уровне сервера.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**class**|**tinyint**|Определяет класс субъекта, в котором существует разрешение.<br /><br /> 100 = Сервер<br /><br /> 101 = Сервер-участник<br /><br /> 105 = Конечная точка<br /><br /> 108 = группа доступности|  
|**class_desc**|**nvarchar(60)**|Описание класса, на который существует разрешение. Одно из следующих значений:<br /><br /> **СЕРВЕРОМ**<br /><br /> **SERVER_PRINCIPAL**<br /><br /> **ENDPOINT**<br /><br /> **AVAILABILITY GROUP**|  
|**major_id**|**int**|Идентификатор защищаемого объекта, на который существует разрешение, интерпретируемое согласно классу объекта. Как правило, это просто идентификатор, который применяется к тому, что представляет собой этот класс. Интерпретация нестандартных значений следующая:<br /><br /> 100 = всегда 0|  
|**minor_id**|**int**|Вторичный идентификатор субъекта, в котором существует разрешение, интерпретируемое согласно классу объекта.|  
|**grantee_principal_id**|**int**|Идентификатор сервера-участника, на который предоставляются разрешения.|  
|**grantor_principal_id**|**int**|Идентификатор сервера-участника, который предоставляет эти разрешения.|  
|**type**|**char (4)**|Тип разрешения сервера. Список типов разрешений см. в следующей таблице.|  
|**permission_name**|**nvarchar(128)**|Имя разрешения.|  
|**state**|**char(1)**|Состояние разрешения:<br /><br /> D = запретить<br /><br /> R = отменить<br /><br /> G = предоставить<br /><br /> W = Разрешить с аргументом Grant|  
|**state_desc**|**nvarchar(60)**|Описание состояния разрешения:<br /><br /> DENY<br /><br /> REVOKE<br /><br /> GRANT<br /><br /> GRANT_WITH_GRANT_OPTION|  
  
|Тип разрешения|Имя разрешения|Применяется к защищаемому объекту|  
|---------------------|---------------------|--------------------------|  
|AAES|ALTER ANY EVENT SESSION|SERVER|
|ADBO|ADMINISTER BULK OPERATIONS|SERVER|  
|AL|ALTER|ENDPOINT, LOGIN|  
|ALAA|ALTER ANY SERVER AUDIT|SERVER|
|ALAG|ALTER ANY AVAILABILITY GROUP|SERVER|
|ALCD|ALTER ANY CREDENTIAL|SERVER|  
|ALCO|ALTER ANY CONNECTION|SERVER|  
|ALDB|ALTER ANY DATABASE|SERVER|  
|ALES|ALTER ANY EVENT NOTIFICATION|SERVER|  
|ALHE|ALTER ANY ENDPOINT|SERVER|  
|ALLG|ALTER ANY LOGIN|SERVER|  
|ALLS|ALTER ANY LINKED SERVER|SERVER|  
|ALRS|ALTER RESOURCES|SERVER|
|ALSR|ALTER ANY SERVER ROLE|SERVER|  
|ALSS|ALTER SERVER STATE|SERVER|  
|ALST|ALTER SETTINGS|SERVER|  
|ALTR|ALTER TRACE|SERVER|  
|AUTH|AUTHENTICATE SERVER|SERVER|
|CADB|CONNECT ANY DATABASE|SERVER|  
|CL|CONTROL|ENDPOINT, LOGIN|  
|CL|CONTROL SERVER|SERVER|  
|CO|CONNECT|ENDPOINT|  
|COSQ|CONNECT SQL|SERVER|
|CRAC|Создание группы доступности|SERVER|  
|CRDB|CREATE ANY DATABASE|SERVER|  
|CRDE|CREATE DDL EVENT NOTIFICATION|SERVER|  
|CRHE|CREATE ENDPOINT|SERVER|
|CRSR|CREATE SERVER ROLE|SERVER|  
|CRTE|CREATE TRACE EVENT NOTIFICATION|SERVER|
|IAL|IMPERSONATE ANY LOGIN|SERVER|  
|IM|IMPERSONATE|Имя_для_входа|  
|SHDN|SHUTDOWN|SERVER|
|SUS|SELECT ALL USER SECURABLES|SERVER|
|TO|TAKE OWNERSHIP|ENDPOINT|  
|VW|VIEW DEFINITION|ENDPOINT, LOGIN|  
|VWAD|VIEW ANY DEFINITION|SERVER|  
|VWDB|VIEW ANY DATABASE|SERVER|  
|VWSS|VIEW SERVER STATE|SERVER|  
|XA|EXTERNAL ACCESS|SERVER|
|XU|UNSAFE ASSEMBLY|SERVER|
  
## <a name="permissions"></a>Разрешения  
 Любой пользователь может видеть свои собственные разрешения. Для просмотра разрешений, относящихся к другим именам входа, требуется разрешение VIEW DEFINITION или ALTER ANY LOGIN либо любое разрешение на имя входа. Для просмотра определяемых пользователем ролей сервера необходимо иметь разрешение ALTER ANY SERVER ROLE или быть членом роли.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Примеры  
 Следующий запрос перечисляет разрешения, явно предоставленные или отклоненные для участников на уровне сервера.  
  
> [!IMPORTANT]  
> Разрешения предопределенных ролей сервера не отображаются в sys.server_permissions. Поэтому участники на уровне сервера могут иметь дополнительные разрешения, не перечисленные здесь.  
  
```  
SELECT pr.principal_id, pr.name, pr.type_desc,   
    pe.state_desc, pe.permission_name   
FROM sys.server_principals AS pr   
JOIN sys.server_permissions AS pe   
    ON pe.grantee_principal_id = pr.principal_id;  
```  
  
## <a name="see-also"></a>См. также  
 [Представления каталога безопасности (Transact-SQL)](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [Securables](../../relational-databases/security/securables.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Разрешения (ядро СУБД)](../../relational-databases/security/permissions-database-engine.md)   
 [Иерархия разрешений (компонент Database Engine)](../../relational-databases/security/permissions-hierarchy-database-engine.md)  

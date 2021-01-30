---
description: sysmail_add_principalprofile_sp (Transact-SQL)
title: sysmail_add_principalprofile_sp (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_add_principalprofile_sp_TSQL
- sysmail_add_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_principalprofile_sp
ms.assetid: b2a0b313-abb9-4c23-8511-db77ca8172b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0252e299f072144999e3c2db1b2404de9aa9b6a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183036"
---
# <a name="sysmail_add_principalprofile_sp-transact-sql"></a>sysmail_add_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Предоставляет разрешение на использование профиля Database Mail участнику базы данных msdb. Участник базы данных должен сопоставляться с пользователем проверки подлинности SQL Server, пользователем Windows или группой Windows.
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sysmail_add_principalprofile_sp  { [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' }  
    [ , [ @is_default ] = 'is_default' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @principal_id = ] principal_id` Идентификатор пользователя или роли базы данных в базе данных **msdb** для сопоставления. *principal_id* имеет **тип int** и значение по умолчанию NULL. Необходимо указать либо *principal_id* , либо *principal_name* . *Principal_id* **0** делает этот профиль открытым профилем, предоставляя доступ ко всем субъектам в базе данных.  
  
`[ @principal_name = ] 'principal_name'` Имя пользователя или роли базы данных в базе данных **msdb** для сопоставления. Аргумент *principal_name* имеет тип **sysname** и значение по умолчанию NULL. Необходимо указать либо *principal_id* , либо *principal_name* . *Principal_name* **"Public"** делает этот профиль открытым профилем, предоставляя доступ ко всем субъектам в базе данных.  
  
`[ @profile_id = ] profile_id` Идентификатор профиля для ассоциации. *profile_id* имеет **тип int** и значение по умолчанию NULL. Необходимо указать либо *profile_id* , либо *profile_name* .  
  
`[ @profile_name = ] 'profile_name'` Имя профиля для ассоциации. Аргумент *profile_name* имеет тип **sysname** и не имеет значения по умолчанию. Необходимо указать либо *profile_id* , либо *profile_name* .  
  
`[ @is_default = ] is_default` Указывает, является ли этот профиль профилем по умолчанию для участника. Участник должен иметь ровно один профиль по умолчанию. *is_default* имеет **бит** и не имеет значения по умолчанию.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 Чтобы сделать профиль общедоступным, укажите **\@ principal_id** **0** или **\@ principal_name** **Public**. Открытый профиль доступен для всех пользователей в базе данных **msdb** , хотя пользователи также должны быть членами **роли DatabaseMailUserRole** для выполнения **sp_send_dbmail**.  
  
 Пользователь базы данных может иметь только один профиль по умолчанию. Если **\@ is_default** имеет значение **1**, а пользователь уже связан с одним или несколькими профилями, указанный профиль становится профилем по умолчанию для пользователя. Профиль, который ранее был профилем по умолчанию, остается ассоциированным с пользователем, но уже не является профилем по умолчанию.  
  
 Если **\@ is_default** имеет значение "**0**", а другая ассоциация не существует, хранимая процедура возвращает ошибку.  
  
 Хранимая процедура **sysmail_add_principalprofile_sp** находится в базе данных **msdb** и принадлежит схеме **dbo** . Процедура должна быть выполнена с именем, сопоставленным с тремя частями, если текущей базой данных не является **msdb**.  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию разрешения EXECUTE для этой процедуры имеют члены предопределенной роли сервера **sysadmin** .  
  
## <a name="examples"></a>Примеры  
 **А. Создание ассоциации с профилем, определение профиля по умолчанию**  
  
 В следующем примере создается ассоциация между профилем `AdventureWorks Administrator Profile` и пользователем базы данных **msdb** `ApplicationUser` . Указанный профиль становится для пользователя профилем по умолчанию.  
  
```  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator Profile',  
    @is_default = 1 ;  
```  
  
 **Б. Определение открытого профиля по умолчанию**  
  
 В следующем примере профиль создается `AdventureWorks Public Profile` в качестве открытого профиля по умолчанию для пользователей в базе данных **msdb** .  
  
```  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @principal_name = 'public',  
    @profile_name = 'AdventureWorks Public Profile',  
    @is_default = 1 ;  
```  
  
## <a name="see-also"></a>См. также:  
 [Database Mail](../../relational-databases/database-mail/database-mail.md)   
 [Database Mail объекты конфигурации](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Database Mail хранимых процедур &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  

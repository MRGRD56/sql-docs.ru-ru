---
description: sp_manage_jobs_by_login (Transact-SQL)
title: sp_manage_jobs_by_login (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_manage_jobs_by_login
- sp_manage_jobs_by_login_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_manage_jobs_by_login
ms.assetid: 832ec15a-6e92-4eb5-8c4a-af4dba79fbaa
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5f367f869092bde5458c732fda3e79bc06e43cb8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185355"
---
# <a name="sp_manage_jobs_by_login-transact-sql"></a>sp_manage_jobs_by_login (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаляет или переназначает задания, принадлежащие указанному имени входа.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_manage_jobs_by_login  
     [ @action = ] 'action'  
     [, [@current_owner_login_name = ] 'current_owner_login_name']  
     [, [@new_owner_login_name = ] 'new_owner_login_name']  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @action = ] 'action'` Действие, предпринимаемое для указанного имени входа. *Action* имеет тип **varchar (10)** и не имеет значения по умолчанию. При **удалении** *действия* **sp_manage_jobs_by_login** удаляет все задания, принадлежащие *current_owner_login_name*. При **ПЕРЕназначении** *действия* всем заданиям назначаются *new_owner_login_name*.  
  
`[ @current_owner_login_name = ] 'current_owner_login_name'` Имя входа текущего владельца задания. Аргумент *current_owner_login_name* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @new_owner_login_name = ] 'new_owner_login_name'` Имя входа нового владельца задания. Используйте этот параметр, только  если действие **переназначено**. Аргумент *new_owner_login_name* имеет тип **sysname** и значение по умолчанию NULL.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="permissions"></a>Разрешения  
 Для выполнения этой хранимой процедуры пользователям должна быть предоставлена предопределенная роль сервера **sysadmin** .  
  
## <a name="examples"></a>Примеры  
 В следующем примере производится передача всех заданий от пользователя `danw` пользователю `françoisa`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_manage_jobs_by_login  
    @action = N'REASSIGN',  
    @current_owner_login_name = N'danw',  
    @new_owner_login_name = N'françoisa' ;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [sp_delete_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-job-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

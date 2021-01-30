---
description: sp_enum_sqlagent_subsystems (Transact-SQL)
title: sp_enum_sqlagent_subsystems (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_enum_sqlagent_subsystems
- sp_enum_sqlagent_subsystems_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_enum_sqlagent_subsystems
ms.assetid: 019a3c9d-bac3-495b-a70a-2c19f1d2e20e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3a65369cb7c4769d930acdaf655a006c60bdd55f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186955"
---
# <a name="sp_enum_sqlagent_subsystems-transact-sql"></a>sp_enum_sqlagent_subsystems (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает список подсистем агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_enum_sqlagent_subsystems  
```  
  
## <a name="arguments"></a>Аргументы  
 None  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**подсистемы**|**nvarchar(40)**|Имя подсистемы.|  
|**description**|**nvarchar(512)**|Описание подсистемы.|  
|**subsystem_dll**|**nvarchar (510)**|Модуль DLL, содержащий подсистемы.|  
|**agent_exe**|**nvarchar (510)**|Исполняемый модуль, который используется подсистемой.|  
|**start_entry_point**|**nvarchar(30)**|Процедура, вызываемая агентом SQL Server при пошаговом выполнении задания.|  
|**event_entry_point**|**nvarchar(30)**|Процедура, вызываемая агентом SQL Server при пошаговом выполнении задания.|  
|**stop_entry_point**|**nvarchar(30)**|Процедура, вызываемая агентом SQL Server при пошаговом выполнении задания.|  
|**max_worker_threads**|**int**|Максимальное число потоков, запускаемых агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для этой подсистемы.|  
|**subsystem_id**|**int**|Идентификатор подсистемы.|  
  
## <a name="remarks"></a>Замечания  
 Процедура перечисляет подсистемы, доступные для экземпляра.  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию эту хранимую процедуру могут выполнять только члены предопределенной роли сервера **sysadmin** . Другим пользователям должна быть предоставлена предопределенная роль базы данных **SQLAgentOperatorRole** в базе данных **msdb** .  
  
 Дополнительные сведения о **SQLAgentOperatorRole** см. в разделе [Агент SQL Server предопределенных ролей базы данных](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="see-also"></a>См. также:  
 [Реализация агент SQL Server безопасности](../../ssms/agent/implement-sql-server-agent-security.md)   
 [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
  

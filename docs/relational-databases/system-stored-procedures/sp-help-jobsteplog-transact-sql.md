---
description: sp_help_jobsteplog (Transact-SQL)
title: sp_help_jobsteplog (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_jobsteplog_TSQL
- sp_help_jobsteplog
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_jobsteplog
ms.assetid: 1a0be7b1-8f31-4b4c-aadb-586c0e00ed04
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0e5311127f4ff825f862cb3aef2317a2c53578cc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200102"
---
# <a name="sp_help_jobsteplog-transact-sql"></a>sp_help_jobsteplog (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает метаданные о конкретном [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] журнале шагов задания агента. **sp_help_jobsteplog** не возвращает фактический журнал.  

  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_help_jobsteplog { [ @job_id = ] 'job_id' | [ @job_name = ] 'job_name' }  
     [ , [ @step_id = ] step_id ]  
     [ , [ @step_name = ] 'step_name' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @job_id = ] 'job_id'` Идентификационный номер задания, для которого возвращаются сведения журнала шагов задания. *job_id* имеет **тип int** и значение по умолчанию NULL.  
  
`[ @job_name = ] 'job_name'` Имя задания. Аргумент *job_name* имеет тип **sysname** и значение по умолчанию NULL.  
  
> [!NOTE]  
>  Необходимо указать либо *job_id* , либо *job_name* , но нельзя указать оба значения.  
  
`[ @step_id = ] step_id` Идентификационный номер этапа в задании. Если не указан, включаются все этапы задания. *step_id* имеет **тип int** и значение по умолчанию NULL.  
  
`[ @step_name = ] 'step_name'` Имя шага в задании. Аргумент *step_name* имеет тип **sysname** и значение по умолчанию NULL.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**job_id**|**uniqueidentifier**|Уникальный идентификатор задания.|  
|**job_name**|**sysname**|Имя задания.|  
|**step_id**|**int**|Идентификатор для этапа задачи. Например, если шаг является первым шагом в задании, его *step_id* имеет значение 1.|  
|**step_name**|**sysname**|Имя шага задания.|  
|**step_uid**|**uniqueidentifier**|Уникальный идентификатор этапа (задается системой) задачи.|  
|**date_created**|**datetime**|Дата создания этапа.|  
|**date_modified**|**datetime**|Дата последнего изменения этапа.|  
|**log_size**|**float**|Размер журнала шага задания в мегабайтах (МБ).|  
|**Журналь**|**nvarchar(max)**|Вывод журнала шага задания.|  
  
## <a name="remarks"></a>Замечания  
 **sp_help_jobsteplog** находится в базе данных **msdb** .  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию эту хранимую процедуру могут выполнять только члены предопределенной роли сервера **sysadmin** . Другим пользователям должна быть предоставлена одна из следующих предопределенных ролей базы данных агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в базе данных **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Дополнительные сведения о разрешениях этих ролей см. в разделе [Предопределенные роли базы данных агента SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Члены **SQLAgentUserRole** могут просматривать метаданные журнала шага задания только для тех шагов заданий, которыми они владеют.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-returns-job-step-log-information-for-all-steps-in-a-specific-job"></a>A. Возвращает сведения из журнала шагов задания для всех шагов в указанной задаче  
 В следующем примере выполняется возврат сведений из журнала шагов задания с именем `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobsteplog  
    @job_name = N'Weekly Sales Data Backup' ;  
GO  
```  
  
### <a name="b-return-job-step-log-information-about-a-specific-job-step"></a>Б. Возвращает сведения из журнала шага задания для определенного шага  
 В следующем примере выполняется возврат сведений журнала шагов задания для первого этапа задачи с именем `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_jobsteplog  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1 ;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [sp_add_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_delete_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [sp_delete_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)   
 [sp_delete_jobsteplog &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobsteplog-transact-sql.md)   
 [Агент SQL Server хранимых процедур &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)  
  
  

---
description: sp_delete_jobstep (Transact-SQL)
title: sp_delete_jobstep (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_jobstep
- sp_delete_jobstep_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_jobstep
ms.assetid: 421ede8e-ad57-474a-9fb9-92f70a3e77e3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9b795b6526941518e476390dba158f56e18040da
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186335"
---
# <a name="sp_delete_jobstep-transact-sql"></a>sp_delete_jobstep (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаляет шаг задания.  
  
 
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_delete_jobstep { [ @job_id = ] job_id | [ @job_name = ] 'job_name' } ,   
     [ @step_id = ] step_id   
```  
  
## <a name="arguments"></a>Аргументы  
`[ @job_id = ] job_id` Идентификационный номер задания, из которого будет удален шаг. *job_id* имеет тип **uniqueidentifier** и значение по умолчанию NULL.  
  
`[ @job_name = ] 'job_name'` Имя задания, из которого будет удален шаг. Аргумент *job_name* имеет тип **sysname** и значение по умолчанию NULL.  
  
> **Примечание.** Необходимо указать либо *job_id* , либо *job_name* . нельзя указывать оба значения.  
  
`[ @step_id = ] step_id` Идентификационный номер удаляемого шага. *step_id* имеет **тип int** и не имеет значения по умолчанию.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="remarks"></a>Remarks  
 При удалении шага задания автоматически обновляются и другие этапы, ссылающиеся на удаляемый шаг.  
  
 Для получения дополнительных сведений о действиях, связанных с определенным заданием, выполните **sp_help_jobstep**.  
  
> **Примечание.** При вызове **sp_delete_jobstep** с нулевым значением *step_id* все шаги задания для задания удаляются.  
  
 Среда Microsoft SQL Server Management Studio предоставляет простой наглядный способ управления заданиями и рекомендуется для создания заданий и работы с ними.  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию эту хранимую процедуру могут выполнять только члены предопределенной роли сервера **sysadmin** . Другим пользователям должна быть предоставлена одна из следующих предопределенных ролей базы данных агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в базе данных **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Дополнительные сведения о разрешениях этих ролей см. в разделе [Предопределенные роли базы данных агента SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Только члены **sysadmin** могут удалять шаги задания, принадлежащие другому пользователю.  
  
## <a name="examples"></a>Примеры  
 В следующем примере производится удаление шага `1` из задания `Weekly Sales Data Backup`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_jobstep  
    @job_name = N'Weekly Sales Data Backup',  
    @step_id = 1 ;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Просмотр или изменение заданий](../../ssms/agent/view-or-modify-jobs.md)   
 [sp_add_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)   
 [sp_update_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)   
 [sp_help_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

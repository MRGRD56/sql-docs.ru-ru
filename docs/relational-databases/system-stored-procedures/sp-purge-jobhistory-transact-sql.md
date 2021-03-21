---
description: sp_purge_jobhistory (Transact-SQL)
title: sp_purge_jobhistory (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_purge_jobhistory_TSQL
- sp_purge_jobhistory
dev_langs:
- TSQL
helpviewer_keywords:
- sp_purge_jobhistory
ms.assetid: 237f9bad-636d-4262-9bfb-66c034a43e88
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 55f8c3d0bd3c5812a123ec0b3d437171fd2a3d71
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748250"
---
# <a name="sp_purge_jobhistory-transact-sql"></a>sp_purge_jobhistory (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Удаляет записи журнала для задания.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_purge_jobhistory   
   {   [ @job_name = ] 'job_name' |   
     | [ @job_id = ] job_id }  
   [ , [ @oldest_date = ] oldest_date ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @job_name = ] 'job_name'` Имя задания, для которого удаляются записи журнала. Аргумент *job_name* имеет тип **sysname** и значение по умолчанию NULL. Необходимо указать либо *job_id* , либо *job_name* , но нельзя указать оба значения.  
  
> [!NOTE]  
>  Члены предопределенной роли сервера **sysadmin** или члены предопределенной роли базы данных **SQLAgentOperatorRole** могут выполнять **sp_purge_jobhistory** без указания *job_name* или *job_id*. Если пользователи **sysadmin** не указали эти аргументы, журнал заданий для всех локальных и многосерверных заданий удаляется в течение времени, указанного в *oldest_date*. Если пользователи **SQLAgentOperatorRole** не указывают эти аргументы, журнал заданий для всех локальных заданий удаляется в течение времени, заданного *oldest_date*.  
  
`[ @job_id = ] job_id` Идентификационный номер задания для удаляемых записей. *job_id* имеет тип **uniqueidentifier** и значение по умолчанию NULL. Необходимо указать либо *job_id* , либо *job_name* , но нельзя указать оба значения. Сведения о том, как пользователи **sysadmin** и **SQLAgentOperatorRole** могут использовать этот аргумент, см. в примечании в описании **\@ job_name** .  
  
`[ @oldest_date = ] oldest_date` Самая старая запись, сохраняемая в журнале. *oldest_date* имеет тип **DateTime** и значение по умолчанию NULL. Если указано *oldest_date* , **sp_purge_jobhistory** удаляет только записи, которые старше указанного значения.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="remarks"></a>Remarks  
 После успешного завершения **sp_purge_jobhistory** возвращается сообщение.  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию эту хранимую процедуру могут выполнять только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **SQLAgentOperatorRole** . Члены **роли sysadmin** могут очищать журнал заданий для всех локальных и многосерверных заданий. Члены **SQLAgentOperatorRole** могут очищать журнал заданий только для всех локальных заданий.  
  
 Другим пользователям, включая члены **SQLAgentUserRole** и Members **SQLAgentReaderRole**, необходимо явно предоставить разрешение EXECUTE на **sp_purge_jobhistory**. После предоставления разрешения EXECUTE на эту хранимую процедуру данные пользователи могут удалять из журнала заданий только те задания, владельцами которых они являются.  
  
 Предопределенные роли базы данных **SQLAgentUserRole**, **SQLAgentReaderRole** и **SQLAgentOperatorRole** находятся в базе данных **msdb** . Дополнительные сведения о разрешениях см. в разделе агент SQL Server предопределенные [роли базы данных](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-remove-history-for-a-specific-job"></a>A. Удаление журнала конкретного задания  
 В следующем примере удаляется история задания с именем `NightlyBackups`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_purge_jobhistory  
    @job_name = N'NightlyBackups' ;  
GO  
```  
  
### <a name="b-remove-history-for-all-jobs"></a>Б. Удаление записи журналов для всех заданий  
  
> [!NOTE]  
>  Только члены предопределенной роли сервера **sysadmin** и члены **SQLAgentOperatorRole** могут удалять журналы для всех заданий. Когда пользователь **sysadmin** выполняет эту хранимую процедуру без параметров, журнал заданий для всех локальных и многосерверных заданий очищается. Когда **SQLAgentOperatorRole** пользователи выполняют эту хранимую процедуру без параметров, очищаются только журнал заданий для всех локальных заданий.  
  
 В следующем примере данная процедура выполняется без указания параметров, и в результате удаляются все записи журнала.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_purge_jobhistory ;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [sp_help_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)   
 [sp_help_jobhistory &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobhistory-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [GRANT, предоставление разрешений на объект (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)  
  
  

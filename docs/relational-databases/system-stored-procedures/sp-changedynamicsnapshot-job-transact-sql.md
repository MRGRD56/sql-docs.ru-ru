---
description: sp_changedynamicsnapshot_job (Transact-SQL)
title: sp_changedynamicsnapshot_job (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changedynamicsnapshot_job
- sp_changedynamicsnapshot_job_TSQL
helpviewer_keywords:
- sp_changedynamicsnapshot_job
ms.assetid: ea0dacd2-a5fd-42f4-88dd-7d289b0ae017
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4583e692a8f0f409b12129a19f9acce723e320ce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207988"
---
# <a name="sp_changedynamicsnapshot_job-transact-sql"></a>sp_changedynamicsnapshot_job (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Модифицирует задание агента, формирующее моментальный снимок для подписки на публикацию с параметризованным фильтром строк. Эта хранимая процедура выполняется на издателе в базе данных публикации.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_changedynamicsnapshot_job [ @publication = ] 'publication'  
    [ , [ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname' ]  
    [ , [ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid' ]  
    [ , [ @frequency_type = ] frequency_type ]   
    [ , [ @frequency_interval = ] frequency_interval ]   
    [ , [ @frequency_subday = ] frequency_subday ]   
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]   
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]   
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]   
    [ , [ @active_start_date = ] active_start_date ]   
    [ , [ @active_end_date = ] active_end_date ]   
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]   
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]   
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publication = ] 'publication'` Имя публикации. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @dynamic_snapshot_jobname = ] 'dynamic_snapshot_jobname'` Имя изменяемого задания моментального снимка. *dynamic_snapshot_jobname* имеет тип **sysname** и значение по умолчанию N "%". Если указан параметр *dynamic_snapshot_jobid* , необходимо использовать значение по умолчанию для *dynamic_snapshot_jobname*.  
  
`[ @dynamic_snapshot_jobid = ] 'dynamic_snapshot_jobid'` Идентификатор изменяемого задания моментального снимка. *dynamic_snapshot_jobid* имеет тип **uniqueidentifier** и значение по умолчанию NULL. Если указан параметр *dynamic_snapshot_jobname*, необходимо использовать значение по умолчанию для *dynamic_snapshot_jobid*.  
  
`[ @frequency_type = ] frequency_type` Частота, с которой будет планироваться агент. *frequency_type* имеет **тип int** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**1**|Один раз.|  
|**2**|По запросу|  
|**4**|Ежедневно|  
|**8**|Еженедельно|  
|**16**|Ежемесячно|  
|**32**|Ежемесячно с относительной датой|  
|**64**|Автозапуск|  
|**128**|Повторяющееся задание|  
|NULL (по умолчанию)||  
  
`[ @frequency_interval = ] frequency_interval` Дни, в которые запускается агент. *frequency_interval* имеет **тип int** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**1**|Воскресенье|  
|**2**|Понедельник|  
|**3**|Вторник|  
|**4**|Среда|  
|**5**|Четверг|  
|**6**|Пятница|  
|**7**|Суббота|  
|**8**|День|  
|**9**|По рабочим дням|  
|**10**|По выходным дням|  
|NULL (по умолчанию)||  
  
`[ @frequency_subday = ] frequency_subday` Частота повторного планирования в течение заданного периода. *frequency_subday* имеет **тип int** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**1**|Однократно|  
|**2**|Секунда|  
|**4**|Минута|  
|**8**|Час|  
|NULL (по умолчанию)||  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Интервал для *frequency_subday*. *frequency_subday_interval* имеет **тип int** и значение по умолчанию NULL.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Дата выполнения агент слияния. Этот параметр используется, если *frequency_type* установлен в значение **32** (ежемесячное относительное расписание). *frequency_relative_interval* имеет **тип int** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**1**|First|  
|**2**|Секунда|  
|**4**|Третье|  
|**8**|Четвертая|  
|**16**|Последний|  
|NULL (по умолчанию)||  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Коэффициент повторения, используемый *frequency_type*. *frequency_recurrence_factor* имеет **тип int** и значение по умолчанию NULL.  
  
`[ @active_start_date = ] active_start_date` Дата первого запланированного запуска агент слияния в формате ГГГГММДД. *active_start_date* имеет **тип int** и значение по умолчанию NULL.  
  
`[ @active_end_date = ] active_end_date` Дата прекращения расписания агент слияния в формате ГГГГММДД. *active_end_date* имеет **тип int** и значение по умолчанию NULL.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Время первого запланированного агент слияния в формате ЧЧММСС. *active_start_time_of_day* имеет **тип int** и значение по умолчанию NULL.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Время суток, когда запланировать агент слияния прекращается в формате ЧЧММСС. *active_end_time_of_day* имеет **тип int** и значение по умолчанию NULL.  
  
`[ @job_login = ] 'job_login'`[!INCLUDE[msCoName](../../includes/msconame-md.md)]Учетная запись Windows, под которой агент моментальных снимков выполняется при создании моментального снимка для подписки с помощью параметризованного фильтра строк. *job_login* имеет тип **nvarchar (257)** и значение по умолчанию NULL.  
  
`[ @job_password = ] 'job_password'` Пароль для учетной записи Windows, от имени которой выполняется агент моментальных снимков при создании моментального снимка для подписки с помощью параметризованного фильтра строк. *job_password* имеет тип **nvarchar (257)** и значение по умолчанию NULL.  
  
> [!IMPORTANT]  
>  По возможности предлагайте пользователям вводить учетные данные системы безопасности во время выполнения приложения. В случае необходимости хранения учетных данных в файле скрипта этот файл следует защищать во избежание несанкционированного доступа.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_changedynamicsnapshot_job** используется в репликации слиянием для публикаций с параметризованными фильтрами строк.  
  
 После изменения имени входа и пароля агента необходимо остановить и повторно запустить агент, чтобы изменения вступили в силу.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_changedynamicsnapshot_job**.  
  
## <a name="see-also"></a>См. также:  
 [Просмотр и изменение параметров безопасности репликации](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md)   
 [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)  
  
  

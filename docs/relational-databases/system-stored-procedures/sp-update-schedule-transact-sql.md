---
description: sp_update_schedule (Transact-SQL)
title: sp_update_schedule (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_schedule
- sp_update_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_schedule
ms.assetid: 97b3119b-e43e-447a-bbfb-0b5499e2fefe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 69664cef88dcc41179c947a4725d4a44e196bb86
ms.sourcegitcommit: 52dd1719d7b63581b1d34b755bf9d077c0fc6c44
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "107373023"
---
# <a name="sp_update_schedule-transact-sql"></a>sp_update_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Изменяет установки для расписания агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_update_schedule   
    {   [ @schedule_id = ] schedule_id   
      | [ @name = ] 'schedule_name' }  
    [ , [ @new_name = ] new_name ]  
    [ , [ @enabled = ] enabled ]  
    [ , [ @freq_type = ] freq_type ]  
    [ , [ @freq_interval = ] freq_interval ]   
    [ , [ @freq_subday_type = ] freq_subday_type ]   
    [ , [ @freq_subday_interval = ] freq_subday_interval ]   
    [ , [ @freq_relative_interval = ] freq_relative_interval ]   
    [ , [ @freq_recurrence_factor = ] freq_recurrence_factor ]   
    [ , [ @active_start_date = ] active_start_date ]   
    [ , [ @active_end_date = ] active_end_date ]   
    [ , [ @active_start_time = ] active_start_time ]   
    [ , [ @active_end_time = ] active_end_time ]   
    [ , [ @owner_login_name = ] 'owner_login_name' ]  
    [ , [ @automatic_post =] automatic_post ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @schedule_id = ] schedule_id` Идентификатор изменяемого расписания. *schedule_id* имеет **тип int** и не имеет значения по умолчанию. Необходимо указать либо *schedule_id* , либо *schedule_name* .  
  
`[ @name = ] 'schedule_name'` Имя изменяемого расписания. Аргумент *schedule_name* имеет тип **sysname** и не имеет значения по умолчанию. Необходимо указать либо *schedule_id* , либо *schedule_name* .  
  
`[ @new_name = ] new_name` Новое имя расписания. Аргумент *new_name* имеет тип **sysname** и значение по умолчанию NULL. Если *new_name* имеет значение null, имя расписания не изменяется.  
  
`[ @enabled = ] enabled` Указывает текущее состояние расписания. *Enabled* имеет тип **tinyint** и значение по умолчанию **1** (включено). Если значение **равно 0**, расписание не включено. Когда расписание не включено, никакие задания не будут запускаться по расписанию.  
  
`[ @freq_type = ] freq_type` Значение, указывающее, когда должно выполняться задание. *freq_type* имеет **тип int**, значение по умолчанию **0** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**1**|Однократно|  
|**4**|Ежедневно|  
|**8**|Еженедельно|  
|**16**|Ежемесячно|  
|**32**|Ежемесячно относительно *интервала FREQ*|  
|**64**|Запустить, когда запускается служба SQLServerAgent|  
|**128**|Запускать, когда компьютер простаивает|  
  
`[ @freq_interval = ] freq_interval` Дни, в которые выполняется задание. *freq_interval* имеет **тип int**, значение по умолчанию **0** и зависит от значения *freq_type*.  
  
|Значение *freq_type*|Воздействие на *freq_interval*|  
|---------------------------|--------------------------------|  
|**1** (один раз)|*freq_interval* не используется.|  
|**4** (ежедневно)|Каждые *freq_interval* дней.|  
|**8** (еженедельно)|*freq_interval* является одним или несколькими следующими (в сочетании с логическим оператором **or** ):<br /><br /> **1** = воскресенье<br /><br /> **2** = понедельник<br /><br /> **4** = вторник<br /><br /> **8** = среда<br /><br /> **16** = четверг<br /><br /> **32** = Пятница<br /><br /> **64** = Суббота|  
|**16** (ежемесячно)|В *freq_interval* день месяца.|  
|**32** (ежемесячная относительная)|*freq_interval* является одним из следующих:<br /><br /> **1** = воскресенье<br /><br /> **2** = понедельник<br /><br /> **3** = вторник<br /><br /> **4** = среда<br /><br /> **5** = четверг<br /><br /> **6** = Пятница<br /><br /> **7** = Суббота<br /><br /> **8** = день<br /><br /> **9** = день недели<br /><br /> **10** = выходной день|  
|**64** (при запуске службы SQLServerAgent)|*freq_interval* не используется.|  
|**128**|*freq_interval* не используется.|  
  
`[ @freq_subday_type = ] freq_subday_type` Задает единицы измерения для *freq_subday_interval * *.* *freq_subday_type* имеет **тип int**, значение по умолчанию **0** и может принимать одно из следующих значений.  
  
|Значение|Описание (единица измерения)|  
|-----------|--------------------------|  
|**0x1**|В указанное время|  
|**0x2**|Секунды|  
|**0x4**|Минуты|  
|**0x8**|Часы|  
  
`[ @freq_subday_interval = ] freq_subday_interval` Число периодов *freq_subday_type* , которые должны быть выполнены между выполнением задания. *freq_subday_interval* имеет **тип int** и значение по умолчанию **0**.  
  
`[ @freq_relative_interval = ] freq_relative_interval` Вхождение *freq_interval* задания в каждый месяц, если *freq_interval* — **32** (ежемесячное относительное значение). *freq_relative_interval* имеет **тип int**, значение по умолчанию **0** и может принимать одно из следующих значений.  
  
|Значение|Описание (единица измерения)|  
|-----------|--------------------------|  
|**1**|First|  
|**2**|Секунда|  
|**4**|Третье|  
|**8**|Четвертая|  
|**16**|Последний|  
  
`[ @freq_recurrence_factor = ] freq_recurrence_factor` Число недель или месяцев между запланированным выполнением задания. *freq_recurrence_factor* используется только в том случае, если *freq_type* имеет значение **8**, **16** или **32**. *freq_recurrence_factor* имеет **тип int** и значение по умолчанию **0**.  
  
`[ @active_start_date = ] active_start_date` Дата, когда может начаться выполнение задания. *active_start_date* имеет **тип int** и значение по умолчанию NULL, которое указывает на сегодняшнюю дату. Формат даты: ГГГГMMДД. Если *active_start_date* не равно null, дата должна быть больше или равна 19900101.  
  
 После создания расписания проверьте дату начала и убедитесь, что она задана правильно. Дополнительные сведения см. в подразделе «Дата начала планирования» раздела [Создание и присоединение расписаний к заданиям](../../ssms/agent/create-and-attach-schedules-to-jobs.md).  
  
`[ @active_end_date = ] active_end_date` Дата, когда выполнение задания может быть прервано. *active_end_date* имеет **тип int** и значение по умолчанию **99991231**, которое означает 31 декабря 9999 г. Используется формат ГГГГММДД.  
  
`[ @active_start_time = ] active_start_time` Время в любой день между *active_start_date* и *active_end_date* , чтобы начать выполнение задания. *active_start_time* имеет **тип int** и значение по умолчанию 000000, которое указывает на 12:00:00 утра. в 24-часовом формате и должно вводиться в формате ЧЧММСС.  
  
`[ @active_end_time = ] active_end_time` Время в любой день между *active_start_date* и *active_end_date* для завершения выполнения задания. *active_end_time* имеет **тип int** и значение по умолчанию **235959**, которое указывает на 11:59:59 вечера. в 24-часовом формате и должно вводиться в формате ЧЧММСС.  
  
`[ @owner_login_name = ] 'owner_login_name']` Имя участника на сервере, владеющего расписанием. Аргумент *owner_login_name* имеет тип **sysname** и значение по умолчанию NULL, означающее, что расписание принадлежит автору.  
  
`[ @automatic_post = ] automatic_post` Процессу.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Примечания  
 Все задания, которые используют расписание, немедленно используют новые установки. Однако изменение расписания не останавливает выполнение заданий.  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию эту хранимую процедуру могут выполнять только члены предопределенной роли сервера **sysadmin** . Другим пользователям должна быть предоставлена одна из следующих предопределенных ролей базы данных агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в базе данных **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Дополнительные сведения о разрешениях этих ролей см. в разделе [Предопределенные роли базы данных агента SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Только члены **sysadmin** могут изменять расписание, принадлежащее другому пользователю.  
  
## <a name="examples"></a>Примеры  
 Следующий пример изменяет состояние расписания `NightlyJobs` на состояние `0` и устанавливает владельцем `terrid`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_schedule  
    @name = 'NightlyJobs',  
    @enabled = 0,  
    @owner_login_name = 'terrid' ;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Создание и присоединение расписаний к заданиям](../../ssms/agent/create-and-attach-schedules-to-jobs.md)   
 [Планирование задания](../../ssms/agent/schedule-a-job.md)   
 [Создание расписания](../../ssms/agent/create-a-schedule.md)   
 [Агент SQL Server хранимых процедур &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_add_jobschedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md)   
 [sp_delete_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_help_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-schedule-transact-sql.md)   
 [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
  

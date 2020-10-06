---
description: sp_add_jobstep (Transact-SQL)
title: sp_add_jobstep (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_add_jobstep_TSQL
- sp_add_jobstep
helpviewer_keywords:
- sp_add_jobstep
ms.assetid: 97900032-523d-49d6-9865-2734fba1c755
author: markingmyname
ms.author: maghan
ms.custom: ''
ms.date: 03/15/2017
ms.openlocfilehash: 8e4b24e5fcab13083c06f53868f462c3b45cc497
ms.sourcegitcommit: 968969b62bc158b9843aba5034c9d913519bc4a7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2020
ms.locfileid: "91753806"
---
# <a name="sp_add_jobstep-transact-sql"></a>sp_add_jobstep (Transact-SQL)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

Добавляет шаг (операцию) в задание агента SQL Server.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

> [!IMPORTANT]
> В [управляемый экземпляр SQL Azure](/azure/sql-database/sql-database-managed-instance)поддерживаются большинство типов заданий, но не все агент SQL Server. Подробные сведения см. в статье [Различия в T-SQL между Управляемым экземпляром SQL Azure и SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

## <a name="syntax"></a>Синтаксис

```sql
sp_add_jobstep [ @job_id = ] job_id | [ @job_name = ] 'job_name'
     [ , [ @step_id = ] step_id ]
     { , [ @step_name = ] 'step_name' }
     [ , [ @subsystem = ] 'subsystem' ]
     [ , [ @command = ] 'command' ]
     [ , [ @additional_parameters = ] 'parameters' ]
          [ , [ @cmdexec_success_code = ] code ]
     [ , [ @on_success_action = ] success_action ]
          [ , [ @on_success_step_id = ] success_step_id ]
          [ , [ @on_fail_action = ] fail_action ]
          [ , [ @on_fail_step_id = ] fail_step_id ]
     [ , [ @server = ] 'server' ]
     [ , [ @database_name = ] 'database' ]
     [ , [ @database_user_name = ] 'user' ]
     [ , [ @retry_attempts = ] retry_attempts ]
     [ , [ @retry_interval = ] retry_interval ]
     [ , [ @os_run_priority = ] run_priority ]
     [ , [ @output_file_name = ] 'file_name' ]
     [ , [ @flags = ] flags ]
     [ , { [ @proxy_id = ] proxy_id
         | [ @proxy_name = ] 'proxy_name' } ]
```  
  
## <a name="arguments"></a>Аргументы

`[ @job_id = ] job_id` Идентификационный номер задания, к которому добавляется шаг. *job_id* имеет тип **uniqueidentifier**и значение по умолчанию NULL.

`[ @job_name = ] 'job_name'` Имя задания, к которому добавляется шаг. Аргумент *job_name* имеет тип **sysname**и значение по умолчанию NULL.

> [!NOTE]
> Необходимо указать либо *job_id* , либо *job_name* , но нельзя указать оба значения.

`[ @step_id = ] step_id` Идентификационный номер последовательности для шага задания. Идентификационные номера шагов начинаются с **1** и увеличиваются без пробелов. Если этап вставляется в существующую последовательность, порядковые номера меняются автоматически. Значение указывается, если *step_id* не указано. *step_id* имеет **тип int**и значение по умолчанию NULL.

`[ @step_name = ] 'step_name'` Имя шага. Аргумент *step_name* имеет тип **sysname**и не имеет значения по умолчанию.

`[ @subsystem = ] 'subsystem'` Подсистема, используемая [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] службой агента для выполнения *команды*. *подсистема* имеет тип **nvarchar (40)** и может принимать одно из следующих значений.

|Значение|Описание|
|-----------|-----------------|
|"**ActiveScripting**"|Активный скрипт.<br /><br /> **\*\* Внимание! \*\*** [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]|
|**CmdExec**|Команда операционной системы или исполняемая программа.|
|"**Distribution**"|Задание агента распространения репликации.|
|"**Snapshot**"|Задание агента моментальных снимков репликации.|
|"Модуль**чтения журнала**"|Задание агента чтения журнала репликации.|
|"**Merge**"|Задание агента слияния репликации.|
|"**QueueReader**"|Задание агента чтения очереди репликации.|
|"**ANALYSISQUERY**"|Запрос служб Analysis Services (многомерное выражение, расширения интеллектуального анализа данных).|
|"**ANALYSISCOMMAND**"|Команда служб Analysis Services (XML для аналитики).|
|"**SSIS**"|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] выполнение пакетов служб|  
|"**PowerShell**"|Скрипт PowerShell|  
|"**Tsql**" (по умолчанию)|инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)]|

`[ @command = ] 'command'` Команды, выполняемые службой **SQLServerAgent** через *подсистему*. *команда* имеет тип **nvarchar (max)** и значение по умолчанию NULL. Агент SQL Server выполняет замену токенов, что обеспечивает такую же гибкость, что и переменные при написании программ.

> [!IMPORTANT]
> Все токены, используемые в шагах заданий, теперь должны сопровождаться экранирующим макросом, в противном случае они вызовут ошибку. Кроме того, теперь имена токенов нужно заключать в круглые скобки и помещать в начале синтаксиса токена знак доллара (`$`). Пример:
>
> `$(ESCAPE_` *имя макроса* `(DATE))`  

Дополнительные сведения об этих токенах и обновлении шагов задания для использования нового синтаксиса маркера см. [в разделе Использование маркеров в шагах задания](../../ssms/agent/use-tokens-in-job-steps.md).

> [!IMPORTANT]
> Все пользователи Windows с разрешением на запись в журнал событий Windows могут получить доступ к шагам заданий, которые активированы предупреждениями агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или инструментария WMI. Чтобы избежать этого нарушения безопасности, в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] токены агента, которые могут использоваться в заданиях, активированных предупреждениями, по умолчанию отключены. Такими токенами являются **A-DBN**, **A-SVR**, **A-ERR**, **A-SEV**, **A-MSG** и **WMI(** _property_ **)** . Обратите внимание, что в этом выпуске использование токенов распространяется на все оповещения.
>
> Если необходимо использовать эти токены, убедитесь, что только члены доверенных групп безопасности Windows, таких как группа «Администраторы», обладают разрешением на работу с журналом событий компьютера, на котором находится [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Затем, чтобы включить эти токены, щелкните правой кнопкой мыши элемент **Агент SQL Server** в обозревателе объектов, выберите пункт меню **Свойства**и на странице **Система предупреждений** установите флажок **Заменить токены всех ответов заданий на предупреждения** .

`[ @additional_parameters = ] 'parameters'`[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] *Параметры* — **ntext**и значение по умолчанию NULL.

`[ @cmdexec_success_code = ] code` Значение, возвращаемое командой подсистемы **CmdExec** , чтобы указать, что *команда* выполнена успешно. *код* имеет **тип int**и значение по умолчанию **0**.

`[ @on_success_action = ] success_action` Действие, выполняемое, если шаг выполнен. *success_action* имеет тип **tinyint**и может принимать одно из следующих значений.
  
|Значение|Описание (действие)|  
|-----------|----------------------------|  
|**1** (по умолчанию)|Завершить с успешным выполнением.|  
|**2**|Завершить с ошибкой.|  
|**3**|Перейти к следующему шагу.|  
|**4**|Перейти к шагу *on_success_step_id*|  

`[ @on_success_step_id = ] success_step_id` Идентификатор шага в этом задании, который должен быть выполнен, если шаг выполнен, а *success_action* — **4**. *success_step_id* имеет **тип int**и значение по умолчанию **0**.

`[ @on_fail_action = ] fail_action` Действие, выполняемое в случае сбоя шага. *fail_action* имеет тип **tinyint**и может принимать одно из следующих значений.

|Значение|Описание (действие)|  
|-----------|----------------------------|  
|**1**|Завершить с успешным выполнением.|  
|**2** (по умолчанию)|Завершить с ошибкой.|  
|**3**|Перейти к следующему шагу.|  
|**4**|Перейти к шагу *on_fail_step_id*|  

`[ @on_fail_step_id = ] fail_step_id` Идентификатор шага в этом задании, который будет выполнен, если шаг завершается ошибкой, а *fail_action* — **4**. *fail_step_id* имеет **тип int**и значение по умолчанию **0**.  

`[ @server = ] 'server'`[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] *Server* имеет тип **nvarchar (30)** и значение по умолчанию NULL.  

`[ @database_name = ] 'database'` Имя базы данных, в которой будет выполняться [!INCLUDE[tsql](../../includes/tsql-md.md)] шаг. Аргумент *Database* имеет тип **sysname**и значение по умолчанию NULL, в этом случае используется база данных **master** . Символы, заключенные в квадратные скобки ([ ]), являются недопустимыми. Для шага задания ActiveX *база данных* — это имя языка сценариев, используемого этим шагом.  

`[ @database_user_name = ] 'user'` Имя учетной записи пользователя, используемой при выполнении [!INCLUDE[tsql](../../includes/tsql-md.md)] шага. Аргумент *User* имеет тип **sysname**и значение по умолчанию NULL. Если *пользователь* имеет значение null, шаг выполняется в пользовательском контексте владельца задания в *базе данных*.  Агент SQL Server включит этот параметр только в случае, если владелец задания — член роли SQL Server sysadmin. Если это так, то данный шаг Transact-SQL будет выполнен в контексте данного имени пользователя SQL Server. Если владелец задания не является SQL Server sysadmin, то шаг Transact-SQL всегда будет выполняться в контексте имени входа, владеющего этим заданием, а @database_user_name параметр будет проигнорирован.  

`[ @retry_attempts = ] retry_attempts` Количество повторных попыток использования в случае сбоя этого шага. *retry_attempts* имеет **тип int**и значение по умолчанию **0**, которое указывает на отсутствие повторных попыток.  

`[ @retry_interval = ] retry_interval` Количество времени в минутах между повторными попытками. *retry_interval* имеет **тип int**и значение по умолчанию **0**, что означает **0**-минутный интервал.  

`[ @os_run_priority = ] run_priority` Процессу.

`[ @output_file_name = ] 'file_name'` Имя файла, в котором сохраняется выходные данные этого шага. *file_name* имеет тип **nvarchar (200)** и значение по умолчанию NULL. *file_name* может содержать один или несколько токенов, перечисленных в разделе *команда*. Этот параметр допустим только для команд, выполняющихся в [!INCLUDE[tsql](../../includes/tsql-md.md)] , **CmdExec**, **PowerShell**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] или [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] подсистемах.  

`[ @flags = ] flags` Параметр, который управляет поведением. *Флаги* имеют **тип int**и могут принимать одно из следующих значений.  

|Значение|Описание|  
|-----------|-----------------|  
|**0** (по умолчанию)|Переписать выходной файл.|  
|**2**|Добавить к выходному файлу.|  
|**4**|Записать вывод шага задания [!INCLUDE[tsql](../../includes/tsql-md.md)] в журнал шагов.|  
|**8**|Записать журнал в таблицу (переписать существующий журнал).|  
|**16**|Записать журнал в таблицу (добавить к существующему журналу).|  
|**32**|Записать все выходные данные в журнал заданий.|  
|**64**|Создать событие Windows для использования в качестве сигнала для прерывания шага задания Cmd.|  

`[ @proxy_id = ] proxy_id` Идентификационный номер учетной записи-посредника, от имени которой выполняется шаг задания. *proxy_id* имеет тип **int**и значение по умолчанию NULL. Если *proxy_id* не указано, *proxy_name* не указана и *user_name* не указана, шаг задания выполняется как учетная запись службы для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] агента.  

`[ @proxy_name = ] 'proxy_name'` Имя учетной записи-посредника, от имени которой выполняется шаг задания. *proxy_name* имеет тип **sysname**и значение по умолчанию NULL. Если *proxy_id* не указано, *proxy_name* не указана и *user_name* не указана, шаг задания выполняется как учетная запись службы для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] агента.  

## <a name="return-code-values"></a>Значения кода возврата

**0** (успешное завершение) или **1** (сбой)

## <a name="result-sets"></a>Результирующие наборы

None

## <a name="remarks"></a>Remarks

**sp_add_jobstep** должны запускаться из базы данных **msdb** .  

Среда SQL Server Management Studio обеспечивает простой и наглядный способ управления заданиями и рекомендуется для создания инфраструктуры заданий и управления ей.  

По умолчанию шаг задания выполняется в качестве учетной записи службы для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] агента, если не указан другой прокси-сервер. Требование этой учетной записи является членом фиксированной роли безопасности **sysadmin** .

Прокси-сервер может идентифицироваться *proxy_name* или *proxy_id*.

## <a name="permissions"></a>Разрешения

 По умолчанию эту хранимую процедуру могут выполнять только члены предопределенной роли сервера **sysadmin** . Другим пользователям должна быть предоставлена одна из следующих предопределенных ролей базы данных агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в базе данных **msdb** :

- **SQLAgentUserRole**

- **SQLAgentReaderRole**

- **SQLAgentOperatorRole**

Дополнительные сведения о разрешениях этих ролей см. в разделе [Предопределенные роли базы данных агента SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).

Создатель шага задания должен иметь доступ к учетной записи-посреднику для шага задания. Члены предопределенной роли сервера **sysadmin** имеют доступ ко всем учетным записям-посредникам. Другим пользователям доступ к учетной записи-посреднику должен быть предоставлен явно.

## <a name="examples"></a>Примеры

Следующий пример создает шаг задания, который изменяет доступ к базе данных на доступ только для чтения для базы данных Sales. Кроме того, этот пример задает число повторных попыток, которое равно 5, и через каждые 5 минут.

> [!NOTE]
> В этом примере предполагается, что задание `Weekly Sales Data Backup` уже существует.  

```sql
USE msdb;
GO
EXEC sp_add_jobstep
    @job_name = N'Weekly Sales Data Backup',
    @step_name = N'Set database to read only',
    @subsystem = N'TSQL',
    @command = N'ALTER DATABASE SALES SET READ_ONLY',
    @retry_attempts = 5,
    @retry_interval = 5 ;
GO
```

## <a name="next-steps"></a>Дальнейшие действия

- [Просмотр или изменение заданий](../../ssms/agent/view-or-modify-jobs.md)
- [sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)
- [sp_add_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)
- [sp_delete_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md)
- [sp_help_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-job-transact-sql.md)
- [sp_help_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md)
- [sp_update_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md)
- [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)
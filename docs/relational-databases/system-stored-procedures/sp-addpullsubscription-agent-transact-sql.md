---
description: sp_addpullsubscription_agent (Transact-SQL)
title: sp_addpullsubscription_agent (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/29/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpullsubscription_agent
- sp_addpullsubscription_agent_TSQL
helpviewer_keywords:
- sp_addpullsubscription_agent
ms.assetid: b9c2eaed-6d2d-4b78-ae9b-73633133180b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8bf3e90f4f1b360a45427241cc2da480765d4da5
ms.sourcegitcommit: 851f47e27512651f809540b77bfbd09e6ddb5362
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105937823"
---
# <a name="sp_addpullsubscription_agent-transact-sql"></a>sp_addpullsubscription_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
 
  Добавляет новое запланированное задание агента, используемое для синхронизации подписки по запросу с публикацией транзакций. Эта хранимая процедура выполняется на подписчике в базе данных подписки.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_addpullsubscription_agent [ @publisher = ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db' ]          , [ @publication = ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @distributor = ] 'distributor' ]  
    [ , [ @distribution_db = ] 'distribution_db' ]  
    [ , [ @distributor_security_mode = ] distributor_security_mode ]  
    [ , [ @distributor_login = ] 'distributor_login' ]  
    [ , [ @distributor_password = ] 'distributor_password' ]  
    [ , [ @optional_command_line = ] 'optional_command_line' ]  
    [ , [ @frequency_type = ] frequency_type ]  
    [ , [ @frequency_interval = ] frequency_interval ]  
    [ , [ @frequency_relative_interval = ] frequency_relative_interval ]  
    [ , [ @frequency_recurrence_factor = ] frequency_recurrence_factor ]  
    [ , [ @frequency_subday = ] frequency_subday ]  
    [ , [ @frequency_subday_interval = ] frequency_subday_interval ]  
    [ , [ @active_start_time_of_day = ] active_start_time_of_day ]  
    [ , [ @active_end_time_of_day = ] active_end_time_of_day ]  
    [ , [ @active_start_date = ] active_start_date ]  
    [ , [ @active_end_date = ] active_end_date ]  
    [ , [ @distribution_jobid = ] distribution_jobid OUTPUT ]  
    [ , [ @encrypted_distributor_password = ] encrypted_distributor_password ]  
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]  
    [ , [ @ftp_address = ] 'ftp_address' ]  
    [ , [ @ftp_port = ] ftp_port ]  
    [ , [ @ftp_login = ] 'ftp_login' ]  
    [ , [ @ftp_password = ] 'ftp_password' ]  
    [ , [ @alt_snapshot_folder = ] 'alternate_snapshot_folder' ]  
    [ , [ @working_directory = ] 'working_directory' ]  
    [ , [ @use_ftp = ] 'use_ftp' ]  
    [ , [ @publication_type = ] publication_type ]  
    [ , [ @dts_package_name = ] 'dts_package_name' ]  
    [ , [ @dts_package_password = ] 'dts_package_password' ]  
    [ , [ @dts_package_location = ] 'dts_package_location' ]  
    [ , [ @reserved = ] 'reserved' ]  
    [ , [ @offloadagent = ] 'remote_agent_activation' ]  
    [ , [ @offloadserver = ] 'remote_agent_server_name']  
    [ , [ @job_name = ] 'job_name' ]  
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publisher = ] 'publisher'` Имя издателя. параметр *Publisher* имеет тип **sysname** и не имеет значения по умолчанию.  

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

> [!NOTE]
> Имя сервера можно указать как `<Hostname>,<PortNumber>` . Может потребоваться указать номер порта для подключения, если SQL Server развертывается в Linux или Windows с помощью настраиваемого порта, а служба браузера отключена. Использование настраиваемых номеров портов для удаленного распространителя применимо только к SQL Server 2019.

::: moniker-end
  
`[ @publisher_db = ] 'publisher_db'_` Имя базы данных издателя. Аргумент *publisher_db* имеет тип **sysname** и значение по умолчанию NULL. *publisher_db* не учитываются издателями Oracle.  
  
`[ @publication = ] 'publication'` Имя публикации. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @subscriber = ] 'subscriber'` Имя экземпляра подписчика или имя прослушивателя AG, если база данных подписчика находится в группе доступности. Аргумент *Subscriber* имеет тип **sysname** и значение по умолчанию NULL.  
  
> [!NOTE]  
>  Данный аргумент является устаревшим и сохранен только для поддержки обратной совместимости скриптов.  
  
`[ @subscriber_db = ] 'subscriber_db'` Имя базы данных подписки. Аргумент *subscriber_db* имеет тип **sysname** и значение по умолчанию NULL.  
  
> [!NOTE]  
>  Данный аргумент является устаревшим и сохранен только для поддержки обратной совместимости скриптов.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Режим безопасности, используемый при соединении с подписчиком при синхронизации. *subscriber_security_mode* имеет **тип int и** значение по умолчанию NULL. **0** — [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Проверка подлинности. **1** указывает проверку подлинности Windows.  
  
> [!NOTE]  
>  Данный аргумент является устаревшим и сохранен только для поддержки обратной совместимости скриптов. Агент распространителя всегда подключается к локальному подписчику с использованием проверки подлинности Windows. Если для этого параметра указано значение, отличное от NULL или **1** , то возвращается предупреждающее сообщение.  
  
`[ @subscriber_login = ] 'subscriber_login'` Имя входа подписчика, используемое при соединении с подписчиком при синхронизации. Аргумент *subscriber_login* имеет тип **sysname** и значение по умолчанию NULL.  
  
> [!NOTE]  
>  Данный аргумент является устаревшим и сохранен только для поддержки обратной совместимости скриптов. Если для этого аргумента указывается значение, то выдается предупреждающее сообщение, а значение не обрабатывается.  
  
`[ @subscriber_password = ] 'subscriber_password'` Пароль подписчика. *subscriber_password* является обязательным, если *subscriber_security_mode* имеет значение **0**. Аргумент *subscriber_password* имеет тип **sysname** и значение по умолчанию NULL. Если пароль подписчика используется, он автоматически шифруется.  
  
> [!NOTE]  
>  Данный аргумент является устаревшим и сохранен только для поддержки обратной совместимости скриптов. Если для этого аргумента указывается значение, то выдается предупреждающее сообщение, а значение не обрабатывается.  
  
`[ @distributor = ] 'distributor'` Имя распространителя. Аргумент *распространитель* имеет тип **sysname** и значение по умолчанию, заданное в параметре *Publisher*.  
  
`[ @distribution_db = ] 'distribution_db'` Имя базы данных распространителя. Аргумент *distribution_db* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @distributor_security_mode = ] distributor_security_mode` Режим безопасности, используемый при соединении с распространителем при синхронизации. *distributor_security_mode* имеет **тип int** и значение по умолчанию **1**. **0** — [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Проверка подлинности. **1** указывает проверку подлинности Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
`[ @distributor_login = ] 'distributor_login'` Имя входа распространителя, используемое при соединении с распространителем при синхронизации. *distributor_login* является обязательным, если *distributor_security_mode* имеет значение **0**. Аргумент *distributor_login* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @distributor_password = ] 'distributor_password'` Пароль распространителя. *distributor_password* является обязательным, если *distributor_security_mode* имеет значение **0**. Аргумент *distributor_password* имеет тип **sysname** и значение по умолчанию NULL.  
  
> [!IMPORTANT]  
>  Не используйте пустые пароли. Выбирайте надежные пароли. По возможности предлагайте пользователям вводить учетные данные системы безопасности во время выполнения приложения. В случае необходимости хранения учетных данных в файле скрипта этот файл следует защищать во избежание несанкционированного доступа.  
  
`[ @optional_command_line = ] 'optional_command_line'` — Необязательная Командная строка, предоставляемая агент распространения. Например, **-DefinitionFile** C:\Distdef.txt или **-CommitBatchSize** 10. *optional_command_line* имеет тип **nvarchar (4000)** и значение по умолчанию, равное пустой строке.  
  
`[ @frequency_type = ] frequency_type` Частота, с которой следует запланировать агент распространения. *frequency_type* имеет **тип int** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**1**|Один раз.|  
|**2** (по умолчанию)|По запросу|  
|**4**|Ежедневно|  
|**8**|Еженедельно|  
|**16**|Ежемесячно|  
|**32**|Ежемесячно с относительной датой|  
|**64**|Автозапуск|  
|**128**|Повторяющееся задание|  
  
> [!NOTE]  
>  Указание значения **64** приводит к запуску агент распространения в непрерывном режиме. Это соответствует установке параметра **-Continuous** для агента. Дополнительные сведения см. в статье [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
`[ @frequency_interval = ] frequency_interval` Значение, применяемое к частоте, установленной *frequency_type*. *frequency_interval* имеет **тип int** и значение по умолчанию 1.  
  
`[ @frequency_relative_interval = ] frequency_relative_interval` Дата агент распространения. Этот параметр используется, если *frequency_type* установлен в значение **32** (ежемесячное относительное расписание). *frequency_relative_interval* имеет **тип int** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**1** (по умолчанию)|First|  
|**2**|Секунда|  
|**4**|Третье|  
|**8**|Четвертая|  
|**16**|Последний|  
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Коэффициент повторения, используемый *frequency_type*. *frequency_recurrence_factor* имеет **тип int** и значение по умолчанию **1**.  
  
`[ @frequency_subday = ] frequency_subday` Частота повторного планирования в течение заданного периода. *frequency_subday* имеет **тип int** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**1** (по умолчанию)|Однократно|  
|**2**|Секунда|  
|**4**|Минута|  
|**8**|Час|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Интервал для *frequency_subday*. *frequency_subday_interval* имеет **тип int** и значение по умолчанию **1**.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Время первого запланированного агент распространения в формате ЧЧММСС. *active_start_time_of_day* имеет **тип int** и значение по умолчанию **0**.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Время суток, когда запланировать агент распространения прекращается в формате ЧЧММСС. *active_end_time_of_day* имеет **тип int** и значение по умолчанию **0**.  
  
`[ @active_start_date = ] active_start_date` Дата первого запланированного запуска агент распространения в формате ГГГГММДД. *active_start_date* имеет **тип int** и значение по умолчанию **0**.  
  
`[ @active_end_date = ] active_end_date` Дата прекращения расписания агент распространения в формате ГГГГММДД. *active_end_date* имеет **тип int** и значение по умолчанию **0**.  
  
`[ @distribution_jobid = ] _distribution_jobidOUTPUT` Идентификатор агент распространения для этого задания. *distribution_jobid* является **двоичным (16)**, имеет значение по умолчанию NULL и является выходным параметром.  
  
`[ @encrypted_distributor_password = ] encrypted_distributor_password` Параметр *encrypted_distributor_password* больше не поддерживается. Попытка присвоить этому параметру **bit** значение **1** приведет к ошибке.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Указывает, можно ли синхронизировать подписку с помощью [!INCLUDE[msCoName](../../includes/msconame-md.md)] диспетчера синхронизации. *enabled_for_syncmgr* имеет тип **nvarchar (5)** и значение по умолчанию false. Если **значение равно false**, подписка не зарегистрирована в диспетчере синхронизации. Если **значение — true**, подписка регистрируется в диспетчере синхронизации и может быть синхронизирована без запуска [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @ftp_address = ] 'ftp_address'` Только для обратной совместимости.  
  
`[ @ftp_port = ] ftp_port` Только для обратной совместимости.  
  
`[ @ftp_login = ] 'ftp_login'` Только для обратной совместимости.  
  
`[ @ftp_password = ] 'ftp_password'` Только для обратной совместимости.  
  
`[ @alt_snapshot_folder = ] 'alternate_snapshot_folder'_` Указывает расположение альтернативной папки для моментального снимка. *alternate_snapshot_folder* имеет тип **nvarchar (255)** и значение по умолчанию NULL.  
  
`[ @working_directory = ] 'working_director'` Имя рабочего каталога, используемого для хранения файлов данных и схемы для публикации. *working_directory* имеет тип **nvarchar (255)** и значение по умолчанию NULL. Имя должно быть задано в формате UNC.  
  
`[ @use_ftp = ] 'use_ftp'` Задает использование FTP вместо обычного протокола для получения моментальных снимков. *use_ftp* имеет тип **nvarchar (5)** и значение по умолчанию false.  
  
`[ @publication_type = ] publication_type` Указывает тип репликации публикации. *publication_type* имеет тип **tinyint** и значение по умолчанию **0**. Если значение **равно 0**, то публикация является типом транзакции. Если значение равно **1**, то публикация является типом моментального снимка. Если значение равно **2**, то публикация является типом слияния.  
  
`[ @dts_package_name = ] 'dts_package_name'` Указывает имя пакета служб DTS. *dts_package_name* имеет тип **sysname** и значение по умолчанию NULL. Например, для задания пакета `DTSPub_Package` параметр должен быть равен `@dts_package_name = N'DTSPub_Package'`.  
  
`[ @dts_package_password = ] 'dts_package_password'` Указывает пароль для пакета при его наличии. Аргумент *dts_package_password* имеет тип **sysname** и значение по умолчанию NULL, что означает, что пароль отсутствует в пакете.  
  
> [!NOTE]  
>  При указании *dts_package_name* необходимо указать пароль.  
  
`[ @dts_package_location = ] 'dts_package_location'` Указывает расположение пакета. *dts_package_location* имеет тип **nvarchar (12)** и значение по умолчанию **подписчика**. Расположение пакета может быть **распространителем** или **подписчиком**.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @offloadagent = ] 'remote_agent_activation'`
 > [!NOTE]  
>  Удаленная активация агента является устаревшей и больше не поддерживается. Этот аргумент поддерживается только для обратной совместимости скриптов. При установке *remote_agent_activation* значение, отличное от **false** , будет выдаваться ошибка.  
  
`[ @offloadserver = ] 'remote_agent_server_name'`
 > [!NOTE]  
>  Удаленная активация агента является устаревшей и больше не поддерживается. Этот аргумент поддерживается только для обратной совместимости скриптов. При установке *remote_agent_server_name* любого значения, отличного от NULL, будет выдаваться ошибка.  
  
`[ @job_name = ] 'job_name'` Имя существующего задания агента. Аргумент *job_name* имеет тип **sysname** и значение по умолчанию NULL. Этот аргумент указывается только тогда, когда подписка будет синхронизироваться с использованием существующего задания, а не вновь создаваемого задания (выбор по умолчанию). Если вы не являетесь членом предопределенной роли сервера **sysadmin** , необходимо указать *job_login* и *job_password* при указании *job_name*.  
  
`[ @job_login = ] 'job_login'` Имя входа для учетной записи Windows, под которой запускается агент. *job_login* имеет тип **nvarchar (257)** и не имеет значения по умолчанию. Для соединений агента с подписчиком всегда используется эта учетная запись Windows.  
  
`[ @job_password = ] 'job_password'` Пароль для учетной записи Windows, под которой запускается агент. Аргумент *job_password* имеет тип **sysname** и не имеет значения по умолчанию.  
  
> [!IMPORTANT]  
>  По возможности предлагайте пользователям вводить учетные данные системы безопасности во время выполнения приложения. В случае необходимости хранения учетных данных в файле скрипта этот файл следует защищать во избежание несанкционированного доступа.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Remarks  
 **sp_addpullsubscription_agent** используется в репликации моментальных снимков и репликации транзакций.  
  
## <a name="example"></a>Пример  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addpullsubscription-a_1.sql)]  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_addpullsubscription_agent**.  
  
## <a name="see-also"></a>См. также:  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_change_subscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md)   
 [sp_droppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  

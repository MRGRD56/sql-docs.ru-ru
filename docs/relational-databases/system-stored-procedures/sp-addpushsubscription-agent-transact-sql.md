---
description: sp_addpushsubscription_agent (Transact-SQL)
title: sp_addpushsubscription_agent (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/29/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpushsubscription_agent_TSQL
- sp_addpushsubscription_agent
helpviewer_keywords:
- sp_addpushsubscription_agent
ms.assetid: 1fdd2052-50d8-4318-8aa7-fc635d5cad18
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 81bf487607d0cfea9f7ab623e12d732e4463aded
ms.sourcegitcommit: 851f47e27512651f809540b77bfbd09e6ddb5362
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105937841"
---
# <a name="sp_addpushsubscription_agent-transact-sql"></a>sp_addpushsubscription_agent (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Добавляет новое запланированное задание агента, используемое для синхронизации принудительной подписки на публикацию транзакций. Эта хранимая процедура выполняется на издателе в базе данных публикации.  
  
> [!IMPORTANT]  
>  Если издатель настраивается с удаленным распространителем, то значения, передаваемые для всех аргументов, включая *job_login* и *job_password*, передаются распространителю в формате обычного (незашифрованного) текста. Прежде чем выполнять эту хранимую процедуру, необходимо зашифровать соединение между издателем и его удаленным распространителем. Дополнительные сведения см. в разделе [Включение шифрования соединений в компоненте Database Engine (диспетчер конфигураций SQL Server)](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_addpushsubscription_agent [ @publication= ] 'publication'  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
    [ , [ @subscriber_security_mode = ] subscriber_security_mode ]  
    [ , [ @subscriber_login = ] 'subscriber_login' ]  
    [ , [ @subscriber_password = ] 'subscriber_password' ]  
    [ , [ @job_login = ] 'job_login' ]   
    [ , [ @job_password = ] 'job_password' ]   
    [ , [ @job_name = ] 'job_name' ]   
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
    [ , [ @dts_package_name = ] 'dts_package_name' ]  
    [ , [ @dts_package_password = ] 'dts_package_password' ]  
    [ , [ @dts_package_location = ] 'dts_package_location' ]  
    [ , [ @enabled_for_syncmgr = ] 'enabled_for_syncmgr' ]  
    [ , [ @distribution_job_name = ] 'distribution_job_name' ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @subscriber_provider = ] 'subscriber_provider' ]   
    [ , [ @subscriber_datasrc = ] 'subscriber_datasrc' ]   
    [ , [ @subscriber_location = ] 'subscriber_location' ]  
    [ , [ @subscriber_provider_string = ] 'subscriber_provider_string' ]   
    [ , [ @subscriber_catalog = ] 'subscriber_catalog' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publication = ] 'publication'` Имя публикации. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @subscriber = ] 'subscriber'` Имя экземпляра подписчика или имя прослушивателя AG, если база данных подписчика является группой доступности. Аргумент *Subscriber* имеет тип **sysname** и значение по умолчанию NULL. 

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

> [!NOTE]
> Имя сервера можно указать как `<Hostname>,<PortNumber>` . Может потребоваться указать номер порта для подключения, если SQL Server развертывается в Linux или Windows с помощью настраиваемого порта, а служба браузера отключена. Использование настраиваемых номеров портов для удаленного распространителя применимо только к SQL Server 2019.

::: moniker-end

`[ @subscriber_db = ] 'subscriber_db'` Имя базы данных подписки. Аргумент *subscriber_db* имеет тип **sysname** и значение по умолчанию NULL. Для подписчика, не являющегося SQL Server, укажите значение **(назначение по умолчанию)** для *subscriber_db*.  
  
`[ @subscriber_security_mode = ] subscriber_security_mode` Режим безопасности, используемый при соединении с подписчиком при синхронизации. *subscriber_security_mode* имеет **тип int** и значение по умолчанию 1. **0** — [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Проверка подлинности. **1** указывает проверку подлинности Windows.  
  
> [!IMPORTANT]  
>  Для обновляемых посредством очередей подписок при соединении с подписчиками используйте проверку подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], указывая для каждого из подписчиков различные учетные записи. Для всех других подписок используйте проверку подлинности Windows.  
  
`[ @subscriber_login = ] 'subscriber_login'` Имя входа подписчика, используемое при соединении с подписчиком при синхронизации. Аргумент *subscriber_login* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @subscriber_password = ] 'subscriber_password'` Пароль подписчика. *subscriber_password* является обязательным, если *subscriber_security_mode* имеет значение **0**. Аргумент *subscriber_password* имеет тип **sysname** и значение по умолчанию NULL. Если пароль подписчика используется, он автоматически шифруется.  
  
> [!IMPORTANT]  
>  Не используйте пустые пароли. Выбирайте надежные пароли. По возможности предлагайте пользователям вводить учетные данные системы безопасности во время выполнения приложения. В случае необходимости хранения учетных данных в файле скрипта этот файл следует защищать во избежание несанкционированного доступа.  
  
`[ @job_login = ] 'job_login'` Имя входа для учетной записи, под которой выполняется агент. В Azure SQL Управляемый экземпляр использовать учетную запись SQL Server. *job_login* имеет тип **nvarchar (257)** и значение по умолчанию NULL. Эта учетная запись Windows всегда используется для подключений к распространителю средствами агентов и для подключений к подписчику в случае применения встроенных средств проверки подлинности Windows.  
  
`[ @job_password = ] 'job_password'` Пароль для учетной записи, под которой выполняется агент. Аргумент *job_password* имеет тип **sysname** и не имеет значения по умолчанию.  
  
> [!IMPORTANT]  
>  По возможности предлагайте пользователям вводить учетные данные системы безопасности во время выполнения приложения. В случае необходимости хранения учетных данных в файле скрипта этот файл следует защищать во избежание несанкционированного доступа.  
  
`[ @job_name = ] 'job_name'` Имя существующего задания агента. Аргумент *job_name* имеет тип **sysname** и значение по умолчанию NULL. Этот аргумент указывается только тогда, когда подписка будет синхронизироваться с использованием существующего задания, а не вновь создаваемого задания (выбор по умолчанию). Если вы не являетесь членом предопределенной роли сервера **sysadmin** , необходимо указать *job_login* и *job_password* при указании *job_name*.  
  
`[ @frequency_type = ] frequency_type` Частота, с которой следует запланировать агент распространения. *frequency_type* имеет **тип int** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**1**|Один раз.|  
|**2**|По запросу|  
|**4**|Ежедневно|  
|**8**|Еженедельно|  
|**16**|Ежемесячно|  
|**32**|Ежемесячно с относительной датой|  
|**64** (по умолчанию)|Автозапуск|  
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
  
`[ @frequency_recurrence_factor = ] frequency_recurrence_factor` Коэффициент повторения, используемый *frequency_type*. *frequency_recurrence_factor* имеет **тип int** и значение по умолчанию 0.  
  
`[ @frequency_subday = ] frequency_subday` Частота повторного планирования в течение заданного периода. *frequency_subday* имеет **тип int** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**1**|Однократно|  
|**2**|Секунда|  
|**4** (по умолчанию)|Минута|  
|**8**|Час|  
  
`[ @frequency_subday_interval = ] frequency_subday_interval` Интервал для *frequency_subday*. *frequency_subday_interval* имеет **тип int** и значение по умолчанию 5.  
  
`[ @active_start_time_of_day = ] active_start_time_of_day` Время первого запланированного агент распространения в формате ЧЧММСС. *active_start_time_of_day* имеет **тип int** и значение по умолчанию 0.  
  
`[ @active_end_time_of_day = ] active_end_time_of_day` Время суток, когда запланировать агент распространения прекращается в формате ЧЧММСС. *active_end_time_of_day* имеет **тип int** и значение по умолчанию 235959.  
  
`[ @active_start_date = ] active_start_date` Дата первого запланированного запуска агент распространения в формате ГГГГММДД. *active_start_date* имеет **тип int** и значение по умолчанию 0.  
  
`[ @active_end_date = ] active_end_date` Дата прекращения расписания агент распространения в формате ГГГГММДД. *active_end_date* имеет **тип int** и значение по умолчанию 99991231.  
  
`[ @dts_package_name = ] 'dts_package_name'` Указывает имя пакета служб DTS. *dts_package_name* имеет тип **sysname** и значение по умолчанию NULL. Например, для задания имени пакета `DTSPub_Package` этот параметр должен иметь значение `@dts_package_name = N'DTSPub_Package'`.  
  
`[ @dts_package_password = ] 'dts_package_password'` Указывает пароль, необходимый для запуска пакета. Аргумент *dts_package_password* имеет тип **sysname** и значение по умолчанию NULL.  
  
> [!NOTE]  
>  При указании *dts_package_name* необходимо указать пароль.  
  
`[ @dts_package_location = ] 'dts_package_location'` Указывает расположение пакета. *dts_package_location* имеет тип **nvarchar (12)** и значение по умолчанию распространителя. Расположение пакета может быть **распространителем** или **подписчиком**.  
  
`[ @enabled_for_syncmgr = ] 'enabled_for_syncmgr'` Указывает, можно ли синхронизировать подписку с помощью [!INCLUDE[msCoName](../../includes/msconame-md.md)] диспетчера синхронизации. *enabled_for_syncmgr* имеет тип **nvarchar (5)** и значение по умолчанию false. Если **значение равно false**, подписка не зарегистрирована в диспетчере синхронизации. Если **значение — true**, подписка регистрируется в диспетчере синхронизации и может быть синхронизирована без запуска [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
`[ @distribution_job_name = ] 'distribution_job_name'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @publisher = ] 'publisher'` Имя издателя. параметр *Publisher* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @subscriber_provider = ] 'subscriber_provider'` Уникальный программный идентификатор (PROGID), с которым регистрируется поставщик OLE DB для источника, не являющегося [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] источником данных. Аргумент *subscriber_provider* имеет тип **sysname** и значение по умолчанию NULL. *subscriber_provider* должны быть уникальными для поставщика OLE DB, установленного на распространителе. *subscriber_provider* поддерживается только для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подписчиков, отличных от.  
  
`[ @subscriber_datasrc = ] 'subscriber_datasrc'` Имя источника данных, понятное поставщику OLE DB. *subscriber_datasrc* имеет тип **nvarchar (4000)** и значение по умолчанию NULL. *subscriber_datasrc* передается как свойство DBPROP_INIT_DATASOURCE для инициализации поставщика OLE DB. *subscriber_datasrc* поддерживается только для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подписчиков, отличных от.  
  
`[ @subscriber_location = ] 'subscriber_location'` Расположение базы данных, понятное поставщику OLE DB. *subscriber_location* имеет тип **nvarchar (4000)** и значение по умолчанию NULL. *subscriber_location* передается как свойство DBPROP_INIT_LOCATION для инициализации поставщика OLE DB. *subscriber_location* поддерживается только для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подписчиков, отличных от.  
  
`[ @subscriber_provider_string = ] 'subscriber_provider_string'` Строка подключения, зависящая от поставщика OLE DB, которая идентифицирует источник данных. *subscriber_provider_string* имеет тип **nvarchar (4000)** и значение по умолчанию NULL. *subscriber_provider_string* передается в IDataInitialize или устанавливается как свойство DBPROP_INIT_PROVIDERSTRING для инициализации поставщика OLE DB. *subscriber_provider_string* поддерживается только для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подписчиков, отличных от.  
  
`[ @subscriber_catalog = ] 'subscriber_catalog'` Каталог, используемый при установлении соединения с поставщиком OLE DB. Аргумент *subscriber_catalog* имеет тип **sysname** и значение по умолчанию NULL. *subscriber_catalog* передается как свойство DBPROP_INIT_CATALOG для инициализации поставщика OLE DB. *subscriber_catalog* поддерживается только для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подписчиков, отличных от.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Комментарии  
 **sp_addpushsubscription_agent** используется в репликации моментальных снимков и репликации транзакций.  
  
## <a name="example"></a>Пример  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/sp-addpushsubscription-a_1.sql)]  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_addpushsubscription_agent**.  
  
## <a name="see-also"></a>См. также:  
 [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md)   
 [Создание подписки для подписчика, отличного от подписчика SQL Server](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Хранимые процедуры репликации (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  

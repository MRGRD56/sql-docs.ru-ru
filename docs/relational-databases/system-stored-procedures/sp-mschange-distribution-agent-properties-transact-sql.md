---
title: sp_MSchange_distribution_agent_properties (T-SQL)
description: Описывает sp_MSchange_distribution_agent_properties хранимую процедуру, используемую для изменения свойств агент распространения для топологии Репликация SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_MSchange_distribution_agent_properties
- sp_MSchange_distribution_agent_properties_TSQL
helpviewer_keywords:
- sp_MSchange_distribution_agent_properties
ms.assetid: 7dac5e68-bf84-433a-a531-66921f35126f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 37bb9e7af79fbc3ef99febd84cce78eb8db8a557
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195462"
---
# <a name="sp_mschange_distribution_agent_properties-transact-sql"></a>sp_MSchange_distribution_agent_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Изменяет свойства задания агент распространения, выполняемого на [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] распространителе или более поздней версии. Эта хранимая процедура используется для изменения свойств, если издатель запущен на экземпляре [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]. Эта хранимая процедура выполняется на распространителе в базе данных распространителя.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_MSchange_distribution_agent_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'   
        , [ @subscriber = ] 'subscriber'   
        , [ @subscriber_db = ] 'subscriber_db'   
        , [ @property = ] 'property'   
        , [ @value = ] 'value' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publisher = ] 'publisher'` Имя издателя. параметр *Publisher* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @publisher_db = ] 'publisher_db'` Имя базы данных публикации. Аргумент *publisher_db* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @publication = ] 'publication'` Имя публикации. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @subscriber = ] 'subscriber'` Имя подписчика. Аргумент *Subscriber* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @subscriber_db = ] 'subscriber_db'` Имя базы данных подписки. Аргумент *subscriber_db* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @property = ] 'property'` Изменяемое свойство публикации. Аргумент *Property* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @value = ] 'value'` Новое значение свойства. *value* имеет тип **nvarchar (524)** и значение по умолчанию NULL.  
  
 В следующей таблице приводятся свойства задания агента распространителя, доступные для изменения, а также ограничения на значения этих свойств.  
  
|Свойство|Значение|Описание|  
|--------------|-----------|-----------------|  
|**distrib_job_login**||Имя входа учетной записи [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, с которой выполняется агент.|  
|**distrib_job_password**||Пароль учетной записи Windows, под которой запускается задание агента.|  
|**subscriber_catalog**||Каталог, используемый при соединении с поставщиком OLE DB. *Это свойство допустимо только для не-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Подписчики.*|  
|**subscriber_datasource**||Имя источника данных, понятное поставщику OLE DB. *Это свойство допустимо только для не-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Подписчики.*|  
|**subscriber_location**||Местоположение базы данных, понятное поставщику OLE DB. *Это свойство допустимо только для не-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Подписчики.*|  
|**subscriber_login**||Имя входа, используемое при подключении к подписчику для синхронизации подписки.|  
|**subscriber_password**||Пароль подписчика.<br /><br /> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]|  
|**subscriber_provider**||Уникальный программный идентификатор (PROGID), с которым зарегистрирован поставщик OLE DB для источника данных, не относящихся к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *Это свойство допустимо только для не-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Подписчики.*|  
|**subscriber_providerstring**||Идентифицирующая источник данных строка соединения, зависящая от поставщика OLE DB. *Данное свойство допустимо только для подписчиков, отличных от подписчика SQL Server.*|  
|**subscriber_security_mode**|**1**|Проверка подлинности Windows.<br /><br /> [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]|  
||**0**|Проверка подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**subscriber_type**|**0**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Абонент|  
||**1**|Сервер источника данных ODBC|  
||**3**|Поставщик OLE DB|  
|**SubscriptionStreams**||Обозначает количество соединений, разрешенных для агента распространителя с тем, чтобы он применял пакеты изменений параллельно с подписчиком. *Не поддерживается для не-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Подписчики, издатели Oracle или* одноранговые подписки.|  
  
> [!NOTE]  
>  После изменения имени входа и пароля агента необходимо остановить и повторно запустить агент, чтобы изменения вступили в силу.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_MSchange_distribution_agent_properties** используется в репликации моментальных снимков и репликации транзакций.  
  
 При запуске издателя на экземпляре [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] или более поздней версии следует использовать [sp_changesubscription](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md) для изменения свойств задания агент слияния, которое синхронизирует принудительную подписку, которая выполняется на распространителе.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** на распространителе могут выполнять **sp_MSchange_distribution_agent_properties**.  
  
## <a name="see-also"></a>См. также:  
 [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)   
 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)  
  
  

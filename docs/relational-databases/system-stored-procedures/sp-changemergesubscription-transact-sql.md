---
description: sp_changemergesubscription (Transact-SQL)
title: sp_changemergesubscription (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergesubscription_TSQL
- sp_changemergesubscription
helpviewer_keywords:
- sp_changemergesubscription
ms.assetid: fd820f35-c189-4e2d-884d-b60c1c469f58
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 83211e61ad8b6e241f46b1af0ba32266c9f673dd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159819"
---
# <a name="sp_changemergesubscription-transact-sql"></a>sp_changemergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Изменяет выбранные свойства принудительной подписки слиянием. Эта хранимая процедура выполняется на издателе в базе данных публикации.  
  
> [!IMPORTANT]  
>  Если издатель настраивается с удаленным распространителем, то значения, передаваемые для всех аргументов, включая *job_login* и *job_password*, передаются распространителю в формате обычного (незашифрованного) текста. Прежде чем выполнять эту хранимую процедуру, необходимо зашифровать соединение между издателем и его удаленным распространителем. Дополнительные сведения см. в разделе [Включение шифрования соединений в компоненте Database Engine (диспетчер конфигураций SQL Server)](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_changemergesubscription [ [ @publication= ] 'publication' ]  
    [ , [ @subscriber= ] 'subscriber'  
    [ , [ @subscriber_db= ] 'subscriber_db' ]  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publication = ] 'publication'` Имя изменяемой публикации. Аргумент *publication* имеет тип **sysname** и значение по умолчанию NULL. Публикация уже должна существовать и соответствовать правилам для идентификаторов.  
  
`[ @subscriber = ] 'subscriber'` Имя подписчика. Аргумент *Subscriber* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @subscriber_db = ] 'subscriber_db'` Имя базы данных подписки. Аргумент *subscriber_db* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @property = ] 'property'` Свойство, которое необходимо изменить для данной публикации. Аргумент *Property* имеет тип **sysname** и может принимать одно из значений в таблице.  
  
`[ @value = ] 'value'` Новое значение для указанного *Свойства*. *значение* равно **nvarchar (255)** и может быть одним из значений в таблице.  
  
|Свойство|Значение|Описание|  
|--------------|-----------|-----------------|  
|**description**||Описание этой подписки слиянием.|  
|**priority**||Приоритет подписки. При обнаружении конфликтов применяемый по умолчанию сопоставитель выбирает победителя исходя из приоритетов.|  
|**merge_job_login**||Имя входа учетной записи [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, с которой выполняется агент.|  
|**merge_job_password**||Пароль учетной записи Windows, от имени которой выполняется агент.|  
|**publisher_security_mode**|**1**|При подключении к подписчику используется проверка подлинности Windows.|  
||**0**|При подключении к издателю используется проверка подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_login**||Имя входа на издатель.|  
|**publisher_password**||Надежный пароль для указанного имени входа на издатель.|  
|**subscriber_security_mode**|**1**|При подключении к подписчику используется проверка подлинности Windows.|  
||**0**|При подключении к подписчику используется проверка подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**subscriber_login**||Имя входа на подписчик.|  
|**subscriber_password**||Надежный пароль для указанного имени входа на подписчик.|  
|**sync_type**|**Автоматически**|Схема и начальные данные для опубликованных таблиц вначале передаются подписчику.|  
||**Нет**|Подписчик уже имеет схему и начальные данные для опубликованных таблиц; системные таблицы и данные передаются всегда.|  
|**use_interactive_resolver**|**true**|Возможно разрешение конфликтов в интерактивном режиме для всех статей, которые позволяют разрешение конфликтов в интерактивном режиме.|  
||**false**|Конфликты разрешаются в автоматическом режиме с помощью применяемого по умолчанию или пользовательского сопоставителя.|  
|NULL (по умолчанию)|NULL (по умолчанию)||  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_changemergesubscription** используется в репликации слиянием.  
  
 После изменения имени входа и пароля агента необходимо остановить и повторно запустить агент, чтобы изменения вступили в силу.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_changemergesubscription**.  
  
## <a name="see-also"></a>См. также:  
 [sp_addmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)   
 [sp_dropmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md)   
 [sp_helpmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

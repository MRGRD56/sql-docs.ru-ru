---
description: sp_changesubscription (Transact-SQL)
title: sp_changesubscription (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 10/28/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- changesubscription
- sp_changesubscription
- changesubscription_TSQL
- sp_changesubscription_TSQL
helpviewer_keywords:
- sp_changesubscription
ms.assetid: f9d91fe3-47cf-4915-b6bf-14c9c3d8a029
author: markingmyname
ms.author: maghan
ms.openlocfilehash: db0e26092acb2a8cb0fc546a18c0b8f4010fa8fc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189591"
---
# <a name="sp_changesubscription-transact-sql"></a>sp_changesubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Изменяет свойства моментального снимка или транзакционной принудительной подписки или подписки по запросу, участвующей в репликации транзакций, обновляемой посредством очередей. Чтобы изменить свойства всех других типов подписок по запросу, используйте [sp_change_subscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md). **sp_changesubscription** выполняется на издателе в базе данных публикации.  
  
> [!IMPORTANT]  
>  Если издатель настраивается с удаленным распространителем, то значения, передаваемые для всех аргументов, включая *job_login* и *job_password*, передаются распространителю в формате обычного (незашифрованного) текста. Прежде чем выполнять эту хранимую процедуру, необходимо зашифровать соединение между издателем и его удаленным распространителем. Дополнительные сведения см. в разделе [Включение шифрования соединений в компоненте Database Engine (диспетчер конфигураций SQL Server)](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_changesubscription [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
        , [ @subscriber = ] 'subscriber'  
        , [ @destination_db = ] 'destination_db'  
        , [ @property = ] 'property'  
        , [ @value = ] 'value'  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publication = ] 'publication'` Имя изменяемой публикации. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию  
  
`[ @article = ] 'article'` Имя статьи, которую необходимо изменить. Аргумент *article* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @subscriber = ] 'subscriber'` Имя подписчика. Аргумент *Subscriber* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @destination_db = ] 'destination_db'` Имя базы данных подписки. Аргумент *destination_db* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @property = ] 'property'` Свойство, которое необходимо изменить для данной подписки. *свойство* имеет тип **nvarchar (30)** и может быть одним из значений в таблице.  
  
`[ @value = ] 'value'` Новое значение для указанного *Свойства*. *value* имеет тип **nvarchar (4000)** и может быть одним из значений в таблице.  
  
|Свойство|Значение|Описание|  
|--------------|-----------|-----------------|  
|**distrib_job_login**||Имя входа учетной записи [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, с которой выполняется агент.|  
|**distrib_job_password**||Пароль учетной записи Windows, от имени которой выполняется агент.|  
|**subscriber_catalog**||Каталог, используемый при соединении с поставщиком OLE DB. Это свойство допустимо только для [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подписчиков, отличных от.|  
|**subscriber_datasource**||Имя источника данных, понятное поставщику OLE DB. *Это свойство допустимо только для не-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Подписчики.*|  
|**subscriber_location**||Местоположение базы данных, понятное поставщику OLE DB. *Это свойство допустимо только для не-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Подписчики.*|  
|**subscriber_login**||Имя входа на подписчик.|  
|**subscriber_password**||Надежный пароль для указанного имени входа.|  
|**subscriber_security_mode**|**1**|При подключении к подписчику используется проверка подлинности Windows.|  
||**0**|При подключении к подписчику используется проверка подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**subscriber_provider**||Уникальный программный идентификатор (PROGID), с которым зарегистрирован поставщик OLE DB для источника данных, не относящихся к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *Это свойство допустимо только для не-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Подписчики.*|  
|**subscriber_providerstring**||Идентифицирующая источник данных строка соединения, зависящая от поставщика OLE DB. *Это свойство допустимо только для не-* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *Подписчики.*|  
|**SubscriptionStreams**||Количество дозволенных соединений на каждого агента распространителя при применении пакета изменения параллельно с подписчиком. Для издателей поддерживается диапазон значений от **1** до **64** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Это свойство должно иметь значение **0** для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подписчиков, отличных от, издателей Oracle или одноранговых подписок.|  
|**subscriber_type**|**1**|Сервер источника данных ODBC|  
||**3**|Поставщик OLE DB|  
|**memory_optimized**|**bit**|Указывает, что подписка поддерживает оптимизированные для памяти таблицы. *memory_optimized* является **битом**, где 1 равно true (подписка поддерживает оптимизированные для памяти таблицы).|  
  
`[ @publisher = ] 'publisher'` Указывает издателя, отличного от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Аргумент *Publisher* имеет тип **sysname** и значение по умолчанию NULL.  
  
> [!NOTE]  
>  для издателя не следует указывать *издателя* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_changesubscription** используется в моментальных снимках и репликации транзакций.  
  
 **sp_changesubscription** можно использовать только для изменения свойств принудительных подписок или подписок по запросу, участвующих в обновлении репликации транзакций посредством очередей. Чтобы изменить свойства всех других типов подписок по запросу, используйте [sp_change_subscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md).  
  
 После изменения имени входа и пароля агента необходимо остановить и повторно запустить агент, чтобы изменения вступили в силу.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_changesubscription**.  
  
## <a name="see-also"></a>См. также:  
 [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)  
  
  

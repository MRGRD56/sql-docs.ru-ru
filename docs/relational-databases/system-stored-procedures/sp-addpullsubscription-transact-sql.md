---
description: sp_addpullsubscription (Transact-SQL)
title: sp_addpullsubscription (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/29/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addpullsubscription
- sp_addpullsubscription_TSQL
helpviewer_keywords:
- sp_addpullsubscription
ms.assetid: 0f4bbedc-0c1c-414a-b82a-6fd47f0a6a7f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4c8ca5e48718ab2d1abbb5aff9071955ccb78b18
ms.sourcegitcommit: 851f47e27512651f809540b77bfbd09e6ddb5362
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105937821"
---
# <a name="sp_addpullsubscription-transact-sql"></a>sp_addpullsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Добавляет подписку по запросу к моментальному снимку или публикации транзакций. Эта хранимая процедура выполняется на подписчике в базе данных, в которой создается подписка по запросу.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_addpullsubscription [ @publisher= ] 'publisher'  
    [ , [ @publisher_db= ] 'publisher_db' ]  
        , [ @publication= ] 'publication'  
    [ , [ @independent_agent= ] 'independent_agent' ]  
    [ , [ @subscription_type= ] 'subscription_type' ]  
    [ , [ @description= ] 'description' ]  
    [ , [ @update_mode= ] 'update_mode' ]  
    [ , [ @immediate_sync = ] immediate_sync ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publisher = ] 'publisher'` Имя издателя. параметр *Publisher* имеет тип **sysname** и не имеет значения по умолчанию.  

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

> [!NOTE]
> Имя сервера можно указать как `<Hostname>,<PortNumber>` . Может потребоваться указать номер порта для подключения, если SQL Server развертывается в Linux или Windows с помощью настраиваемого порта, а служба браузера отключена. Использование настраиваемых номеров портов для удаленного распространителя применимо только к SQL Server 2019.

::: moniker-end
  
`[ @publisher_db = ] 'publisher_db'` Имя базы данных издателя. Аргумент *publisher_db* имеет тип **sysname** и значение по умолчанию NULL. *publisher_db* не учитываются издателями Oracle.  
  
`[ @publication = ] 'publication'` Имя публикации. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @independent_agent = ] 'independent_agent'` Указывает, существует ли изолированная агент распространения для этой публикации. *independent_agent* имеет тип **nvarchar (5)** и значение по умолчанию true. Если **значение — true**, то для этой публикации существует изолированный агент распространения. Если **значение равно false**, то для каждой пары базы данных издателя или подписчика существует один агент распространения. *independent_agent* является свойством публикации и должно иметь то же значение, что и на издателе.  
  
`[ @subscription_type = ] 'subscription_type'` Тип подписки. *subscription_type* имеет тип **nvarchar (9)** и значение по умолчанию **anonymous**. Необходимо указать значение **Pull** для *subscription_type*, если не требуется создавать подписку без регистрации подписки на издателе. В этом случае необходимо указать значение **anonymous**. Это необходимо в тех случаях, когда невозможно установить соединение [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с издателем в момент настройки подписки.  
  
`[ @description = ] 'description'` Описание публикации. *Description* имеет тип **nvarchar (100)** и значение по умолчанию NULL.  
  
`[ @update_mode = ] 'update_mode'` Тип обновления. *update_mode* имеет тип **nvarchar (30)** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**только для чтения** (по умолчанию)|Подписка только для чтения. Никакие изменения на подписчике не передаются на издатель. Используется в том случае, когда данные на подписчике изменяться не будут.|  
|**синктран**|Включает поддержку немедленно обновляемых подписок.|  
|**queued tran**|Обеспечивает подписку обновляемую посредством очередей. Изменение данных можно выполнять у подписчика, сохранять в очереди и после этого передавать издателю.|  
|**Перемещение**|Включает для подписки немедленное обновление с обновлением по очереди при переходе на другой ресурс в случае отработки отказа. Изменение данных можно выполнять на подписчике и немедленно передавать издателю. Если пропало соединение между издателем и подписчиком, изменения данных, выполненные на подписчике, сохраняются в очереди, пока связь не будет восстановлена.|  
|**queued failover**|Включает подписку с обновлением по очереди в качестве обновляемой посредством очередей подписки, при этом поддерживает возможность переключения в режим немедленного обновления. Изменение данных можно выполнять у подписчика и сохранять в очереди до установления соединения между подписчиком и издателем. При установлении постоянного соединения можно переключиться в режим немедленного обновления. *Не поддерживается для издателей Oracle*.|  
  
`[ @immediate_sync = ] immediate_sync` Указывает, создаются или повторно создаются файлы синхронизации при каждом запуске агент моментальных снимков. *immediate_sync* имеет **бит** со значением по умолчанию 1 и должен иметь то же значение, что и *immediate_sync* в **sp_addpublication**. *immediate_sync* является свойством публикации и должно иметь то же значение, что и на издателе.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Комментарии  
 **sp_addpullsubscription** используется в репликации моментальных снимков и репликации транзакций.  
  
> [!IMPORTANT]  
>  Для обновляемых посредством очередей подписок при соединении с подписчиками используйте проверку подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], указывая для каждого из подписчиков различные учетные записи. При создании подписки по запросу, поддерживающей обновление посредством очередей, репликация всегда устанавливает соединение с использованием проверки подлинности Windows (так как для подписок по запросу репликация не сможет получить доступ к метаданным на подписчике, необходимым для выполнения проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). В этом случае следует выполнить [sp_changesubscription](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md) , чтобы изменить подключение для использования [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] проверки подлинности после настройки подписки.  
  
 Если [MSreplication_subscriptions &#40;таблицы&#41;Transact-SQL](../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md) не существует на подписчике, **sp_addpullsubscription** создает ее. Она также добавляет строку в [MSreplication_subscriptions &#40;таблице&#41;Transact-SQL ](../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md) . Для подписок по запросу [sp_addsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) должны быть вызваны на издателе первыми.  
  
## <a name="example"></a>Пример  
 [!code-sql[HowTo#sp_addtranpullsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/sp-addpullsubscription-t_1.sql)]  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_addpullsubscription**.  
  
## <a name="see-also"></a>См. также:  
 [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md)   
 [Создание обновляемой подписки на публикацию транзакций](../../relational-databases/replication/publish/create-an-updatable-subscription-to-a-transactional-publication.md) [подписки на публикации](../../relational-databases/replication/subscribe-to-publications.md)   
 [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)   
 [sp_change_subscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md)   
 [sp_droppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md)   
 [sp_helppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

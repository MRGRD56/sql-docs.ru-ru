---
description: sp_addmergealternatepublisher (Transact-SQL)
title: sp_addmergealternatepublisher (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addmergealternatepublisher_TSQL
- sp_addmergealternatepublisher
helpviewer_keywords:
- sp_addmergealternatepublisher
ms.assetid: de46e0b1-d946-4021-bff6-2d8e3187656d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7c0caf60fb3b1323c24924f177ca4d8cdf1105ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202358"
---
# <a name="sp_addmergealternatepublisher-transact-sql"></a>sp_addmergealternatepublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Добавляет подписчику возможность использовать альтернативного партнера синхронизации. В свойствах публикации должно быть указано, что подписчики могут синхронизироваться с другими издателями. Эта хранимая процедура выполняется на подписчике в базе данных подписки.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_addmergealternatepublisher [ @publisher= ] 'publisher'  
          , [ @publisher_db= ] 'publisher_db'  
          , [ @publication= ] 'publication'  
          , [ @alternate_publisher= ] 'alternate_synchronization_partner'  
          , [ @alternate_publisher_db= ] 'alternate_publisher_db'  
          , [ @alternate_publication= ] 'alternate_synchronization_partner'  
     , [ @alternate_distributor= ] 'alternate_distributor'   
    [ , [ @friendly_name= ] 'friendly_name' ]   
    [ , [ @reserved= ] 'reserved' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publisher = ] 'publisher'` Имя издателя. параметр *Publisher* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @publisher_db = ] 'publisher_db'` Имя базы данных публикации. Аргумент *publisher_db* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @publication = ] 'publication'` Имя публикации. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @alternate_publisher = ] 'alternate_synchronization_partner'` Имя альтернативного издателя. Аргумент *alternate_synchronization_partner* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @alternate_publisher_db = ] 'alternate_publisher_db'` Имя базы данных публикации на альтернативном издателе. Аргумент *alternate_publisher_db* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @alternate_publication = ] 'alternate_synchronization_partner'` Имя публикации на альтернативном участнике синхронизации. Аргумент *alternate_synchronization_partner* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @alternate_distributor = ] 'alternate_distributor'` Имя распространителя для альтернативного участника синхронизации. Аргумент *alternate_distributor* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @friendly_name = ] 'friendly_name'` Отображаемое имя, по которому может быть идентифицирована Ассоциация издателя, публикации и распространителя, которая состоит из альтернативного участника синхронизации. *friendly_name* имеет тип **nvarchar (255)** и значение по умолчанию NULL.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_addmergealternatepublisher** используется в репликации слиянием.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_addmergealternatepublisher**.  
  
## <a name="see-also"></a>См. также:  
 [sp_dropmergealternatepublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergealternatepublisher-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

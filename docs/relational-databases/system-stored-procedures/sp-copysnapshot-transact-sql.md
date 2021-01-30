---
description: sp_copysnapshot (Transact-SQL)
title: sp_copysnapshot (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_copysnapshot
- sp_copysnapshot_TSQL
helpviewer_keywords:
- sp_copysnapshot
ms.assetid: a012a32f-6f26-45bf-8046-b51cd7fec455
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 76e8f1bead2aaf10289f724b300a79eacd3ca11f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185770"
---
# <a name="sp_copysnapshot-transact-sql"></a>sp_copysnapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Копирует папку моментальных снимков указанной публикации в папку, указанную в **\@ destination_folder**. Эта хранимая процедура выполняется на издателе в базе данных публикации. Данная хранимая процедура полезна для копирования моментального снимка на извлекаемые носители, такие как CD-ROM.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_copysnapshot [ @publication = ] 'publication', [ @destination_folder = ] 'destination_folder' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publication = ] 'publication'` Имя публикации, содержимое моментального снимка которого должно быть скопировано. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @destination_folder = ] 'destination_folder'` Имя папки, в которую будет скопировано содержимое моментального снимка публикации. *destination_folder* имеет тип **nvarchar (255)** и не имеет значения по умолчанию. *Destination_folder* может быть альтернативным расположением, например на другом сервере, на сетевом диске или на съемном носителе (например, CD-ROMs или съемных дисках).  
  
`[ @subscriber = ] 'subscriber'` Имя подписчика. Аргумент *Subscriber* имеет тип sysname и значение по умолчанию NULL.  
  
`[ @subscriber_db = ] 'subscriber_db'` Имя базы данных подписки. Аргумент *subscriber_db* имеет тип sysname и значение по умолчанию NULL.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_copysnapshot** используется во всех типах репликации. Подписчики с [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] версией 7,0 и более ранними версиями не могут использовать альтернативное расположение моментального снимка.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_copysnapshot**.  
  
## <a name="see-also"></a>См. также:  
 [Альтернативные расположения папки моментальных снимков](../../relational-databases/replication/snapshot-options.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

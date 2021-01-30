---
description: sp_helppublication_snapshot (Transact-SQL)
title: sp_helppublication_snapshot (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helppublication_snapshot
- sp_helppublication_snapshot_TSQL
helpviewer_keywords:
- sp_helppublication_snapshot
ms.assetid: 97b4a7ae-40a5-4328-88f1-ff5d105bbb34
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ae7357cd8bc8f03805c6de948095ff2dbc4d541
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210866"
---
# <a name="sp_helppublication_snapshot-transact-sql"></a>sp_helppublication_snapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Возвращает сведения об агенте моментальных снимков для данной публикации. Эта хранимая процедура выполняется на издателе в базе данных публикации.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_helppublication_snapshot [ @publication = ] 'publication'  
    [ , [ @publisher = ] 'publisher' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publication = ] 'publication'` Имя публикации. Аргумент *publication* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @publisher = ] 'publisher'` Указывает издателя, отличного от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Аргумент *Publisher* имеет тип **sysname** и значение по умолчанию NULL.  
  
> [!NOTE]  
>  *Издатель* не следует использовать при добавлении статьи к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] издателю.  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**идентификатор**|**int**|Идентификатор агента моментальных снимков.|  
|**name**|**nvarchar (100)**|Имя агента моментальных снимков.|  
|**publisher_security_mode**|**smallint**|Режим безопасности, используемый агентом при соединении с издателем, который может быть одним из следующих:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Проверка подлинности<br /><br /> **1** = проверка подлинности Windows.|  
|**publisher_login**|**sysname**|Имя входа в систему, используемое при соединении с издателем.|  
|**publisher_password**|**nvarchar (524)**|По соображениям безопасности **\*\*\*\*\*\*\*\*\*\*** всегда возвращается значение.|  
|**job_id**|**uniqueidentifier**|Уникальный идентификатор задания агента.|  
|**job_login**|**nvarchar(512)**|Учетная запись Windows, под которой выполняется агент моментальных снимков, которая возвращается в формате *домен* \\ *имя_пользователя*.|  
|**job_password**|**sysname**|По соображениям безопасности **\*\*\*\*\*\*\*\*\*\*** всегда возвращается значение.|  
|**schedule_name**|**sysname**|Имя расписания, которое используется для этого задания агента.|  
|**frequency_type**|**int**|Частота, с которой агент выполняется по расписанию, может иметь одно из следующих значений.<br /><br /> **1** = один раз<br /><br /> **2** = по запросу<br /><br /> **4** = ежедневно<br /><br /> **8** = еженедельно<br /><br /> **16** = ежемесячно<br /><br /> **32** = ежемесячное относительное<br /><br /> **64** = Автозапуск<br /><br /> **128** = повторяющаяся|  
|**frequency_interval**|**int**|День, когда агент выполняется, может иметь одно из следующих значений.<br /><br /> **1** = воскресенье<br /><br /> **2** = понедельник<br /><br /> **3** = вторник<br /><br /> **4** = среда<br /><br /> **5** = четверг<br /><br /> **6** = Пятница<br /><br /> **7** = Суббота<br /><br /> **8** = день<br /><br /> **9** = рабочие дни<br /><br /> **10** = выходные дни|  
|**frequency_subday_type**|**int**|Тип, определяющий частоту запуска агента, когда *frequency_type* равен **4** (ежедневно) и может принимать одно из следующих значений.<br /><br /> **1** = в указанное время<br /><br /> **2** = секунды<br /><br /> **4** = минуты<br /><br /> **8** = часы|  
|**frequency_subday_interval**|**int**|Количество интервалов *frequency_subday_type* , которые происходят между запланированным выполнением агента.|  
|**frequency_relative_interval**|**int**|Неделя, в которую агент работает в заданном месяце, если *frequency_type* равен **32** (ежемесячное относительное) и может принимать одно из следующих значений.<br /><br /> **1** = сначала<br /><br /> **2** = секунда<br /><br /> **4** = третий<br /><br /> **8** = четвертый<br /><br /> **16** = Последняя|  
|**frequency_recurrence_factor**|**int**|Число недель или месяцев между запланированными выполнениями агента.|  
|**active_start_date**|**int**|Дата, когда агент будет впервые выполнен, в формате ГГГГММДД.|  
|**active_end_date**|**int**|Дата, когда агент будет выполнен в последний раз, в формате ГГГГММДД.|  
|**active_start_time**|**int**|Время, когда агент будет впервые выполнен, в формате ЧЧММСС.|  
|**active_end_time**|**int**|Время, когда агент будет выполнен в последний раз, в формате ЧЧММСС.|  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_help_publication_snapshot** используется во всех типах репликации.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** на издателе или члены предопределенной роли базы данных **db_owner** в базе данных публикации могут выполнять **sp_help_publication_snapshot**.  
  
## <a name="see-also"></a>См. также:  
 [Просмотр и изменение свойств публикации](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [sp_addpublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-snapshot-transact-sql.md)   
 [sp_changepublication_snapshot &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changepublication-snapshot-transact-sql.md)   
 [sp_dropmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [sp_droppublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md)  
  
  

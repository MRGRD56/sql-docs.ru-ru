---
description: sys.dm_db_index_usage_stats (Transact-SQL)
title: sys.dm_db_index_usage_stats (Transact-SQL)
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_index_usage_stats_TSQL
- sys.dm_db_index_usage_stats
- sys.dm_db_index_usage_stats_TSQL
- dm_db_index_usage_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_index_usage_stats dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 246eb108870a487c3d58ca2c4df3acb7a9fb2aa7
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551605"
---
# <a name="sysdm_db_index_usage_stats-transact-sql"></a>sys.dm_db_index_usage_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает количество различных операций с индексами и время, которое было затрачено на последнее выполнение операции каждого типа.  
  
 Динамические административные представления в среде [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не могут предоставлять информацию, которая может повлиять на автономность базы данных, или информацию о других базах данных, к которым имеет доступ пользователь. Во избежание раскрытия этой информации все строки, содержащие данные, не принадлежащие подключенному клиенту, отфильтровываются.  
  
> [!NOTE]  
> Динамическое административное представление `sys.dm_db_index_usage_stats` не возвращает сведения о индексах, оптимизированных для памяти, или пространственных индексах. Сведения об использовании индексов, оптимизированных для памяти, см. в разделе [sys.dm_db_xtp_index_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md).  
  
> [!NOTE]  
>  Чтобы вызвать это представление из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте `sys.dm_pdw_nodes_db_index_usage_stats` .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**database_id**|**smallint**|Идентификатор базы данных, в которой определены таблица или представление.|  
|**object_id**|**int**|Идентификатор таблицы или представления, для которых определен индекс.|  
|**index_id**|**int**|Идентификатор индекса.|  
|**user_seeks**|**bigint**|Количество операций поиска по запросам пользователя.|  
|**user_scans**|**bigint**|Число просмотров пользовательских запросов, не использующих предикат SEEK.|  
|**user_lookups**|**bigint**|Количество уточняющих запросов для запросов пользователей.|  
|**user_updates**|**bigint**|Количество операций обновления по запросам пользователя. Сюда входят операции вставки, удаления и обновления, представляющие количество операций, которые не были затронуты реальными строками. Например, при удалении 1000 строк в одной инструкции это число увеличивается на 1.|  
|**last_user_seek**|**datetime**|Время последней операции поиска, выполненной пользователем.|  
|**last_user_scan**|**datetime**|Время последней операции просмотра, выполненной пользователем.|  
|**last_user_lookup**|**datetime**|Время последней операции поиска соответствия, выполненной пользователем.|  
|**last_user_update**|**datetime**|Время последней операции обновления, выполненной пользователем.|  
|**system_seeks**|**bigint**|Количество операций поиска по системным запросам.|  
|**system_scans**|**bigint**|Количество операций просмотра по системным запросам.|  
|**system_lookups**|**bigint**|Количество уточняющих запросов для системных запросов.|  
|**system_updates**|**bigint**|Количество операций обновления по системным запросам.|  
|**last_system_seek**|**datetime**|Время последней системной операции поиска.|  
|**last_system_scan**|**datetime**|Время последней системной операции просмотра.|  
|**last_system_lookup**|**datetime**|Время последнего системного уточняющего запроса.|  
|**last_system_update**|**datetime**|Время последней системной операции обновления.|  
|pdw_node_id|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="remarks"></a>Примечания  
 Каждая отдельная операция поиска, просмотра, уточняющего запроса или обновления на заданном индексе при выполнении одного запроса засчитывается как использование этого индекса и увеличивает на единицу соответствующий счетчик в данном представлении. Данные выводятся как для операций, вызванных пользовательскими запросами, так и для операций, вызванных внутренними запросами, например при выполнении операции просмотра для сбора статистики.  
  
 `user_updates`Столбец является счетчиком обслуживания индекса, вызванного операциями вставки, обновления или удаления базовой таблицы или представления. С помощью этого представления можно определять, какие индексы используются приложениями лишь в незначительной степени. Можно также определять, какие индексы привносят дополнительную нагрузку, связанную с обслуживанием. Стоит рассмотреть возможность удаления индексов, вызывающих дополнительную нагрузку, связанную с их обслуживанием, но не использующихся для выполнения запросов или использующихся лишь иногда.  
  
 Счетчики инициализируются пустыми при запуске ядра СУБД. Используйте `sqlserver_start_time` столбец в [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) , чтобы найти время последнего запуска ядра СУБД. Кроме того, когда база данных отсоединяется или останавливается (например, если аргументу AUTO_CLOSE присвоено значение ON), из представления удаляются все строки, связанные с этой базой данных.  
  
 При использовании индекса добавляется строка, `sys.dm_db_index_usage_stats` Если для индекса еще не существует строка. При добавлении строки ее счетчики вначале сбрасываются в нуль.  
  
 Во время обновления до [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] , [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] или [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] , записи в `sys.dm_db_index_usage_stats` удаляются. Начиная с версии [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] , записи сохраняются в том виде, в котором они были раньше [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] .  
  
## <a name="permissions"></a>Разрешения  
В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.  
  
## <a name="see-also"></a>См. также:  

 [Динамические административные представления и функции, связанные с индексами &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_db_index_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_operational_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](sys-dm-os-sys-info-transact-sql.md)    
 [Наблюдение и настройка производительности](../../relational-databases/performance/monitor-and-tune-for-performance.md)    

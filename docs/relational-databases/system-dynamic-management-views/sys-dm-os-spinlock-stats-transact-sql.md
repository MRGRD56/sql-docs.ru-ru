---
description: sys.dm_os_spinlock_stats (Transact-SQL)
title: sys.dm_os_spinlock_stats (Transact-SQL)
ms.custom: ''
ms.date: 02/10/2021
ms.prod: sql-non-specified
ms.prod_service: database-engine
ms.service: ''
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
f1_keywords:
- sys.dm_os_spinlock_stats_TSQL
- dm_os_spinlock_stats_TSQL
- dm_os_spinlock_stats
- sys.dm_os_spinlock_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_spinlock_stats dynamic management view
author: bluefooted
ms.author: pamela
ms.reviewer: wiassaf
manager: amitban
ms.openlocfilehash: e41cb56133deb7cc12899840af877e7aa91023c3
ms.sourcegitcommit: f1a571b6ce02a39c385ad32508ceff23475ed9f5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2021
ms.locfileid: "106377454"
---
# <a name="sysdm_os_spinlock_stats-transact-sql"></a>sys.dm_os_spinlock_stats (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает сведения обо всех ожиданиях взаимоблокировок, упорядоченных по типу.  
  

|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(256)**|Имя типа спин.|  
|конфликтов|**bigint**|Количество раз, когда поток пытается получить спин-блокировку и блокируется, так как в настоящее время объект взаимоблокировки находится в другом потоке.|  
|запускает|**bigint**|Число раз, когда поток выполняет цикл при попытке получить спин-блокировку.|  
|spins_per_collision|**real**|Количество вращений на один конфликт.|  
|sleep_time|**bigint**|Время в миллисекундах, в течение которого потоки потратили на спящий режим в случае задержки.|  
|логарифмы отношения вероятностей|**bigint**|Количество случаев, когда поток "вращается" не может получить спин-блокировку и выдает планировщик.|  


## <a name="permissions"></a>Разрешения  
В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.    
  
## <a name="remarks"></a>Remarks  
 
 sys.dm_os_spinlock_stats можно использовать для обнаружения источника конфликтов спина. В некоторых случаях может быть разрешено или устранено состязание за взаимоблокировки. Но возможны и такие ситуации, когда будет необходимо обратиться в службу поддержки пользователей [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
 Вы можете сбросить содержимое sys.dm_os_spinlock_stats, используя следующую команду `DBCC SQLPERF` :  
  
```  
DBCC SQLPERF ('sys.dm_os_spinlock_stats', CLEAR);  
GO  
```  
  
 В результате все счетчики будут обнулены.  
  
> [!NOTE]  
>  При перезапуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] эти статистические данные не сохраняются. Все данные представляют собой совокупные значения, полученные за время после последнего сброса статистических сведений или со времени запуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="spinlocks"></a>Спин-блокировки  
 Спин-блокировка — это объект облегченной синхронизации, используемый для сериализации доступа к структурам данных, которые обычно удерживаются в течение короткого периода времени. Когда поток пытается получить доступ к ресурсу, защищенному с помощью спин-блокировки, которая удерживается другим потоком, поток выполняет цикл или "Spin" и пытается получить доступ к ресурсу еще раз, вместо того чтобы сразу же получать планировщик, как в случае кратковременной блокировки или другого времени ожидания ресурса. Поток будет продолжать вращаться до тех пор, пока ресурс не будет доступен, или завершится цикл, после чего поток выдаст планировщик и вернется в очередь готовности. Такой подход позволяет уменьшить чрезмерное переключение контекста потока, но если состязание происходит очень высоко, может наблюдаться значительная загрузка ЦП.

> [!NOTE]  
>  Если вы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] установили на процессоры Intel Skylake, ознакомьтесь с [этой статьей](https://support.microsoft.com/topic/kb4538688-fix-severe-spinlock-contention-occurs-in-sql-server-2019-43faea65-fdcb-6835-f7fe-93abdb235837) , чтобы применить требуемое обновление и включить флаг трассировки 8101.

 В следующей таблице содержатся краткие описания некоторых наиболее распространенных типов взаимоблокировок.  
  
|Тип спин|Описание|  
|-----------------|-----------------|  
|ABR|Только для внутреннего применения.|
|ADB_CACHE|Только для внутреннего применения.|
|ALLOC_CACHES_HASH|Только для внутреннего применения.|
|APPENDONLY_STORAGE|Только для внутреннего применения.|
|APRC_BACK_OFF_STATS|Только для внутреннего применения.|
|APRC_EVENT_LIST|Только для внутреннего применения.|
|APRC_QUEUE_LIST|Только для внутреннего применения.|
|APRC_VALIDATION_QUEUE_LIST|Только для внутреннего применения.|
|ASYNC_OP_ADMIN_CLIENT_REGISTRATION_LIST|Только для внутреннего применения.|
|ASYNC_OP_ADMIN_WORK_REGISTRATION_HASH_TABLE|Только для внутреннего применения.|
|асинкстатслист|Только для внутреннего применения.|
|BACKUP|Только для внутреннего применения.|
|BACKUP_COPY_CONTEXT|Только для внутреннего применения.|
|BACKUP_CTX|Только для внутреннего применения.|
|BASE_XACT_HASH|Только для внутреннего применения.|
|BLOCKER_ENUM|Только для внутреннего применения.|
|бпрепартитион|Только для внутреннего применения.|
|бпворкфиле|Только для внутреннего применения.|
|BUF_HASH|Только для внутреннего применения.|
|BUF_LINK|Только для внутреннего применения.|
|BUF_WRITE_LOG|Только для внутреннего применения.|
|CACHEOBJ_DBG|Только для внутреннего применения.|
|чаннелфорцеклосеманажер|Только для внутреннего применения.|
|CHECK_AGGREGATE_STATE|Только для внутреннего применения.|
|CLR_HOSTTASK|Только для внутреннего применения.|
|CLR_SPIN_LOCK|Только для внутреннего применения.|
|CMED_DATABASE|Только для внутреннего применения.|
|CMED_HASH_SET|Только для внутреннего применения.<br><br>**Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] через [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] CU1)<br><br> **Примечание.** это имя взаимоблокировки изменяется на LOCK_RW_CMED_HASH_SET после применения [SQL Server 2016 Cu2](https://support.microsoft.com/topic/kb3195888-fix-high-cpu-usage-causes-performance-issues-in-sql-server-2016-and-2017-9514b80d-938f-e179-3131-74e6c757c4d5).|
|колумндатасетсессионлист|Только для внутреннего применения.|
|COLUMNSTORE_HASHTABLE|Только для внутреннего применения.|
|COLUMNSTOREBUILDSTATE_LIST|Только для внутреннего применения.|
|COM_INIT|Только для внутреннего применения.|
|НЕФИКСИРУЕМУЮ|Только для внутреннего применения.|
|COMPPLAN_SKELETON|Только для внутреннего применения.|
|CONNECTION_MANAGER|Только для внутреннего применения.|
|СОЕДИНЯЮЩ|Только для внутреннего применения.|
|ксибуилдмем|Только для внутреннего применения.|
|CURSOR|Только для внутреннего применения.|
|курскл|Только для внутреннего применения.|
|датапортконсумер|Только для внутреннего применения.|
|датапортсаурцеинфокредит|Только для внутреннего применения.|
|датапортсаурцеинфокуеуе|Только для внутреннего применения.|
|DATASET_FREELIST|Только для внутреннего применения.|
|DBCC_CHECK|Только для внутреннего применения.|
|DBSEEDING_OPERATION|Только для внутреннего применения.|
|DBT_HASH|Только для внутреннего применения.|
|DBT_IO_LIST|Только для внутреннего применения.|
|DBTABLE|Управляет доступом к структуре данных в памяти для каждой базы данных в [!INCLUDE[ssde_md](../../includes/ssde_md.md)] , содержащей свойства этой базы данных. Дополнительные сведения см. в [этой статье](https://techcommunity.microsoft.com/t5/SQL-Server/Improving-Concurrency-Scalability-of-SQL-Server-workload-by/ba-p/384789). |
|DEFERRED_WF_EXT_DROP|Только для внутреннего применения.|
|DEK_INSTANCE|Только для внутреннего применения.|
|DELAYED_PARTITIONED_STACK|Только для внутреннего применения.|
|делетебитмап|Только для внутреннего применения.|
|DIAG_MANAGER|Только для внутреннего применения.|
|DIAG_OBJECT|Только для внутреннего применения.|
|DIGEST_CACHE|Только для внутреннего применения.|
|динпбуф|Только для внутреннего применения.|
|директлогконсумер|Только для внутреннего применения.|
|DP_LIST|Управляет доступом к списку «грязных» страниц для базы данных с включенной косвенной контрольной точкой. Примените исправления из [статьи 4497928](https://support.microsoft.com/kb/4497928), в [статье 4040276](https://support.microsoft.com/kb/4040276)или используйте [флаг трассировки 3468](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md). Дополнительные сведения см. в [этой статье](https://techcommunity.microsoft.com/t5/SQL-Server/Indirect-Checkpoint-and-tempdb-8211-the-good-the-bad-and-the-non/ba-p/385510).|
|DROP|Только для внутреннего применения.|
|DROP_TEMPO|Только для внутреннего применения.|
|DROPPED_ALLOC_UNIT|Только для внутреннего применения.|
|DTC_HASHTABLE|Только для внутреннего применения.|
|DTT_LIST|Только для внутреннего применения.|
|ENDD_LIST|Только для внутреннего применения.|
|EXT_CACHE|Только для внутреннего применения.|
|EXTENT_ACTIVATION|Только для внутреннего применения.|
|FABRIC_DB_MGR_PTR|Только для внутреннего применения.|
|FABRIC_LOG_MANAGEMENT_INPUT_VALUE|Только для внутреннего применения.|
|FABRIC_REPLICA_TRANSPORT|Только для внутреннего применения.|
|FABRIC_TVF_DATA_CONSUMER_LIST|Только для внутреннего применения.|
|FABRIC_TVF_LOAD_LIB|Только для внутреннего применения.|
|FCB_REPLICA_SYNC|Только для внутреннего применения.|
|FGCB_PRP_FILL|Только для внутреннего применения.|
|FILE_HANDLE_CACHE|Только для внутреннего применения.|
|FILE_TABLE|Только для внутреннего применения.|
|FILESTREAM_CHUNKER|Только для внутреннего применения.|
|FREE_SPACE_CACHE_ENTRY|Только для внутреннего применения.|
|FS_CONTAINER_LIST_WITH_DELETE|Только для внутреннего применения.|
|FS_DELETED_FOLDER_CLEANUP|Только для внутреннего применения.|
|FSAGENT|Только для внутреннего применения.|
|FSGHOST_STATUS|Только для внутреннего применения.|
|FT_INIT|Только для внутреннего применения.|
|GHOST_FREE|Только для внутреннего применения.|
|GHOST_HASH|Только для внутреннего применения.|
|GLOBAL_SCHEDULER_LIST|Только для внутреннего применения.|
|GLOBAL_TRACE_FLAGS|Только для внутреннего применения.|
|глобалтранс|Только для внутреннего применения.|
|GROUP_COMMIT_FEEDBACK_LOOP|Только для внутреннего применения.|
|GUARDIAN|Только для внутреннего применения.|
|HADR_AGH_X_ACCESS|Только для внутреннего применения.|
|HADR_AR_CONTROLLER_COLLECTION|Только для внутреннего применения.|
|HADR_AR_DB_MGR|Только для внутреннего применения.|
|HADR_AR_TRANSPORT|Только для внутреннего применения.|
|HADR_COMPRESSION_MGR_POOL|Только для внутреннего применения.|
|HADR_FABRIC_FACTORY|Только для внутреннего применения.|
|HADR_PRIORITY_QUEUE|Только для внутреннего применения.|
|HADR_TRANSPORT_CONTROL|Только для внутреннего применения.|
|HADR_TRANSPORT_LIST|Только для внутреннего применения.|
|хадрсидинглист|Только для внутреннего применения.|
|HOBT_DROPPED|Только для внутреннего применения.|
|HOBT_HASH|Только для внутреннего применения.|
|HTTP|Только для внутреннего применения.|
|HTTP_CONNCACHE|Только для внутреннего применения.|
|HTTP_ENDPOINT|Только для внутреннего применения.|
|IDENTITY|Только для внутреннего применения.|
|INDEX_CREATE|Только для внутреннего применения.|
|IO_DISPENSER_PAUSE|Только для внутреннего применения.|
|IO_RG_VOLUME_HASHTABLE|Только для внутреннего применения.|
|иорек|Только для внутреннего применения.|
|иссресаурце|Только для внутреннего применения.|
|KTM_ENLISTMENT|Только для внутреннего применения.|
|LANG_RES_LOAD|Только для внутреннего применения.|
|LIVE_TARGET_TVF|Только для внутреннего применения.|
|LOCK_FREE_LIST|Только для внутреннего применения.|
|LOCK_HASH|Защищает доступ к хэш-таблице диспетчера блокировок, в которой хранятся сведения о блокировках, удерживаемых в базе данных. Дополнительные сведения см. в [этой статье](https://support.microsoft.com/kb/2926217) и [руководстве по блокировке транзакций и управлении версиями строк](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine).|
|LOCK_NOTIFICATION|Только для внутреннего применения.|
|LOCK_RESOURCE_ID|Только для внутреннего применения.|
|LOCK_RW_ABTX_HASH_SET|Только для внутреннего применения.|
|LOCK_RW_AGDB_HEALTH_DIAG|Только для внутреннего применения.|
|LOCK_RW_CMED_HASH_SET|Только для внутреннего применения.<br><br>**Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Начиная с [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] CU2), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] и [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]|
|LOCK_RW_DPT_TABLE|Только для внутреннего применения.|
|LOCK_RW_IN_ROW_TRACKER|Только для внутреннего применения.|
|LOCK_RW_LOGIN_RATE_STATS|Только для внутреннего применения.|
|LOCK_RW_PVS_PAGE_TRACKER|Только для внутреннего применения.|
|LOCK_RW_RBIO_REQ|Только для внутреннего применения.|
|LOCK_RW_SECURITY_CACHE|Защищает записи кэша, связанные с маркерами безопасности и проверками доступа. <br><br>**Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (Начиная с [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] CU2), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] и [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]<br><br> Если записи в хранилище кэша TokenAndPermUserStore постоянно увеличиваются, вы можете заметить большие обороты для этого взаимоблокировки. Оцените использование [флагов трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4610 и 4618 для ограничения записей. Дополнительные сведения см. в разделе [доступ к параметрам конфигурации сервера для проверки кэша](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md), этой [статье](https://support.microsoft.com/topic/queries-take-a-longer-time-to-finish-running-when-the-size-of-the-tokenandpermuserstore-cache-grows-in-sql-server-2005-ad1622e7-3bb5-7902-19a0-5d0e6271033d)и в этом [блоге](https://techcommunity.microsoft.com/t5/sql-server-support/query-performance-issues-associated-with-a-large-sized-security/ba-p/315494).|
|LOCK_RW_TEST|Только для внутреннего применения.|
|LOCK_RW_WPR_BUCKET|Только для внутреннего применения.|
|LOCK_SORT_STREAM|Только для внутреннего применения.|
|LOCK_SQLSATELLITE_MESSAGE|Только для внутреннего применения.|
|LOG_CONSOLIDATION|Только для внутреннего применения.|
|LOG_RG_GOVERNOR|Только для внутреннего применения.|
|LOGCACHE_ACCESS|Только для внутреннего применения.|
|логфлушк|Только для внутреннего применения.|
|логиосек|Только для внутреннего применения.|
|логиосекмаппендингмессажекуеуе|Только для внутреннего применения.|
|логлк|Только для внутреннего применения.|
|логлфм|Только для внутреннего применения.|
|LOGON_TRIGGER_CACHE|Только для внутреннего применения.|
|LOGPOOL_HASHBUCKET|Только для внутреннего применения.|
|LOGPOOL_REFCOUNTEDOBJECT|Только для внутреннего применения.|
|LOGPOOL_SHAREDCACHEBUFFER|Только для внутреннего применения.|
|LOGPOOL_SIZEPERRESOURCEPOOL|Только для внутреннего применения.|
|LPE_BATCH|Только для внутреннего применения.|
|LPE_SESSION|Только для внутреннего применения.|
|LPE_SXTP|Только для внутреннего применения.|
|лсид|Только для внутреннего применения.|
|лслист|Только для внутреннего применения.|
|лснрефлист|Только для внутреннего применения.|
|LSS_SYNC_DTC|Только для внутреннего применения.|
|MD_CHANGE_NOTIFICATION|Только для внутреннего применения.|
|MDB_REMOTE_BATCH_STATS_HASH_TABLE|Только для внутреннего применения.|
|MDB_REMOTE_SESSION_HASH_TABLE|Только для внутреннего применения.|
|MEM_MGR|Только для внутреннего применения.|
|MGR_CACHE|Только для внутреннего применения.|
|MIGRATION_BUF_LIST|Только для внутреннего применения.|
|ПРИНАДЛЕЖАЩ|Защищает записи кэша, связанные с маркерами безопасности и проверками доступа. <br><br>**Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (До [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)] )<br><br>Если записи в хранилище кэша TokenAndPermUserStore постоянно увеличиваются, вы можете заметить большие обороты для этого взаимоблокировки. Оцените использование [флагов трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4610 и 4618 для ограничения записей. Дополнительные сведения см. в разделе [доступ к параметрам конфигурации сервера для проверки кэша](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md), этой [статье](https://support.microsoft.com/topic/queries-take-a-longer-time-to-finish-running-when-the-size-of-the-tokenandpermuserstore-cache-grows-in-sql-server-2005-ad1622e7-3bb5-7902-19a0-5d0e6271033d)и в этом [блоге](https://techcommunity.microsoft.com/t5/sql-server-support/query-performance-issues-associated-with-a-large-sized-security/ba-p/315494).|
|NETCONN_ADDRESS|Только для внутреннего применения.|
|ONDEMAND_TASK|Только для внутреннего применения.|
|ONE_PROC_SIM_NODE_CONTEXT|Только для внутреннего применения.|
|ONE_PROC_SIM_NODE_CONTEXT_LIST|Только для внутреннего применения.|
|ONE_PROC_SIM_REPLICA_CONTEXT|Только для внутреннего применения.|
|ONE_PROC_SIM_SERVICE_PARTITION|Только для внутреннего применения.|
|OPT_IDX_MISS_ID|Только для внутреннего применения.|
|OPT_IDX_MISS_KEY|Только для внутреннего применения.|
|OPT_IDX_STATS|Только для внутреннего применения.|
|OPT_INFO_MGR|Только для внутреннего применения.|
|PAGE_WORKITEMLIST|Только для внутреннего применения.|
|пажекопиер|Только для внутреннего применения.|
|параллелредокаче|Только для внутреннего применения.|
|PARTITIONED_HEAP_FREE_LIST|Только для внутреннего применения.|
|PROGRESS_REPORT|Только для внутреннего применения.|
|QE_SHUTDOWN|Только для внутреннего применения.|
|QSCAN_CACHE|Только для внутреннего применения.|
|QUERY_EXEC_STATS|Только для внутреннего применения.|
|QUERY_STORE_ASYNC_PERSIST|Только для внутреннего применения.|
|QUERY_STORE_ASYNC_QUEUE_TLIST|Только для внутреннего применения.|
|QUERY_STORE_CAPTURE_POLICY_INTERVAL|Только для внутреннего применения.|
|QUERY_STORE_CAPTURE_POLICY_STATS|Только для внутреннего применения.|
|QUERY_STORE_CAPTURE_POLICY_THRESHOLD|Только для внутреннего применения.|
|QUERY_STORE_CURRENT_INTERVAL|Только для внутреннего применения.|
|QUERY_STORE_HT_CACHE|Только для внутреннего применения.|
|QUERY_STORE_LIST|Только для внутреннего применения.|
|QUERY_STORE_PLAN_COMP_AGG|Только для внутреннего применения.|
|QUERY_STORE_PLAN_LIST|Только для внутреннего применения.|
|QUERY_STORE_READ_ONLY_FLAGS|Только для внутреннего применения.|
|QUERY_STORE_SELF_AGG|Только для внутреннего применения.|
|QUERY_STORE_STMT_COMP_AGG|Только для внутреннего применения.|
|куерексек|Только для внутреннего применения.|
|куерискан|Только для внутреннего применения.|
|RANGE_GENERATION|Только для внутреннего применения.|
|READ_AHEAD|Только для внутреннего применения.|
|редомгрстате|Только для внутреннего применения.|
|REMOTE_SESSION_CACHE|Только для внутреннего применения.|
|ремотеблоккио|Только для внутреннего применения.|
|ремотеоп|Только для внутреннего применения.|
|REPL_LOGREADER_HISTORY_CACHE|Только для внутреннего применения.|
|REPL_LOGREADER_PERDB_HISTORY_CACHE|Только для внутреннего применения.|
|ресманажер|Только для внутреннего применения.|
|РЕСУРС|Только для внутреннего применения.|
|рескуеуе|Только для внутреннего применения.|
|RFS_THREAD_QUEUE|Только для внутреннего применения.|
|RG_TIMER|Только для внутреннего применения.|
|ROWGROUP_VERSIONS|Только для внутреннего применения.|
|рпкчаннелпул|Только для внутреннего применения.|
|рпкпаккаже|Только для внутреннего применения.|
|рпкрекуесторконтекст|Только для внутреннего применения.|
|RWLOCK_LAST|Только для внутреннего применения.|
|SATELLITE_CONNECTION|Только для внутреннего применения.|
|SBS_CLIENT_ENDPOINTS|Только для внутреннего применения.|
|SBS_CLIENT_REQUESTS|Только для внутреннего применения.|
|SBS_DISPATCH|Только для внутреннего применения.|
|SBS_PENDING|Только для внутреннего применения.|
|SBS_SERVER_XACT_TASK_PROXY|Только для внутреннего применения.|
|SBS_TRANSPORT|Только для внутреннего применения.|
|SBS_UCS_DISPATCH|Только для внутреннего применения.|
|Безопасность|Только для внутреннего применения.|
|SECURITY_CACHE|Защищает записи кэша, связанные с маркерами безопасности и проверками доступа. <br><br>**Применимо к:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] через [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] CU1)<br><br> Если записи в хранилище кэша TokenAndPermUserStore постоянно увеличиваются, вы можете заметить большие обороты для этого взаимоблокировки. Оцените использование [флагов трассировки](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 4610 и 4618 для ограничения записей. Дополнительные сведения см. в разделе [доступ к параметрам конфигурации сервера для проверки кэша](../../database-engine/configure-windows/access-check-cache-server-configuration-options.md), этой [статье](https://support.microsoft.com/topic/queries-take-a-longer-time-to-finish-running-when-the-size-of-the-tokenandpermuserstore-cache-grows-in-sql-server-2005-ad1622e7-3bb5-7902-19a0-5d0e6271033d)и в этом [блоге](https://techcommunity.microsoft.com/t5/sql-server-support/query-performance-issues-associated-with-a-large-sized-security/ba-p/315494).<br><br>**Примечание.** это имя взаимоблокировки изменяется на LOCK_RW_SECURITY_CACHE после применения [SQL Server 2016 Cu2](https://support.microsoft.com/topic/kb3195888-fix-high-cpu-usage-causes-performance-issues-in-sql-server-2016-and-2017-9514b80d-938f-e179-3131-74e6c757c4d5).|
|SECURITY_FEDAUTH_AAD_BECWSCONNS|Только для внутреннего применения.|
|SEMANTIC_TICACHE|Только для внутреннего применения.|
|SEQUENCED_OBJECT|Только для внутреннего применения.|
|SEQUEUE_SIZED_THREADSAFE|Только для внутреннего применения.|
|SESSION_KILLER|Только для внутреннего применения.|
|SESSION_MANAGER|Только для внутреннего применения.|
|SESSION_SEC_CONTEXT|Только для внутреннего применения.|
|SETRANGE_SYNC|Только для внутреннего применения.|
|SHARABLE_SESSION_OBJECTS|Только для внутреннего применения.|
|SLO_INFO_LIST|Только для внутреннего применения.|
|ФУНКЦИЙ|Только для внутреннего применения.|
|SNI_NODE_PENDING_IO_QUEUE|Только для внутреннего применения.|
|соапсессионс|Только для внутреннего применения.|
|SOS_ABORT_TASK|Только для внутреннего применения.|
|SOS_ACTIVEDESCRIPTOR|Только для внутреннего применения.|
|SOS_BLOCKALLOCPARTIALLIST|Только для внутреннего применения.|
|SOS_BLOCKDESCRIPTORBUCKET|Только для внутреннего применения.|
|SOS_CACHESTORE|Синхронизирует доступ к различным кэшам в памяти в [!INCLUDE[ssde_md](../../includes/ssde_md.md)] , таким как кэш планов или кэш временных таблиц. Интенсивное состязание за этот тип спин-блокировки может означать множество различных моментов в зависимости от конкретного кэша, который находится в состязании. Обратитесь [!INCLUDE[msCoName](../../includes/msconame-md.md)] в службу поддержки пользователей за помощью по устранению неполадок этого типа взаимоблокировки. |
|SOS_CACHESTORE_CLOCK|Только для внутреннего применения.|
|SOS_CLOCKALG_INTERNODE_SYNC|Только для внутреннего применения.|
|SOS_DEBUG_HOOK|Только для внутреннего применения.|
|SOS_DESCDATABUFFERLIST|Только для внутреннего применения.|
|SOS_LARGEPAGE_ALLOCATOR|Только для внутреннего применения.|
|SOS_MINITHREAD|Только для внутреннего применения.|
|SOS_NODE|Только для внутреннего применения.|
|SOS_OBJECT_POOL|Только для внутреннего применения.|
|SOS_OBJECT_STORE|Только для внутреннего применения.|
|SOS_OOM_CHECK|Только для внутреннего применения.|
|SOS_PHYS_PAGE_CACHE|Только для внутреннего применения.|
|SOS_RESOURCE_CLERK_LIST|Только для внутреннего применения.|
|SOS_RINGBUFFER_RECORD|Только для внутреннего применения.|
|SOS_RW|Только для внутреннего применения.|
|SOS_SATELLITE_USER_POOL|Только для внутреннего применения.|
|SOS_SCHEDULER|Только для внутреннего применения.|
|SOS_SELIST_SIZED_SLOCK|Только для внутреннего применения.|
|SOS_SUSPEND_QUEUE|Только для внутреннего применения.|
|SOS_SYSTHREAD|Только для внутреннего применения.|
|SOS_SYSTHREAD_DISPATCHER|Только для внутреннего применения.|
|SOS_TASK|Только для внутреннего применения.|
|SOS_TLIST|Только для внутреннего применения.|
|SOS_VM_LOW|Только для внутреннего применения.|
|SOS_WAIT_STATS|Только для внутреннего применения.|
|SOS_WAITABLE_ADDRESS_HASHBUCKET|Только для внутреннего применения.|
|SPIN_EVENT_MUTEX|Только для внутреннего применения.|
|SPL_DISPATCHER_LIST|Только для внутреннего применения.|
|SPL_DISPATCHER_QUEUE|Только для внутреннего применения.|
|SPL_NONYIELD_ANALYSIS|Только для внутреннего применения.|
|SPL_QUERY_STORE_CTX_INITIALIZED|Только для внутреннего применения.|
|SPL_QUERY_STORE_EXEC_STATS_AGG|Только для внутреннего применения.|
|SPL_QUERY_STORE_EXEC_STATS_READ|Только для внутреннего применения.|
|SPL_QUERY_STORE_STATS_COOKIE_CACHE|Только для внутреннего применения.|
|SPL_SOS_DISPATCHER|Только для внутреннего применения.|
|SPL_TDS_PKT_QUEUE|Только для внутреннего применения.|
|SPL_XE_BUFFER_MGR|Только для внутреннего применения.|
|SPL_XE_DISPATCHER_QUEUE|Только для внутреннего применения.|
|SPL_XE_NOTIFICATION_CALLBACK_LIST|Только для внутреннего применения.|
|SPL_XE_SESSION_EVENT_MGR|Только для внутреннего применения.|
|SPL_XE_SESSION_MGR|Только для внутреннего применения.|
|SPL_XE_SESSION_TARGET_MGR|Только для внутреннего применения.|
|SPT_PROFILE|Только для внутреннего применения.|
|SQL_MGR|Только для внутреннего применения.|
|SQL_NORM|Только для внутреннего применения.|
|SQLTRACE_FILE_BUFFER|Только для внутреннего применения.|
|SRVPROC|Только для внутреннего применения.|
|STACK_HASHER|Только для внутреннего применения.|
|Подблокировка|Только для внутреннего применения.|
|субпдеск|Только для внутреннего применения.|
|SUBPDESC_LIST|Только для внутреннего применения.|
|SVC_BROKER_CTRL|Только для внутреннего применения.|
|SVC_BROKER_DEBUG_LIST|Только для внутреннего применения.|
|SVC_BROKER_LIST|Только для внутреннего применения.|
|SVC_BROKER_OBJECT|Только для внутреннего применения.|
|SYNCPOINT_RESOURCE|Только для внутреннего применения.|
|таскелапседексекутионмонитор|Только для внутреннего применения.|
|TDS_TVP|Только для внутреннего применения.|
|тесттеам|Только для внутреннего применения.|
|тесттеамекспонентиал|Только для внутреннего применения.|
|тесттеамекспонентиалтастас|Только для внутреннего применения.|
|тесттеамтастас|Только для внутреннего применения.|
|TMP_SESS_KEY|Только для внутреннего применения.|
|TSQL_DEBUG|Только для внутреннего применения.|
|TXFRM_REPL|Только для внутреннего применения.|
|VDI_OPERATION|Только для внутреннего применения.|
|WINFAB_REPORT_FAULT|Только для внутреннего применения.|
|WRITE_PAGE_RECORDER|Только для внутреннего применения.|
|X_PACKET_LIST|Только для внутреннего применения.|
|X_PIPE|Только для внутреннего применения.|
|X_PIPE_DEMAND|Только для внутреннего применения.|
|X_PORT|Только для внутреннего применения.|
|XACT_LOCK_INFO|Только для внутреннего применения.|
|XACT_LOCKINFO_TASK|Только для внутреннего применения.|
|XACT_WORKSPACE|Только для внутреннего применения.|
|кскб|Только для внутреннего применения.|
|XCB_FREE_LIST|Только для внутреннего применения.|
|XCB_HASH|Только для внутреннего применения.|
|XCHNG_TRACE|Только для внутреннего применения.|
|XDES|Только для внутреннего применения.|
|XDES_HASH|Только для внутреннего применения.|
|ксдесмгр|Только для внутреннего применения.|
|ксдестаблелист|Только для внутреннего применения.|
|XE_RATE_LIMITER_STRETCHDB|Только для внутреннего применения.|
|XE_SESSION_STORAGE|Только для внутреннего применения.|
|XID_ARRAY|Только для внутреннего применения.|
|XIO_BLOCKLIST|Только для внутреннего применения.|
|XIO_REQSTR|Только для внутреннего применения.|
|XIO_SEQNUMBUMP|Только для внутреннего применения.|
|ксиостатс|Только для внутреннего применения.|
|XTP_RT_DATA_LIST|Только для внутреннего применения.|
|XTS_MGR|Только для внутреннего применения.|
|XVB_CSN|Только для внутреннего применения.|
|XVB_LIST|Только для внутреннего применения.|
 
## <a name="see-also"></a>См. также:  
 
 [DBCC SQLPERF (Transact-SQL)](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md)   
 
 [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  

 [Когда происходит деспин-фактор использования ЦП в SQL Server?](https://techcommunity.microsoft.com/t5/SQL-Server-Support/When-is-Spinlock-a-Significant-Driver-of-CPU-utilization-in-SQL/ba-p/530142)

 [Диагностика и разрешение конфликтов спин/блокировок на SQL Server](../diagnose-resolve-spinlock-contention.md)
  

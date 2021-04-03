---
description: sys.dm_hadr_physical_seeding_stats (Transact-SQL)
title: sys.dm_hadr_physical_seeding_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/17/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_physical_seeding_stats_TSQL
- sys.dm_hadr_physical_seeding_stats
- sys.dm_hadr_physical_seeding_stats_TSQL
- sys.dm_hadr_physical_seeding_stats
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- physical seeding
- sys.dm_hadr_physical_seeding_stats dynamic management view
author: cawrites
ms.author: chadam
ms.openlocfilehash: 89736fd8a97e9532f7729003e4750ac07e703735
ms.sourcegitcommit: 14f2051d329b69a7b5ff7bce1d136cf7f25bb219
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/02/2021
ms.locfileid: "106232280"
---
# <a name="sysdm_hadr_physical_seeding_stats-transact-sql"></a>sys.dm_hadr_physical_seeding_stats (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

В первичной реплике запросите sys.dm_hadr_physical_seeding_stats динамического административного представления, чтобы увидеть физическую статистику для каждого выполняемого в данный момент процесса заполнения.

В следующей таблице определяется значение различных столбцов.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**local_physical_seeding_id**|**uniqueidentifier**|Уникальный идентификатор этой операции заполнения для локальной реплики.|  
|**remote_physical_seeding_id**|**uniqueidentifier**|Уникальный идентификатор этой операции заполнения на удаленной реплике.|  
|**local_database_id**|**int**|Идентификатор базы данных в локальной реплике.|  
|**local_database_name**|**nvarchar**|Имя базы данных в локальной реплике. |
|**remote_machine_name**|**nvarchar**|Имя компьютера удаленной реплики.|  
|**role_desc**|**nvarchar**|Описание роли заполнения. (доступные значения: источник, назначение, сервер пересылки, Форвардердестинатион)|
|**internal_state_desc**|**nvarchar**|Описание состояния локальной реплики.|
|**transfer_rate_bytes_per_second**|**bigint**|Текущая скорость передачи заполнения (в байтах в секунду).|
|**transfered_size_bytes**|**bigint**|Общее число уже переданных байтов.|
|**database_size_bytes**|**bigint**|Общий размер заполняемой базы данных в байтах.|
|**start_time_utc**|**datetime**|Время начала операции заполнения в формате UTC.|
|**end_time_utc**|**datetime**|Время окончания операции заполнения в формате UTC.|
|**estimate_time_complete_utc**|**datetime**|Оценка времени завершения для выполняемой в процессе операции заполнения в формате UTC.|
|**total_disk_io_wait_time_ms**|**bigint**|Сумма времени ожидания ввода-вывода диска в мс.|
|**total_network_wait_time_ms**|**bigint**|Сумма времени ожидания сетевого ввода-вывода в мс.|
|**failure_code**|**int**|Код ошибки для операции заполнения.|
|**failure_message**|**nvarchar**|Сообщение, соответствующее коду ошибки.|
|**failure_time_utc**|**datetime**|Время сбоя в формате UTC.|
|**is_compression_enabled**|**bit**|Указывает, включено ли сжатие для операции заполнения.|
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="see-also"></a>См. также:  
 [Автоматическое восстановление страниц (группы доступности: зеркальное отображение баз данных)](../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [suspect_pages &#40;Transact-SQL&#41;](../../relational-databases/system-tables/suspect-pages-transact-sql.md)   
 [Управление таблицей suspect_pages (SQL Server)](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
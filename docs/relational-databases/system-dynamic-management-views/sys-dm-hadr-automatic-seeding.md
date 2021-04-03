---
description: sys.dm_hadr_automatic_seeding (Transact-SQL)
title: sys.dm_hadr_automatic_seeding (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/01/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_automatic_seeding_TSQL
- sys.dm_hadr_automatic_seeding
- sys.dm_hadr_automatic_seeding_TSQL
- dm_hadr_automatic_seeding
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- automatic seeding
- sys.dm_hadr_automatic_seeding dynamic management view
ms.assetid: d7840adf-4a1b-41ac-bc94-102c07ad1c79
author: cawrites
ms.author: chadam
ms.openlocfilehash: b53e5077ee42357afc66426044d1caf8f4469c1c
ms.sourcegitcommit: 14f2051d329b69a7b5ff7bce1d136cf7f25bb219
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/02/2021
ms.locfileid: "106232283"
---
# <a name="sysdm_hadr_automatic_seeding-transact-sql"></a>sys.dm_hadr_automatic_seeding (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

На первичной реплике запросите sys.dm_hadr_automatic_seeding, чтобы проверить состояние процесса автоматического заполнения. Представление возвращает одну строку для каждого процесса заполнения.  
  
В следующей таблице определяется значение различных столбцов.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**start_time**|**datetime**|Время, когда была инициирована операция.|
|**completion_time**|**datetime**|Время завершения операции (null, если текущая).|  
|**ag_id**|**uniqueidentifier**|Уникальный идентификатор для каждой группы доступности.|  
|**ag_db_id**|**uniqueidentifier**|Уникальный идентификатор для каждой базы данных в доступной группе.|  
|**ag_remote_replica_id**|**uniqueidentifier**|Уникальный идентификатор для другой реплики, которая включается в эту операцию заполнения.|
|**operation_id**|**uniqueidentifier**|Уникальный идентификатор для этой операции заполнения.|  
|**is_source**|**bit**|Возвращает значение, указывающее, является ли эта реплика источником (первичной) операции заполнения.|
|**current_state**|**bit**|Возвращает текущее состояние заполнения, в котором находится операция.|
|**performed_seeding**|**bit**|Инициализируется потоковая передача базы данных для заполнения.|
|**failure_state**|**int**|Возвращает причину сбоя операции.|
|**failure_state_desc**|**нчарвар**|Возвращает описание сбоя операции.|
|**error_code**|**int**|Возвращает код ошибки SQL, обнаруженный во время заполнения.|
|**number_of_attempts**|**int**|Возвращает количество предпринятых операций заполнения.|


## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 необходимо разрешение VIEW SERVER STATE на сервере.  
  
## <a name="see-also"></a>См. также:  
 [Автоматическое восстановление страниц (группы доступности: зеркальное отображение баз данных)](../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [suspect_pages &#40;Transact-SQL&#41;](../../relational-databases/system-tables/suspect-pages-transact-sql.md)   
 [Управление таблицей suspect_pages (SQL Server)](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  

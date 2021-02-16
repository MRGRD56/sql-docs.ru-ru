---
title: Идентификация задержек, связанных с группами доступности
description: Идентификация задержек, связанных с группами доступности Always On, с помощью Transact-SQL (T-SQL) и расширенных событий.
ms.custom: ag-guide, seodec18
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: afa8caff-f325-48d9-a8ef-a30beab60389
author: rothja
ms.author: jroth
ms.openlocfilehash: 4be170149181263e7d904f5f820a9dd50f6d3801
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347490"
---
# <a name="identify-waits-associated-with-availability-groups"></a>Идентификация задержек, связанных с группами доступности
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  При устранении неполадок с задержкой для групп доступности AlwaysOn можно отслеживать накапливаемую статистику ожидания с помощью зависящих от групп доступности типов ожидания в динамическом административном представлении [sys.dm_os_wait_stats (Transact-SQL)](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).  
  
 Общие сведения об использовании статистики ожидания см. в разделе [Ожидания и очереди в SQL Server 2005](/previous-versions/sql/sql-server-2005/administrator/cc966413(v=technet.10)). Этот документ был написан для SQL Server 2005, но сведения в нем актуальны и для более поздних версий SQL Server.  
  
## <a name="query-for-availability-groups-wait-types"></a>Запрос для типов ожидания групп доступности  
 Чтобы получить всю статистику ожидания по типам ожидания групп доступности, используйте следующий запрос T-SQL:  
  
```sql  
SELECT * FROM sys.dm_os_wait_stats   
WHERE wait_type LIKE '%hadr%'  
ORDER BY wait_time_ms DESC  
```  
  
 Чтобы отслеживать статистику ожидания путем записи расширенных событий, используйте приведенную ниже команду T-SQL.  
  
```sql
CREATE EVENT SESSION [alwayson] ON SERVER   
ADD EVENT sqlos.wait_info(  
    WHERE ([wait_type]=(758) OR [wait_type]=(776) OR [wait_type]=(853) OR [wait_type]=(833)))  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,  
MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)  
GO  
```  
  
 Сопоставление ключа и значения для типа ожидания можно просмотреть, выполнив следующий запрос:  
  
```sql
SELECT * FROM sys.dm_xe_map_values   
WHERE name='wait_types' AND map_value LIKE '%hadr%'   
ORDER BY map_key ASC  
```  
  
## <a name="next-steps"></a>Дальнейшие действия  
 [Типы ожиданий](~/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md#WaitTypes)  
  

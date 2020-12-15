---
description: sys.dm_db_xtp_object_stats (Transact-SQL)
title: sys.dm_db_xtp_object_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_object_stats_TSQL
- sys.dm_db_xtp_object_stats
- dm_db_xtp_object_stats
- sys.dm_db_xtp_object_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_object_stats dynamic management view
ms.assetid: 07300b59-3cab-4d3e-8138-5ea8f584f88f
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a082e789e05a5ca59afb9db74790a5cfb54232d7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468475"
---
# <a name="sysdm_db_xtp_object_stats-transact-sql"></a>sys.dm_db_xtp_object_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Возвращает количество строк, затронутых операциями, выполненными с каждым объектом [!INCLUDE[hek_2](../../includes/hek-2-md.md)] с момента последнего запуска базы данных. Статистика обновляется при выполнении операции независимо от того, была ли транзакция зафиксирована, или выполнен ее откат.  
  
 С помощью sys.dm_db_xtp_object_stats можно определить, какие оптимизированные для памяти таблицы больше всего изменяются. Можно удалить индексы в таблицах, которые не используются или используются редко, поскольку каждый индекс влияет на производительность. При наличии хэш-индексов следует периодически анализировать значение bucket-count. Дополнительные сведения см. в разделе [Determining the Correct Bucket Count for Hash Indexes](/previous-versions/sql/).  
  
 С помощью sys.dm_db_xtp_object_stats можно определить, в каких оптимизированных для памяти таблицах есть конфликты между операциями записи, которые могут повлиять на производительность приложения. Например, если имеется логика повтора транзакций, одна инструкция может выполняться несколько раз. Кроме того, с помощью этой информации можно определять таблицы (а соответственно, и бизнес-логику), для которых требуется обработка конфликтов операций записи.  
  
 Представление содержит одну строку для каждой таблицы, оптимизированной для памяти, в базе данных.  
  
 Дополнительные сведения см. в разделе [In-Memory OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|object_id|**bigint**|Идентификатор объекта.|  
|row_insert_attempts|**bigint**|Количество строк, вставленных в таблицу с момента последнего перезапуска базы данных зафиксированными или прерванными транзакциями.|  
|row_update_attempts|**bigint**|Количество строк, обновленных в таблице с момента последнего перезапуска базы данных зафиксированными или прерванными транзакциями.|  
|row_delete_attempts|**bigint**|Количество строк, удаленных из таблицы с момента последнего перезапуска базы данных зафиксированными или прерванными транзакциями.|  
|write_conflicts|**bigint**|Количество конфликтов записи, возникших с момента последнего перезапуска базы данных.|  
|unique_constraint_violations|**bigint**|Число нарушений ограничений уникальности, произошедших с момента последнего перезапуска базы данных.|  
|object_address|**varbinary(8)**|Только для внутреннего использования.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение VIEW DATABASE STATE на текущую базу данных.  
  
## <a name="see-also"></a>См. также:  
 [Оптимизированные для памяти динамические административные представления таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  

---
description: sys.dm_exec_external_work (Transact-SQL)
title: sys.dm_exec_external_work (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/10/2021
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DM_EXEC_EXTERNAL_WORK
- DM_EXEC_EXTERNAL_WORK_TSQL
- SYS.DM_EXEC_EXTERNAL_WORK_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_external_work management view
- dm_exec_external_work management view
- PolyBase,views
- PolyBase
ms.assetid: 7597d97b-1fde-4135-ac35-4af12968f300
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba2068fa58bdf711deecf90612ee4bf1e47d374b
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622892"
---
# <a name="sysdm_exec_external_work-transact-sql"></a>sys.dm_exec_external_work (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

Возвращает сведения о рабочей нагрузке для каждого рабочего узла на каждом из вычислений.  
  
Запрос `sys.dm_exec_external_work` для обнаружения работы, которая будет использоваться для связи с внешним источником данных (например, Hadoop или MongoDB).  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|  
|execution_id|`nvarchar(32)`|Уникальный идентификатор связанного запроса Polybase.|См. *request_ID* в [sys.dm_exec_requests &#40;&#41;Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|step_index|`int`|Запрос, выполняемый этим исполнителем.|См. *step_index* в  [sys.dm_exec_requests &#40;&#41;Transact-SQL](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|  
|dms_step_index|`int`|Шаг в плане DMS, который выполняется этим исполнителем.|См. раздел [sys.dm_exec_dms_workers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-dms-workers-transact-sql.md).|  
|compute_node_id|`int`|Узел, на котором запущена Рабочая роль.|См. раздел [sys.dm_exec_compute_nodes &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-compute-nodes-transact-sql.md).|  
|тип|`nvarchar(60)`|Тип внешней работы.|"Разделение файлов" (для Hadoop и службы хранилища Azure)<br/><br/>"Разделение данных ODBC" (для других внешних источников данных) |  
|work_id|`int`|Идентификатор фактического разбиения.|Больше или равно 0.|  
|input_name|`nvarchar(4000)`|Имя входных данных для чтения|Имя файла (с путем) при использовании Hadoop или службы хранилища Azure. Для других внешних источников данных это объединение расположения внешнего источника данных и расположения внешней таблицы. `scheme://DataSourceHostname[:port]/[DatabaseName.][SchemaName.]TableName`|  
|read_location|`bigint`|Смещение места чтения.| `0` число байтов в файле минус 1.<br/><br/>`NULL` для хранилища, отличного от Hadoop или не являющегося хранилищем Azure. |  
|read_command|`nvarchar(4000)`|Запрос, который отправляется внешнему источнику данных. Представлено в [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)].|Текст, представляющий запрос. Для Hadoop и службы хранилища Azure возвращает `NULL` .|
|bytes_processed|`bigint`|Всего байт, выделенных для обработки данных этим исполнителем. Это значение может не быть необязательно представлять итоговые данные, возвращаемые запросом. |Больше или равно 0.|  
|length|`bigint`|Длина разбиения или блока HDFS для Hadoop|Определяемый пользователем. Значение по умолчанию — 64M|  
|status|`nvarchar(32)`|Состояние рабочей роли|Ожидание, обработка, завершение, сбой, прервано|  
|start_time|`datetime`|Начало работы||  
|end_time|`datetime`|Окончание работы||  
|total_elapsed_time|`int`|Общее время в миллисекундах||
|compute_pool_id|`int`|Уникальный идентификатор для пула, в котором работает Рабочая роль. Применяется только к SQL Server кластеру больших данных. См. [sys.dm_exec_compute_pools (Transact-SQL)](sys-dm-exec-compute-pools.md).|Возвращает `0` для [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] в Windows и Linux.|

## <a name="see-also"></a>См. также:  
 [Устранение неполадок в Polybase с помощью динамических административных представлений](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления, связанные с базами данных &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  

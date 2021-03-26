---
description: sys.dm_db_missing_index_columns (Transact-SQL)
title: sys.dm_db_missing_index_columns (Transact-SQL)
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_missing_index_columns_TSQL
- sys.dm_db_missing_index_columns_TSQL
- sys.dm_db_missing_index_columns
- dm_db_missing_index_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_missing_index_columns dynamic management function
- missing indexes feature [SQL Server], sys.dm_db_missing_index_columns dynamic management function
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f893a961bb2ecc0f3c1d0238017bf7592699301e
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551547"
---
# <a name="sysdm_db_missing_index_columns-transact-sql"></a>sys.dm_db_missing_index_columns (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает информацию о столбцах таблицы в базе данных, для которых отсутствует индекс (исключая пространственные индексы). `sys.dm_db_missing_index_columns` является функцией динамического управления.  

## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
sys.dm_db_missing_index_columns(index_handle)  
```  
  
## <a name="arguments"></a>Аргументы  
 *index_handle*  
 Целочисленное значение, уникальным образом идентифицирующее отсутствующий индекс. Его можно получить из следующих объектов DMO.  
  
 [sys.dm_db_missing_index_details &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)  
  
 [sys.dm_db_missing_index_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**column_id**|**int**|Идентификатор столбца.|  
|**column_name**|**sysname**|Имя столбца таблицы.|  
|**column_usage**|**varchar (20)**|Способ использования столбца запросом. Возможные значения и их описания:<br /><br /> EQUALITY: столбец участвует в предикате, который выражает равенство в форме: <br />                        *таблица. столбец*  =  *constant_value*<br /><br /> Неравенство: столбец участвует в предикате, который выражает неравенство, например предикат вида: *Table. Column*  >  *constant_value*. Любой оператор сравнения, кроме «=», выражает неравенство.<br /><br /> ВКЛЮЧИТЬ: столбец не используется для вычисления предиката, но используется по другой причине, например, для охвата запроса.|  
  
## <a name="remarks"></a>Примечания  
 Сведения, возвращаемые, `sys.dm_db_missing_index_columns` обновляются, когда запрос оптимизирован оптимизатором запросов и не сохраняется. Сведения об отсутствующих индексах хранятся только до перезапуска ядра СУБД. Администраторы базы данных должны периодически делать резервные копии сведений об отсутствующих индексах, чтобы сохранить их после перезагрузки сервера. Используйте `sqlserver_start_time` столбец в [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) , чтобы найти время последнего запуска ядра СУБД.   

  
## <a name="transaction-consistency"></a>Согласованность транзакций  
 Если транзакция создает или удаляет таблицу, то строки, содержащие сведения отсутствующих индексов об удаленных объектах, удаляются из данного объекта DMO, сохраняя согласованность транзакций.  
  
## <a name="permissions"></a>Разрешения  
 Пользователям необходимо предоставить разрешение VIEW SERVER STATE или любое разрешение, которое подразумевает разрешение VIEW SERVER STATE, чтобы выполнить запрос к этой функции динамического управления.  
  
## <a name="examples"></a>Примеры  
 В следующем примере выполняется запрос к таблице `Address`, а затем выполняется запрос с использованием динамического административного представления `sys.dm_db_missing_index_columns` для возврата столбцов таблицы, отсутствующих в индексе.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT City, StateProvinceID, PostalCode  
FROM Person.Address  
WHERE StateProvinceID = 9;  
GO  
SELECT mig.*, statement AS table_name,  
    column_id, column_name, column_usage  
FROM sys.dm_db_missing_index_details AS mid  
CROSS APPLY sys.dm_db_missing_index_columns (mid.index_handle)  
INNER JOIN sys.dm_db_missing_index_groups AS mig ON mig.index_handle = mid.index_handle  
ORDER BY mig.index_group_handle, mig.index_handle, column_id;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.dm_db_missing_index_details &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
 [sys.dm_db_missing_index_group_stats_query &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-query-transact-sql.md)     
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](sys-dm-os-sys-info-transact-sql.md)  

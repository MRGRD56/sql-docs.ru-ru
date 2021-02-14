---
title: sys.dm_exec_query_optimizer_memory_gateways (Transact-SQL)
description: Возвращает текущее состояние семафоров ресурсов, используемых для регулирования параллельной оптимизации запросов
ms.custom: seo-dt-2019
ms.date: 04/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: reference
f1_keywords:
- dm_exec_query_optimizer_memory_gateways_TSQL
- dm_exec_query_optimizer_memory_gateways
- sys.dm_exec_query_optimizer_memory_gateways_TSQL
- sys.dm_exec_query_optimizer_memory_gateways
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_query_optimizer_memory_gateways dynamic management view
author: josack
ms.author: josack
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 66e5d3e4509b8593a41a53348e466054ee01b4f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342909"
---
# <a name="sysdm_exec_query_optimizer_memory_gateways-transact-sql"></a>sys.dm_exec_query_optimizer_memory_gateways (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

Возвращает текущее состояние семафоров ресурсов, используемых для регулирования параллельной оптимизации запросов.

|Столбец|Type|Описание|  
|----------|---------------|-----------------|  
|**pool_id**|**int**|Идентификатор пула ресурсов в Resource Governor|  
|**name**|**sysname**|Имя шлюза компиляции (малый шлюз, средний шлюз, крупный шлюз)|
|**max_count**|**int**|Максимальное настроенное число параллельных компиляций|
|**active_count**|**int**|Текущее активное число компиляций в этом шлюзе|
|**waiter_count**|**int**|Число ожидающих в этом шлюзе|
|**threshold_factor**|**bigint**|Пороговый коэффициент, определяющий максимальную часть памяти, используемую при оптимизации запроса.  Для малого шлюза threshold_factor указывает максимальное использование памяти оптимизатора в байтах для одного запроса, прежде чем он понадобится для получения доступа к маленькому шлюзу.  Для среднего и крупного шлюза threshold_factor показывает часть общего объема памяти сервера, доступной для данного шлюза. Он используется в качестве делителя при вычислении порогового значения использования памяти для шлюза.|
|**threshold**|**bigint**|Следующий пороговый объем памяти в байтах.  Запрос необходим для получения доступа к этому шлюзу, если его потребление памяти достигает этого порогового значения.  "-1", если запрос не требуется для получения доступа к этому шлюзу.|
|**is_active**|**bit**|Требуется ли запрос для передачи текущего вентиля.|


## <a name="permissions"></a>Разрешения  
SQL Server требуется разрешение VIEW SERVER STATE на сервере.

Для работы с базой данных SQL Azure в базе данных требуется разрешение VIEW DATABASE STATE.


## <a name="remarks"></a>Remarks  
SQL Server использует многоуровневый шлюз для регулирования числа разрешенных параллельных компиляций.  Используются три шлюза, включая небольшие, средние и большие. Шлюзы помогают предотвратить исчерпание общих ресурсов памяти за счет большего объема памяти компиляции, требующего потребителей.

Ожидает результат шлюза при отложенной компиляции. Помимо задержек при компиляции, регулируемые запросы будут иметь связанный RESOURCE_SEMAPHORE_QUERY_COMPILE тип ожидания. Тип ожидания RESOURCE_SEMAPHORE_QUERY_COMPILE может указывать на то, что запросы используют большой объем памяти для компиляции и что память была исчерпана, или же достаточно памяти в целом, но доступные единицы в определенном шлюзе исчерпаны. Выходные данные **sys.dm_exec_query_optimizer_memory_gateways** можно использовать для устранения неполадок в сценариях, где недостаточно памяти для компиляции плана выполнения запроса.  

## <a name="examples"></a>Примеры  

### <a name="a-viewing-statistics-on-resource-semaphores"></a>A. Просмотр статистики по семафорам ресурсов  
Какова текущая статистика шлюза памяти оптимизатора для данного экземпляра SQL Server?

```  
SELECT [pool_id], [name], [max_count], [active_count],
       [waiter_count], [threshold_factor], [threshold],
       [is_active]
FROM sys.dm_exec_query_optimizer_memory_gateways;   

```  

## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](./system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с выполнением (Transact-SQL)](./execution-related-dynamic-management-views-and-functions-transact-sql.md)  
[Использование команды DBCC MEMORYSTATUS для мониторинга использования памяти в SQL Server 2005](https://support.microsoft.com/help/907877/how-to-use-the-dbcc-memorystatus-command-to-monitor-memory-usage-on-sql-server-2005) 
 [Компиляция больших запросов ожидает RESOURCE_SEMAPHORE_QUERY_COMPILE в SQL Server 2014](https://support.microsoft.com/help/3024815/large-query-compilation-waits-on-resource-semaphore-query-compile-in-sql-server-2014)

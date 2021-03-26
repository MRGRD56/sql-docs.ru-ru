---
description: sys.dm_os_performance_counters (Transact-SQL)
title: sys.dm_os_performance_counters (Transact-SQL)
ms.custom: ''
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_performance_counters
- sys.dm_os_performance_counters_TSQL
- dm_os_performance_counters_TSQL
- sys.dm_os_performance_counters
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_performance_counters dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 62489b131eea77ed67b1207a7606056cea39066d
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551644"
---
# <a name="sysdm_os_performance_counters-transact-sql"></a>sys.dm_os_performance_counters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает по строке на каждый счетчик производительности, хранимый на сервере. Сведения о каждом счетчике производительности см. в разделе [Use SQL Server Objects](../../relational-databases/performance-monitor/use-sql-server-objects.md).  
  
> [!NOTE]  
>  Чтобы вызвать это из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя `sys.dm_pdw_nodes_os_performance_counters` .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_name**|**nchar (128)**|Категория, к которой принадлежит счетчик.|  
|**counter_name**|**nchar (128)**|Имя счетчика. Для получения дополнительных сведений о счетчике это имя раздела для выбора из списка счетчиков, [используемых SQL Server объектов](../../relational-databases/performance-monitor/use-sql-server-objects.md). |  
|**instance_name**|**nchar (128)**|Имя заданного экземпляра счетчика. Обычно содержит имя базы данных.|  
|**cntr_value**|**bigint**|Текущее значение счетчика.<br /><br /> **Примечание.** Для счетчиков за секунду это значение является кумулятивным. Значение частоты должно быть вычислено выборкой значений в дискретные интервалы времени. Разность между двумя последовательными значениям выборки равна частоте используемого интервала времени.|  
|**cntr_type**|**int**|Тип счетчика, как определено архитектурой производительности Windows. Дополнительные сведения о типах счетчиков производительности см. в разделе [типы счетчиков производительности WMI](/windows/desktop/WmiSdk/wmi-performance-counter-types) в документации или на сервере Windows Server.|  
|**pdw_node_id**|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="remarks"></a>Примечания  
 Если экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не отображает счетчики производительности операционной системы Windows, выполните следующий запрос [!INCLUDE[tsql](../../includes/tsql-md.md)], чтобы убедиться, что счетчики производительности отключены.  
  
```sql  
SELECT COUNT(*) FROM sys.dm_os_performance_counters;  
```  
  
Если возвращено 0 строк, значит, счетчики производительности отключены. Затем следует просмотреть журнал установки и выполнить поиск ошибки 3409 `Reinstall sqlctr.ini for this instance, and ensure that the instance login account has correct registry permissions.` . Это означает, что счетчики производительности не были включены. Ошибки, находящиеся непосредственно перед ошибкой 3409, должны указывать первопричину сбоя счетчиков производительности. Дополнительные сведения о файлах журнала установки см. в разделе [Просмотр и чтение SQL Server файлов журнала установки](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  

Счетчики производительности, в которых `cntr_type` значение столбца 65792, отображают моментальный снимок только последнего наблюдаемого значения, а не среднее значение. 

Счетчики производительности, в которых `cntr_type` значение столбца — 272696320 или 272696576, отображают среднее число операций, выполненных в течение каждой секунды интервала выборки. Счетчики этого типа измеряют время в тактах системных часов. Например, чтобы получить моментальный снимок, прочитав последнюю секунду только для `Buffer Manager:Lazy writes/sec` `Buffer Manager:Checkpoint pages/sec` счетчиков и, необходимо сравнить разницу между двумя точками сбора, которые находятся в одной секунде.    

Счетчики производительности, в которых `cntr_type` значение столбца 537003264, отображают отношение подмножества к его набору в процентах. Например, `Buffer Manager:Buffer cache hit ratio` Счетчик сравнивает общее число попаданий в кэш и общее число поисков в кэше. Таким образом, чтобы получить моментальный снимок только за последнюю секунду, необходимо сравнить разницу между текущим значением и базовым значением (знаменатель) между двумя точками сбора, которые находятся в одной секунде друг за другом. Соответствующее базовое значение — это счетчик производительности, `Buffer Manager:Buffer cache hit ratio base` в котором `cntr_type` значение столбца равно 1073939712.

Счетчики производительности, в которых `cntr_type` значение столбца равно 1073874176, показывают количество элементов, обрабатываемых в среднем, в виде соотношения элементов, обрабатываемых в количестве операций. Например, `Locks:Average Wait Time (ms)` счетчики сравнивают число ожиданий блокировок в секунду с запросами блокировки в секунду, чтобы отобразить среднее время ожидания (в миллисекундах) для каждого запроса блокировки, который привел к ожиданию. Таким образом, чтобы получить моментальный снимок только за последнюю секунду, необходимо сравнить разницу между текущим значением и базовым значением (знаменатель) между двумя точками сбора, которые находятся в одной секунде друг за другом. Соответствующее базовое значение — это счетчик производительности, `Locks:Average Wait Time Base` в котором `cntr_type` значение столбца равно 1073939712.

Данные в `sys.dm_os_performance_counters` динамическом административном элементе не сохраняются после перезапуска ядра СУБД. Используйте `sqlserver_start_time` столбец в [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) , чтобы найти время последнего запуска ядра СУБД.   

## <a name="permission"></a>Разрешение

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
 
## <a name="examples"></a>Примеры  
 В следующем примере возвращаются все счетчики производительности, отображающие значения счетчика моментальных снимков.  
  
```sql  
SELECT object_name, counter_name, instance_name, cntr_value, cntr_type  
FROM sys.dm_os_performance_counters
WHERE cntr_type = 65792 OR cntr_type = 272696320 OR cntr_type = 537003264;  
```  
  
## <a name="see-also"></a>См. также:  
  [SQL Server динамические административные представления, связанные с операционной системой &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.sysperfinfo (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)  
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](sys-dm-os-sys-info-transact-sql.md)

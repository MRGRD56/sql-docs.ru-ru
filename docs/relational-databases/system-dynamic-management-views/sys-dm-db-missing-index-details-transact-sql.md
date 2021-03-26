---
description: sys.dm_db_missing_index_details (Transact-SQL)
title: sys.dm_db_missing_index_details (Transact-SQL)
ms.custom: ''
ms.date: 03/12/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_db_missing_index_details
- dm_db_missing_index_details
- sys.dm_db_missing_index_details_TSQL
- dm_db_missing_index_details_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- missing indexes feature [SQL Server], sys.dm_db_missing_index_details dynamic management view
- sys.dm_db_missing_index_details dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 994fdc45a49486f1f57214be7646aa89ffb7d7f9
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551627"
---
# <a name="sysdm_db_missing_index_details-transact-sql"></a>sys.dm_db_missing_index_details (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает подробные сведения об отсутствующих индексах, за исключением пространственных индексов.  
  
 Динамические административные представления в среде [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не могут предоставлять информацию, которая может повлиять на автономность базы данных, или информацию о других базах данных, к которым имеет доступ пользователь. Во избежание раскрытия этой информации все строки, содержащие данные, не принадлежащие подключенному клиенту, отфильтровываются.  

  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**index_handle**|**int**|Идентифицирует специфический отсутствующий индекс. Этот идентификатор уникален для сервера. `index_handle` ключ этой таблицы.|  
|**database_id**|**smallint**|Идентифицирует базу данных, в которой находится таблица с отсутствующим индексом.|  
|**object_id**|**int**|Идентифицирует таблицу, в которой отсутствует индекс.|  
|**equality_columns**|**nvarchar(4000)**|Список столбцов с разделителями-запятыми, соответствующих предикатам равенства в форме:<br /><br /> *таблица. столбец*  = *constant_value*|  
|**inequality_columns**|**nvarchar(4000)**|Список столбцов с разделителями-запятыми, который соответствует предикатам неравенства, например предикатам в форме:<br /><br /> *таблица. столбец*  >  *constant_value*<br /><br /> Любой оператор сравнения, кроме «=», выражает неравенство.|  
|**included_columns**|**nvarchar(4000)**|Список столбцов с разделителями-запятыми, необходимых в качестве столбцов для запроса. Дополнительные сведения о охватывающих или включаемых столбцах см. в разделе [Создание индексов с включением столбцов](../../relational-databases/indexes/create-indexes-with-included-columns.md).<br /><br /> Для оптимизированных для памяти индексов (хэш-и оптимизированные для памяти некластеризованные) игнорируются `included_columns` . Все столбцы таблицы включаются в каждый индекс с оптимизацией для памяти.|  
|**инструкция**|**nvarchar(4000)**|Имя таблицы, в которой отсутствует индекс.|  
  
## <a name="remarks"></a>Примечания  
 Сведения, возвращаемые, `sys.dm_db_missing_index_details` обновляются, когда запрос оптимизирован оптимизатором запросов и не сохраняется. Сведения об отсутствующих индексах хранятся только до перезапуска ядра СУБД. Администраторы базы данных должны периодически делать резервные копии сведений об отсутствующих индексах, чтобы сохранить их после перезагрузки сервера. Используйте `sqlserver_start_time` столбец в [sys.dm_os_sys_info](sys-dm-os-sys-info-transact-sql.md) , чтобы найти время последнего запуска ядра СУБД.   

  
 Чтобы определить, какие группы отсутствующих индексов относятся к определенному отсутствующему индексу, можно выполнить запрос к `sys.dm_db_missing_index_groups` динамическому административному представлению, екуижоининг его на `sys.dm_db_missing_index_details` основе `index_handle` столбца.  

  >[!NOTE]
  >Результирующий набор для этого динамического административного представления ограничен 600 строк. Каждая строка содержит один отсутствующий индекс. Если у вас больше 600 отсутствующих индексов, следует устранить существующие отсутствующие индексы, чтобы можно было просмотреть новые. 
  
## <a name="using-missing-index-information-in-create-index-statements"></a>Использование сведений об отсутствующих индексах в инструкциях CREATE INDEX  
 Для преобразования данных, возвращаемых `sys.dm_db_missing_index_details` в инструкцию CREATE INDEX для оптимизированных для памяти и дисковых индексов, столбцы равенства должны быть размещены перед столбцами неравенства, и вместе они должны сделать ключ индекса. Включенные столбцы должны быть добавлены в инструкцию CREATE INDEX с помощью предложения INCLUDE. Чтобы определить эффективный порядок столбцов равенства, расположите их на основе их выборности, перечисляя наиболее выбираемые столбцы первыми (крайние левые в списке столбцов).  
  
 Дополнительные сведения об индексах, оптимизированных для памяти, см. в разделе [индексы для таблиц Memory-Optimized](../../relational-databases/in-memory-oltp/indexes-for-memory-optimized-tables.md).  
  
## <a name="transaction-consistency"></a>Согласованность транзакций  
 Если транзакция создает или удаляет таблицу, то строки, содержащие сведения отсутствующих индексов об удаленных объектах, удаляются из данного объекта DMO, сохраняя согласованность транзакций.  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   

## <a name="see-also"></a>См. также:  
 [sys.dm_db_missing_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-columns-transact-sql.md)   
 [sys.dm_db_missing_index_groups &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-groups-transact-sql.md)   
 [sys.dm_db_missing_index_group_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-transact-sql.md)  
 [sys.dm_db_missing_index_group_stats_query &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-group-stats-query-transact-sql.md)     
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](sys-dm-os-sys-info-transact-sql.md)

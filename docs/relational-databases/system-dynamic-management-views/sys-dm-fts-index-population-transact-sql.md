---
description: sys.dm_fts_index_population (Transact-SQL)
title: sys.dm_fts_index_population (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_index_population
- dm_fts_index_population
- sys.dm_fts_index_population_TSQL
- dm_fts_index_population_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_population dynamic management view
ms.assetid: 82d1c102-efcc-4b60-9a5e-3eee299bcb2b
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b790fcddb6e8275f70b67cc1d0e1ff9fce97a5bf
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835308"
---
# <a name="sysdm_fts_index_population-transact-sql"></a>sys.dm_fts_index_population (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает сведения о выполняющемся в настоящее время заполнении полнотекстового индекса и индекса семантических ключевых фраз в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Идентификатор базы данных, в которой содержится заполняемый полнотекстовый индекс.|  
|**catalog_id**|**int**|Идентификатор полнотекстового каталога, в котором содержится полнотекстовый индекс.|  
|**table_id**|**int**|Идентификатор таблицы, в которой содержится заполняемый полнотекстовый индекс.|  
|**memory_address**|**varbinary(8)**|Адрес внутренней структуры данных в памяти, используемый, чтобы представлять активное заполнение.|  
|**population_type**|**int**|Тип заполнения. Это может быть:<br /><br /> 1 = полное заполнение;<br /><br /> 2 = добавочное заполнение на основе отметок времени;<br /><br /> 3 = ручное обновление отслеженных изменений;<br /><br /> 4 = фоновое обновление отслеженных изменений.|  
|**population_type_description**|**nvarchar(120)**|Описание типа заполнения.|  
|**is_clustered_index_scan**|**bit**|Указывает, включает ли заполнение просмотр кластеризованных индексов.|  
|**range_count**|**int**|Число поддиапазонов, на которые распараллелена операция заполнения.|  
|**completed_range_count**|**int**|Число диапазонов, для которых обработка завершена.|  
|**outstanding_batch_count**|**int**|Число необработанных пакетов для данного заполнения. Дополнительные сведения см. в разделе [sys.dm_fts_outstanding_batches &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md).|  
|**status**|**int**|**Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.<br /><br /> Состояние операции заполнения. Примечание. Некоторые состояния являются временными. Это может быть:<br /><br /> 3 = запускается<br /><br /> 5 = выполняется нормально<br /><br /> 7 = обработка остановлена<br /><br /> Например, это состояние появляется в процессе автоматического слияния.<br /><br /> 11 = заполнение прервано<br /><br /> 12 = извлечение данных о семантическом подобии|  
|**status_description**|**nvarchar(120)**|Описание состояния заполнения.|  
|**completion_type**|**int**|Состояние завершения данного заполнения.|  
|**completion_type_description**|**nvarchar(120)**|Описание типа завершения.|  
|**worker_count**|**int**|Это значение всегда равно 0.|  
|**queued_population_type**|**int**|Тип заполнения на основе отслеженных изменений, которое последует за текущим заполнением, если таковое выполняется.|  
|**queued_population_type_description**|**nvarchar(120)**|Описание следующего заполнения, оно должно произойти. Например, при параметре CHANGE TRACKING = AUTO и в процессе первоначального полного заполнения в этом столбце будет отображаться «Самозаполнение».|  
|**start_time**|**datetime**|Время начала заполнения.|  
|**incremental_timestamp**|**timestamp**|Для полного заполнения содержит отметку времени его начала. Для остальных типов заполнения содержит последнюю зафиксированную контрольную точку (это значение отражает процесс заполнения).|  
  
## <a name="remarks"></a>Комментарии  
 Если в дополнение к полнотекстовому индексированию включено статистическое семантическое индексирование, то выделение и заполнение семантических ключевых фраз, а также извлечение данных о подобии документов происходит одновременно с полнотекстовым индексированием. Заполнение индекса подобия документов выполняется позже, на втором этапе. Дополнительные сведения см. в разделе [Управление семантическим поиском и наблюдение за](../../relational-databases/search/manage-and-monitor-semantic-search.md)ним.  
  
## <a name="permissions"></a>Разрешения  

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
  
## <a name="physical-joins"></a>Физические соединения  
 ![Существенные соединения данного динамического административного представления](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-index-population-1.gif "Существенные соединения данного динамического административного представления")  
  
## <a name="relationship-cardinalities"></a>Количество элементов связей  
  
|От|Кому|Relationship|  
|----------|--------|------------------|  
|dm_fts_active_catalogs.database_id|dm_fts_index_population.database_id|"Одна к одной"|  
|dm_fts_active_catalogs.catalog_id|dm_fts_index_population.catalog_id|"Одна к одной"|  
|dm_fts_population_ranges.parent_memory_address|dm_fts_index_population.memory_address|«многие к одному»|  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции полнотекстового поиска и семантического поиска &#40;языке Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  

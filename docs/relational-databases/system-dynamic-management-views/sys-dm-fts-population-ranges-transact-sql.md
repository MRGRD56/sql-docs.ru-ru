---
description: Динамическое административное представление sys.dm_fts_population_ranges (Transact-SQL)
title: sys.dm_fts_population_ranges (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_fts_population_ranges
- sys.dm_fts_population_ranges_TSQL
- dm_fts_population_ranges_TSQL
- dm_fts_population_ranges
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_population_ranges dynamic management view
ms.assetid: 58d8564b-9c43-4965-a31c-2893890334ef
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ae7ec8d8a4dde2a30e86b5c6a47e2af4d0c6c73b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484636"
---
# <a name="sysdm_fts_population_ranges-transact-sql"></a>Динамическое административное представление sys.dm_fts_population_ranges (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает сведения о конкретных диапазонах, соответствующих выполняемому в настоящий момент заполнению полнотекстового индекса.  
   
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**memory_address**|**varbinary(8)**|Адрес буферов памяти, выделенных на текущей стадии полнотекстового заполнения индекса.|  
|**parent_memory_address**|**varbinary(8)**|Адрес буферов памяти, в которых хранится родительский объект для всех диапазонов полнотекстового заполнения индекса.|  
|**is_retry**|**bit**|Если значение равно 1, то этот поддиапазон обеспечивает повторную обработку строк, в которых возникли ошибки.|  
|**session_id**|**smallint**|Идентификатор сеанса, в данный момент выполняющего указанную задачу.|  
|**processed_row_count**|**int**|Количество строк, обработанных в этом диапазоне. Ход выполнения сохраняется и подсчитывается каждые 5 минут, а не во время фиксации каждого пакета.|  
|**error_count**|**int**|Количество строк, в которых обнаружены ошибки в этом диапазоне. Ход выполнения сохраняется и подсчитывается каждые 5 минут, а не во время фиксации каждого пакета.|  
  
## <a name="permissions"></a>Разрешения  

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
 
## <a name="physical-joins"></a>Физические соединения  
 ![Существенные соединения данного динамического административного представления](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-population-ranges-1.gif "Существенные соединения данного динамического административного представления")  
  
## <a name="relationship-cardinalities"></a>Количество элементов связей  
  
|От|Кому|Связь|  
|----------|--------|------------------|  
|dm_fts_population_ranges.parent_memory_address|dm_fts_index_population.memory_address|«многие к одному»|  
  
## <a name="see-also"></a>См. также:  
  [Динамические административные представления и функции полнотекстового поиска и семантического поиска &#40;языке Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  


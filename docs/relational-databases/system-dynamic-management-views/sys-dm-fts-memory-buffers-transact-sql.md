---
description: sys.dm_fts_memory_buffers (Transact-SQL)
title: sys.dm_fts_memory_buffers (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_fts_memory_buffers
- dm_fts_memory_buffers_TSQL
- dm_fts_memory_buffers
- sys.dm_fts_memory_buffers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_memory_buffers dynamic management view
ms.assetid: 56895fe5-e8df-4d75-9adc-c1f7757cdef8
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d4948ec70d4073f3c3fbae8774b1974c84af84e2
ms.sourcegitcommit: 2991ad5324601c8618739915aec9b184a8a49c74
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2020
ms.locfileid: "97323953"
---
# <a name="sysdm_fts_memory_buffers-transact-sql"></a>sys.dm_fts_memory_buffers (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает данные о буферах памяти, принадлежащих конкретному пулу памяти, используемому в качестве части полнотекстового сканирования или диапазона полнотекстового сканирования.  
  
> [!NOTE]
> Следующий столбец будет удален в следующем выпуске [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : **row_count**. Не пользуйтесь ими в новых разработках и запланируйте изменение приложений, которые сейчас их используют.  

  
|Столбец|Тип данных|Описание|  
|------------|---------------|-----------------|  
|**pool_id**|**int**|Идентификатор выделенного пула памяти.<br /><br /> 0 = небольшие буферы<br /><br /> 1 = большие буферы|  
|**memory_address**|**varbinary(8)**|Адрес выделенного буфера памяти.|  
|**name**|**nvarchar(4000)**|Имя общего буфера памяти, для которого было произведено данное выделение.|  
|**is_free**|**bit**|Текущее состояние буфера памяти.<br /><br /> 0 = свободен<br /><br /> 1 = занят|  
|**row_count**|**int**|Число строк, обрабатываемое в данный момент буфером.|  
|**bytes_used**|**int**|Объем памяти, используемой данным буфером, в байтах.|  
|**percent_used**|**int**|Процент использования выделенной памяти.|  
  
## <a name="permissions"></a>Разрешения  

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах `Server admin` `Azure Active Directory admin` требуется учетная запись или. Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
  
## <a name="physical-joins"></a>Физические соединения  
 ![Существенные соединения данного динамического административного представления](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-memory-buffers-1.gif "Существенные соединения данного динамического административного представления")  
  
## <a name="relationship-cardinalities"></a>Количество элементов связей  
  
|От|Кому|Связь|  
|----------|--------|------------------|  
|dm_fts_memory_buffers.pool_id|dm_fts_memory_pools.pool_id|«многие к одному»|  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции полнотекстового поиска и семантического поиска &#40;языке Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  


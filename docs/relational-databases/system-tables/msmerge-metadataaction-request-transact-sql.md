---
description: MSmerge_metadataaction_request (Transact-SQL)
title: MSmerge_metadataaction_request (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_metadataaction_request
- MSmerge_metadataaction_request_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_metadataaction_request system table
ms.assetid: cd31a114-900a-4218-ab58-d959e547c647
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7cc1c01f8418e6b3b5015fc406f6814573d4ec7e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158934"
---
# <a name="msmerge_metadataaction_request-transact-sql"></a>MSmerge_metadataaction_request (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В **MSmerge_metadataaction_request** таблице хранится по одной строке для каждого из необходимых компенсирующих действий. При использовании веб-синхронизации, если возникает ошибка и требуется повторная попытка синхронизации, выполняется запись в **MSmerge_metadataaction_request**. Во время фазы передачи последующего слияния из этой таблицы извлекаются и выгружаются запросы для всех статей, принадлежащих синхронизируемой публикации. После успешного завершения синхронизации соответствующая строка в **MSmerge_metadataaction_request** таблице удаляется. Эта таблица сохраняется на издателе в базе данных публикации и на подписчике в базе данных подписки.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**tablenick**|**int**|Псевдоним опубликованной таблицы.|  
|**уникаль**|**uniqueidentifier**|Идентификатор данной строки.|  
|**action**|**tinyint**|Идентифицирует необходимое действие по исправлению ошибок.|  
|**поколения**|**bigint**|Значение поколения, для которого нужно действие по исправлению ошибок.|  
|**изменившиеся**|**int**|Только для внутреннего использования.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [Web Synchronization for Merge Replication](../../relational-databases/replication/web-synchronization-for-merge-replication.md)  
  
  

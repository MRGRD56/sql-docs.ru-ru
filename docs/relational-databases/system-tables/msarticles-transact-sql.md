---
description: MSarticles (Transact-SQL)
title: Мсартиклес (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSarticles
- MSarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSarticles system table
ms.assetid: 1acd79a5-b3e2-4161-9592-7acc2a41ba38
author: cawrites
ms.author: chadam
ms.openlocfilehash: e840c03ac365fcc7afeb5c50454a09affd14e31e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160502"
---
# <a name="msarticles-transact-sql"></a>MSarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **мсартиклес** содержит по одной строке для каждой статьи, реплицируемой издателем. Эта таблица хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|Идентификатор издателя.|  
|**publisher_db**|**sysname**|Имя базы данных издателя.|  
|**publication_id**|**int**|Идентификатор публикации.|  
|**статья**|**sysname**|Имя статьи.|  
|**article_id**|**int**|Идентификатор статьи.|  
|**destination_object**|**sysname**|Имя таблицы, созданной на стороне подписчика.|  
|**source_owner**|**sysname**|Имя владельца схемы исходной таблицы на стороне издателя.|  
|**source_object**|**sysname**|Имя исходного объекта, из которого добавляется статья.|  
|**description**|**nvarchar(255)**|Описание статьи.|  
|**destination_owner**|**sysname**|Имя владельца схемы таблицы, созданной на стороне подписчика.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

---
description: MSdynamicsnapshotjobs (Transact-SQL)
title: Мсдинамикснапшотжобс (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSdynamicsnapshotjobs_TSQL
- MSdynamicsnapshotjobs
dev_langs:
- TSQL
helpviewer_keywords:
- MSdynamicsnapshotjobs system table
ms.assetid: 4f36a325-0e3c-46c4-aeeb-416346cce0bc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 8a03bec5ad0e9835ca60099ed9d11583477d27fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212336"
---
# <a name="msdynamicsnapshotjobs-transact-sql"></a>MSdynamicsnapshotjobs (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В таблице **мсдинамикснапшотжобс** отслеживаются параметризованные сведения о фильтре строк, применяемые для создания моментального снимка отфильтрованных данных. Эта таблица хранится в базах данных публикации и подписки.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**идентификатор**|**int**|Идентификатор для задания моментального снимка отфильтрованных данных.|  
|**name**|**sysname**|Имя задания моментального снимка отфильтрованных данных.|  
|**pubid**|**uniqueidentifier**|Уникальный идентификационный номер этой публикации.|  
|**job_id**|**uniqueidentifier**|Идентификатор задания агента SQL Server на распространителе.|  
|**agent_id**|**int**|Идентификатор агента SQL Server.|  
|**dynamic_filter_login**|**sysname**|Значение, используемое для вычисления функции [SUSER_SNAME](../../t-sql/functions/suser-sname-transact-sql.md) в параметризованных фильтрах строк, определенных для публикации.|  
|**dynamic_filter_hostname**|**sysname**|Значение, используемое для вычисления функции [HOST_NAME](../../t-sql/functions/host-name-transact-sql.md) в параметризованных фильтрах строк, определенных для публикации.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Путь к папке, откуда будут считываться файлы моментального снимка, если используется моментальный снимок отфильтрованных данных.|  
|**partition_id**|**int**|Идентификатор секции данных, которой принадлежит задание.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации (Transact-SQL)](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  

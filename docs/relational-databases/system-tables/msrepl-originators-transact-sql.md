---
description: MSrepl_originators (Transact-SQL)
title: MSrepl_originators (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSrepl_originators_TSQL
- MSrepl_originators
dev_langs:
- TSQL
helpviewer_keywords:
- MSrepl_originators system table
ms.assetid: a3ac20a6-73f6-4fdc-ad5f-5f72746c9871
author: cawrites
ms.author: chadam
ms.openlocfilehash: b9c5a5690f6349975a2edafd7715d81c769ce31c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210711"
---
# <a name="msrepl_originators-transact-sql"></a>MSrepl_originators (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSrepl_originators** содержит по одной строке для каждого обновляемого подписчика, от которого была создана транзакция. Эта таблица хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**идентификатор**|**int**|Определяет обновляемого подписчика.|  
|**publisher_database_id**|**int**|Определяет базу данных публикации.|  
|**srvname**|**sysname**|Имя обновляемого сервера.|  
|**dbname**|**sysname**|Имя обновляемой базы данных.|  
|**publication_id**|**int**|Определяет публикацию.|  
|**DBVersion**|**int**|Определяет версию базы данных.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

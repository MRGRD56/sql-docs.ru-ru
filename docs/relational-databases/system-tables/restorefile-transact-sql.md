---
description: restorefile (Transact-SQL)
title: restorefile (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- restorefile
- restorefile_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- restorefile system table
- restoring files [SQL Server], restorefile system table
- file restores [SQL Server], restorefile system table
ms.assetid: 8e40145a-8559-4abe-8e2a-39b818928009
author: cawrites
ms.author: chadam
ms.openlocfilehash: f13bd0f42d4dde20689e953389be263dde03617b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201202"
---
# <a name="restorefile-transact-sql"></a>restorefile (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Содержит по одной строке для каждого восстановленного файла, включая файлы, восстановленные косвенно по имени файловой группы. Эта таблица хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**restore_history_id**|**int**|Уникальный идентификационный номер, который определяет соответствующую операцию восстановления. Ссылается на **restorehistory (restore_history_id)**.|  
|**file_number**|**numeric (10, 0)**|Идентификационный номер восстановленного файла. Этот номер должен быть уникальным в каждой базе данных. Может иметь значение NULL.<br /><br /> Когда база данных возвращается в моментальный снимок базы данных, это значение заполняется так же, как и в случае полного восстановления.|  
|**destination_phys_drive**|**nvarchar(260)**|Диск или секция, где был восстановлен файл. Может иметь значение NULL.<br /><br /> Когда база данных возвращается в моментальный снимок базы данных, это значение заполняется так же, как и в случае полного восстановления.|  
|**destination_phys_name**|**nvarchar(260)**|Имя файла без указания сведений о диске или секции, где был восстановлен файл. Может иметь значение NULL.<br /><br /> Когда база данных возвращается в моментальный снимок базы данных, это значение заполняется так же, как и в случае полного восстановления.|  
  
## <a name="remarks"></a>Замечания  
 Чтобы уменьшить количество строк в этой таблице и в других таблицах резервного копирования и журнала, выполните хранимую процедуру [sp_delete_backuphistory](../../relational-databases/system-stored-procedures/sp-delete-backuphistory-transact-sql.md) .  
  
## <a name="see-also"></a>См. также:  
 [Резервное копирование и восстановление таблиц &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backup-and-restore-tables-transact-sql.md)   
 [restorefilegroup &#40;Transact-SQL&#41;](../../relational-databases/system-tables/restorefilegroup-transact-sql.md)   
 [restorehistory &#40;Transact-SQL&#41;](../../relational-databases/system-tables/restorehistory-transact-sql.md)   
 [Системные таблицы (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  

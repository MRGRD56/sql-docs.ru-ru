---
description: MSmerge_sessions (Transact-SQL)
title: MSmerge_sessions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_sessions
- MSmerge_sessions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_sessions system table
ms.assetid: 09ada8fc-c148-4379-9524-7826b1b0216c
author: cawrites
ms.author: chadam
ms.openlocfilehash: af57f9ae760c09fa14bffe42ceba987734b7c5ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200283"
---
# <a name="msmerge_sessions-transact-sql"></a>MSmerge_sessions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Таблица **MSmerge_sessions** содержит строки журнала с результатами предыдущих сеансов задания агент слияния. При каждом запуске агента слияния к указанной таблице добавляется новая строка. Эта таблица хранится в базе данных распространителя.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Идентификатор сеанса задания агента слияния.|  
|**agent_id**|**int**|Идентификатор агента слияния.|  
|**start_time**|**datetime**|Время выполнения текущего задания.|  
|**end_time**|**datetime**|Время выполнения завершенного задания.|  
|**duration**|**int**|Общая продолжительность сеанса задания в секундах.|  
|**delivery_time**|**int**|Время, затраченное на применение пакета изменений, в секундах.|  
|**upload_time**|**int**|Время, затраченное на передачу изменений издателю, в секундах.|  
|**download_time**|**int**|Время, затраченное на загрузку изменений подписчиком, в секундах.|  
|**delivery_rate**|**float**|Среднее число команд, передаваемых в секунду.|  
|**time_remaining**|**int**|Предполагаемое время до окончания текущего сеанса в секундах.|  
|**percent_complete**|**decimal**|Предполагаемый процент уже примененных в течение данного сеанса всех изменений.|  
|**upload_inserts**|**int**|Количество изменений, примененных на издателе.|  
|**upload_updates**|**int**|Количество обновлений, примененных на издателе.|  
|**upload_deletes**|**int**|Количество объектов, удаленных с издателя.|  
|**upload_conflicts**|**int**|Количество неполадок, возникших во время применения изменений на издателе.|  
|**upload_conflicts_resolved**|**int**|Количество успешно разрешенных неполадок, возникших во время применения изменений на издателе.|  
|**upload_rows_retried**|**int**|Количество строк, повторно передаваемых издателю.|  
|**download_inserts**|**int**|Количество изменений, примененных на подписчике.|  
|**download_updates**|**int**|Количество обновлений, примененных на подписчике.|  
|**download_deletes**|**int**|Количество объектов, удаленных с подписчика.|  
|**download_conflicts**|**int**|Количество неполадок, возникших во время применения изменений на подписчике.|  
|**download_conflicts_resolved**|**int**|Количество успешно разрешенных неполадок, возникших во время применения изменений на подписчике.|  
|**download_rows_retried**|**int**|Количество строк, повторно передаваемых подписчику.|  
|**schema_changes**|**int**|Количество изменений, примененных в схеме в течение сеанса.|  
|**metadata_rows_cleanedup**|**int**|Количество строк метаданных, очищенных за сеанс.|  
|**runstatus**|**int**|Состояние выполнения:<br /><br /> **1** = запуск.<br /><br /> **2** = выполнена.<br /><br /> **3** = выполняется.<br /><br /> **4** = бездействие.<br /><br /> **5** = повторная попытка.<br /><br /> **6** = ошибка.|  
|**estimated_upload_changes**|**int**|Предполагаемое количество изменений, которые необходимо применить на издателе.|  
|**estimated_download_changes**|**int**|Предполагаемое количество изменений, которые необходимо применить на подписчике.|  
|**connection_type**|**int**|Тип подключения, используемого для загрузки:<br /><br /> **1** = локальная сеть (LAN).<br /><br /> **2** = коммутируемое сетевое подключение.<br /><br /> **3** = веб-синхронизация.|  
|**timestamp**|**timestamp**|Столбец отметок времени этой таблицы.|  
  
## <a name="see-also"></a>См. также:  
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

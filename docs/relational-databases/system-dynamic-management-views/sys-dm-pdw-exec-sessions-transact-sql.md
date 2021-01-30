---
description: sys.dm_pdw_exec_sessions (Transact-SQL)
title: sys.dm_pdw_exec_sessions (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/22/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 31c262b3-7e4d-44c4-af71-aaef0fd1a980
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 829eb5d53fc4e60c7c975a75a2e3159885b85171
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99142451"
---
# <a name="sysdm_pdw_exec_sessions-transact-sql"></a>sys.dm_pdw_exec_sessions (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Содержит сведения обо всех сеансах, которые в настоящее время или недавно открыты на устройстве. В нем отображается по одной строке за сеанс.  
  
|Имя столбца|Тип данных|Description|Диапазон|  
|-----------------|---------------|-----------------|-----------|  
|session_id|**nvarchar(32)**|Идентификатор текущего запроса или последнего выполнения запроса (если сеанс завершен, а запрос выполнялся во время завершения). Ключ для этого представления.|Уникальный для всех сеансов в системе.|  
|status|**nvarchar (10)**|Для текущих сеансов определяет, активен ли сеанс в данный момент. Для прошлых сеансов состояние сеанса может отображаться как закрытое или уничтоженное (если сеанс был принудительно закрыт).|"ACTIVE", "CLOSED", "IDLE", "TERMINATE"|  
|request_id|**nvarchar(32)**|Идентификатор текущего запроса или последнего выполнения запроса.|Уникальный для всех запросов в системе. Значение null, если ни один из них не был запущен.|  
|security_id|**varbinary(85)**|Идентификатор безопасности участника, выполняющего сеанс.||  
|login_name|**nvarchar(128)**|Имя входа участника, запустившего сеанс.|Любая строка, удовлетворяющая соглашениям об именовании пользователей.|  
|login_time|**datetime**|Дата и время, когда пользователь вошел в систему и был создан этот сеанс.|Допустимое **значение даты и** времени до текущего времени.|  
|query_count|**int**|Захватывает число запросов или сеансов рекуестссис, выполненных с момента создания.|Больше или равно 0.|  
|is_transactional|**bit**|Захватывает, находится ли в данный момент сеанс в пределах транзакции.|0 для автоматической фиксации, 1 — для транзакций.|  
|client_id|**nvarchar(255)**|Захватывает сведения о клиенте для сеанса.|Любая допустимая строка.|  
|app_name|**nvarchar(255)**|Захватывает сведения об имени приложения, которые дополнительно задаются в рамках процесса подключения.|Любая допустимая строка.|  
|sql_spid|**int**|Идентификационный номер SPID. Используйте `session_id` этот сеанс. Используйте `sql_spid` столбец, чтобы присоединиться к **sys.dm_pdw_nodes_exec_sessions**.<br /><br /> Предупреждение. Этот столбец содержит закрытые SPID. **\* \* \* \***||  
  
 Сведения о максимальном объеме строк, хранящихся в этом представлении, см. в разделе метаданные статьи [ограничения емкости](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) .  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение `VIEW SERVER STATE`.  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления Azure синапсе Analytics и Параллельное хранилище данных &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  

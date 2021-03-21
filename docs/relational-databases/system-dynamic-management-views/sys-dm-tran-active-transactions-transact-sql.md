---
description: sys.dm_tran_active_transactions (Transact-SQL)
title: 'sys.dm_tran_active_transactions (Transact-SQL) '
ms.custom: ''
ms.date: 03/18/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_active_transactions
- sys.dm_tran_active_transactions_TSQL
- dm_tran_active_transactions_TSQL
- dm_tran_active_transactions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_active_transactions dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2d826f396758c9b6fdffffdec89ef9d0bbf1538b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756434"
---
# <a name="sysdm_tran_active_transactions-transact-sql"></a>sys.dm_tran_active_transactions (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает данные о транзакциях для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_tran_active_transactions**.  
  
|Имя столбца|Тип данных|Description|  
|-----------------|---------------|-----------------|  
|transaction_id|**bigint**|Идентификатор транзакции на уровне экземпляра, а не на уровне базы данных. Он уникален во всех базах данных только в пределах экземпляра, но не уникален во всех экземплярах сервера.|  
|name|**nvarchar(32)**|Имя транзакции. Оно перезаписывается, если транзакция помечена, и помеченное имя заменяет имя транзакции.|  
|transaction_begin_time|**datetime**|Время начала транзакции.|  
|transaction_type|**int**|Тип транзакции.<br /><br /> 1 = транзакция чтения-записи<br /><br /> 2 = транзакция только чтения<br /><br /> 3 = системная транзакция<br /><br /> 4 = распределенная транзакция|  
|transaction_uow|**uniqueidentifier**|Идентификатор единицы работы транзакции (UOW) для распределенных транзакций. MS DTC использует идентификатор UOW для работы с распределенной транзакцией.|  
|transaction_state|**int**|0 = Транзакция еще не была полностью инициализирована.<br /><br /> 1 = Транзакция была инициализирована, но еще не началась.<br /><br /> 2 = Транзакция активна.<br /><br /> 3 = Транзакция закончилась. Используется для транзакций «только для чтения».<br /><br /> 4 = Фиксирующий процесс был инициализирован на распределенной транзакции. Предназначено только для распределенных транзакций. Распределенная транзакция все еще активна, но дальнейшая обработка не может иметь место.<br /><br /> 5 = Транзакция находится в готовом состоянии и ожидает разрешения.<br /><br /> 6 = Транзакция зафиксирована.<br /><br /> 7 = Производится откат транзакции.<br /><br /> 8 = транзакция была отменена.|  
|transaction_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|transaction_status2|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|dtc_state|**int**|**Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (начальный выпуск в [текущем выпуске](/previous-versions/azure/ee336279(v=azure.100))).<br /><br /> 1 = ACTIVE<br /><br /> 2 = PREPARED<br /><br /> 3 = COMMITTED<br /><br /> 4 = ABORTED<br /><br /> 5 = RECOVERED|  
|dtc_status|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|dtc_isolation_level|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|filestream_transaction_id|**varbinary(128)**|**Применимо к**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (начальный выпуск в [текущем выпуске](/previous-versions/azure/ee336279(v=azure.100))).<br /><br /> [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|pdw_node_id|**int**|**Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение.|  
  
## <a name="permissions"></a>Разрешения

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   


## <a name="examples"></a>Примеры  
  
### <a name="a-using-sysdm_tran_active_transactions-with-other-dmvs-to-find-information-about-active-transactions"></a>A. Использование sys.dm_tran_active_transactions с другими динамическими административными представлениями для поиска сведений об активных транзакциях
 В следующем примере показаны активные транзакции в системе и предоставляются подробные сведения о транзакции, сеансе пользователя, отправленном приложении и запросе, который его запустил, и много других.  
  
```sql  
SELECT
  GETDATE() as now,
  DATEDIFF(SECOND, transaction_begin_time, GETDATE()) as tran_elapsed_time_seconds,
  st.session_id,
  txt.text, 
  *
FROM
  sys.dm_tran_active_transactions at
  INNER JOIN sys.dm_tran_session_transactions st ON st.transaction_id = at.transaction_id
  LEFT OUTER JOIN sys.dm_exec_sessions sess ON st.session_id = sess.session_id
  LEFT OUTER JOIN sys.dm_exec_connections conn ON conn.session_id = sess.session_id
    OUTER APPLY sys.dm_exec_sql_text(conn.most_recent_sql_handle)  AS txt
ORDER BY
  tran_elapsed_time_seconds DESC;
```


## <a name="see-also"></a>См. также:  
 [sys.dm_tran_session_transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)   
 [sys.dm_tran_database_transactions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-database-transactions-transact-sql.md)   
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с транзакциями (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  

---
description: sys.dm_tran_version_store_space_usage (Transact-SQL)
title: sys.dm_tran_version_store_space_usage (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_version_store_space_usage_TSQL
- sys.dm_tran_version_store_space_usage
- dm_tran_version_store_space_usage
- dm_tran_version_store_space_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_version_store_space_usage dynamic management view
ms.assetid: 7ab44517-0351-4f91-bdd9-7cf940f03c51
author: savjani
ms.author: pariks
manager: ajayj
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c36dce670a89883b436421ae26c36d54308f2457
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99132096"
---
# <a name="sysdm_tran_version_store_space_usage-transact-sql"></a>sys.dm_tran_version_store_space_usage (Transact-SQL)
[!INCLUDE[tsql-appliesto-2016sp2-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-2016sp2-asdb-xxxx-xxx-md.md)]

Возвращает таблицу, в которой отображается общее пространство tempdb, используемое записями хранилища версий для каждой базы данных. **sys.dm_tran_version_store_space_usage** является эффективным и недорогостоящим для выполнения, так как он не переходит по отдельным записям хранилища версий и возвращает общее используемое пространство хранилища версий в базе данных tempdb для каждой базы.
  
Каждая запись с версиями хранится в виде двоичных данных вместе с некоторыми сведениями об отслеживании или состоянии. Как и в таблицах базы данных, записи в хранилище версий хранятся в страницах размером 8192 байта. Если размер записи превышает 8192 байта, она разбивается на две различные записи.  
  
Так как запись версии хранится в двоичном виде, не возникает проблем с разными параметрами сортировки из разных баз данных. Используйте **sys.dm_tran_version_store_space_usage** , чтобы отслеживать и планировать размер базы данных tempdb в зависимости от используемого пространства хранилища версий, в котором хранятся базы в экземпляре SQL Server.
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Идентификатор базы данных.|  
|**reserved_page_count**|**bigint**|Общее число страниц, зарезервированных в базе данных tempdb для записей хранилища версий.|  
|**reserved_space_kb**|**bigint**|Общее пространство, используемое в килобайтах в базе данных tempdb для записей в хранилище версий.|  
  
## <a name="permissions"></a>Разрешения  
В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   

## <a name="examples"></a>Примеры  
Следующий запрос можно использовать для определения пространства, потребляемого в базе данных tempdb, по хранилищу версий каждой из них в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] экземпляре. 
  
```sql  
SELECT 
  DB_NAME(database_id) as 'Database Name',
  reserved_page_count,
  reserved_space_kb 
FROM sys.dm_tran_version_store_space_usage;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Database Name            reserved_page_count reserved_space_kb  
------------------------ -------------------- -----------  
msdb                      0                    0             
AdventureWorks2016        10                   80             
AdventureWorks2016DW      0                    0             
WideWorldImporters        20                   160             
```
 
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с транзакциями (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  

---
description: core.sp_create_snapshot (Transact-SQL)
title: core.sp_create_snapshot (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_create_snapshot
- sp_create_snapshot_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- management data warehouse, data collector stored procedures
- data collector [SQL Server], stored procedures
- core.sp_create_snapshot stored procedure
- sp_create_snapshot
ms.assetid: ff297bda-0ee2-4fda-91c8-7000377775e3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 737739cfa627e6668d95e6453d66ed1bad4ad637
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210511"
---
# <a name="coresp_create_snapshot-transact-sql"></a>core.sp_create_snapshot (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Вставляет строку в представление core.snapshots хранилища данных управления. Эта процедура вызывается каждый раз, когда пакет передачи начинает передавать данные в хранилище данных управления.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
core.sp_create_snapshot [ @collection_set_uid = ] 'collection_set_uid'  
    , [ @collector_type_uid = ] 'collector_type_uid'  
    ,[ @machine_name = ] 'machine_name'  
    , [ @named_instance = ] 'named_instance'  
    , [ @log_id = ] log_id  
    , [ @snapshot_id = ] snapshot_id OUTPUT  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @collection_set_uid =] "*collection_set_uid*"  
 Имеет значение GUID для набора элементов сбора. *collection_set_uid* имеет тип **uniqueidentifier** и не имеет значения по умолчанию. Чтобы получить идентификатор GUID, запросите представление dbo.syscollector_collection_sets в базе данных msdb.  
  
 [ @collector_type_uid =] "*collector_type_uid*"  
 Идентификатор GUID для типа сборщика. *collector_type_uid* имеет тип **uniqueidentifier** и не имеет значения по умолчанию. Чтобы получить идентификатор GUID, запросите представление dbo.syscollector_collector_types в базе данных msdb.  
  
 [ @machine_name =] "*machine_name*"  
 Имя сервера, на котором находится набор элементов сбора. Аргумент *machine_name* имеет тип **sysname** и не имеет значения по умолчанию.  
  
 [ @named_instance =] "*named_instance*"  
 Имя экземпляра набора элементов сбора. Аргумент *named_instance* имеет тип **sysname** и не имеет значения по умолчанию.  
  
 [ @log_id =] *log_id*  
 Уникальный идентификатор, соответствующий журналу событий набора элементов сбора на сервере, который собирал данные. *log_id* имеет тип **bigint** и не имеет значения по умолчанию. Чтобы получить значение для *log_id*, запросите представление collector_execution_log dbo.sysв базе данных msdb.  
  
 [ @snapshot_id =] *snapshot_id*  
 Уникальный идентификатор строки, которая вставляется в представление "основные. моментальные снимки". *snapshot_id* имеет **тип int** и возвращается в виде выходных данных.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 Каждый раз, когда пакет передачи начинает загружать данные в хранилище управляющих данных, исполняемый компонент сборщика данных вызывает функцию core.sp_create_snapshot.  
  
 Эта процедура проверяет следующее:  
  
-   Аргумент collection_set_uid соответствует существующей записи в таблице core.source_info_internal.  
  
-   Аргумент collector_type_uid соответствует существующей записи в представлении core.supported_collector_types.  
  
 Если хотя бы одно из вышеперечисленных условий не выполняется, процедура возвращает ошибку.  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в предопределенной роли базы данных **mdw_writer** (с разрешением EXECUTE).  
  
## <a name="examples"></a>Примеры  
 В следующем примере создается моментальный снимок для набора сбора «Занято места на диске», который добавляется в хранилище данных управления, а затем возвращается идентификатор моментального снимка. В примере используется экземпляр по умолчанию.  
  
```  
USE <management_data_warehouse>;  
DECLARE @snapshot_id int;  
EXEC core.sp_create_snapshot   
    @collection_set_uid = '7B191952-8ECF-4E12-AEB2-EF646EF79FEF',   
    @collector_type_uid = '302E93D1-3424-4BE7-AA8E-84813ECF2419',  
    @machine_name = '<computername>',  
    @named_instance = 'MSSQLSERVER',  
    @log_id = 11, -- ID of the log for the collection set  
    @snapshot_id = @snapshot_id OUTPUT;  
```  
  
## <a name="see-also"></a>См. также:  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Хранимые процедуры сборщика данных (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Хранилище данных управления](../../relational-databases/data-collection/management-data-warehouse.md)  
  
  

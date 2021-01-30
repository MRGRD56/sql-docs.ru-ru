---
description: core.sp_update_data_source (Transact-SQL)
title: core.sp_update_data_source (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_data_source
- sp_update_data_source_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_update_data_source
- management data warehouse, data collector stored procedures
- core.sp_update_data_source stored procedure
- data collector [SQL Server], stored procedures
ms.assetid: 66b95f96-6df7-4657-9b3c-86a58c788ca5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45bfb2c042ea491516aa4d3caadff72c45451202
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180596"
---
# <a name="coresp_update_data_source-transact-sql"></a>core.sp_update_data_source (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Обновляет существующую строку или вставляет новую строку в таблицу core.source_info_internal хранилища данных управления. Эта процедура вызывается компонентом времени выполнения сборщика данных каждый раз, когда пакет передачи начинает загружать данные в хранилище данных управления.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
core.sp_update_data_source [ @collection_set_uid = ] 'collection_set_uid'  
    ,[ @machine_name = ] 'machine_name'  
    , [ @named_instance = ] 'named_instance'  
    , [ @days_until_expiration = ] days_until_expiration  
    , [ @source_id = ] source_id OUTPUT  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @collection_set_uid =] "*collection_set_uid*"  
 Имеет значение GUID для набора элементов сбора. *collection_set_uid* имеет тип **uniqueidentifier** и не имеет значения по умолчанию. Чтобы получить идентификатор GUID, запросите представление dbo.syscollector_collection_sets в базе данных msdb.  
  
 [ @machine_name =] "*machine_name*"  
 Имя сервера, на котором находится набор элементов сбора. *machine_name* имеет тип **sysname** и не имеет значения по умолчанию.  
  
 [ @named_instance =] "*named_instance*"  
 Имя экземпляра набора элементов сбора. Аргумент *named_instance* имеет тип **sysname** и не имеет значения по умолчанию.  
  
> [!NOTE]  
>  *named_instance* должно быть полным именем экземпляра, состоящим из имени компьютера и имени экземпляра в формате *ComputerName* \\ *имя_экземпляра*.  
  
 [ @days_until_expiration =] *days_until_expiration*  
 Количество дней, остающихся до окончания срока хранения данных в моментальном снимке. *days_until_expiration* имеет **smallint**.  
  
 [ @source_id =] *source_id*  
 Уникальный идентификатор для источника обновления. *source_id* имеет **тип int** и возвращается в виде выходных данных.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 Каждый раз, когда пакет передачи начинает загружать данные в хранилище данных управления, исполняемый компонент сборщика данных вызывает функцию core.sp_update_data_source. Таблица core.source_info_internal обновляется, если со времени последней передачи данных произошло одно из следующих изменений.  
  
-   Был добавлен новый набор элементов сбора.  
  
-   Изменилось значение days_until_expiration.  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в предопределенной роли базы данных **mdw_writer** (с разрешением EXECUTE).  
  
## <a name="examples"></a>Примеры  
 В следующем примере обновляется источник данных (в данном случае — набор сбора «Занято места на диске»), устанавливается новое количество дней до истечения срока и возвращается идентификатор источника. В примере используется экземпляр по умолчанию.  
  
```  
USE <management_data_warehouse>;  
GO  
DECLARE @source_id int;  
EXEC core.sp_update_data_source   
@collection_set_uid = '7B191952-8ECF-4E12-AEB2-EF646EF79FEF',   
@machine_name = '<computername>',  
@named_instance = 'MSSQLSERVER',  
@days_until_expiration = 10,  
@source_id = @source_id OUTPUT;  
```  
  
## <a name="see-also"></a>См. также:  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Хранимые процедуры сборщика данных (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Хранилище данных управления](../../relational-databases/data-collection/management-data-warehouse.md)  
  
  

---
description: sp_primarykeys (Transact-SQL)
title: sp_primarykeys (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_primarykeys_TSQL
- sp_primarykeys
dev_langs:
- TSQL
helpviewer_keywords:
- sp_primarykeys
ms.assetid: 0f76dd31-5b7b-4209-9e2e-b9ed5cac164d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 23117c882ee0c2c55e8d29089327e9dd84643878
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199590"
---
# <a name="sp_primarykeys-transact-sql"></a>sp_primarykeys (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает столбцы первичных ключей для указанной удаленной таблицы по одной строке на ключевой столбец.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_primarykeys [ @table_server = ] 'table_server'   
     [ , [ @table_name = ] 'table_name' ]   
     [ , [ @table_schema = ] 'table_schema' ]   
     [ , [ @table_catalog = ] 'table_catalog' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @table_server = ] 'table_server'_` Имя связанного сервера, с которого возвращаются сведения о первичном ключе. Аргумент *table_server* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @table_name = ] 'table_name'` Имя таблицы, для которой необходимо предоставить сведения о первичном ключе. Аргумент *table_name* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @table_schema = ] 'table_schema'` Схема таблицы. Аргумент *table_schema* имеет тип **sysname** и значение по умолчанию NULL. В среде [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] соответствует владельцу таблицы.  
  
`[ @table_catalog = ] 'table_catalog'` Имя каталога, в котором находится заданный *table_name* . В среде [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] соответствует имени базы данных. Аргумент *table_catalog* имеет тип **sysname** и значение по умолчанию NULL.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 Нет  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**TABLE_CAT**|**sysname**|Каталог таблицы.|  
|**TABLE_SCHEM**|**sysname**|Схема таблицы.|  
|**TABLE_NAME**|**sysname**|Имя таблицы.|  
|**COLUMN_NAME**|**sysname**|Имя столбца.|  
|**KEY_SEQ**|**int**|Порядковый номер столбца в первичном ключе, состоящем из нескольких столбцов.|  
|**PK_NAME**|**sysname**|Идентификатор первичного ключа. Возвращает NULL, если не применим к источнику данных.|  
  
## <a name="remarks"></a>Замечания  
 **sp_primarykeys** выполняется путем запроса набора строк PRIMARY_KEYS интерфейса **IDBSchemaRowset** поставщика OLE DB, соответствующего *table_server*. Параметры *table_name*, *table_schema*, *table_catalog* и *столбцов* передаются этому интерфейсу для ограничения возвращаемых строк.  
  
 **sp_primarykeys** возвращает пустой результирующий набор, если поставщик OLE DB указанного связанного сервера не поддерживает набор строк PRIMARY_KEYS интерфейса **IDBSchemaRowset** .  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение SELECT для схемы.  
  
## <a name="examples"></a>Примеры  
 В следующем примере с сервера `LONDON1` возвращаются первичные ключевые столбцы для таблицы `HumanResources.JobCandidate` в базе данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
EXEC sp_primarykeys @table_server = N'LONDON1',   
   @table_name = N'JobCandidate',  
   @table_catalog = N'AdventureWorks2012',   
   @table_schema = N'HumanResources';  
```  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры распределенных запросов &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/distributed-queries-stored-procedures-transact-sql.md)   
 [sp_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-catalogs-transact-sql.md)   
 [sp_column_privileges &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-column-privileges-transact-sql.md)   
 [sp_foreignkeys &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-foreignkeys-transact-sql.md)   
 [sp_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-indexes-transact-sql.md)   
 [sp_linkedservers (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-linkedservers-transact-sql.md)   
 [sp_tables_ex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tables-ex-transact-sql.md)   
 [sp_table_privileges &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-table-privileges-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

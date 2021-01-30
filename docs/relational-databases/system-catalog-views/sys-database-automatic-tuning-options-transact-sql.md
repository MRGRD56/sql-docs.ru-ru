---
title: sys.database_automatic_tuning_options (Transact-SQL) | Документация Майкрософт
description: Узнайте, как просматривать параметры автоматической настройки базы данных SQL. См. раздел необходимые разрешения и просмотрите дополнительные доступные ресурсы.
ms.custom: ''
ms.date: 07/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_automatic_tuning_options_tsql
- database_automatic_tuning_options
- sys.database_automatic_tuning_options_tsql
- sys.database_automatic_tuning_options
dev_langs:
- TSQL
helpviewer_keywords:
- database_automatic_tuning_options catalog view
- sys.database_automatic_tuning_options catalog view
ms.assetid: 16b47d55-8019-41ff-ad34-1e0112178067
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0dbedd65c5f98016e4e001fd863845079da68578
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210380"
---
# <a name="sysdatabase_automatic_tuning_options-transact-sql"></a>sys. Database, \_ автоматическое \_ Tuning_options (TRANSACT-SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Возвращает параметры автоматической настройки для этой базы данных.  

|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar(128)**|Имя параметра автоматической настройки. Доступные параметры см. в разделе [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) .|  
|**desired_state**|**smallint**|Указывает требуемый режим работы для автоматической настройки, явно заданный пользователем.<br />0 = выключен.<br />1 = включен;|  
|**desired_state_desc**|**nvarchar(60)**|Текстовое описание требуемого режима работы для параметра автоматической настройки.<br />OFF<br />ON|  
|**actual_state**|**smallint**|Указывает режим работы параметра автоматической настройки.<br />0 = выключен.<br />1 = включен;|  
|**actual_state_desc**|**nvarchar(60)**|Текстовое описание режима фактической работы параметра автоматической настройки.<br />OFF<br />ON|  
|**reason**|**smallint**|Указывает, почему фактические и требуемые состояния отличаются.<br />2 = ОТКЛЮЧЕНО<br />11 = QUERY_STORE_OFF<br />12 = QUERY_STORE_READ_ONLY<br />13 = NOT_SUPPORTED|   
|**reason_desc**|**nvarchar(60)**|Текстовое описание причины, по которой фактические и требуемые состояния различаются.<br />DISABLEd = параметр отключен системой<br />QUERY_STORE_OFF = хранилище запросов отключено<br />QUERY_STORE_READ_ONLY = хранилище запросов находится в режиме только для чтения<br />NOT_SUPPORTED = доступно только в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выпуске Enterprise Edition| 
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>См. также:  
 [Автоматическая настройка](../../relational-databases/automatic-tuning/automatic-tuning.md)   
 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;&#41;Transact-SQL ](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.dm_db_tuning_recommendations &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 

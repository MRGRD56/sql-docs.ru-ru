---
description: sys.dm_database_copies (база данных SQL Azure)
title: sys.dm_database_copies (база данных SQL Azure) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- dm_database_copies_TSQL
- sys.dm_database_copies
- dm_database_copies
- sys.dm_database_copies_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dm_database_copies
- sys.dm_database_copies
ms.assetid: d03d4657-86d1-4496-97e6-cc3bc292e0b1
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: acdb347af36812095df03f61ae9f118493496e51
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364777"
---
# <a name="sysdm_database_copies-azure-sql-database"></a>sys.dm_database_copies (база данных SQL Azure)
[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Возвращает сведения о копии базы данных.  
  
Чтобы получить сведения о связях георепликации, используйте представления [sys.geo_replication_links](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md) или [sys.dm_geo_replication_link_status](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md) (доступны в базе данных SQL версии 12).
  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Идентификатор текущей базы данных в представлении `sys.databases`.|  
|**start_date**|**datetimeoffset**|Время начала копирования базы данных в формате UTC в региональном центре обработки данных [!INCLUDE[ssSDS](../../includes/sssds-md.md)].|  
|**modify_date**|**datetimeoffset**|Время завершения копирования базы данных в формате UTC в региональном центре обработки данных [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. После копирования новая база данных транзакционно согласована с базой данных-источником. Сведения о завершении обновляются каждые 1 минуту.<br /><br />Время в формате UTC, отражающее Последнее обновление поля percent_complete.|  
|**percent_complete**|**real**|Процентное соотношение скопированных данных в байтах. Допустимы значения от 0 до 100. [!INCLUDE[ssSDS](../../includes/sssds-md.md)] может автоматически восстановиться после некоторых ошибок, например отработки отказа, и перезапустить копирование базы данных. В этом случае percent_complete перезапустится при значении 0.|  
|**error_code**|**int**|Если значение больше 0, это код ошибки, возникшей при копировании. Значение равно 0, если ошибки не возникли.|  
|**error_desc**|**nvarchar (4096)**|Описание ошибки, возникшей при копировании.|  
|**error_severity**|**int**|Возвращает 16, если во время копирования базы данных произошла ошибка.|  
|**error_state**|**int**|Возвращает значение 1, если при копировании возникла ошибка.|  
|**copy_guid**|**uniqueidentifier**|Уникальный идентификатор операции копирования.|  
|**partner_server**|**sysname**|Имя сервера базы данных SQL, на котором создана копия.|  
|**partner_database**|**sysname**|Имя копии базы данных на сервере-участнике.|  
|**replication_state**|**tinyint**|Состояние репликации непрерывного копирования для этой базы данных. Доступны следующие значения:<br /><br /> 0 = ожидание. Создание копии базы данных запланировано, но необходимые шаги подготовки еще не завершены или временно заблокированы квотой заполнения.<br /><br /> 1 = заполнение. Заполненная база данных копирования еще не полностью синхронизирована с базой данных-источником. В этом состоянии невозможно подключиться к копии. Чтобы отменить выполняемую операцию заполнения, необходимо удалить базу данных копирования.|  
|**replication_state_desc**|**nvarchar(256)**|Описание replication_state (одно из следующих значений):<br /><br /> PENDING<br /><br /> SEEDING<br />|  
|**maximum_lag**|**int**|Зарезервированное поле.|  
|**is_continuous_copy**|**bit**|0 = возвращает 0|  
|**is_target_role**|**bit**|0 = база данных источника<br /><br /> 1 = копировать базу данных|  
|**is_interlink_connected**|bit|Зарезервированное поле.|  
|**is_offline_secondary**|bit|Зарезервированное поле.|  
  
## <a name="permissions"></a>Разрешения  
 Это представление доступно в базе данных **master** только для входа субъекта уровня сервера.  
  
## <a name="remarks"></a>Комментарии  
 Можно использовать представление **sys.dm_database_copies** в базе данных **master** исходного или целевого [!INCLUDE[ssSDS](../../includes/sssds-md.md)] сервера. Когда копирование базы данных завершается успешно и новая база данных переходит в режим «в сети», строка в представлении **sys.dm_database_copies** удаляется автоматически.  
  
  

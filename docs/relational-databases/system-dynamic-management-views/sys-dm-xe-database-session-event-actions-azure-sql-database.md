---
description: sys.dm_xe_database_session_event_actions (база данных SQL Azure)
title: sys.dm_xe_database_session_event_actions
titleSuffix: Azure SQL Database
ms.date: 06/10/2016
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: language-reference
ms.assetid: 48519fd9-c7c2-434b-848d-ccbf41133fdd
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-current
ms.custom: seo-lt-2019
ms.openlocfilehash: 0a43b7f39199a05b5ef864de0819459b05363931
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440699"
---
# <a name="sysdm_xe_database_session_event_actions-azure-sql-database"></a>sys.dm_xe_database_session_event_actions (база данных SQL Azure)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Возвращает сведения о действиях сеанса событий. Действия выполняются, когда происходят события. В этом административном представлении собраны статистические данные о количестве выполнений действия и общем времени выполнения этого действия.  
  
||  
|-|  
|**Применимо к** [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] версии 12 и всем будущим версиям.|  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|event_session_address|**varbinary(8)**|Адрес сеанса событий в памяти. Не допускает значение NULL.|  
|action_name|**nvarchar(60)**|Имя действия. Не допускает значение NULL.|  
|action_package_guid|**uniqueidentifier**|Идентификатор GUID пакета, который содержит это действие. Не допускает значение NULL.|  
|event_name|**nvarchar(60)**|Имя события, для которого предназначено действие. Не допускает значение NULL.|  
|event_package_guid|**uniqueidentifier**|Идентификатор GUID пакета, который содержит это событие. Не допускает значение NULL.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение VIEW DATABASE STATE.  
  
### <a name="relationship-cardinalities"></a>Количество элементов связей  
  
|От|Кому|Связь|  
|----------|--------|------------------|  
|sys.dm_xe_database_session_event_actions sys.dm_xe_database_session_event_actions.event_session_address|sys.dm_xe_database_sessions. Address|«многие к одному»|  
|sys.dm_xe_database_session_event_actions sys.dm_xe_database_session_event_actions.action_name<br /><br /> sys.dm_xe_session_event_actions.action_package_guid|sys.dm_xe_objects.name<br /><br /> sys.dm_xe_database_session_events sys.dm_xe_database_session_events.event_package_guid|«многие к одному»|  
|sys.dm_xe_database_session_event_actions sys.dm_xe_database_session_event_actions.event_name<br /><br /> sys.dm_xe_database_session_event_actions sys.dm_xe_database_session_event_actions.event_package_guid|sys.dm_xe_objects.name<br /><br /> sys.dm_xe_objects.package_guid|«многие к одному»|  
  
## <a name="see-also"></a>См. также:  
 [Расширенные события](../../relational-databases/extended-events/extended-events.md)  
  
  

---
description: sys.dm_io_pending_io_requests (Transact-SQL)
title: sys.dm_io_pending_io_requests (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_io_pending_io_requests
- dm_io_pending_io_requests
- dm_io_pending_io_requests_TSQL
- sys.dm_io_pending_io_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_io_pending_io_requests dynamic management view
ms.assetid: d1fb46dd-5c74-4c04-9ecf-8934b1bedb5b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 845beacd63dfe2a1e67679467840a9bb0edff03d
ms.sourcegitcommit: f1a571b6ce02a39c385ad32508ceff23475ed9f5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2021
ms.locfileid: "106377471"
---
# <a name="sysdm_io_pending_io_requests-transact-sql"></a>sys.dm_io_pending_io_requests (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает по строке для каждого ожидающего выполнения запроса ввода-вывода в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Чтобы вызвать эту функцию из [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] или [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , используйте имя **sys.dm_pdw_nodes_io_pending_io_requests**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**io_completion_request_address**|**varbinary(8)**|Адрес запроса ввода-вывода в памяти. Не допускает значение NULL.|  
|**io_type**|**nvarchar(60)**|Тип запроса ввода-вывода, ожидающего выполнения. Не допускает значение NULL.|  
|**io_pending_ms_ticks**|**bigint**|Только для внутреннего применения. Не допускает значение NULL.| 
|**io_pending**|**int**|Указывает, находится ли запрос ввода-вывода в состоянии ожидания (1) или завершился операционной системой (0). Запрос ввода-вывода по-прежнему может быть отложен даже после того, как операционная система завершила запрос, но [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] еще не выполнил переключение контекста, при котором он обработал бы запрос ввода-вывода и удалит его из этого списка. Не допускает значение NULL. <br /> **Значение** <br /> 0 = ожидающие SQL Server <br /> 1 = операционная система, ожидающая <br />|  

|**io_completion_routine_address** | **varbinary (8)**| Внутренняя функция, вызываемая по завершении запроса ввода-вывода. Допускает значение null.  
|**io_user_data_address** | **varbinary (8)**| Только для внутреннего использования. Допускает значение null.  
|**scheduler_address** | **varbinary (8)**| Планировщик, на котором был выдан этот запрос ввода-вывода. Запрос ввода-вывода появляется в списке планировщика запросов ввода-вывода, ожидающих выполнения. Дополнительные сведения см. в разделе [sys.dm_os_schedulers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md). Не допускает значения NULL. |  
|**io_handle** | **varbinary (8)**| Описатель файла, который используется в запросе ввода-вывода. Допускает значение null.  
|**io_offset** | **bigint**| Смещение запроса ввода-вывода. Не допускает значения NULL. |  
|**io_handle_path** | **nvarchar (256)**| Путь к файлу, используемому в запросе ввода-вывода. Допускает значение null. | **pdw_node_id** | **тип int** | **Применимо к**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] ,[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Идентификатор узла, на котором находится данное распределение. |  
  
## <a name="permissions"></a>Разрешения  

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления и функции, связанные с I O &#40;языке Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md)  
  

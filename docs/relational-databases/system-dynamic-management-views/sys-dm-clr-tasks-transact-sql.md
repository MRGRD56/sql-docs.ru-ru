---
description: sys.dm_clr_tasks (Transact-SQL)
title: sys.dm_clr_tasks (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_clr_tasks
- sys.dm_clr_tasks_TSQL
- dm_clr_tasks
- dm_clr_tasks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_tasks dynamic management view
ms.assetid: 462b9061-09fa-4858-9707-03d6cc19c769
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c16eea33c1c0211b37823ecff1e2ae1131d2a335
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837621"
---
# <a name="sysdm_clr_tasks-transact-sql"></a>sys.dm_clr_tasks (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает по одной строке для всех задач среды CLR, выполняющихся в данный момент. Пакет языка [!INCLUDE[tsql](../../includes/tsql-md.md)], который содержит ссылку на процедуру среды CLR, создает отдельную задачу для выполнения всего управляемого кода в пакете. Несколько инструкций пакета, которые требуют выполнения управляемого кода, используют одну и ту же задачу среды CLR. Задача среды CLR отвечает за поддержку объектов и состояний, относящихся к выполнению управляемого кода, а также за обмен данными между экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и средой CLR.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**task_address**|**varbinary(8)**|Адрес задачи среды CLR.|  
|**sos_task_address**|**varbinary(8)**|Адрес базовой задачи пакета языка [!INCLUDE[tsql](../../includes/tsql-md.md)].|  
|**appdomain_address**|**varbinary(8)**|Адрес домена приложений, в котором выполняется задача.|  
|**state**|**nvarchar(128)**|Текущее состояние задачи.|  
|**abort_state**|**nvarchar(128)**|Указывает на состояние прерывания (если выполнение задачи было отменено). Существует несколько состояний, связанных с прерыванием задач.|  
|**type**|**nvarchar(128)**|Тип задачи.|  
|**affinity_count**|**int**|Родственность задачи.|  
|**forced_yield_count**|**int**|Количество раз, когда задача принудительно выдавала результат.|  
  
## <a name="permissions"></a>Разрешения  

В [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] необходимо `VIEW SERVER STATE` разрешение.   
В базах данных SQL Basic, S0 и S1, а также для баз данных в эластичных пулах требуется учетная запись [администратора сервера](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) или учетная запись [администратора Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . Для всех остальных целей службы базы данных SQL `VIEW DATABASE STATE` разрешение требуется в базе данных.   
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Динамические административные представления, связанные со средой CLR &#40;языке Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  

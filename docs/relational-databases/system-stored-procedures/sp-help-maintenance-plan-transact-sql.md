---
description: sp_help_maintenance_plan (Transact-SQL)
title: sp_help_maintenance_plan (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_maintenance_plan_TSQL
- sp_help_maintenance_plan
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_maintenance_plan
ms.assetid: e972a510-960e-41d6-93c5-c71cd581a585
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4d996179f089b95a3270c9045778afaaf5c13cd9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200015"
---
# <a name="sp_help_maintenance_plan-transact-sql"></a>sp_help_maintenance_plan (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения об указанном плане обслуживания. Если конкретный план не указан, то данная хранимая процедура возвращает сведения обо всех планах обслуживания.  
  
> **Примечание.** Эта хранимая процедура используется с планами обслуживания базы данных. Эта возможность заменена планами обслуживания, не использующими данную хранимую процедуру. Используйте данную процедуру для поддержки планов обслуживания баз данных в установках, которые были обновлены с предыдущих версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_help_maintenance_plan [ [ @plan_id = ] 'plan_id' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @plan_id = ] 'plan\_id'` Указывает идентификатор плана плана обслуживания. *plan_id* имеет тип **uniqueidentifier**. Значение по умолчанию — NULL.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 Нет  
  
## <a name="result-sets"></a>Результирующие наборы  
 Если указано значение *plan_id* , **sp_help_maintenance_plan** вернет три таблицы: Plan, Database и Job.  
  
### <a name="plan-table"></a>Таблица Plan  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**plan_id**|**uniqueidentifier**|Идентификатор плана обслуживания.|  
|**plan_name**|**sysname**|Имя плана обслуживания.|  
|**date_created**|**datetime**|Дата создания плана обслуживания.|  
|**владельцев**|**sysname**|Владелец плана обслуживания.|  
|**max_history_rows**|**int**|Максимальное количество строк, выделенное для журнала плана обслуживания в системной таблице.|  
|**remote_history_server**|**int**|Имя удаленного сервера, на который может быть записан хронологический отчет.|  
|**max_remote_history_rows**|**int**|Максимальное количество строк, выделенное в системной таблице на удаленном сервере, куда может быть записан хронологический отчет.|  
|**user_defined_1**|**int**|Значение по умолчанию — NULL.|  
|**user_defined_2**|**nvarchar (100)**|Значение по умолчанию — NULL.|  
|**user_defined_3**|**datetime**|Значение по умолчанию — NULL.|  
|**user_defined_4**|**uniqueidentifier**|Значение по умолчанию — NULL.|  
  
### <a name="database-table"></a>Таблица Database  
  
|Имя столбца|Описание|  
|-----------------|-----------------|  
|**database_name**|Имя всех баз данных, связанных с планом обслуживания. Аргумент *database_name* имеет тип **sysname**.|  
  
### <a name="job-table"></a>Таблица Job  
  
|Имя столбца|Описание|  
|-----------------|-----------------|  
|**job_id**|Идентификатор всех заданий, связанных с планом обслуживания. *job_id* имеет тип **uniqueidentifier**.|  
  
## <a name="remarks"></a>Замечания  
 **sp_help_maintenance_plan** находится в базе данных **msdb** .  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** могут выполнять **sp_help_maintenance_plan**.  
  
## <a name="examples"></a>Примеры  
 В этом примере возвращаются сведения описательного характера о плане обслуживания FAD6F2AB-3571-11D3-9D4A-00C04FB925FC.  
  
```  
EXECUTE   sp_help_maintenance_plan   
   N'FAD6F2AB-3571-11D3-9D4A-00C04FB925FC';  
```  
  
## <a name="see-also"></a>См. также  
 [Планы обслуживания](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Хранимые процедуры плана обслуживания базы данных &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)  
  
  

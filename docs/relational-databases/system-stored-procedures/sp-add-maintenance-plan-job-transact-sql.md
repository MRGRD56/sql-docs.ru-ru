---
description: sp_add_maintenance_plan_job (Transact-SQL)
title: sp_add_maintenance_plan_job (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_maintenance_plan_job_TSQL
- sp_add_maintenance_plan_job
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_maintenance_plan_job
ms.assetid: 7205855c-964f-4f55-bf75-39a55f6fe7bd
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c0d4d5652c26e2d5fd2947953c7865b247b9448a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208032"
---
# <a name="sp_add_maintenance_plan_job-transact-sql"></a>sp_add_maintenance_plan_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Связывает план обслуживания с существующим заданием.  
  
> [!NOTE]  
>  Эта хранимая процедура используется планами обслуживания базы данных. Эта возможность заменена планами обслуживания, не использующими данную хранимую процедуру. Используйте данную процедуру для поддержки планов обслуживания баз данных в установках, которые были обновлены с предыдущих версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_add_maintenance_plan_job [ @plan_id = ] 'plan_id' , [ @job_id = ] 'job_id'  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @plan_id = ] 'plan_id'` Указывает идентификатор плана обслуживания. *plan_id* имеет тип **uniqueidentifier** и должен быть допустимым идентификатором.  
  
`[ @job_id = ] 'job_id'` Указывает идентификатор задания, связанного с планом обслуживания. *job_id* имеет тип **uniqueidentifier** и должен быть допустимым идентификатором. Чтобы создать задание или задания, выполните **sp_add_job** или используйте SQL Server Management Studio.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Замечания  
 **sp_add_maintenance_plan_job** должны запускаться из базы данных **msdb** .  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** могут выполнять **sp_add_maintenance_plan_job**.  
  
## <a name="examples"></a>Примеры  
 В этом примере к плану обслуживания, созданному с помощью **sp_add_maintenance_plan_job**, добавляется задание "B8FCECB1-E22C-11D2-AA64-00C04F688EAE".  
  
```  
EXECUTE   sp_add_maintenance_plan_job N'FAD6F2AB-3571-11D3-9D4A-00C04FB925FC', N'B8FCECB1-E22C-11D2-AA64-00C04F688EAE';  
```  
  
## <a name="see-also"></a>См. также  
 [Планы обслуживания](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Хранимые процедуры плана обслуживания базы данных &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)  
  
  

---
description: Хранимая процедура sp_delete_maintenance_plan_db (Transact-SQL)
title: sp_delete_maintenance_plan_db (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_maintenance_plan_db_TSQL
- sp_delete_maintenance_plan_db
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_maintenance_plan_db
- maintenance plans [SQL Server], deleting
- removing maintenance plan
- disassociating maintenance plan
ms.assetid: d1e8afb5-12ee-492b-a770-ba708ed7c8a4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cc76fc7d139da1440f7bf22ca8ab452ff98bc3bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193761"
---
# <a name="sp_delete_maintenance_plan_db-transact-sql"></a>Хранимая процедура sp_delete_maintenance_plan_db (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Отсоединяет определенный план обслуживания от указанной базы данных.  
  
> [!NOTE]  
>  Эта хранимая процедура используется планами обслуживания базы данных. Эта возможность заменена планами обслуживания, не использующими данную хранимую процедуру. Используйте данную процедуру для поддержки планов обслуживания баз данных в установках, которые были обновлены с предыдущих версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_delete_maintenance_plan_db [ @plan_id = ] 'plan_id' ,   
     [ @db_name = ] 'database_name'   
```  
  
## <a name="arguments"></a>Аргументы  
`[ @plan_id = ] 'plan\_id'` Указывает идентификатор плана обслуживания. *plan_id* имеет тип **uniqueidentifier**.  
  
`[ @db_name = ] 'database\_name'` Указывает имя базы данных, удаляемой из плана обслуживания. Аргумент *database_name* имеет тип **sysname**.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="remarks"></a>Замечания  
 **sp_delete_maintenance_plan_db** должны запускаться из базы данных **msdb** .  
  
 **Sp_delete_maintenance_plan_db** хранимая процедура удаляет связь между планом обслуживания и указанной базой данных. она не удаляет и не уничтожает базу данных.  
  
 Когда **sp_delete_maintenance_plan_db** удаляет последнюю базу данных из плана обслуживания, хранимая процедура также удаляет план обслуживания.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** могут выполнять **sp_delete_maintenance_plan_db**.  
  
## <a name="examples"></a>Примеры  
 Удаляет план обслуживания в базе данных **AdventureWorks2012** , ранее добавленный с помощью **sp_add_maintenance_plan_db**.  
  
```  
EXECUTE   sp_delete_maintenance_plan_db N'FAD6F2AB-3571-11D3-9D4A-00C04FB925FC', N'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>См. также  
 [Планы обслуживания](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Хранимые процедуры плана обслуживания базы данных &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)  
  
  

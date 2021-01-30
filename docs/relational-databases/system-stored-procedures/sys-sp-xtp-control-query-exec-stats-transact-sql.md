---
description: sys.sp_xtp_control_query_exec_stats (Transact-SQL)
title: sys.sp_xtp_control_query_exec_stats (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 10/13/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_xtp_control_query_exec_stats_TSQL
- sys.sp_xtp_control_query_exec_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_xtp_control_query_exec_stats
ms.assetid: 4838125d-ad1e-479e-b7d2-42655e8f4f02
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a1047918fd6d337d863dbf43d7058d1e3571e1cb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180214"
---
# <a name="syssp_xtp_control_query_exec_stats-transact-sql"></a>sys.sp_xtp_control_query_exec_stats (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  Включает сбор статистики запросов для всех скомпилированных в собственном коде хранимых процедур экземпляра или определенных, скомпилированных в собственном коде хранимых процедур.  
  
 При включении сбора статистики производительность снижается. Если требуется устранить неполадки только одной или нескольких скомпилированных в собственном коде хранимых процедур, то включить сбор статистики можно только для этих хранимых процедур.  
  
 Сведения о включении сбора статистики на уровне процедуры для всех скомпилированных в собственном режиме хранимых процедур см. в разделе [sys.sp_xtp_control_proc_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
sp_xtp_control_query_exec_stats [ [ @new_collection_value = ] collection_value ],  
[ [ @database_id = ] database_id   
[ , [ @xtp_object_id = ] procedure_id ] ,   
[ @old_collection_value] ]  
```  
  
## <a name="arguments"></a>Аргументы  
 @new_collection_value = *значение*  
 Определяет, включен (1) или выключен (0) сбор статистики на уровне процедуры.  
  
 @new_collection_value При запуске присваивается значение 0 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 @database_id = = *database_id*, @xtp_object_id = *procedure_id*  
 Идентификатор базы данных и идентификатор объекта для скомпилированной в собственном коде хранимой процедуры. Если сбор статистики включен для экземпляра ([sys.sp_xtp_control_proc_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md)), выполняется сбор статистики по хранимой процедуре, скомпилированной в собственном режиме. При отключении сбора статистики для экземпляра сбор статистики для отдельных, скомпилированных в собственном коде хранимых процедур не отключается.  
  
 Используйте представление [sys. databases &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md), [sys. Procedures &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md), [DB_ID &#40;transact-SQL&#41;](../../t-sql/functions/db-id-transact-sql.md)или [object_id &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md) , чтобы получить идентификаторы для базы данных и хранимой процедуры.  
  
 @old_collection_value = *значение*  
 Возвращает текущее состояние.  
  
## <a name="return-code"></a>Код возврата  
 0 — успешное завершение. Ненулевое значение — ошибка.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо членство в предопределенной роли sysadmin.  
  
## <a name="code-sample"></a>Образец кода  
 В следующем примере кода показано, как включить сбор статистики для всех скомпилированных в собственном коде хранимых процедур экземпляра, а затем для определенной процедуры.  
  
```sql   
DECLARE @c bit  
  
EXEC [sys].[sp_xtp_control_query_exec_stats] @new_collection_value = 1;  
  
EXEC sp_xtp_control_query_exec_stats @old_collection_value=@c output;  
SELECT @c AS 'collection status';  
  
EXEC [sys].[sp_xtp_control_query_exec_stats] @new_collection_value = 1,   
@database_id = 5, @xtp_object_id = 341576255;  
  
EXEC sp_xtp_control_query_exec_stats @database_id = 5,   
@xtp_object_id = 341576255, @old_collection_value=@c output;  
  
SELECT @c AS 'collection status';  
```  
  
## <a name="see-also"></a>См. также:  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Выполняющаяся в памяти OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  

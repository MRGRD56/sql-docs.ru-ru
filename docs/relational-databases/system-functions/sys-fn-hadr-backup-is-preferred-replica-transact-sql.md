---
description: sys.fn_hadr_backup_is_preferred_replica (Transact-SQL)
title: sys.fn_hadr_backup_is_preferred_replica (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fn_hadr_backup_is_preferred_replica_TSQL
- sys.fn_hadr_backup_is_preferred_replica
- fn_hadr_backup_is_preferred_replica_TSQL
- fn_hadr_backup_is_preferred_replica
dev_langs:
- TSQL
helpviewer_keywords:
- backup on secondary replicas
- active secondary replicas [SQL Server], backup on secondary replicas
- sys.fn_hadr_backup_is_preferred_replica function
ms.assetid: 61b9be77-e2f6-4da1-b2ae-a62cbe226145
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 2b0cf6856f06b4afe5f73915be85d4d4e6163413
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338114"
---
# <a name="sysfn_hadr_backup_is_preferred_replica--transact-sql"></a>sys.fn_hadr_backup_is_preferred_replica (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Используется для определения, является ли текущая реплика предпочитаемой резервной репликой.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.fn_hadr_backup_is_preferred_replica ( 'dbname' )  
```  
  
## <a name="arguments"></a>Аргументы  
 "*dbname*"  
 Имя базы данных, резервная копия которой создается. Аргумент *dbname* имеет тип sysname.  
  
## <a name="returns"></a>Возвращаемое значение  
 Возвращает тип данных **bool**: 1, если база данных в текущем экземпляре находится на предпочтительной реплике; в противном случае — значение 0.  
  
## <a name="remarks"></a>Remarks  
 Используйте данную функцию в скрипте резервного копирования для определения, доступна ли текущая база данных на реплике, которая является предпочитаемой для резервного копирования. Вы можете запустить скрипт на любой доступной реплике. Каждая из данных задач обращается к одним и тем же данным для определения того, какую задачу следует выполнить, поэтому для создания резервной копии фактически обрабатывается только одна запланированная задача. Образец кода должен быть аналогичен следующему.  
  
```  
If sys.fn_hadr_backup_is_preferred_replica( @dbname ) <> 1   
BEGIN  
-- If this is not the preferred replica, exit (probably without error).  
END  
-- If this is the preferred replica, continue to do the backup.  
  
```  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-sysfn_hadr_backup_is_preferred_replica"></a>A. Использование sys.fn_hadr_backup_is_preferred_replica  
 В следующем примере возвращаемое значение равно 1, если текущая база данных является предпочитаемой репликой для резервного копия.  
  
```  
SELECT sys.fn_hadr_backup_is_preferred_replica ('TestDB');  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Связанные задачи  
  
-   [Настройка резервного копирования в репликах доступности (SQL Server)](../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [Функции групп доступности Always On &#40;Transact-SQL&#41;](../../relational-databases/system-functions/always-on-availability-groups-functions-transact-sql.md)   
 [Группы доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [CREATE AVAILABILITY GROUP (Transact-SQL)](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [ALTER AVAILABILITY GROUP (Transact-SQL)](../../t-sql/statements/alter-availability-group-transact-sql.md)   
 [Активные вторичные реплики: резервное копирование на второстепенных серверах &#40;Always on групп доступности&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md) [Always on представления каталога групп доступности &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)      
  
  

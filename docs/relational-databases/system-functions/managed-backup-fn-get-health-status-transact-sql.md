---
description: managed_backup.fn_get_health_status managed_backup (Transact-SQL)
title: managed_backup managed_backup.fn_get_health_status (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fn_get_health_status_TSQL
- smart_admin.fn_get_health_status_TSQL
- smart_admin.fn_get_health_status
- fn_get_health_status
dev_langs:
- TSQL
helpviewer_keywords:
- smart_admin.fn_get_health_status
- fn_get_health_status
ms.assetid: b376711d-444a-4b5e-b483-8df323b4e31f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b139114cb7be4094c652f1fbf69313d4f7f1f2db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207402"
---
# <a name="managed_backupfn_get_health_status-transact-sql"></a>managed_backup.fn_get_health_status managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Возвращает таблицу, состоящую из 0, одной или более строк объединенного числа ошибок, указанных расширенными событиями в течение заданного периода времени.  
  
 Функция используется для сообщения о состоянии работоспособности служб в разделе интеллектуального администрирования.  В настоящее время [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] поддерживается в области смарт-администрирования. Поэтому возвращаемые ошибки связаны с [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
managed_backup.fn_get_health_status([@begin_time = ] 'time_1' , [ @end_time = ] 'time_2')  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Аргументы  
 [@begin_time]  
 Начало периода времени, для которого вычисляется общее количество ошибок.  @begin_timeПараметр имеет тип DateTime. Значение по умолчанию — NULL. Если значение равно NULL, функция будет обрабатывать все события, возникшие за 30 минут до текущего времени.  
  
 [ @end_time]  
 Конец периода времени, для которого вычисляется общее количество ошибок. @end_timeПараметр имеет тип DateTime и значение по умолчанию NULL. Если значение равно NULL, функция будет обрабатывать расширенные события до текущего времени.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|number_of_storage_connectivity_errors|INT|Количество ошибок подключения при подключении программы к учетной записи хранения Azure.|  
|number_of_sql_errors|INT|Число ошибок, возвращаемых при подключении программы к компоненту SQL Server Engine.|  
|number_of_invalid_credential_errors|INT|Число ошибок, возвращаемых, когда программа пытается выполнить проверку подлинности с использованием учетных данных SQL.|  
|number_of_other_errors|INT|Количество ошибок в других категориях, помимо обмена данными, SQL или учетных данных.|  
|number_of_corrupted_or_deleted_backups|INT|Число удаленных или поврежденных файлов резервных копий.|  
|number_of_backup_loops|INT|Число сканирований агентом резервного копирования всех баз данных, настроенных с помощью [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].|  
|number_of_retention_loops|INT|Количество сканирований баз данных, выполняемых для оценки заданного срока хранения.|  
  
## <a name="best-practices"></a>Рекомендации  
 Эти суммарные значения можно использовать для контроля состояния работоспособности системы. Например, если столбец number_of_retention_loops получает значение 0 за 30 минут, то, возможно, управление хранением работает слишком долго или неправильно. Ненулевые столбцы ошибок могут означать неполадки, и для обнаружения проблемы следует ознакомиться с журналами расширенных событий. Кроме того, можно использовать хранимую процедуру **managed_backup. sp_get_backup_diagnostics** для получения списка расширенных событий, чтобы найти подробные сведения об ошибке.  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Требуются разрешения **SELECT** на функцию.  
  
## <a name="examples"></a>Примеры  
  
-   В следующем примере возвращается объединенное число ошибок за последние 30 минут времени выполнения.  
  
    ```  
    SELECT *  
    FROM managed_backup.fn_get_health_status(NULL, NULL)  
  
    ```  
  
-   В следующем примере возвращается объединенное число ошибок за текущую неделю.  
  
    ```  
    Use msdb  
    Go  
    DECLARE @startofweek datetime  
    DECLARE @endofweek datetime  
    SET @startofweek = DATEADD(Day, 1-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)   
    SET @endofweek = DATEADD(Day, 7-DATEPART(WEEKDAY, CURRENT_TIMESTAMP), CURRENT_TIMESTAMP)  
    SELECT *  
    FROM managed_backup.fn_get_health_status(@startofweek, @endofweek)  
  
    ```  
  
  

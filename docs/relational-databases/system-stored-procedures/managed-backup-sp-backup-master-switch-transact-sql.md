---
description: managed_backup.sp_backup_master_switch managed_backup (Transact-SQL)
title: managed_backup managed_backup.sp_backup_master_switch (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_ backup_master_switch
- smart_admin.sp_backup_master_switch
- sp_ backup_master_switch_TSQL
- smart_admin.sp_backup_master_switch_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_ backup_master_switch
- smart_admin.sp_backup_master_switch
ms.assetid: 1ed2b2b2-c897-41cc-bed5-1c6bc47b9dd2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 97202d989a21002d7f24c886378fd3f6c6b35ebd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205305"
---
# <a name="managed_backupsp_backup_master_switch-transact-sql"></a>managed_backup.sp_backup_master_switch managed_backup (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Приостанавливает или возобновляет компонент [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
 Эта хранимая процедура используется для приостановки и возобновления компонента [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Она обеспечивает сохранность всех параметров конфигурации и их применение при возобновлении работы компонента. При приостановке компонента [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] срок хранения не действует. Это означает отсутствие проверки для определения, следует ли удалять файлы из хранилища, имеются ли поврежденные файлы резервной копии или разрывы в цепочке журналов.  
  

  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```sql  
EXEC managed_backup.sp_backup_master_switch   
                     [@new_state = ] { 0 | 1}  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Аргументы  
 @state  
 Задает состояние [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. @stateПараметр имеет значение **bit**. При установке в значение 0 работа приостанавливается, а при установке в значение 1 — возобновляется.  
  
## <a name="return-code-value"></a>Значения кодов возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="security"></a>Безопасность  
 Описаны проблемы безопасности, связанные с разрешениями statement.Include как подраздела (заголовок H3). Рассмотрите включение других подразделов для цепочки владения и аудита, если потребуется.  
  
### <a name="permissions"></a>Разрешения  
 Требуется членство в роли базы данных **db_backupoperator** , с разрешениями **ALTER ANY CREDENTIAL** и **EXECUTE** для хранимой процедуры **sp_delete_backuphistory**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] приостанавливается в том экземпляре, в котором выполняется пример.  
  
```  
Use msdb;  
GO  
EXEC managed_backup.sp_backup_master_switch @new_state=0;  
Go  
  
```  
  
 В следующем примере [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] возобновляется.  
  
```  
Use msdb;  
GO  
EXEC managed_backup.sp_backup_master_switch @new_state=1;  
Go  
  
```  
  
## <a name="see-also"></a>См. также:  
 [Управляемое резервное копирование SQL Server в Microsoft Azure](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md)  
  
  

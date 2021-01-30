---
description: managed_backup.sp_backup_config_basic (Transact-SQL)
title: managed_backup managed_backup.sp_backup_config_basic (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 10/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_backup_config_basic_TSQL
- sp_backup_config_basic
- managed_backup.sp_backup_config_basic
- managed_backup.sp_backup_config_basic_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- managed_backup.sp_backup_config_basic
- sp_backup_config_basic
ms.assetid: 3ad73051-ae9a-4e41-a889-166146e5508f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0ef304af8088ee35aeb19022f5d7f6cf539f554d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193599"
---
# <a name="managed_backupsp_backup_config_basic-transact-sql"></a>managed_backup.sp_backup_config_basic (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Настраивает [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] Основные параметры для конкретной базы данных или для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Эту процедуру можно вызвать самостоятельно, чтобы создать базовую управляемую конфигурацию резервного копирования. Однако если вы планируете добавить дополнительные функции или пользовательское расписание, сначала настройте эти параметры с помощью [managed_backup. sp_backup_config_advanced &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-advanced-transact-sql.md) и [managed_backup. sp_backup_config_schedule &#40;transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md) , прежде чем включить управляемое резервное копирование с помощью этой процедуры.  
   
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```Transact-SQL   
EXEC managed_backup.sp_backup_config_basic  
    [@enable_backup = ] { 0 | 1}    ,[@database_name = ] 'database_name'    ,[@container_url = ] 'Azure_Storage_blob_container  
    ,[@retention_days = ] 'retention_period_in_days'    ,[@credential_name = ] 'sql_credential_name'  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> Аргументы  
 @enable_backup  
 Включает или выключает [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] для указанной базы данных. @enable_backupБит имеет значение **bit**. Обязательный параметр при настройке [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] для первого экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . При изменении существующей [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] конфигурации этот параметр является необязательным. В этом случае все значения конфигурации, которые не указаны, сохраняют свои существующие значения.  
  
 @database_name  
 Имя базы данных для включения управляемого резервного копирования в определенной базе данных.  
  
 @container_url  
 URL-адрес, указывающий расположение резервной копии. Если @credential_name параметр имеет значение null, этот URL-адрес является URL-адресом для контейнера больших двоичных объектов в службе хранилища Azure, а резервные копии используют новую резервную копию для блокировки функциональных возможностей больших двоичных объектов. Дополнительные сведения см. в обзоре [SAS](/azure/storage/common/storage-sas-overview). Если @credential_name указан параметр, то это URL-адрес учетной записи хранения, и резервные копии используют устаревшие функции резервного копирования для страничных BLOB-объектов.  
  
> [!NOTE]  
>  В настоящее время для этого параметра поддерживается только URL-адрес SAS.  
  
 @retention_days  
 Срок хранения файлов резервной копии в днях. Значение @storage_url равно int. Этот параметр является обязательным при настройке в [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] первый раз на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . При изменении [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] конфигурации этот параметр является необязательным. Если аргумент не указан, сохраняются существующие значения конфигурации.  
  
 @credential_name  
 Имя учетных данных SQL, используемых для проверки подлинности в учетной записи хранения Azure. @credentail_name Аргумент имеет тип **sysname**. Если этот параметр указан, резервная копия сохраняется в страничном BLOB-объекте. Если этот параметр имеет значение NULL, резервная копия будет храниться как блочный BLOB-объект. Резервное копирование на страничный BLOB-объект является устаревшим, поэтому рекомендуется использовать новые функции резервного копирования блочных больших двоичных объектов. При использовании для изменения конфигурации [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] этот параметр не обязателен. Если этот параметр не указан, существующие значения конфигурации сохраняются.  
  
> [!WARNING]
>  В настоящее время параметр **\@ credential_name** не поддерживается. Поддерживается только резервное копирование в блочный BLOB-объект, для которого этот параметр должен иметь значение NULL.  
  
## <a name="return-code-value"></a>Значения кодов возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Требуется членство в роли базы данных **db_backupoperator** , с разрешениями **ALTER ANY CREDENTIAL** и **EXECUTE** для хранимой процедуры **sp_delete_backuphistory** .  
  
## <a name="examples"></a>Примеры  
 Вы можете создать контейнер учетной записи хранения и URL-адрес SAS с помощью последних Azure PowerShell команд. В следующем примере создается новый контейнер MyContainer в учетной записи хранения mystorageaccount, а затем получается URL-адрес SAS с полными разрешениями.  
  
```powershell  
$context = New-AzureStorageContext -StorageAccountName mystorageaccount -StorageAccountKey (Get-AzureStorageKey -StorageAccountName mystorageaccount).Primary  
New-AzureStorageContainer -Name mycontainer -Context $context  
New-AzureStorageContainerSASToken -Name mycontainer -Permission rwdl -FullUri -Context $context  
```  
  
 В следующем примере показано [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] , как включить экземпляр SQL Server, в котором он выполняется, и устанавливает для политики хранения значение 30 дней, задает для назначения контейнер с именем "MyContainer" в учетной записи хранения с именем "mystorageaccount".  
  
```Transact-SQL 
Use msdb;  
Go  
   EXEC managed_backup.sp_backup_config_basic  
                @enable_backup=1  
                ,@container_url = 'https://mystorageaccount.blob.core.windows.net/mycontainer'  
                ,@retention_days=30;   
GO  
  
```
  
 Следующий пример отключает [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] для экземпляра SQL Server, в котором выполняется.  
  
```Transact-SQL  
Use msdb;  
Go  
EXEC managed_backup.sp_backup_config_basic  
                @enable_backup=0;  
GO  
  
```  
  
## <a name="see-also"></a>См. также:  
 [managed_backup managed_backup.sp_backup_config_advanced &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-advanced-transact-sql.md)   
 [managed_backup.sp_backup_config_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-schedule-transact-sql.md)  
  

---
title: Отключение управляемого резервного копирования в хранилище BLOB-объектов Azure
description: В этой статье показано, как отключить или приостановить управляемое резервное копирование SQL Server в Microsoft Azure на уровне базы данных и экземпляра с помощью Transact-SQL.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 3e02187f-363f-4e69-a82f-583953592544
author: cawrites
ms.author: chadam
ms.openlocfilehash: 30450a1ea8f6304e02ef2b544be31d08f69b51fd
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2020
ms.locfileid: "96129208"
---
# <a name="disable-sql-server-managed-backup-to-microsoft-azure"></a>Отключение управляемого резервного копирования SQL Server в Microsoft Azure
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описано, как отключить или приостановить [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] на уровне базы данных и экземпляра.  
  
##  <a name="disable-ss_smartbackup-for-a-database"></a><a name="DatabaseDisable"></a> Отключение [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] для базы данных  
 Настройки [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] можно отключить с помощью системной хранимой процедуры [managed_backup.sp_backup_config_basic (Transact-SQL)](../../relational-databases/system-stored-procedures/managed-backup-sp-backup-config-basic-transact-sql.md). Параметр *\@enable_backup* служит для включения и отключения конфигураций [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] для конкретной базы данных. При этом значение 1 включает, а значение 0 отключает параметры конфигурации.  
  
#### <a name="to-disable-ss_smartbackup-for-a-specific-database"></a>Чтобы отключить [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] для конкретной базы данных, выполните следующие действия.  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
```sql
EXEC msdb.managed_backup.sp_backup_config_basic  
                @database_name = 'TestDB'   
                ,@enable_backup = 0;  
GO
```

> [!NOTE]
> В зависимости от конфигурации также может потребоваться задать параметр `@container_url`.
  
##  <a name="disable-ss_smartbackup-for-all-the-databases-on-the-instance"></a><a name="DatabaseAllDisable"></a> Отключение [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] для всех баз данных в экземпляре  
 Следующая процедура позволяет отключить параметры конфигурации [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] для всех баз данных, для которых функция [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] включена в экземпляре.  Такие параметры конфигурации, как URL-адрес хранилища, политика хранения и учетные данные SQL, сохранятся в метаданных, и их можно будет использовать, если функция [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] будет включена для базы данных в дальнейшем. Если нужно просто временно приостановить службы [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] , можно использовать основной переключатель, описанный далее в этом разделе.  
  
#### <a name="to-disable-ss_smartbackup-for-all-the-databases"></a>Чтобы отключить [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] для всех баз данных, выполните указанные ниже действия.  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В приведенном ниже примере определяется, настроена ли функция [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] на уровне экземпляра и во всех базах данных с активированной функцией [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] , а затем выполняется хранимая процедура **sp_backup_config_basic** для отключения [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
```sql
-- Create a working table to store the database names  
Declare @DBNames TABLE  
  
       (  
             RowID int IDENTITY PRIMARY KEY  
             ,DBName varchar(500)  
  
       )  
-- Define the variables  
DECLARE @rowid int  
DECLARE @dbname varchar(500)  
DECLARE @SQL varchar(2000)  
-- Get the database names from the system function  
  
INSERT INTO @DBNames (DBName)  
  
SELECT db_name  
       FROM   
  
       msdb.managed_backup.fn_backup_db_config (NULL)  
       WHERE is_managed_backup_enabled = 1 
       AND is_dropped = 0
  
       --Select DBName from @DBNames  
  
       select @rowid = min(RowID)  
       FROM @DBNames  
  
       WHILE @rowID IS NOT NULL  
       Begin  
  
             Set @dbname = (Select DBName From @DBNames Where RowID = @rowid)  
             Begin  
             Set @SQL = 'EXEC msdb.managed_backup.sp_backup_config_basic    
                @database_name= '''+'' + @dbname+ ''+''',   
                @enable_backup=0'  
  
            EXECUTE (@SQL)  
  
             END  
             Select @rowid = min(RowID)  
             From @DBNames Where RowID > @rowid  
  
       END  
```  
  
 Чтобы просмотреть параметры конфигурации для всех баз данных в экземпляре, выполните следующий запрос.  
  
```sql
Use msdb;  
GO  
SELECT * FROM managed_backup.fn_backup_db_config (NULL);  
GO  
```  
  
##  <a name="disable-default-ss_smartbackup-settings-for-the-instance"></a><a name="InstanceDisable"></a> Отключение параметров [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] по умолчанию для экземпляра  
 Параметры по умолчанию на уровне экземпляра применяются ко всем новым базам данных, созданным в этом экземпляре.  Если параметры по умолчанию больше не нужны, то можно отключить эту конфигурацию с помощью системной хранимой процедуры **managed_backup.sp_backup_config_basic** с параметром *\@database_name*, имеющим значение NULL. При отключении остальные параметры конфигурации, такие как URL-адрес хранилища, политика хранения или учетные данные SQL, не удаляются. Эти настройки используются в том случае, если функция [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] будет включена для экземпляра позднее.  
  
#### <a name="to-disable-ss_smartbackup-default-configuration-settings"></a>Чтобы отключить параметры конфигурации [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] по умолчанию, выполните следующие действия.  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```sql
    EXEC msdb.managed_backup.sp_backup_config_basic  
                    @enable_backup = 0;  
    GO
    ```  
  
##  <a name="pause-ss_smartbackup-at-the-instance-level"></a><a name="InstancePause"></a> Приостановка [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] на уровне экземпляра  
 Возможны ситуации, когда требуется приостановить службы [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] на короткий период времени.  Системная хранимая процедура **managed_backup.sp_backup_master_switch** позволяет отключить службу [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] на уровне экземпляра.  Та же хранимая процедура используется для возобновления [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]. Параметр \@state определяет, следует ли включить или отключить [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)].  
  
#### <a name="to-pause-ss_smartbackup-services-using-transact-sql"></a>Чтобы приостановить службы [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] с помощью Transact-SQL, выполните следующие действия.  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте и вставьте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
```sql
Use msdb;  
GO  
EXEC managed_backup.sp_backup_master_switch @new_state=0;  
Go
```  
  
#### <a name="to-resume-ss_smartbackup-using-transact-sql"></a>Возобновление работы [!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)] с помощью Transact-SQL  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте и вставьте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
```sql
Use msdb;  
Go  
EXEC managed_backup.sp_backup_master_switch @new_state=1;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Включение управляемого резервного копирования SQL Server в Microsoft Azure](../../relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure.md)  
  
  

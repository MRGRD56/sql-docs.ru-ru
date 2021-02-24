---
description: Перемещение пользовательских баз данных
title: Перемещение пользовательских баз данных | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- disaster recovery [SQL Server], moving database files
- database files [SQL Server], moving
- data files [SQL Server], moving
- editions [SQL Server], moving databases between
- moving full-text catalogs
- scheduled disk maintenace [SQL Server]
- moving databases
- full-text catalogs [SQL Server], moving
- moving database files
- moving user databases
- relocating database files
- planned database relocations [SQL Server]
- databases [SQL Server], moving
ms.assetid: ad9a4e92-13fb-457d-996a-66ffc2d55b79
author: stevestein
ms.author: sstein
ms.openlocfilehash: 64b8416375cdc61c43e8799b33891096858a79f7
ms.sourcegitcommit: 059722ff78a6061b801807416b312ae9f721ec7d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100636727"
---
# <a name="move-user-databases"></a>Перемещение пользовательских баз данных
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]позволяет переносить в новое место файлы данных, журнала и полнотекстового каталога пользовательской базы данных; новое место указывается при помощи предложения FILENAME инструкции [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) . Этот метод подходит для перемещения файлов базы данных в пределах одного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Для переноса базы данных на другой экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или другой сервер применяются операции [резервного копирования и восстановления](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md) или [отключения и подключения](../../relational-databases/databases/move-a-database-using-detach-and-attach-transact-sql.md).  
  
## <a name="considerations"></a>Рекомендации  
 Чтобы обеспечить целостность работы пользователей и приложений при перемещении базы данных на другой экземпляр сервера, необходимо повторно создать некоторые или все метаданные базы данных. Дополнительные сведения см. в статье [Управление метаданными при обеспечении доступности базы данных на другом экземпляре сервера (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).  
  
 Некоторые функции компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] изменяют способ, с помощью которого [!INCLUDE[ssDE](../../includes/ssde-md.md)] хранит информацию в файлах базы данных. Эти функции зависят от конкретных выпусков [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. База данных, содержащая данные функции, не может быть перемещена в выпуск [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , который их не поддерживает. Используйте динамическое административное представление sys.dm_db_persisted_sku_features чтобы просмотреть список всех зависящих от выпуска функций, включенных в текущей базе данных.  
  
 Для выполнения процедур, описанных в данном разделе, необходимо логическое имя файлов базы данных. Это имя можно получить из столбца name представления каталога [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) .  
  
 Начиная с [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], полнотекстовые каталоги интегрированы в базу данных, а не хранятся в файловой системе. Полнотекстовые каталоги теперь перемещаются автоматически при перемещении базы данных.  
  
## <a name="planned-relocation-procedure"></a>Процедура запланированного перемещения  
 Для запланированного перемещения файлов журнала или данных выполните следующие действия.  
  
1.  Для каждого перемещаемого файла выполните следующую инструкцию.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name, FILENAME = 'new_path\os_file_name' );  
    ```  
  
2.  Выполните следующую инструкцию:  
  
    ```  
    ALTER DATABASE database_name SET OFFLINE;  
    ```  
  
3.  Переместите файл или файлы в новое расположение.  
  
4.  Выполните следующую инструкцию:  
  
    ```  
    ALTER DATABASE database_name SET ONLINE;  
    ```  
  
5.  Проверьте изменения в файле с помощью следующего запроса.  

    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="relocation-for-scheduled-disk-maintenance"></a>Перемещение для запланированного обслуживания дисков  
 Чтобы переместить файл во время процесса запланированного обслуживания дисков, необходимо выполнить нижеприведенные шаги.  
  
1.  Для каждого перемещаемого файла выполните следующую инструкцию.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name , FILENAME = 'new_path\os_file_name' );  
    ```  
  
2.  Остановите работу экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или выключите систему для проведения работ по обслуживанию дисков. Дополнительные сведения см. в статье [Iniciar, parar, pausar, retomar e reiniciar os serviços SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Переместите файл или файлы в новое расположение.  
  
4.  Перезапустите экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или сервер. Дополнительные сведения см. в разделе [Запуск, остановка, приостановка, возобновление и перезапуск ядра СУБД, агента SQL Server и обозревателя SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
5.  Проверьте изменения в файле с помощью следующего запроса.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="failure-recovery-procedure"></a>Процедура восстановления после сбоя  
 Если файл необходимо переместить в новое место из-за аппаратного сбоя, выполните следующие действия.  
  
> [!IMPORTANT]  
>  Если базу данных запустить нельзя, она находится в подозрительном режиме или в невосстановленном состоянии, то файл может быть перемещен только членом предопределенной роли sysadmin.  
  
1.  Остановите работу экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , если он запущен.  
  
2.  Запустите экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в режиме восстановления «только master», запустив из командной строки одну из следующих команд.  
  
    -   В случае с экземпляром по умолчанию (MSSQLSERVER) выполните следующую команду.  
  
        ```  
        NET START MSSQLSERVER /f /T3608  
        ```  
  
    -   В случае с именованным экземпляром выполните следующую команду.  
  
        ```  
        NET START MSSQL$instancename /f /T3608  
        ```  
  
     Дополнительные сведения см. в статье [Iniciar, parar, pausar, retomar e reiniciar os serviços SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Для каждого перемещаемого файла используйте команды **sqlcmd** или [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] для выполнения следующей инструкции.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE( NAME = logical_name , FILENAME = 'new_path\os_file_name' );  
    ```  
  
     Дополнительные сведения об использовании программы **sqlcmd** см. в статье [Использование программы sqlcmd](../../ssms/scripting/sqlcmd-use-the-utility.md).  
  
4.  Завершите работу программы **sqlcmd** или [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
5.  Остановите экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
6.  Переместите файл или файлы в новое расположение.  
  
7.  Запустите экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Например, выполните команду `NET START MSSQLSERVER`.  
  
8.  Проверьте изменения в файле с помощью следующего запроса.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
## <a name="examples"></a>Примеры  
 В следующем примере файл журнала базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] переносится в новое место во время запланированного перемещения.  
  
```  
USE master;  
GO  
-- Return the logical file name.  
SELECT name, physical_name AS CurrentLocation, state_desc  
FROM sys.master_files  
WHERE database_id = DB_ID(N'AdventureWorks2012')  
    AND type_desc = N'LOG';  
GO  
ALTER DATABASE AdventureWorks2012 SET OFFLINE;  
GO  
-- Physically move the file to a new location.  
-- In the following statement, modify the path specified in FILENAME to  
-- the new location of the file on your server.  
ALTER DATABASE AdventureWorks2012   
    MODIFY FILE ( NAME = AdventureWorks2012_Log,   
                  FILENAME = 'C:\NewLoc\AdventureWorks2012_Log.ldf');  
GO  
ALTER DATABASE AdventureWorks2012 SET ONLINE;  
GO  
--Verify the new location.  
SELECT name, physical_name AS CurrentLocation, state_desc  
FROM sys.master_files  
WHERE database_id = DB_ID(N'AdventureWorks2012')  
    AND type_desc = N'LOG';  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)   
 [Присоединение и отсоединение базы данных (SQL Server)](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [Перемещение системных баз данных](../../relational-databases/databases/move-system-databases.md)   
 [Перемещение файлов базы данных](../../relational-databases/databases/move-database-files.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Запуск, остановка, приостановка, возобновление и перезапуск компонента Database Engine, агента SQL и службы браузера SQL Server](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  

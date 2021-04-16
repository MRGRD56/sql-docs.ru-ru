---
title: Перенос базы данных SQL Server из Windows в Linux
description: В этом руководстве рассказывается о том, как создать резервную копию базы данных SQL Server в Windows и восстановить ее на компьютере Linux с SQL Server.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/08/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 9ac64d1a-9fe5-446e-93c3-d17b8f55a28f
ms.openlocfilehash: 23a575bfabdd16f30007a8b70d84a568f45b8d3e
ms.sourcegitcommit: 8050df4db7a3a76e4fa03e5c79dcb49031defed7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/08/2021
ms.locfileid: "107210858"
---
# <a name="migrate-a-sql-server-database-from-windows-to-linux-using-backup-and-restore"></a>Перенос базы данных SQL Server из Windows в Linux с помощью резервного копирования и восстановления

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Если нужно перенести базу данных из SQL Server в Windows в SQL Server на Linux, рекомендуется использовать функцию резервного копирования и восстановления SQL Server. В этом руководстве приводятся пошаговые инструкции по переносу базы данных в Linux с помощью резервного копирования и восстановления.

> [!div class="checklist"]
> * Создание файла резервной копии в Windows с помощью SSMS
> * установка оболочки Bash в Windows;
> * Перенос файла резервной копии в Linux из оболочки Bash
> * Восстановление файла резервной копии в Linux с помощью Transact-SQL
> * Выполнение запроса для проверки переноса

Для переноса базы данных SQL Server из Windows в Linux можно также создать группу доступности Always On SQL Server. См. статью [Настройка кроссплатформенной группы доступности Always On SQL Server в Windows и Linux](sql-server-linux-availability-group-cross-platform.md).

## <a name="prerequisites"></a>предварительные требования

Для работы с этим руководством необходимо выполнить следующие условия.

* Компьютер Windows со следующими компонентами:
  * установленный экземпляр [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads);
  * установленная среда [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md);
  * целевая база данных для переноса.

* Компьютер Linux со следующими компонентами:
  * SQL Server ([RHEL](quickstart-install-connect-red-hat.md), [SLES](quickstart-install-connect-suse.md) или [Ubuntu](quickstart-install-connect-ubuntu.md)) с программами командной строки.

## <a name="create-a-backup-on-windows"></a>Создание резервной копии в Windows

Создать файл резервной копии базы данных в Windows можно несколькими способами. В приведенных ниже инструкциях используется SQL Server Management Studio (SSMS).

1. Запустите **SQL Server Management Studio** на компьютере Windows.

1. В диалоговом окне подключения введите **localhost**.

1. В обозревателе объектов разверните узел **Базы данных**.

1. Щелкните целевую базу данных правой кнопкой мыши, выберите пункт **Задачи**, а затем **Резервное копирование...**.

   ![Создание файла резервной копии с помощью SSMS](./media/sql-server-linux-migrate-restore-database/ssms-create-backup.png)

1. В диалоговом окне **Резервное копирование базы данных** в поле **Тип резервной копии** должно быть выбрано значение **Полная**, а в поле **Создать резервную копию на** — **Диск**. Запишите имя и расположение файла. Например, база данных с именем **YourDB** в SQL Server 2016 имеет путь резервного копирования по умолчанию `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\YourDB.bak`.

1. Нажмите кнопку **ОК**, чтобы создать резервную копию базы данных.

> [!NOTE]
> Другой вариант — выполнить запрос Transact-SQL для создания файла резервной копии. Следующая команда Transact-SQL выполняет те же действия, что и предыдущие инструкции, для базы данных с именем **YourDB**:
>
> ```sql
> BACKUP DATABASE [YourDB] TO  DISK =
> N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\YourDB.bak'
> WITH NOFORMAT, NOINIT, NAME = N'YourDB-Full Database Backup',
> SKIP, NOREWIND, NOUNLOAD, STATS = 10
> GO
> ```

## <a name="install-a-bash-shell-on-windows"></a>установка оболочки Bash в Windows;

Чтобы восстановить базу данных, необходимо сначала перенести файл резервной копии с компьютера Windows на целевой компьютер Linux. В этом руководстве файл переносится в Linux из оболочки Bash (окна терминала), запущенной в Windows.

1. Установите оболочку Bash на компьютере Windows, который поддерживает команды **scp** (безопасное копирование) и **ssh** (удаленный вход в систему). Вот два примера:

   * [подсистема Windows для Linux](/windows/wsl/about) (Windows 10);
   * оболочка Bash GIT ([https://git-scm.com/downloads](https://git-scm.com/downloads)).

1. Откройте сеанс Bash в Windows.

## <a name="copy-the-backup-file-to-linux"></a><a id="scp"></a> Копирование файла резервной копии в Linux

1. В сеансе Bash перейдите к каталогу, в котором содержится файл резервной копии. Пример.

   ```bash
   cd 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Backup\'
   ```

1. Перенесите файл на конечный компьютер Linux с помощью команды **scp**. В следующем примере файл **YourDB.bak** переносится в домашний каталог пользователя *user1* на сервере Linux с IP-адресом *192.0.2.9*:

   ```bash
   scp YourDB.bak user1@192.0.2.9:./
   ```
   ![Команда scp](./media/sql-server-linux-migrate-restore-database/scp-command.png)

> [!TIP]
> При передаче файлов есть альтернативы команде scp. Одна из них — настройка сетевой папки SMB между Windows и Linux с помощью [Samba](https://help.ubuntu.com/community/Samba). Пошаговое руководство для Ubuntu см. в статье [How to Create a Network Share Via Samba](https://help.ubuntu.com/community/How%20to%20Create%20a%20Network%20Share%20Via%20Samba%20Via%20CLI%20%28Command-line%20interface/Linux%20Terminal%29%20-%20Uncomplicated,%20Simple%20and%20Brief%20Way!) (Создание сетевой папки с помощью Samba). После ее создания вы можете получать к ней доступ как к общей сетевой папке из Windows, например **\\\\machinenameorip\\share**.

## <a name="move-the-backup-file-before-restoring"></a>Перенос файла резервной копии перед восстановлением

Сейчас файл резервной копии находится на сервере Linux в домашнем каталоге пользователя. Перед восстановлением базы данных в SQL Server необходимо поместить резервную копию в подкаталог **/var/opt/mssql**, так как ей владеет пользователь `mssql` и группа `mssql`. Если вы хотите изменить расположение резервной копии по умолчанию, см. сведения в статье [Настройка с помощью mssql-conf](sql-server-linux-configure-mssql-conf.md#backupdir).

1. В том же сеансе Bash в Windows подключитесь удаленно к целевому компьютеру Linux с помощью команды **ssh**. В следующем примере устанавливается подключение к компьютеру Linux с IP-адресом **192.0.2.9** от имени пользователя **user1**:

   ```bash
   ssh user1@192.0.2.9
   ```

   Теперь вы выполняете команды на удаленном сервере Linux.

1. Перейдите в режим суперпользователя.

   ```bash
   sudo su
   ```

1. Создайте каталог резервного копирования. Если каталог уже существует, параметр -p не оказывает никакого действия.

   ```bash
   mkdir -p /var/opt/mssql/backup
   ```

1. Переместите файл резервной копии в этот каталог. В приведенном ниже примере файл резервной копии находится в домашнем каталоге пользователя *user1*. Измените команду в соответствии с расположением и именем вашего файла резервной копии.

   ```bash
   mv /home/user1/YourDB.bak /var/opt/mssql/backup/
   ```

1. Выйдите из режима суперпользователя.

   ```bash
   exit
   ```

## <a name="restore-your-database-on-linux"></a>Восстановление базы данных в Linux

Чтобы восстановить резервную копию базы данных, можно воспользоваться командой Transact-SQL (TQL) **RESTORE DATABASE**.

> [!NOTE]
> В приведенных ниже инструкциях используется программа **sqlcmd**. Если вы еще не установили средства SQL Server, см. статью [Установка программ командной строки SQL Server в Linux](sql-server-linux-setup-tools.md).

1. В том же окне терминала запустите **sqlcmd**. В приведенном ниже пользователь **SA** подключается к локальному экземпляру SQL Server. Введите пароль, когда появится запрос, или укажите пароль с помощью параметра **-P**.

   ```bash
   sqlcmd -S localhost -U SA
   ```

1. В строке `>1` введите приведенную ниже команду **RESTORE DATABASE**, нажимая клавишу ВВОД после каждой строки (многострочную команду нельзя скопировать и вставить за один раз). Замените все вхождения `YourDB` на имя вашей базы данных.

   ```sql
   RESTORE DATABASE YourDB
   FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   WITH MOVE 'YourDB' TO '/var/opt/mssql/data/YourDB.mdf',
   MOVE 'YourDB_Log' TO '/var/opt/mssql/data/YourDB_Log.ldf'
   GO
   ```

   Должно появиться сообщение об успешном восстановлении базы данных.

   Команда `RESTORE DATABASE` может вернуть ошибку, как в следующем примере:

   ```bash
   File 'YourDB_Product' cannot be restored to 'Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf'. Use WITH MOVE to identify a valid location for the file.
   Msg 5133, Level 16, State 1, Server servername, Line 1
   Directory lookup for the file "Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf" failed with the operating system error 2(The system cannot find the file specified.).
   ```
   
   Это значит, что база данных содержит дополнительные файлы. Если эти файлы не указаны в предложении `MOVE` команды `RESTORE DATABASE`, процедура восстановления попытается создать их по тому же пути, что и на исходном сервере. 

   Вы можете вывести список всех файлов, содержащихся в резервной копии:
   ```sql
   RESTORE FILELISTONLY FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   GO
   ```
   Вы должны получить список, аналогичный следующему (приводятся только два первых столбца):
   ```sql
   LogicalName         PhysicalName                                                                 ..............
   ----------------------------------------------------------------------------------------------------------------------
   YourDB              Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB.mdf          ..............
   YourDB_Product      Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Product.ndf  ..............
   YourDB_Customer     Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Customer.ndf ..............
   YourDB_log          Z:\Microsoft SQL Server\MSSQL11.GLOBAL\MSSQL\Data\YourDB\YourDB_Log.ldf      ..............
   ```
   
   Этот список можно использовать для создания предложений `MOVE` для дополнительных файлов. В этом примере команда `RESTORE DATABASE` имеет следующий вид:

   ```sql
   RESTORE DATABASE YourDB
   FROM DISK = '/var/opt/mssql/backup/YourDB.bak'
   WITH MOVE 'YourDB' TO '/var/opt/mssql/data/YourDB.mdf',
   MOVE 'YourDB_Product' TO '/var/opt/mssql/data/YourDB_Product.ndf',
   MOVE 'YourDB_Customer' TO '/var/opt/mssql/data/YourDB_Customer.ndf',
   MOVE 'YourDB_Log' TO '/var/opt/mssql/data/YourDB_Log.ldf'
   GO
   ```


1. Проверьте восстановление, получив список всех баз данных на сервере. Восстановленная база данных должна быть указана в списке.

   ```sql
   SELECT Name FROM sys.Databases
   GO
   ```

1. Выполните другие запросы к перенесенной базе данных. Приведенная ниже команда переключает контекст на базу данных **YourDB** и выбирает строки из одной из ее таблиц.

   ```sql
   USE YourDB
   SELECT * FROM YourTable
   GO
   ```

1. Когда вы завершите работу с **sqlcmd**, введите `exit`.

1. Когда вы завершите работу в удаленном сеансе **ssh**, введите `exit` еще раз.

## <a name="next-steps"></a>Дальнейшие шаги

В этом руководстве вы узнали, как создать резервную копию базы данных в Windows и переместить ее на сервер Linux с SQL Server. Вы ознакомились с выполнением следующих задач:
> [!div class="checklist"]
> * создание файла резервной копии в Windows с помощью SSMS и Transact-SQL;
> * установка оболочки Bash в Windows;
> * перенос файлов резервных копий из Windows в Linux с помощью команды **scp**;
> * удаленное подключение к компьютеру Linux с помощью команды **ssh**;
> * перемещение файла резервной копии для подготовки к восстановлению;
> * выполнение команд Transact-SQL с помощью **sqlcmd**;
> * восстановление резервной копии базы данных с помощью команды **RESTORE DATABASE**; 
> * выполнение запроса для проверки переноса.

Далее вы можете ознакомиться с другими сценариями переноса для SQL Server на Linux. 

> [!div class="nextstepaction"]
>[Перенос баз данных в SQL Server на Linux](sql-server-linux-migrate-overview.md)

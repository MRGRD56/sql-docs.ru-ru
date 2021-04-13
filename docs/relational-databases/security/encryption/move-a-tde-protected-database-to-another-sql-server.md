---
title: Перенос базы данных с защитой TDE на другой SQL Server
description: Инструкции по защите базы данных с применением прозрачного шифрования данных (TDE) и перемещению базы данных в другой экземпляр SQL Server с помощью SQL Server Management Studio (SSMS) или Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Transparent Data Encryption, moving
- TDE, moving a database
ms.assetid: fb420903-df54-4016-bab6-49e6dfbdedc7
author: shohamMSFT
ms.author: shohamd
ms.openlocfilehash: d4024c5f1833eb7013e67cfa082d6924c868d0eb
ms.sourcegitcommit: cfffd03fe39b04034fa8551165476e53c4bd3c3b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "107298844"
---
# <a name="move-a-tde-protected-database-to-another-sql-server"></a>Переместить базу данных, защищаемую прозрачным шифрованием, в другой экземпляр SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описано, как защитить базу данных с применением прозрачного шифрования данных и переместить базу данных в другой экземпляр [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с помощью [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Функция прозрачного шифрования данных выполняет шифрование и дешифрование ввода-вывода в реальном времени для файлов данных и журналов. При шифровании используется ключ шифрования базы данных (DEK), который хранится в загрузочной записи базы данных, где можно получить к нему доступ при восстановлении. Ключ шифрования базы данных является симметричным ключом, защищенным сертификатом, который хранится в базе данных **master** на сервере, или асимметричным ключом, защищенным модулем расширенного управления ключами.   
   
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   В случае перемещения базы данных, защищаемой прозрачным шифрованием, также необходимо переместить сертификат или асимметричный ключ, который служит для открытия ключа шифрования базы данных. Сертификат или асимметричный ключ должен быть установлен в базе данных **master** целевого сервера, чтобы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] имел доступ к файлам базы данных. Дополнительные сведения см. в статье [Прозрачное шифрование данных (TDE)](../../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
-   Для восстановления сертификата необходимо хранить копии файла сертификата и файла закрытого ключа. Пароль для закрытого ключа не обязательно должен совпадать с паролем главного ключа базы данных.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] хранит созданные файлы здесь в папке **C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA** по умолчанию. В других системах имена и расположения файлов могут быть иными.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permissions  
  
-   Требуется разрешение **CONTROL DATABASE** в базе данных **master** для создания главного ключа базы данных.  
  
-   Требуется разрешение **CREATE CERTIFICATE** в базе данных **master** для создания сертификата, защищающего ключ шифрования базы данных.  
  
-   Требуется разрешение **CONTROL DATABASE** в зашифрованной базе данных и разрешение **VIEW DEFINITION** на сертификат или асимметричный ключ, используемый для шифрования ключа шифрования базы данных.  
  
##  <a name="to-create-a-database-protected-by-transparent-data-encryption"></a><a name="SSMSProcedure"></a> Создание базы данных, защищаемой с применением прозрачного шифрования данных  

В следующих процедурах показано, как создать базу данных, защищенную с помощью TDE, с использованием SQL Server Management Studio и Transact-SQL.
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSCreate"></a> Использование среды SQL Server Management Studio  
  
1.  Создание главного ключа базы данных и сертификата в базе данных **master** . Дополнительные сведения см. в статье **Использование Transact-SQL** ниже.  
  
2.  Создайте резервную копию сертификата сервера в базе данных **master** . Дополнительные сведения см. в статье **Использование Transact-SQL** ниже.  
  
3.  В обозревателе объектов щелкните правой кнопкой мыши папку **Базы данных** и выберите **Создать базу данных**.  
  
4.  В диалоговом окне **Создание базы данных** в поле **Имя базы данных** введите имя новой базы данных.  
  
5.  В поле **Владелец** введите имя владельца новой базы данных. Также можно нажать кнопку с многоточием **(...)** , чтобы открыть диалоговое окно **Выбор владельца базы данных**. Дополнительные сведения о создании новой базы данных см. в разделе [Create a Database](../../../relational-databases/databases/create-a-database.md).  
  
6.  В обозревателе объектов щелкните значок «плюс», чтобы развернуть папку **Базы данных** .  
  
7.  Щелкните правой кнопкой мыши созданную базу данных, выберите **Задачи**, затем **Управление шифрованием базы данных**.  
  
     В диалоговом окне **Управление шифрованием базы данных** доступны следующие параметры.  
  
     **Алгоритм шифрования**  
     Отображает или устанавливает алгоритм для шифрования базы данных. Алгоритм шифрования по умолчанию —**AES128** . Это поле не может быть пустым. Дополнительные сведения об алгоритмах шифрования см. в разделе [Choose an Encryption Algorithm](../../../relational-databases/security/encryption/choose-an-encryption-algorithm.md).  
  
     **Использовать сертификат сервера**  
     Задает шифрование с защитой сертификатом. Выберите его из списка. Если разрешение **VIEW DEFINITION** для сертификатов сервера отсутствует, этот список будет пустым. Если выбран метод шифрования с сертификатом, это значение не может быть пустым. Дополнительные сведения о сертификатах см. в разделе [SQL Server Certificates and Asymmetric Keys](../../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
     **Использовать асимметричный ключ сервера**  
     Задает шифрование с защитой асимметричным ключом. Отображаются только доступные асимметричные ключи. Только асимметричный ключ, защищенный модулем расширенного управления ключами, позволяет шифровать базу данных с помощью прозрачного шифрования данных.  
  
     **Задать шифрование базы данных для**  
     Переключает базу данных между включенным (флажок установлен) или выключенным (флажок снят) режимом прозрачного шифрования данных.  
  
8.  После завершения нажмите кнопку **ОК**.  

###  <a name="using-transact-sql"></a><a name="TsqlCreate"></a> Использование Transact-SQL  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```sql  
    -- Create a database master key and a certificate in the master database.  
    USE master ;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '*rt@40(FL&dasl1';  
    GO  
    CREATE CERTIFICATE TestSQLServerCert   
    WITH SUBJECT = 'Certificate to protect TDE key'  
    GO  
    -- Create a backup of the server certificate in the master database.  
    -- The following code stores the backup of the certificate and the private key file in the default data location for this instance of SQL Server   
    -- (C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA).  
  
    BACKUP CERTIFICATE TestSQLServerCert   
    TO FILE = 'TestSQLServerCert'  
    WITH PRIVATE KEY   
    (  
        FILE = 'SQLPrivateKeyFile',  
        ENCRYPTION BY PASSWORD = '*rt@40(FL&dasl1'  
    );  
    GO  
    -- Create a database to be protected by TDE.  
    CREATE DATABASE CustRecords ;  
    GO  
    -- Switch to the new database.  
    -- Create a database encryption key, that is protected by the server certificate in the master database.   
    -- Alter the new database to encrypt the database using TDE.  
    USE CustRecords;  
    GO  
    CREATE DATABASE ENCRYPTION KEY  
    WITH ALGORITHM = AES_128  
    ENCRYPTION BY SERVER CERTIFICATE TestSQLServerCert;  
    GO  
    ALTER DATABASE CustRecords  
    SET ENCRYPTION ON;  
    GO  
    ```  
  
 Дополнительные сведения см. в разделе:  
  
-   [CREATE MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE (Transact-SQL)](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [BACKUP CERTIFICATE (Transact-SQL)](../../../t-sql/statements/backup-certificate-transact-sql.md)  
  
-   [CREATE DATABASE (SQL Server Transact-SQL)](../../../t-sql/statements/create-database-transact-sql.md)  
  
-   [CREATE DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)  
  
-   [ALTER DATABASE (Transact-SQL)](../../../t-sql/statements/alter-database-transact-sql.md)  
  
##  <a name="to-move-a-database-protected-by-transparent-data-encryption"></a><a name="TsqlProcedure"></a> Перемещение базы данных, защищаемой с применением прозрачного шифрования данных 

В следующих процедурах показано, как переместить базу данных, защищенную с помощью TDE, с использованием SQL Server Management Studio и Transact-SQL.
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSMove"></a> Использование среды SQL Server Management Studio  
  
1.  В обозревателе объектов щелкните правой кнопкой мыши зашифрованную ранее базу данных, выберите **Задачи**, затем **Отсоединить…** .  
  
     В диалоговом окне **Отсоединение базы данных** доступны следующие параметры.  
  
     **Базы данных для отсоединения**  
     Перечисляет базы данных для отсоединения.  
  
     **Имя базы данных**  
     Отображает имя базы данных для отсоединения.  
  
     **Удалить соединения**  
     Завершить соединения с указанной базой данных.  
  
    > [!NOTE]  
    >  Невозможно отсоединить базу данных с активными соединениями.  
  
     **Обновить статистику**  
     По умолчанию операция отсоединения сохраняет устаревшую статистику оптимизации. Для ее обновления установите этот флажок.  
  
     **Сохранять полнотекстовые каталоги**  
     По умолчанию операция отсоединения сохраняет связанные с базой данных полнотекстовые каталоги. Для удаления этих каталогов сбросьте флажок **Сохранять полнотекстовые каталоги** . Этот параметр доступен только при обновлении базы данных с версии [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
     **Состояние**  
     Отображает одно из следующих состояний: **Готово** или **Не готово**.  
  
     **Сообщение**  
     Столбец **Сообщение** может отображать сведения о базе данных следующим образом:  
  
    -   Если база данных участвует в репликации, то ее **Состояние** имеет значение **Не готово** , а в столбце **Сообщение** отображается строка **База данных реплицирована**.  
  
    -   Если имеется одно активное подключение к базе данных или несколько, то ее **состояние** имеет значение **Не готово**, а в столбце **Message** отображается _\<number\_of\_active\_connections\>_ **Active connection(s)** . Например: **Активных подключений: 1**. Прежде чем можно будет отсоединить базу данных, необходимо отключить активные соединений, выбрав команду **Удалить соединения**.  
  
     Чтобы получить сведения о сообщении, откройте монитор активности, щелкнув текст с гиперссылкой.  
  
2.  Нажмите кнопку **ОК**.  
  
3.  В проводнике Windows переместите или скопируйте файлы базы данных с исходного сервера в то же расположение на целевом сервере.  
  
4.  В проводнике Windows переместите или скопируйте резервную копию сертификата сервера и файла закрытого ключа с исходного сервера в то же расположение на целевом сервере.  
  
5.  Создайте главный ключ базы данных на целевом экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в статье **Использование Transact-SQL** ниже.  
  
6.  Повторно создайте сертификат сервера с помощью файла резервной копии исходного сертификата сервера. Дополнительные сведения см. в статье **Использование Transact-SQL** ниже.  
  
7.  В обозревателе объектов среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]щелкните правой кнопкой мыши папку **Базы данных** и выберите команду **Присоединить...** .  
  
8.  В диалоговом окне **Присоединение баз данных** в области **Базы данных для присоединения** нажмите кнопку **Добавить**.  
  
9. В диалоговом окне **Поиск файлов базы данных—** _имя\_сервера_ выберите присоединяемый к новому серверу файл базы данных и нажмите кнопку **ОК**.  
  
     В диалоговом окне **Присоединение базы данных** доступны следующие параметры.  
  
     **Базы данных для присоединения**  
     Отобразятся сведения о выбранных базах данных.  
  
     \<no column header>  
     Отображается значок, указывающий на состояние операции присоединения. Возможные значки описываются в приводимом ниже описании **Состояние** .  
  
     **Расположение файла MDF**  
     Отображается путь и имя выбранного MDF-файла.  
  
     **Имя базы данных**  
     Отображается имя базы данных.  
  
     **Присоединить как**  
     Необязательный параметр, указывает другое имя, под которым присоединяется база данных.  
  
     **Владелец**  
     Содержит раскрывающийся список возможных владельцев базы данных, из которого при необходимости можно выбрать другого владельца.  
  
     **Состояние**  
     Отображается состояние базы данных в соответствии со следующей таблицей.  
  
    |Значок|Текст состояния|Описание|  
    |----------|-----------------|-----------------|  
    |(Нет значка)|(Нет текста)|Операция присоединения не была запущена или находится в режиме ожидания для этого объекта. Это состояние по умолчанию при открытии диалогового окна.|  
    |Зеленый, указывающий направо треугольник|Выполняется|Операция присоединения была запущена, но не завершена.|  
    |Зеленый флажок|Успешно|Объект успешно присоединен.|  
    |Красный кружок с белым крестом внутри|Error|При выполнении операции присоединения возникла ошибка, и операция не была успешно завершена.|  
    |Кружок с двумя черными квадратами (слева и справа) и двумя белыми квадратами (сверху и снизу)|Остановлена|Операция присоединения не была успешно завершена, т.к. пользователь остановил операцию.|  
    |Кружок, содержащий изогнутую стрелку, указывающую в направлении против часовой стрелки|Выполнен откат|Операция присоединения была успешной, но был выполнен ее откат из-за ошибки, возникшей при вложении другого объекта.|  
  
     **Сообщение**  
     Отображается пустое сообщение или гиперссылка «Файл не найден».  
  
     **Добавление**  
     Найдите необходимые основные файлы базы данных. Если пользователь выбирает mdf-файл, необходимые сведения автоматически вводятся в соответствующие поля сетки **Базы данных для присоединения** .  
  
     **Удалить**  
     Удаляет выбранный файл из сетки **Базы данных для присоединения** .  
  
     **Сведения о базе данных "** _<имя_базы_данных>_ **"**  
     Отображаются имена файлов, которые необходимо присоединить. Чтобы проверить или изменить путь к файлу, нажмите кнопку **Обзор** ( **...** ).  
  
    > [!NOTE]  
    >  Если файл не существует, в столбце **Сообщение** отображается сообщение «Не найден». Если файл журнала не найден, то он существует в другом каталоге или был удален. Необходимо или обновить путь файла в сетке **Сведения о базе данных** таким образом, чтобы этот путь указывал на правильное расположение, или удалить файл журнала из сетки. Если MDF-файл не найден, необходимо обновить путь этого файла в сетке таким образом, чтобы этот путь указывал на правильное расположение.  
  
     **Имя исходного файла**  
     Отображается имя присоединенного файла, принадлежащего базе данных.  
  
     **Тип файла**  
     Указывается тип файла: **Данные** или **Журнал**.  
  
     **Текущий путь к файлу**  
     Отображается путь к выбранному файлу базы данных. Путь может быть изменен вручную.  
  
     **Сообщение**  
     Отображается пустое сообщение или гиперссылка "**Файл не найден**".  
  
###  <a name="using-transact-sql"></a><a name="TsqlMove"></a> Использование Transact-SQL  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```sql  
    -- Detach the TDE protected database from the source server.   
    USE master ;  
    GO  
    EXEC master.dbo.sp_detach_db @dbname = N'CustRecords';  
    GO  
    -- Move or copy the database files from the source server to the same location on the destination server.   
    -- Move or copy the backup of the server certificate and the private key file from the source server to the same location on the destination server.   
    -- Create a database master key on the destination instance of SQL Server.   
    USE master;  
    GO  
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '*rt@40(FL&dasl1';  
    GO  
    -- Recreate the server certificate by using the original server certificate backup file.   
    -- The password must be the same as the password that was used when the backup was created.  
  
    CREATE CERTIFICATE TestSQLServerCert   
    FROM FILE = 'TestSQLServerCert'  
    WITH PRIVATE KEY   
    (  
        FILE = 'SQLPrivateKeyFile',  
        DECRYPTION BY PASSWORD = '*rt@40(FL&dasl1'  
    );  
    GO  
    -- Attach the database that is being moved.   
    -- The path of the database files must be the location where you have stored the database files.  
    CREATE DATABASE [CustRecords] ON   
    ( FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\CustRecords.mdf' ),  
    ( FILENAME = N'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA\CustRecords_log.LDF' )  
    FOR ATTACH ;  
    GO  
    ```  
  
 Дополнительные сведения см. в разделе:  
  
-   [sp_detach_db (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md)  
  
-   [CREATE MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-master-key-transact-sql.md)  
  
-   [CREATE CERTIFICATE (Transact-SQL)](../../../t-sql/statements/create-certificate-transact-sql.md)  
  
-   [CREATE DATABASE (SQL Server Transact-SQL)](../../../t-sql/statements/create-database-transact-sql.md)  
  
## <a name="see-also"></a>См. также:  
 [Присоединение и отсоединение базы данных (SQL Server)](../../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [Прозрачное шифрование данных в Базе данных SQL Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
  

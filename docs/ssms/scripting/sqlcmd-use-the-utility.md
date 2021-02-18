---
title: Использование программы sqlcmd
description: Узнайте, как использовать служебную программу sqlcmd для нерегламентированного интерактивного выполнения инструкций и скриптов Transact-SQL, а также для автоматизации задач скриптов Transact-SQL.
ms.custom: seo-lt-2019
ms.date: 06/06/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- Transact-SQL statements, executing
- command prompt utilities [SQL Server], sqlcmd
- statements [SQL Server], executing
- sqlcmd utility, about sqlcmd utility
ms.assetid: 3ec89119-7314-43ef-9e91-12e72bb63d62
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6a1be2303d4496957f7352d4b103b548902b48d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339381"
---
# <a name="sqlcmd---use-the-utility"></a>Использование программы sqlcmd
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Служебная программа **sqlcmd** представляет собой программу командной строки для нерегламентированного интерактивного выполнения инструкций и скриптов [!INCLUDE[tsql](../../includes/tsql-md.md)] , а также для автоматизации задач скрипта [!INCLUDE[tsql](../../includes/tsql-md.md)] . Интерактивная работа с **sqlcmd** и создание файлов скриптов, исполняемых с помощью **sqlcmd**, требует от пользователя понимания языка [!INCLUDE[tsql](../../includes/tsql-md.md)]. Программа **sqlcmd** обычно применяется следующим образом.  
  
-   Пользователь вводит инструкции на языке [!INCLUDE[tsql](../../includes/tsql-md.md)] точно так же, как при работе в командной строке. Результаты выводятся в окно командной строки. Чтобы открыть окно командной строки, введите "cmd" в поле поиска Windows и выберите пункт **Командная строка**. В окне командной строки введите **sqlcmd** и необходимые параметры. Полный перечень параметров, поддерживаемых программой **sqlcmd**, см. в разделе [Программа sqlcmd](../../tools/sqlcmd-utility.md).  
  
-   Пользователь отправляет задание **sqlcmd** на выполнение, указывая либо одиночную инструкцию [!INCLUDE[tsql](../../includes/tsql-md.md)] , либо текстовый файл с выполняемыми инструкциями [!INCLUDE[tsql](../../includes/tsql-md.md)] . Вывод обычно перенаправляется в текстовый файл, но может также быть отображен в окне командной строки.  
  
-   Режим[SQLCMD](./edit-sqlcmd-scripts-with-query-editor.md) в редакторе запросов среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
-   Управляющие объекты SQL Server (SMO)  
  
-   Задания CmdExec агента SQL Server.  
  
## <a name="typically-used-sqlcmd-options"></a>Часто используемые параметры sqlcmd  
  
-   Серверный параметр ( **-S**) определяет экземпляр [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], к которому подключается программа **sqlcmd**.  
  
-   Параметры проверки подлинности ( **-E**, **-U** и **-P**), с помощью которых задаются учетные данные, используемые программой **sqlcmd** для подключения к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. **ПРИМЕЧАНИЕ.** Параметр **-E** используется по умолчанию, и нет необходимости его указывать.  
  
-   Параметры входа ( **-Q**, **-q** и **-i**) определяют расположение входных данных для программы **sqlcmd**.  
  
-   Параметр выходных данных ( **-o**) определяет файл, в который программа **sqlcmd** помещает выходные данные.  
  
## <a name="connect-to-the-sqlcmd-utility"></a>Подключение к программе sqlcmd  
  
-   Соединение с экземпляром по умолчанию с использованием проверки подлинности Windows для запуска инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] в интерактивном режиме:  
  
    ```  
    sqlcmd -S <ComputerName>  
    ```  
  
    > **ПРИМЕЧАНИЕ.** В предыдущем примере параметр **-E** не указывается, так как он используется по умолчанию, и программа **sqlcmd** подключается к экземпляру по умолчанию, используя проверку подлинности Windows.  
  
-   Соединение с именованным экземпляром с использованием проверки подлинности Windows для запуска инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] в интерактивном режиме:  
  
    ```  
    sqlcmd -S <ComputerName>\<InstanceName>  
    ```  
  
     или диспетчер конфигурации служб  
  
    ```  
    sqlcmd -S .\<InstanceName>  
    ```  
  
-   Соединение с именованным экземпляром с использованием проверки подлинности Windows и указанием входного и выходного файла:  
  
    ```  
    sqlcmd -S <ComputerName>\<InstanceName> -i <MyScript.sql> -o <MyOutput.rpt>  
    ```  
  
-   Соединение с экземпляром по умолчанию на локальном компьютере с использованием проверки подлинности Windows, выполнение запроса и продолжение выполнения программы **sqlcmd** после завершения запроса:  
  
    ```  
    sqlcmd -q "SELECT * FROM AdventureWorks2012.Person.Person"  
    ```  
  
-   Соединение с экземпляром по умолчанию на локальном компьютере с использованием проверки подлинности Windows, выполнение запроса, запись в файл выходных данных и выход из программы **sqlcmd** после завершения запроса:  
  
    ```  
    sqlcmd -Q "SELECT * FROM AdventureWorks2012.Person.Person" -o MyOutput.txt  
    ```  
  
-   Соединение с именованным экземпляром с использованием проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для выполнения инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)] в интерактивном режиме, при этом программа **sqlcmd** ожидает ввода пароля:  
  
    ```  
    sqlcmd -U MyLogin -S <ComputerName>\<InstanceName>  
    ```  
  
    > **ПОДСКАЗКА.** Для просмотра полного перечня параметров, поддерживаемых служебной программой **sqlcmd** , введите `sqlcmd -?`.  
  
## <a name="run-transact-sql-statements-interactively-by-using-sqlcmd"></a>Интерактивный запуск инструкций Transact-SQL с помощью программы sqlcmd  
 С помощью программы **sqlcmd** можно интерактивно запускать инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] из окна командной строки. Для интерактивного выполнения инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] с помощью программы **sqlcmd** запустите ее без параметров **-Q**, **-q**, **-Z** или **-i** , чтобы задать входные файлы или запросы. Пример:  
  
 `sqlcmd -S <ComputerName>\<InstanceName>`  
  
 Если команда выполняется без входных файлов или запросов, то программа **sqlcmd** подключается к заданному экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и выводит новую строку с символами `1>` и мигающим знаком подчеркивания, которая называется приглашением **sqlcmd** . Цифра `1` означает, что это первая строка инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] , а приглашение **sqlcmd** является точкой, с которой начинается инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 В приглашении **sqlcmd** можно вводить как инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] , так и команды **sqlcmd** , например **GO** и **EXIT**. Каждая инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] помещается в буфер, называемый кэш инструкций. После ввода команды [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и нажатия клавиши ВВОД эти инструкции отправляются на сервер **GO** . Чтобы выйти из **sqlcmd**, введите **EXIT** или **QUIT** в начале новой строки.  
  
 Чтобы очистить кэш инструкций, введите **:RESET**. Ввод **^C** ведет к выходу из **sqlcmd** . Кроме того, с помощью команды **^C** можно останавливать выполнение кэша инструкций после запуска команды **GO** .  
  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] , введенные во время интерактивного сеанса, можно изменить с помощью ввода команды **:ED** и приглашения **sqlcmd** . Откроется редактор, и после изменения инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] и закрытия редактора измененная инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] появится в окне командной строки. Введите **GO** для запуска измененной инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
## <a name="quoted-strings"></a>Строки в кавычках  
 Символы, заключенные в кавычки, используются без какой-либо дополнительной предварительной обработки, за исключением кавычек, которые вставляются в строку путем ввода двух последовательных кавычек. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] рассматривает такую последовательность символов как одни кавычки. однако на сервере выполняется преобразование. Переменные скрипта при появлении в строке не раскрываются.  
  
 Пример:  
  
 `sqlcmd`  
  
 `PRINT "Length: 5"" 7'";`  
  
 `GO`  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Length: 5" 7'`  
  
## <a name="strings-that-span-multiple-lines"></a>Многострочные символьные строки  
 Программа **sqlcmd** поддерживает скрипты, в которых одна строка занимает несколько строк. Например, следующая инструкция `SELECT` занимает несколько строк, но является одной символьной строкой, которая выполняется после ввода команды `GO`и нажатия клавиши ВВОД.  
  
 `SELECT First line`  
  
 `FROM Second line`  
  
 `WHERE Third line;`  
  
 `GO`  
  
## <a name="interactive-sqlcmd-example"></a>Пример интерактивной команды sqlcmd  
 Ниже приведен пример интерактивного выполнения программы **sqlcmd** .  
  
 При открытии окна командной строки там отображается только одна строка:  
  
 `C:\> _`  
  
 Это означает, что текущей папкой является `C:\` , и если задать имя файла, то ОС Windows будет искать его в этой папке.  
  
 Для подключения к экземпляру **sqlcmd** по умолчанию на локальном компьютере введите [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Командная строка будет выглядеть следующим образом:  
  
 `C:\>sqlcmd`  
  
 `1> _`  
  
 Это означает, что вы подключились к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и что программа `sqlcmd` готова обрабатывать инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] и команды `sqlcmd` . Мерцающий знак подчеркивания после `1>` является приглашением `sqlcmd` , отмечающим местоположение вводимых инструкций и команд. Теперь введите **USE AdventureWorks2012** и нажмите клавишу ВВОД, а затем введите **GO** и снова нажмите клавишу ВВОД. Содержимое окна командной строки будет выглядеть следующим образом.  
  
 `sqlcmd`  
  
 `USE AdventureWorks2012;`  
  
 `GO`  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Changed database context to 'AdventureWorks2012'.`  
  
 `1> _`  
  
 После ввода команды `USE AdventureWorks2012` и нажатия клавиши ВВОД программа `sqlcmd` получила команду начать новую строку. Нажатие клавиши ENTER после ввода `GO,` дает сигнал программе `sqlcmd` к отправке инструкции `USE AdventureWorks2012` экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. `sqlcmd` возвращает сообщение, указывающее, что инструкция `USE` успешно завершена, и отображает новое приглашение `1>` , которое служит сигналом готовности к вводу новой  
  
 В следующем примере показано содержимое окна командной строки после ввода инструкции `SELECT` , команды `GO` для выполнения `SELECT`и команды `EXIT` для завершения программы `sqlcmd`:  
  
 `sqlcmd`  
  
 `USE AdventureWorks2012;`  
  
 `GO`  
  
 `SELECT TOP (3) BusinessEntityID, FirstName, LastName`  
  
 `FROM Person.Person;`  
  
 `GO`  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `BusinessEntityID   FirstName                 LastName`  
  
 `----------- -------------------------------- -----------`  
  
 `1           Syed                             Abbas`  
  
 `2           Catherine                        Abel`  
  
 `3           Kim                              Abercrombie`  
  
 `(3 rows affected)`  
  
 `1> EXIT`  
  
 `C:\>`  
  
 Строки после `3> GO` — это выходные данные инструкции `SELECT` . Когда выходные данные сформированы, программа `sqlcmd` сбрасывает приглашение `sqlcmd` и отображает `1>`. После ввода команды `EXIT` в строке `1>`командная строка приобретает первоначальный вид. Это означает завершение сеанса `sqlcmd` . Теперь можно закрыть окно командной строки. Для этого введите еще одну команду `EXIT` .  
  
## <a name="running-transact-sql-script-files-using-sqlcmd"></a>Выполнение файлов скрипта Transact-SQL с использованием программы sqlcmd  
 Вы можете использовать программу **sqlcmd** для выполнения файлов скриптов базы данных. Файлы скриптов — это текстовые файлы, содержащие сочетание инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] , команд **sqlcmd** и переменных скриптов. Дополнительные сведения об использовании переменных скрипта см. в разделе [Использование программы sqlcmd с переменными скрипта](./sqlcmd-use-with-scripting-variables.md). Программа **sqlcmd** работает с инструкциями, командами и переменными скрипта, помещенными в файл скрипта, подобно тому как она работает с инструкциями и командами, вводимыми в интерактивном режиме. Главное отличие заключается в том, что программа **sqlcmd** без остановок считывает входной файл, а не ждет, пока пользователь введет инструкции, команды или переменные скрипта.  
  
 Существуют различные способы создания файлов скрипта базы данных:  
  
-   можно в интерактивном режиме создать и отладить набор инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]и сохранить содержимое окна запроса в файл скрипта;  
  
-   можно создать текстовый файл инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] , используя текстовый редактор, например «Блокнот».  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-running-a-script-by-using-sqlcmd"></a>A. Запуск скрипта с помощью программы sqlcmd  
 Откройте «Блокнот» и введите следующие инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
 `USE AdventureWorks2012;`  
  
 `GO`  
  
 `SELECT TOP (3) BusinessEntityID, FirstName, LastName`  
  
 `FROM Person.Person;`  
  
 `GO`  
  
 Создайте папку с именем `MyFolder` и сохраните скрипт в виде файла `MyScript.sql` в папке `C:\MyFolder`. Введите следующий текст в командной строке, чтобы запустить скрипт, и поместите выходные данные `MyOutput.txt` в папку `MyFolder`:  
  
 `sqlcmd -i C:\MyFolder\MyScript.sql -o C:\MyFolder\MyOutput.txt`  
  
 Файл `MyOutput.txt` , просматриваемый в «Блокноте», содержит следующие сведения:  
  
 `Changed database context to 'AdventureWorks2012'.`  
  
 `BusinessEntityID FirstName   LastName`  
  
 `---------------- ----------- -----------`  
  
 `1                Syed        Abbas`  
  
 `2                Catherine   Abel`  
  
 `3                Kim         Abercrombie`  
  
 `(3 rows affected)`  
  
### <a name="b-using-sqlcmd-with-a-dedicated-administrative-connection"></a>Б. Использование программы sqlcmd с выделенным административным соединением  
 В следующем примере программа `sqlcmd` используется для подключения к серверу, на котором возникла проблема с блокировкой, с помощью выделенного административного соединения.  
  
 `C:\>sqlcmd -S ServerName -A`  
  
 `1> SELECT session_id, blocking_session_id FROM sys.dm_exec_requests WHERE blocking_session_id <> 0;`  
  
 `2> GO`  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `session_id   blocking_session_id`  
  
 `------ -------`  
  
 `62       64`  
  
 `(1 rows affected)`  
  
 С помощью `sqlcmd` завершите блокирующий процесс.  
  
 `1> KILL 64;`  
  
 `2> GO`  
  
### <a name="c-using-sqlcmd-to-execute-a-stored-procedure"></a>В. Использование программы sqlcmd для выполнения хранимых процедур  
 В следующем примере представлено действие, выполняющее хранимую процедуру с помощью `sqlcmd`. Создайте следующую хранимую процедуру.  
  
 `USE AdventureWorks2012;`  
  
 `IF OBJECT_ID ( ' dbo.ContactEmailAddress, 'P' ) IS NOT NULL`  
  
 `DROP PROCEDURE dbo.ContactEmailAddress;`  
  
 `GO`  
  
 `CREATE PROCEDURE dbo.ContactEmailAddress`  
  
 `(`  
  
 `@FirstName nvarchar(50)`  
  
 `,@LastName nvarchar(50)`  
  
 `)`  
  
 `AS`  
  
 `SET NOCOUNT ON`  
  
 `SELECT EmailAddress`  
  
 `FROM Person.Person`  
  
 `WHERE FirstName = @FirstName`  
  
 `AND LastName = @LastName;`  
  
 `SET NOCOUNT OFF`  
  
 После приглашения `sqlcmd` введите следующее:  
  
 `C:\sqlcmd`  
  
 `1> :Setvar FirstName Gustavo`  
  
 `1> :Setvar LastName Achong`  
  
 `1> EXEC dbo.ContactEmailAddress $(FirstName),$(LastName)`  
  
 `2> GO`  
  
 `EmailAddress`  
  
 `-----------------------------`  
  
 `gustavo0@adventure-works.com`  
  
### <a name="d-using-sqlcmd-for-database-maintenance"></a>Г. Использование программы sqlcmd для обслуживания базы данных  
 В следующем примере кода показано, как использовать программу `sqlcmd` для задач обслуживания базы данных. Создайте `C:\BackupTemplate.sql` со следующим кодом.  
  
 `USE master;`  
  
 `BACKUP DATABASE [$(db)] TO DISK='$(bakfile)';`  
  
 После приглашения `sqlcmd` введите следующее:  
  
 `C:\ >sqlcmd`  
  
 `1> :connect <server>`  
  
 `Sqlcmd: Successfully connected to server <server>.`  
  
 `1> :setvar db msdb`  
  
 `1> :setvar bakfile c:\msdb.bak`  
  
 `1> :r c:\BackupTemplate.sql`  
  
 `2> GO`  
  
 `Changed database context to 'master'.`  
  
 `Processed 688 pages for database 'msdb', file 'MSDBData' on file 2.`  
  
 `Processed 5 pages for database 'msdb', file 'MSDBLog' on file 2.`  
  
 `BACKUP DATABASE successfully processed 693 pages in 0.725 seconds (7.830 MB/sec)`  
  
### <a name="e-using-sqlcmd-to-execute-code-on-multiple-instances"></a>Д. Использование программы sqlcmd для выполнения кода на нескольких экземплярах  
 Следующий код представляет собой скрипт для соединения двух экземпляров. Обратите внимание на команду `GO` перед подключением ко второму экземпляру.  
  
 `:CONNECT <server>\,<instance1>`  
  
 `EXEC dbo.SomeProcedure`  
  
 `GO`  
  
 `:CONNECT <server>\,<instance2>`  
  
 `EXEC dbo.SomeProcedure`  
  
 `GO`  
  
### <a name="e-returning-xml-output"></a>Д. Возврат выходных XML-данных  
 Следующий пример показывает, как выходные данные XML возвращаются неформатированными, в виде непрерывного потока.  
  
 `C:\>sqlcmd -d AdventureWorks2012`  
  
 `1> :XML ON`  
  
 `1> SELECT TOP 3 FirstName + ' ' + LastName + ', '`  
  
 `2> FROM Person.Person`  
  
 `3> GO`  
  
 `Syed Abbas, Catherine Abel, Kim Abercrombie,`  
  
### <a name="f-using-sqlcmd-in-a-windows-script-file"></a>Е. Использование программы sqlcmd в файлах скриптов Windows  
 Команда **sqlcmd**, например `sqlcmd -i C:\InputFile.txt -o C:\OutputFile.txt,` , может выполняться в BAT-файле вместе со сценарием VBScript. В этом случае интерактивные параметры не используются. Программа **sqlcmd** должна быть установлена на компьютере, на котором выполняется BAT-файл.  
  
 Сначала создайте следующие четыре файла.  
  
-   C:\badscript.sql  
  
    ```  
    SELECT batch_1_this_is_an_error  
    GO  
    SELECT 'batch #2'  
    GO  
    ```  
  
-   C:\goodscript.sql  
  
    ```  
    SELECT 'batch #1'  
    GO  
    SELECT 'batch #2'  
    GO  
    ```  
  
-   C:\returnvalue.sql  
  
    ```  
    :exit(select 100)  
    ```  
  
-   C:\windowsscript.bat  
  
    ```  
    @echo off  
  
    echo Running badscript.sql  
    sqlcmd -i badscript.sql -b -o out.log  
    if not errorlevel 1 goto next1  
    echo == An error occurred   
  
    :next1  
  
    echo Running goodscript.sql  
    sqlcmd -i goodscript.sql -b -o out.log  
    if not errorlevel 1 goto next2  
    echo == An error occurred   
  
    :next2  
    echo Running returnvalue.sql  
    sqlcmd -i returnvalue.sql -o out.log  
    echo SQLCMD returned %errorlevel% to the command shell  
  
    :exit  
    ```  
  
 Затем из командной строки запустите `C:\windowsscript.bat`:  
  
 `C:\>windowsscript.bat`  
  
 `Running badscript.sql`  
  
 `== An error occurred`  
  
 `Running goodscript.sql`  
  
 `Running returnvalue.sql`  
  
 `SQLCMD returned 100 to the command shell`  
  
### <a name="g-using-sqlcmd-to-set-encryption-on-azure-sql-database"></a>Ж. Использование программы sqlcmd для включения шифрования в Базе данных SQL Azure  
 Программа **sqlcmd** может работать на соединении с данными [!INCLUDE[ssSDS](../../includes/sssds-md.md)] для определения шифрования и отношения доверия сертификата. Доступны два параметра **sqlcmd**:  
  
-   Ключ -N используется клиентом для запроса зашифрованного соединения. Этот параметр аналогичен параметру ADO.net `ENCRYPT = true`.  
  
-   C помощью переключателя -C клиент настраивает неявное доверие к сертификату сервера без проверки. Этот параметр аналогичен параметру ADO.net `TRUSTSERVERCERTIFICATE = true`.  
  
 Служба [!INCLUDE[ssSDS](../../includes/sssds-md.md)] не поддерживает все параметры `SET` , которые доступны в экземпляре SQL Server. Следующие параметры вызывают ошибку, если соответствующий параметр `SET` имеет значение `ON` или `OFF`:  
  
-   SET ANSI_DEFAULTS  
  
-   SET ANSI_NULLS  
  
-   SET REMOTE_PROC_TRANSACTIONS  
  
-   SET ANSI_NULL_DEFAULT  
  
 Следующие параметры SET не вызывают исключений, но использовать их нельзя. Они являются устаревшими.  
  
-   SET CONCAT_NULL_YIELDS_NULL  
  
-   SET ANSI_PADDING  
  
-   SET QUERY_GOVERNOR_COST_LIMIT  
  
#### <a name="syntax"></a>Синтаксис  
 Следующий пример относится к случаям, когда поставщик для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] имеет следующие параметры: `ForceProtocolEncryption = False`, `Trust Server Certificate = No`  
  
 Подключение с использованием учетных данных Windows и шифрование соединения:  
  
```  
SQLCMD -E -N  
  
```  
  
 Подключение с использованием учетных данных Windows и доверие сертификату сервера:  
  
```  
SQLCMD -E -C  
  
```  
  
 Подключение с использованием учетных данных Windows, шифрование соединения и доверие сертификату сервера:  
  
```  
SQLCMD -E -N -C  
  
```  
  
 Следующий пример относится к случаям, когда поставщик для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] имеет следующие параметры: `ForceProtocolEncryption = True`, `TrustServerCertificate = Yes`.  
  
 Подключение с использованием учетных данных Windows, шифрование соединения и доверие сертификату сервера:  
  
```  
SQLCMD -E  
  
```  
  
 Подключение с использованием учетных данных Windows, шифрование соединения и доверие сертификату сервера:  
  
```  
SQLCMD -E -N  
  
```  
  
 Подключение с использованием учетных данных Windows, шифрование соединения и доверие сертификату сервера:  
  
```  
SQLCMD -E -C  
  
```  
  
 Подключение с использованием учетных данных Windows, шифрование соединения и доверие сертификату сервера:  
  
```  
SQLCMD -E -N -C  
  
```  
  
 Если поставщик установил значение `ForceProtocolEncryption = True` , шифрование включается даже в случае, если в строке соединения указано значение `Encrypt=No` .  
  
## <a name="more-about-sqlcmd"></a>Дополнительные сведения о программе sqlcmd  
 [sqlcmd Utility](../../tools/sqlcmd-utility.md)   
 [Использование программы sqlcmd с переменными скрипта](./sqlcmd-use-with-scripting-variables.md)   
 [Изменение скриптов SQLCMD при помощи редактора запросов](./edit-sqlcmd-scripts-with-query-editor.md)   
 [Управление шагами задания](../../ssms/agent/manage-job-steps.md)   
 [Create a CmdExec Job Step](../../ssms/agent/create-a-cmdexec-job-step.md)  
  

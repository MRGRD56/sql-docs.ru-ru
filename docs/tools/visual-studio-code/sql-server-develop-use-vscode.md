---
title: Использование расширения mssql в Visual Studio Code
description: Расширение mssql для Visual Studio Code служит для изменения и выполнения скриптов Transact-SQL для SQL Server на Linux.
ms.topic: conceptual
ms.prod: sql
ms.technology: tools-other
ms.assetid: 9766ee75-32d3-4045-82a6-4c7968bdbaa6
author: markingmyname
ms.author: maghan
ms.date: 10/28/2019
ms.openlocfilehash: fdb3d5e695824117def89347da883a0a7a5d7a9c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340540"
---
# <a name="use-visual-studio-code-to-create-and-run-transact-sql-scripts"></a>Создание и выполнение скриптов Transact-SQL с помощью Visual Studio Code

[!INCLUDE [SQL Server - Linux](../../includes/applies-to-version/sql-linux.md)]

В этой статье описывается, как использовать расширение **mssql** для Visual Studio Code с целью разработки баз данных SQL Server. Так как средство Visual Studio Code является кроссплатформенным, вы можете использовать расширение **mssql** в Linux, macOS и Windows.

## <a name="install-and-start-visual-studio-code"></a>Установка и запуск Visual Studio Code

Visual Studio Code — это кроссплатформенный графический редактор кода, поддерживающий расширения.

1. [Скачайте и установите Visual Studio Code](https://code.visualstudio.com/) на своем компьютере.

2. Запустите Visual Studio Code.

    >[!NOTE]
    >Если Visual Studio Code не запускается при подключении через сеанс удаленного входа в систему xrdp, см. описание проблемы [VS Code not working on Ubuntu when connected using XRDP](https://github.com/Microsoft/vscode/issues/3451) (VS Code не работает в Ubuntu при подключении с помощью XRDP).

## <a name="install-the-mssql-extension"></a>Установка расширения mssql

[Расширение mssql для Visual Studio Code](https://aka.ms/mssql-marketplace) позволяет подключаться к SQL Server, выполнять запросы с помощью Transact-SQL (T-SQL) и просматривать результаты.

1. В Visual Studio Code выберите пункт **Вид** > **Палитра команд** либо нажмите клавиши **CTRL**+**SHIFT**+**P** или клавишу **F1**, чтобы открыть **палитру команд**.

2. В **палитре команд** в раскрывающемся списке выберите пункт **Расширения: установить расширения**.

3. В области **Расширения** введите *mssql*.

4. Выберите расширение **SQL Server (mssql)** и нажмите кнопку **Установить**.

   ![Установка расширения mssql](./media/sql-server-develop-use-vscode/vscode-extension.png)

5. По завершении установки выберите **Перезагрузить**, чтобы включить расширение.

## <a name="create-or-open-a-sql-file"></a>Создание или открытие файла SQL

Чтобы выполнять команды mssql и пользоваться технологией IntelliSense для T-SQL в редакторе кода, необходимо выбрать языковой режим **SQL**.

1. Выберите пункт **Файл** > **Создать файл** или нажмите клавиши **CTRL**+**N**. По умолчанию в Visual Studio Code открывается обычный текстовый файл. 

2. В нижней строке состояния выберите **Обычный текст** или нажмите клавиши **CTRL**+**K** > **M**, а затем выберите в раскрывающемся списке языков пункт **SQL**. 

   ![Языковой режим SQL](./media/sql-server-develop-use-vscode/vscode-language-mode.png)

   > [!NOTE]
   > Если расширение используется впервые, устанавливаются вспомогательные средства SQL Server.

При открытии существующего файла с расширением *.sql* языковой режим SQL устанавливается автоматически.  

## <a name="connect-to-sql-server"></a>Подключение к SQL Server

Чтобы создать профиль подключения и подключиться к SQL Server, выполните указанные ниже действия.

1. Нажмите клавиши **CTRL**+**SHIFT**+**P** или **F1**, чтобы открыть **палитру команд**. 

2. Введите *sql*, чтобы отобразить команды mssql, или введите *sqlcon*, а затем выберите в раскрывающемся списке пункт **MS SQL: подключение**.

   ![Команды mssql](./media/sql-server-develop-use-vscode/vscode-commands.png)

   >[!NOTE]
   >Чтобы можно было выполнять команды mssql, необходимо установить фокус на файл SQL, например созданный пустой файл SQL, в редакторе кода.

3. Выберите команду **MS SQL: управление профилями подключения**.

4. Затем нажмите **Создать**, чтобы создать профиль подключения для SQL Server.

5. Следуя указаниям, настройте свойства для нового профиля подключения. После указания каждого значения нажимайте клавишу **ВВОД**, чтобы продолжить.

   | Свойства подключения | Описание |
   |---|---|
   | **Имя сервера или строка подключения ADO** | Укажите имя экземпляра SQL Server. Чтобы подключиться к экземпляру SQL Server на локальном компьютере, введите *localhost*. Чтобы подключиться к удаленному серверу SQL Server, введите имя целевого сервера SQL Server или его IP-адрес. Чтобы подключиться к контейнеру SQL Server, укажите IP-адрес хост-компьютера контейнера. Если необходимо указать порт, отделите его от имени запятой. Например, для сервера, ожидающего передачи данных через порт 1401, введите `<servername or IP>,1401`.<br/><br/>В качестве альтернативы можно ввести строку подключения ADO для своей базы данных. |
   | **Имя базы данных** (необязательно) | База данных, которую необходимо использовать. Чтобы подключиться к базе данных по умолчанию, не указывайте имя. |
   | **Тип проверки подлинности** | Выберите **Встроенная** или **Вход SQL**. |
   | **Имя пользователя** | Если выбран **Вход SQL**, введите имя пользователя с доступом к базе данных на сервере. |
   | **Пароль** | Введите пароль для указанного пользователя. |
   | **Сохранить пароль** | Нажмите клавишу **ВВОД**, чтобы выбрать вариант **Да** и сохранить пароль. Выберите **Нет**, чтобы пароль запрашивался при каждом использовании профиля подключения. |
   | **Имя профиля** (необязательно) | Введите имя профиля подключения, например *профиль localhost*. |

   После ввода всех значений и нажатия клавиши **ВВОД** Visual Studio Code создает профиль подключения и подключается к SQL Server.

   > [!TIP]
   > Если подключиться не удается, попробуйте диагностировать проблему в сообщении об ошибке на панели **Вывод** в Visual Studio Code. Чтобы открыть панель **Вывод**, выберите пункт **Вид** > **Вывод**. Кроме того, ознакомьтесь с [рекомендациями по устранению неполадок с подключением](../../linux/sql-server-linux-troubleshooting-guide.md).

6. Проверьте состояние подключения в нижней строке состояния.

   ![Состояние подключения](./media/sql-server-develop-use-vscode/vscode-connection-status.png)

Вместо выполнения предыдущих инструкций создавать и изменять профили подключения можно также в файле параметров пользователя (*settings.json*). Чтобы открыть файл параметров, выберите **Файл** > **Настройки** > **Параметры**. Дополнительные сведения см. в статье [Управление профилями подключения](https://github.com/Microsoft/vscode-mssql/wiki/manage-connection-profiles).

## <a name="create-a-sql-database"></a>Создание базы данных SQL

1. В новом файле SQL, открытом ранее, введите *sql*, чтобы получить список редактируемых фрагментов кода.

   ![Фрагменты кода SQL](./media/sql-server-develop-use-vscode/vscode-sql-snippets.png)

2. Выберите **sqlCreateDatabase**.

3. Во фрагменте введите `TutorialDB` вместо DatabaseName:

   ```sql
   -- Create a new database called 'TutorialDB'
   -- Connect to the 'master' database to run this snippet
   USE master
   GO
   IF NOT EXISTS (
      SELECT name
      FROM sys.databases
      WHERE name = N'TutorialDB'
   )
   CREATE DATABASE [TutorialDB]
   GO
   ```

4. Нажмите клавиши **CTRL**+**SHIFT**+**E**, чтобы выполнить команды Transact-SQL. Просмотрите результаты в окне запроса.

    ![Создание сообщений базы данных](./media/sql-server-develop-use-vscode/vscode-create-database-messages.png)

    > [!TIP]
    > Сочетания клавиш для команд mssql можно настроить. См. статью [Customize Shortcuts](https://github.com/Microsoft/vscode-mssql/wiki/customize-shortcuts) (Настройка сочетаний клавиш).

## <a name="create-a-table"></a>Создание таблицы

1. Удалите содержимое в окне редактора кода.

2. Нажмите клавиши **CTRL**+**SHIFT**+**P** или **F1**, чтобы открыть **палитру команд**.

3. Введите *sql*, чтобы отобразить команды mssql, или введите *sqluse*, а затем выберите команду **MS SQL: использовать базу данных**.

4. Выберите новую базу данных **TutorialDB**.

   ![Использование базы данных](./media/sql-server-develop-use-vscode/vscode-use-database.png)

5. В редакторе кода введите *sql*, чтобы отобразить фрагменты кода, выберите фрагмент **sqlCreateTable** и нажмите клавишу **ВВОД**.

6. Во фрагменте введите `Employees` в качестве имени таблицы.

7. Нажмите клавишу **TAB**, чтобы перейти к следующему полю, а затем введите `dbo` в качестве имени схемы.

8. Замените определения столбцов следующими столбцами:

   ```sql
   EmployeesId INT NOT NULL PRIMARY KEY,
   Name [NVARCHAR](50)  NOT NULL,
   Location [NVARCHAR](50)  NOT NULL
   ```

9. Нажмите клавиши **CTRL**+**SHIFT**+**E**, чтобы создать таблицу.

## <a name="insert-and-query"></a>Вставка и запрос

1. Добавьте приведенные ниже инструкции, чтобы вставить четыре строки в таблицу **Employees**.

   ```sql
   -- Insert rows into table 'Employees'
   INSERT INTO Employees
      ([EmployeesId],[Name],[Location])
   VALUES
      ( 1, N'Jared', N'Australia'),
      ( 2, N'Nikita', N'India'),
      ( 3, N'Tom', N'Germany'),
      ( 4, N'Jake', N'United States')
   GO
   -- Query the total count of employees
   SELECT COUNT(*) as EmployeeCount FROM dbo.Employees;
   -- Query all employee information
   SELECT e.EmployeesId, e.Name, e.Location
   FROM dbo.Employees as e
   GO
   ```

   В процессе ввода технология IntelliSense для T-SQL помогает завершать инструкции:

   ![IntelliSense для T-SQL](./media/sql-server-develop-use-vscode/vscode-intellisense.png)

   > [!TIP]
   > Расширение mssql также предоставляет команды, помогающие создавать инструкции INSERT и SELECT. В предыдущем примере они не использовались.

2. Нажмите клавиши **CTRL**+**SHIFT**+**E**, чтобы выполнить команды. В окне **Результаты** отобразятся два результирующих набора.

   ![Результаты](./media/sql-server-develop-use-vscode/vscode-result-grid.png)

## <a name="view-and-save-the-result"></a>Просмотр и сохранение результата

1. Выберите пункты **Вид** > **Макет редактора** > **Перевернуть макет**, чтобы выбрать макет с вертикальным или горизонтальным разбиением.

2. Чтобы свернуть или развернуть панели **Результаты** и **Сообщение**, щелкните их заголовки.

   ![Переключение заголовков](./media/sql-server-develop-use-vscode/vscode-toggle-messages-pannel.png)

   > [!TIP]
   > Поведение по умолчанию расширения mssql можно настроить. См. статью [Customize extension options](https://github.com/Microsoft/vscode-mssql/wiki/customize-options) (Настройка параметров расширения).

3. Щелкните значок увеличения сетки во второй сетке результатов, чтобы увеличить масштаб.

   ![Увеличение сетки](./media/sql-server-develop-use-vscode/vscode-maximize-grid.png)

   > [!NOTE]
   > Значок увеличения отображается, если скрипт T-SQL создает две или несколько сеток результатов.

4. Откройте контекстное меню сетки, щелкнув ее правой кнопкой мыши.

   ![Контекстное меню](./media/sql-server-develop-use-vscode/vscode-grid-context-menu.png)

5. Выберите команду **Выбрать все**.

6. Снова откройте контекстное меню сетки и выберите команду **Сохранить как JSON**, чтобы сохранить результат в файле *JSON*.

7. Укажите имя файла JSON.

8. Убедитесь в том, что файл JSON сохранился и открывается в Visual Studio Code.

   ![Сохранение в формате JSON](./media/sql-server-develop-use-vscode/vscode-save-as-json.png)

Если скрипт SQL может потребоваться позднее для администрирования или более крупного проекта разработки, сохраните его с расширением *SQL*.

## <a name="next-steps"></a>Дальнейшие действия

Если вы не знакомы с T-SQL, изучите разделы [Учебник. Составление инструкций Transact-SQL](../../t-sql/tutorial-writing-transact-sql-statements.md) и [Справочник по Transact-SQL (ядро СУБД)](../../t-sql/language-reference.md).

Дополнительные сведения об использовании расширения mssql или участии в его создании см. на [вики-сайте проекта расширения mssql](https://github.com/Microsoft/vscode-mssql/wiki).

Дополнительные сведения об использовании Visual Studio Code см. в [документации по Visual Studio Code](https://code.visualstudio.com/docs).
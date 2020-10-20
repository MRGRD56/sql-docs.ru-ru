---
title: Подключение к экземпляру SQL Server и запросы к нему
description: Подключение к экземпляру SQL Server с помощью SQL Server Management Studio и выполнению базовых запросов T-SQL.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein
ms.custom: seo-lt-2019
ms.date: 09/28/2020
ms.openlocfilehash: d44e59e8dfdd9ba38feb2c860348f44af325c768
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92038392"
---
# <a name="quickstart-connect-to-and-query-a-sql-server-instance-by-using-sql-server-management-studio-ssms"></a>Краткое руководство. Подключение к экземпляру SQL Server и выполнение запросов с помощью SQL Server Management Studio (SSMS)

[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

Это краткое руководство научит вас подключаться к экземпляру SQL Server с использованием SQL Server Management Studio (SSMS) и выполнять некоторые базовые команды Transact-SQL (T-SQL). В статье показано, как выполнять следующие задачи:

> [!div class="checklist"]
> * Подключение к экземпляру SQL Server
> * Создание базы данных (TutorialDB)
> * Создание таблицы (Customers) в новой базе данных
> * Вставка строк в новую таблицу
> * Выполнение запросов к новой таблице и просмотр результатов
> * Проверка свойств подключения с помощью таблицы окна запросов
> * Изменение сервера, к которому подключено окно запросов

## <a name="prerequisites"></a>Предварительные требования

Для работы с этой статьей необходима среда SQL Server Management Studio и доступ к экземпляру SQL Server.

* Установите [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md).

Если у вас нет доступа к экземпляру SQL Server, выберите свою платформу в следующих ссылках. При выборе проверки подлинности SQL используйте учетные данные SQL Server.

* **Windows**: [скачать выпуск SQL Server 2019 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
* **macOS**: [скачать SQL Server 2019 для Docker](../../linux/quickstart-install-connect-docker.md).

## <a name="connect-to-a-sql-server-instance"></a>Подключение к экземпляру SQL Server

[!INCLUDE[ssms-connect-azure-ad](../../includes/ssms-connect-azure-ad.md)]

1. Запустите среду SQL Server Management Studio. При первом запуске SSMS откроется окно **Подключение к серверу**. Если этого не происходит, вы можете открыть его вручную, последовательно выбрав **Обозреватель объектов** > **Подключить** > **Ядро СУБД**.

    ![Ссылка для подключения в обозревателе объектов](media/connect-query-sql-server/connect-object-explorer.png)

2. В окне **Подключение к серверу** сделайте следующее по списку ниже.

    * В поле **Тип сервера** выберите **Ядро СУБД** (обычно это параметр по умолчанию).
    * В поле **Имя сервера** введите имя своего экземпляра SQL Server. (В этой статье используется имя экземпляра SQL2016ST и имя узла NODE5: NODE5\SQL2016ST.) Если вы не знаете, как определить имя экземпляра SQL Server, см. раздел [Дополнительные советы и рекомендации по использованию SSMS](../tutorials/ssms-tricks.md#find-sql-server-instance-name).
    * В поле **Проверка подлинности** выберите **Проверка подлинности Windows**. В этой статье используется проверка подлинности Windows, но поддерживаются также и учетные данные SQL Server. При выборе **Учетных данных SQL** отобразится запрос на ввод имени пользователя и пароля. Дополнительные сведения о типах проверки подлинности см. в разделе [Подключение к серверу (ядро СУБД)](../f1-help/connect-to-server-database-engine.md).

    ![Поле "Имя сервера" с возможностью использовать экземпляр SQL Server](media/connect-query-sql-server/connection-2.png)

    Вы также можете изменить дополнительные параметры подключения, выбрав **Параметры**. Примеры параметров подключения: база данных, к которой вы подключаетесь, время ожидания подключения и сетевой протокол. Эта статья использует во всех параметрах значения по умолчанию.

3. После заполнения всех полей выберите **Подключить**.

### <a name="examples-of-successful-connections"></a>Примеры успешных соединений

Чтобы проверить, успешно ли установлено подключение к серверу SQL Server, просмотрите объекты в **обозревателе объектов**. Эти объекты будут различаться в зависимости от типа сервера, к которому установлено подключение.

* Подключение к локальному серверу SQL Server — NODE5\SQL2016ST: ![Подключение к локальному серверу](media/connect-query-sql-server/connect-on-prem.png)

* Подключение к базе данных SQL Azure — msftestserver.database.windows.net: ![Подключение к базе данных SQL Azure](media/connect-query-sql-server/connect-sql-azure.png)

> [!NOTE]
> Ранее в этой статье вы подключились к локальному серверу SQL Server с помощью *проверки подлинности Windows*, но для базы данных SQL Azure этот способ не поддерживается. На этом рисунке показано подключение к базе данных SQL Azure с помощью проверки подлинности SQL. Дополнительные сведения см. в разделах, посвященных [локальной проверке подлинности SQL](../../relational-databases/security/choose-an-authentication-mode.md) и [проверке подлинности SQL в Azure](/azure/sql-database/sql-database-security-overview#access-management).

## <a name="create-a-database"></a>Создание базы данных

Сделайте следующее, чтобы создать базу данных с именем TutorialDB.

1. Щелкните правой кнопкой мыши экземпляр сервера в обозревателе объектов и выберите **Создать запрос**.

   ![Ссылка "Создать запрос"](media/connect-query-sql-server/new-query.png)

2. Вставьте в окно запросов следующий фрагмент кода T-SQL.

   ```sql
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

3. Чтобы запустить запрос, нажмите кнопку **Выполнить** (или клавишу F5).

   ![Команда "Выполнить"](media/connect-query-sql-server/execute.png)
  
    После выполнения запроса в списке баз данных в обозревателе объектов появится новая база данных TutorialDB. Если она не отображается, щелкните правой кнопкой мыши узел **Базы данных** и выберите **Обновить**.  

## <a name="create-a-table-in-the-new-database"></a>Создание таблицы в новой базе данных

В этом разделе вы создадите таблицу в новой базе данных TutorialDB. Так как редактор запросов все еще находится в контексте базы данных *master*, переключите контекст подключения на базу *TutorialDB*, сделав следующее.

1. Выберите нужную базу данных в раскрывающемся списке, как показано здесь:

   ![Изменение базы данных](media/connect-query-sql-server/change-db.png)

2. Вставьте в окно запросов следующий фрагмент кода T-SQL, выберите его, а затем нажмите кнопку **Выполнить** (или клавишу F5).  
   Вы можете заменить имеющийся текст в окне запроса или добавить новый текст в конце. Чтобы выполнить весь код в окне запросов, нажмите кнопку **Выполнить**. Если вы добавили текст, вам необходимо выполнить только его часть, поэтому сначала выделите ее, а затем нажмите кнопку **Выполнить**.  
  
   ```sql
   USE [TutorialDB]
   -- Create a new table called 'Customers' in schema 'dbo'
   -- Drop the table if it already exists
   IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
   DROP TABLE dbo.Customers
   GO
   -- Create the table in the specified schema
   CREATE TABLE dbo.Customers
   (
      CustomerId        INT    NOT NULL   PRIMARY KEY, -- primary key column
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```

После выполнения запроса в списке таблиц в обозревателе объектов появится новая таблица Customers. Если таблица не отображается, щелкните правой кнопкой мыши узел **TutorialDB** > **Таблицы** в обозревателе объектов, а затем выберите **Обновить**.

## <a name="insert-rows-into-the-new-table"></a>Вставка строк в новую таблицу

Вставьте в созданную ранее таблицу Customers какие-нибудь строки. Для этого вставьте следующий фрагмент кода T-SQL в окно запросов и нажмите кнопку **Выполнить**.

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
   VALUES
      ( 1, N'Orlando', N'Australia', N''),
      ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
      ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
      ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
   GO
   ```

## <a name="query-the-table-and-view-the-results"></a>Запрос к таблице и просмотр результатов

Результаты запроса выводятся под текстовым окном запроса. Чтобы запросить таблицу Customers и просмотреть ранее вставленные строки, сделайте следующее.

1. Вставьте следующий фрагмент кода T-SQL в окно запросов и нажмите кнопку **Выполнить**.

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

    Результаты запроса отображаются под областью, где был введен текст: 

   ![Список результатов](media/connect-query-sql-server/query-results.png)

2. Вы можете изменить представление результатов следующими способами.

     ![Три варианта отображения результатов запроса](media/connect-query-sql-server/results.png)

    * Кнопка посередине отображает результаты в **представлении сетки**; это параметр по умолчанию.
    * Первая кнопка отображает результаты в **текстовом представлении**, как показано на снимке в следующем разделе.
    * Третья кнопка позволяет сохранить результаты в файл, по умолчанию имеющий расширение .RPT.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Проверка свойств подключения с помощью таблицы окна запросов

Сведения о свойствах подключения приводятся под результатами запроса. После выполнения запроса из предыдущего этапа просмотрите свойства подключения в нижней части окна запросов.

* Вы можете определить, к какому серверу и какой базе данных вы подключены и под каким именем пользователя выполнен вход.
* Кроме того, вы можете проверить длительность запроса и число строк, возвращенных предыдущим запросом.

    ![Свойства подключения](media/connect-query-sql-server/connection-properties.png)

    > [!Note]
    > На рисунке результаты отображаются **в виде текста**.

## <a name="change-the-server-based-on-the-query-window"></a>Изменение сервера на основании окна запроса

Чтобы изменить сервер, к которому подключено текущее окно запросов, сделайте следующее.

1. Щелкните окно запросов правой кнопкой мыши и выберите **Подключение** > **Изменить подключение**. Снова откроется окно **Подключение к серверу**.

2. Измените сервер, который использует ваш запрос.

   ![Команда "Изменить подключение"](media/connect-query-sql-server/change-connection.png)

    > [!NOTE]
    > Это действие изменяет только сервер, к которому подключено окно запросов, но не меняет сервер, к которому подключен обозреватель объектов.

## <a name="azure-data-studio"></a>Azure Data Studio

Также с помощью Azure Data Studio вы можете выполнять подключения и запросы к [SQL Server](../../azure-data-studio/quickstart-sql-server.md), [Базе данных SQL Azure](../../azure-data-studio/quickstart-sql-database.md) и [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md).

## <a name="next-steps"></a>Дальнейшие действия

Лучший способ познакомиться с SSMS — это поработать в среде самостоятельно. Эти статьи помогут вам ознакомиться с различными функциями SSMS. С их помощью вы научитесь работать с компонентами SSMS и легко находить регулярно используемые функции.

* [Создание скриптов](../tutorials/scripting-ssms.md)
* [Использование шаблонов в SSMS](../template/templates-ssms.md)
* [Конфигурация SSMS](../tutorials/ssms-configuration.md)
* [Дополнительные советы и рекомендации по использованию SSMS](../tutorials/ssms-tricks.md)
* [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md)
---
title: Подключение и отправка запросов к Azure Synapse Analytics
description: В этом кратком руководстве показано, как использовать Azure Data Studio для подключения с помощью выделенного пула SQL в Azure Synapse Analytics и выполнения запроса.
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.reviewer: alayu, maghan, sstein
ms.topic: quickstart
author: yualan
ms.author: alayu
ms.custom: seodec18; seo-lt-2019
ms.date: 09/24/2018
ms.openlocfilehash: c2282220dff18a7f054cc5fd01b3670b6fd14d43
ms.sourcegitcommit: a5398f107599102af7c8cda815d8e5e9a367ce7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "92005485"
---
# <a name="quickstart-use-azure-data-studio-to-connect-and-query-data-using-dedicated-sql-pool-in-azure-synapse-analytics"></a>Краткое руководство. Использование Azure Data Studio для подключения и запроса данных с помощью выделенного пула SQL в Azure Synapse Analytics

В этом кратком руководстве показано, как использовать Azure Data Studio для подключения с помощью выделенного пула SQL в Azure Synapse Analytics, а затем с помощью инструкций Transact-SQL создавать, вставлять и выбирать данные. 

## <a name="prerequisites"></a>Предварительные требования
Для работы с этим кратким руководством потребуется Azure Data Studio, выделенный пул SQL и Azure Synapse Analytics.

- [Установите Azure Data Studio](./download-azure-data-studio.md?view=sql-server-ver15).

Если у вас еще нет выделенного пула SQL, см. статью [Создание выделенного пула SQL](/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision).

Запомните имя сервера и учетные данные для входа.


## <a name="connect-to-your-dedicated-sql-pool"></a>Подключение к выделенному пулу SQL

С помощью Azure Data Studio установите подключение к Azure Synapse Analytics.

1. При первом запуске Azure Data Studio должна открыться страница **подключения**. Если страница **Подключение** не появилась, щелкните **Добавить подключение** или значок **Новое подключение** на боковой панели **Серверы**.
   
   ![Значок нового подключения](media/quickstart-sql-dw/new-connection-icon.png)

2. В этой статье используется *имя входа SQL*, но также поддерживается *проверка подлинности Windows*. Заполните поля, указав имя сервера, имя пользователя и пароль для *вашего* сервера SQL Azure.

   | Параметр       | Рекомендуемое значение | Описание |
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Имя сервера** | Полное имя сервера | Имя должно быть в следующем формате: **sqldwsample.database.windows.net** |
   | **Аутентификация** | Имя входа SQL| В этом руководстве используется проверка подлинности SQL. |
   | **User name** | Учетная запись администратора сервера | Это учетная запись, которая была указана при создании сервера. |
   | **Пароль (имя входа SQL)** | Пароль для учетной записи администратора сервера | Это пароль, указанный при создании сервера. |
   | **Сохранить пароль?** | "Да" или "Нет". | Чтобы не вводить пароль каждый раз, выберите "Да". |
   | **Имя базы данных** | *Оставьте пустым* | Имя базы данных, с которой необходимо установить соединение. |
   | **Группа серверов** | Выберите <Default> | Если вы создали группу серверов, можно выбрать ее. | 

   ![Значок нового подключения](media/quickstart-sql-dw/new-connection-screen.png) 

3. Если на сервере не настроено правило брандмауэра, разрешающее подключение Azure Data Studio, откроется форма **Создание нового правила брандмауэра**. Заполните форму, чтобы создать правило брандмауэра. Подробные сведения см. в разделе [Правила брандмауэра](/azure/sql-database/sql-database-firewall-configure).

   ![Новое правило брандмауэра](media/quickstart-sql-dw/firewall.png)  

4. После успешного подключения сервер откроется на боковой панели *Серверы*.

## <a name="create-the-tutorial-dedicated-sql-pool"></a>Создание выделенного пула SQL для учебника
1. В обозревателе объектов щелкните сервер правой кнопкой мыши и выберите команду **Создать запрос**.

1. Вставьте в редакторе запросов следующий фрагмент и нажмите кнопку **Выполнить**.

   ```sql
    IF NOT EXISTS (
       SELECT name
       FROM sys.databases
       WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB] (EDITION = 'datawarehouse', SERVICE_OBJECTIVE='DW100');
    GO  
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
   ```


## <a name="create-a-table"></a>Создание таблицы

Редактор запросов все еще подключен к базе данных *master*, но нам нужно создать таблицу в базе данных *TutorialDB*. 

1. Измените контекст подключения на **TutorialDB**.

   ![Изменение контекста](media/quickstart-sql-database/change-context.png)


1. Вставьте в редакторе запросов следующий фрагмент и нажмите кнопку **Выполнить**.

   > [!NOTE]
   > Этот фрагмент можно добавить в предыдущий запрос либо можно перезаписать предыдущий запрос в редакторе. Обратите внимание, что при нажатии кнопки **Выполнить** выполняется только выбранный запрос. Если ничего не выбрано, при нажатии кнопки **Выполнить** выполняются все запросы в редакторе.

   ```sql
   -- Create a new table called 'Customers' in schema 'dbo'
   -- Drop the table if it already exists
   IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
   DROP TABLE dbo.Customers
   GO
   -- Create the table in the specified schema
   CREATE TABLE dbo.Customers
   (
      CustomerId        INT     NOT NULL,
      Name      [NVARCHAR](50)  NOT NULL,
      Location  [NVARCHAR](50)  NOT NULL,
      Email     [NVARCHAR](50)  NOT NULL
   );
   GO
   ```


## <a name="insert-rows"></a>Вставка строк

1. Вставьте в редакторе запросов следующий фрагмент и нажмите кнопку **Выполнить**.

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
      SELECT 1, N'Orlando',N'Australia', N'' UNION ALL
      SELECT 2, N'Keith', N'India', N'keith0@adventure-works.com' UNION ALL
      SELECT 3, N'Donna', N'Germany', N'donna0@adventure-works.com' UNION ALL
      SELECT 4, N'Janet', N'United States', N'janet1@adventure-works.com'
   ```


## <a name="view-the-result"></a>Просмотр результата
1. Вставьте в редакторе запросов следующий фрагмент и нажмите кнопку **Выполнить**.

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

1. Будут отображены результаты запроса:

   ![Выбор результатов](media/quickstart-sql-dw/select-results.png)


## <a name="clean-up-resources"></a>Очистка ресурсов

Другие статьи данной серии основаны на этом кратком руководстве. Если вы собираетесь изучать их, не удаляйте ресурсы, созданные в этом кратком руководстве. Если вы не собираетесь продолжать работу, выполните приведенные ниже действия, чтобы удалить ресурсы, созданные в этом кратком руководстве, на портале Azure.
Очистите ресурсы, удалив группы ресурсов, которые больше не нужны. Подробные сведения см. в разделе [Очистка ресурсов](/azure/sql-database/sql-database-get-started-portal#clean-up-resources).


## <a name="next-steps"></a>Дальнейшие действия

После успешного подключения к Azure Synapse Analytics и выполнения запроса можно перейти к [руководству по редактору кода](tutorial-sql-editor.md).
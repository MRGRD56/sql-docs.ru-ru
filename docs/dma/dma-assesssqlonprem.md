---
title: Выполнение SQL Server оценки миграции
titleSuffix: Data Migration Assistant
description: Узнайте, как использовать Помощник по миграции данных для оценки локальной SQL Server перед миграцией на другой SQL Server или в базу данных SQL Azure.
ms.date: 01/15/2020
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.custom: seo-lt-2019
ms.openlocfilehash: 300d88b66c2105235ab04ff616d9fcf81b24e944
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247331"
---
# <a name="perform-a-sql-server-migration-assessment-with-data-migration-assistant"></a>Выполнение оценки переноса SQL Server с использованием Помощника по миграции данных

Следующие пошаговые инструкции помогут вам выполнить первую оценку перехода на локальные SQL Server, SQL Server, выполняющихся на виртуальной машине Azure, или базы данных SQL Azure с помощью Помощник по миграции данных.

В Помощник по миграции данных версии 5.0 появилась поддержка анализа подключения к базе данных и внедренных запросов SQL в коде приложения. Дополнительные сведения см. в записи блога [использование помощник по миграции данных для оценки уровня доступа к данным приложения](https://techcommunity.microsoft.com/t5/Microsoft-Data-Migration/Using-Data-Migration-Assistant-to-assess-an-application-s-data/ba-p/990430).

[!INCLUDE [online-offline](../includes/azure-migrate-to-assess-sql-data-estate.md)]

## <a name="create-an-assessment"></a>Создание оценки

1. Щелкните значок " **создать** " (+), а затем выберите тип проекта **оценки** .

2. Укажите тип исходного и целевого серверов.

    При обновлении локального экземпляра SQL Server до современного локального экземпляра SQL Server или SQL Server, размещенного на виртуальной машине Azure, задайте для параметра Тип исходного и целевого сервера значение **SQL Server**. Если выполняется миграция в базу данных SQL Azure, задайте тип целевого сервера в **базе данных SQL Azure**.

3. Нажмите кнопку **Создать**.

   ![Создание оценки](../dma/media/dma-assesssqlonprem/new-assessment.png)

## <a name="choose-assessment-options"></a>Выбор параметров оценки

1. Выберите целевую версию SQL Server, до которой планируется выполнить миграцию.

2. Выберите тип отчета.

   При оценке исходного экземпляра SQL Server для перехода на локальные SQL Server или SQL Server, размещенных на целевых объектах виртуальной машины Azure, можно выбрать один или оба следующих типа отчетов по оценке:

    - **Проблемы совместимости**
    - **Рекомендации по новым возможностям**

   ![Выберите тип отчета оценки для SQL Server целевого объекта](../dma/media/dma-assesssqlonprem/assessment-types.png)

   При оценке экземпляра SQL Server источника для миграции в базу данных SQL Azure можно выбрать один или оба следующих типа отчетов оценки:

    - **проверка совместимости базы данных;**
    - **проверка четности компонентов.**

    ![Выбор типа отчета "Оценка" для целевого объекта базы данных SQL](../dma/media/dma-assesssqlonprem/assessment-types-azure.png)

## <a name="add-databases-and-extended-events-trace-to-assess"></a>Добавление баз данных и трассировки расширенных событий для оценки

1. Выберите **Добавить источники** , чтобы открыть всплывающее меню подключения.

2. Введите имя экземпляра SQL Server, выберите тип проверки подлинности, задайте правильные свойства соединения, а затем нажмите кнопку **подключить**.

3. Выберите базы данных для оценки, а затем нажмите кнопку **Добавить**.

    > [!NOTE]
    > Можно удалить несколько баз данных, выбрав их, удерживая клавишу Shift или CTRL, а затем выбрав **удалить источники**. Можно также добавить базы данных из нескольких экземпляров SQL Server, выбрав **Добавить источники**.

4. При наличии любых нерегламентированных или динамических SQL-запросов или инструкций DML, инициированных на уровне данных приложения, введите путь к папке, в которую были помещены все файлы сеансов расширенных событий, собранные для сбора рабочей нагрузки на исходном SQL Server.

     В следующем примере показано, как создать сеанс расширенного события на исходном SQL Server для записи рабочей нагрузки уровня данных приложения.  Запишите рабочую нагрузку в течение времени, представляющего пиковую рабочую нагрузку.

    ```
    DROP EVENT SESSION [DatalayerSession] ON SERVER
    go
    CREATE EVENT SESSION [DatalayerSession] ON SERVER  
    ADD EVENT sqlserver.sql_batch_completed( 
        ACTION (sqlserver.sql_text,sqlserver.client_app_name,sqlserver.client_hostname,sqlserver.database_id))
    ADD TARGET package0.asynchronous_file_target(SET filename=N'C:\temp\Demos\DataLayerAppassess\DatalayerSession.xel')  
    WITH (MAX_MEMORY=2048 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=3 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)
    go
    ---Start the session
    ALTER EVENT SESSION [DatalayerSession]
          ON SERVER
        STATE = START;
    ---Wait for few minutes
    
    ---Query events
        
        SELECT 
        object_name,
        CAST(event_data as xml) as event_data,
        file_name, 
        file_offset
    FROM sys.fn_xe_file_target_read_file('C:\temp\Demos\DataLayerAppassess\DatalayerSession*xel', 
                'C:\\temp\\Demos\\DataLayerAppassess\\DatalayerSession*xem', 
                null,
                null)
    ---Stop the session after capturing the peak load.
    ALTER EVENT SESSION [DatalayerSession]
          ON SERVER
        STATE = STOP;
        
        go
    ```

5. Нажмите кнопку **Далее** для запуска оценки.

    ![Добавление источников и начальная оценка](../dma/media/dma-assesssqlonprem/select-database1.png)

> [!NOTE]
> Вы можете одновременно выполнить несколько оценок и просмотреть их состояние, открыв страницу **​All Assessments** (Все оценки).

## <a name="view-results"></a>Просмотр результатов

Длительность оценки зависит от количества добавленных баз данных и размера схемы каждой базы данных. Результаты по каждой базе данных будут отображены, как только они станут доступны.

1. Выберите базу данных, которая завершила оценку, а затем переключайтесь между **проблемами совместимости** и **рекомендациями по функциям** с помощью переключателя.

2. Проверьте проблемы совместимости для всех уровней совместимости, поддерживаемых целевой SQL Server версией, выбранной на странице " **Параметры** ".

Можно просматривать проблемы совместимости, анализируя затронутый объект, сведения о нем и, возможно, исправление для всех проблем, обнаруженных в **критических изменениях**, **изменениях в работе** и **устаревших функциях**.

![Просмотр результатов оценки](../dma/media/dma-assesssqlonprem/review-results.png)

Аналогичным образом можно просматривать рекомендации по **производительности**, **хранению** и **безопасности** .

Рекомендации по функциям охватывают различные виды функций, таких как In-Memory OLTP, columnstore, Stretch Database, Always Encrypted, динамическое маскирование данных и прозрачное шифрование данных.

![Просмотр рекомендаций по функциям](../dma/media/dma-assesssqlonprem/feature-recommendations.png)

Для базы данных SQL Azure оценки обеспечивают проблемы блокировки миграции и проблемы с контролем четности компонентов. Проверьте результаты для обеих категорий, выбрав соответствующие параметры.

- Категория **контроля четности SQL Server функций** содержит исчерпывающий набор рекомендаций, альтернативных подходов, доступных в Azure, и меры по устранению рисков. Он помогает спланировать эти усилия в проектах миграции.

  ![Просмотр сведений о четности компонентов SQL Server](../dma/media/dma-assesssqlonprem/sql-feature-parity.png)

- Категория **проблемы совместимости** предоставляет частично поддерживаемые или неподдерживаемые функции, которые блокируют миграцию локальных SQL Server баз данных в базы данных SQL Azure. Затем он предоставляет рекомендации по устранению этих проблем.

  ![Просмотр проблем совместимости](../dma/media/dma-assesssqlonprem/compatibility-issues.png)

## <a name="assess-a-data-estate-for-target-readiness"></a>Оценка свободного расположения данных для обеспечения готовности к работе

Если вы хотите дополнительно расширить эти оценки до всей области данных и найти относительную готовность SQL Server экземпляров и баз данных для миграции в базу данных SQL Azure, отправьте результаты в центр миграции Azure, выбрав **отправить в службу "миграция Azure**".

Это позволяет просматривать объединенные результаты в проекте центра миграции Azure.

Подробное пошаговое руководство по оценке готовности к работе можно найти [здесь](./dma-assess-sql-data-estate-to-sqldb.md).

   ![Отправка результатов в службу "миграция Azure"](../dma/media/dma-assesssqlonprem/upload-to-azure-migrate.png)

## <a name="export-results"></a>Экспорт результатов

После того как все базы данных завершат оценку, выберите **Экспорт отчета** , чтобы экспортировать результаты в файл JSON или CSV-файл. Затем можно проанализировать данные в удобном для вас месте.

## <a name="save-and-load-assessments"></a>Сохранение и загрузка оценок

Помимо экспорта результатов оценки можно сохранить сведения об оценке в файл и загрузить файл оценки для последующего просмотра.  Дополнительные сведения см. в статье [Сохранение и загрузка оценок с помощью помощник по миграции данных](../dma/dma-save-load-assessments.md).

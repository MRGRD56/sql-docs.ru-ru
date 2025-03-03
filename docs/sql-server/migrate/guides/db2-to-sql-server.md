---
title: Руководство. Миграция из IBM Db2 в SQL Server
description: Из этого руководства вы узнаете, как перенести базы данных Db2 в Microsoft SQL Server с использованием Помощника по миграции SQL Server для Db2 (SSMA для Db2).
ms.custom: ''
ms.date: 03/19/2021
ms.prod: sql
ms.reviewer: ''
ms.technology: migration-guide
ms.topic: how-to
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 994d78b9e1eec46659e97c114d99355ccf4a974b
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981052"
---
# <a name="migration-guide-ibm-db2-to-sql-server"></a>Руководство по миграции c IBM Db2 в SQL Server
[!INCLUDE[sqlserver](../../../includes/applies-to-version/sqlserver.md)]

В этом руководстве показано, как перенести пользовательские базы данных с IBM Db2 в SQL Server с помощью Помощника по миграции SQL Server для Db2 (SSMA для Db2). 

Другие рекомендации по миграции см. в [руководствах по переносу баз данных в Azure](https://docs.microsoft.com/data-migration). 


## <a name="prerequisites"></a>Предварительные требования

Прежде чем приступить к переносу базы данных Db2 в SQL Server, сделайте следующее:

- Убедитесь, что ваша [исходная среда поддерживается](../../../ssma/db2/installing-ssma-for-db2-client-db2tosql.md#prerequisites).
- Скачайте и установите [SSMA для Db2](https://www.microsoft.com/download/details.aspx?id=54254).
- Получите возможность подключения и требуемые разрешения для доступа к исходному и целевому объектам. 

## <a name="pre-migration"></a>Подготовка к миграции

После выполнения необходимых условий можно приступать к обнаружению топологии среды и оценке возможности миграции. 

### <a name="assess-and-convert"></a>Оценка и преобразование 

С помощью SSMA для Db2 проверьте все объекты и данные в базе данных, чтобы убедиться в готовности баз данных к миграции. 

Чтобы создать оценку, сделайте следующее:

1. Откройте SSMA для Db2. 
1. Выберите **File** (Файл) и **New Project** (Создать проект). 
1. Укажите имя и расположение проекта, а затем в раскрывающемся списке выберите целевой объект для миграции в SQL Server. Щелкните **ОК**.

   :::image type="content" source="media/db2-to-sql-server/new-project.png" alt-text="Снимок экрана: область &quot;Создание проекта&quot; в SSMA для Db2.":::


1. Выберите **Соединение с базой данных DB2** и введите сведения о подключении к Db2.

   :::image type="content" source="media/db2-to-sql-server/connect-to-db2.png" alt-text="Снимок экрана: область &quot;Соединение с базой данных DB2&quot;.":::


1. Щелкните правой кнопкой мыши схему Db2, которую вы хотите перенести, и выберите **Создать отчет**, чтобы создать отчет в формате HTML. Также можно выбрать **Создать отчет** в правом верхнем углу.

   :::image type="content" source="media/db2-to-sql-server/create-report.png" alt-text="Снимок экрана: ссылки &quot;Создать отчет&quot; в обозревателе метаданных Db2.":::

1. Ознакомьтесь с отчетом в формате HTML, чтобы получить сведения о статистике преобразований, а также об ошибках или предупреждениях. Также можно открыть отчет в Excel, чтобы получить список объектов Db2 и действий, необходимых для выполнения преобразований схемы. По умолчанию отчет находится в папке report в каталоге SSMAProjects, как показано ниже.  

   `drive:\<username>\Documents\SSMAProjects\MyDb2Migration\report\report_<date>` 

   :::image type="content" source="media/db2-to-sql-server/report.png" alt-text="Снимок экрана: отчет о преобразовании в SSMA.":::


### <a name="validate-data-types"></a>Обновление типов данных

Проверьте сопоставления типов данных по умолчанию и при необходимости измените их с учетом предоставленных рекомендаций. Для этого сделайте следующее: 

1. Щелкните **Tools** (Средства) и выберите **Project Settings** (Параметры проекта).  
1. Перейдите на вкладку **Type mapping** (Сопоставление типов).

   :::image type="content" source="media/db2-to-sql-server/type-mapping.png" alt-text="Снимок экрана: панель Type mapping (Сопоставление типов) в SSMA для Db2.":::

1. Сопоставление типов для каждой таблицы можно изменить, выбрав имя нужной таблицы в области **Db2 Metadata explorer** (Обозреватель метаданных Db2). 

### <a name="convert-schema"></a>Преобразовать схему 

Чтобы преобразовать схему, сделайте следующее:

1. (Необязательно.) Чтобы преобразовать динамические или специализированные запросы, щелкните нужный узел правой кнопкой мыши и выберите пункт **Add statement** (Добавить инструкцию). 

1. Перейдите на вкладку **Connect to SQL Server** (Подключение к SQL Server), а затем введите сведения о подключении к экземпляру SQL Server. 

    а. В раскрывающемся списке **Database** (База данных) выберите целевую базу данных или введите неиспользуемое имя, чтобы создать базу данных на целевом сервере.   
    b. Предоставьте сведения о проверке подлинности.   
    c. Выберите **Подключиться**.

   :::image type="content" source="media/db2-to-sql-server/connect-to-sql-server.png" alt-text="Снимок экрана: область Connect to SQL Server (Подключение к SQL Server) в SSMA для Db2.":::

1. Щелкните правой кнопкой мышки схему, с которой вы работаете, и выберите **Convert Schema** (Преобразовать схему). Также можно выбрать вкладку **Convert Schema** (Преобразовать схему) в правом верхнем углу.

   :::image type="content" source="media/db2-to-sql-server/convert-schema.png" alt-text="Снимок экрана: пункт Convert Schema (Преобразовать схему) в области Db2 Metadata Explorer (Обозреватель метаданных Db2).":::

1. Когда преобразование завершится, сравните преобразованные объекты с исходными, чтобы оценить возможные проблемы, и устраните их в соответствии с рекомендациями.

   :::image type="content" source="media/db2-to-sql-server/compare-review-schema-structure.png" alt-text="Снимок экрана: сравнение преобразованных объектов с исходными.":::

1. На панели выходных данных щелкните значок **Review results** (Проверка результатов), а затем просмотрите ошибки в области **Error list** (Список ошибок). 
1. В качестве упражнения по исправлению схемы в автономном режиме сохраните проект на локальном устройстве, выбрав **File** > **Save Project** (Файл > Сохранить проект). Это позволит вам оценить исходную и целевую схемы в автономном режиме и устранить проблемы перед публикацией схемы в экземпляре SQL Server.



## <a name="migrate"></a>Миграция

Когда вы завершите оценку баз данных и устраните все несоответствия, можно переходить к процессу миграции.

Чтобы опубликовать схему и перенести данные, сделайте следующее:

1. Опубликуйте схему. В области **SQL Server Metadata Explorer** (Обозреватель метаданных SQL Server) щелкните базу данных правой кнопкой мыши и выберите **Synchronize with Database** (Синхронизировать с базой данных).

   :::image type="content" source="media/db2-to-sql-server/synchronize-with-database.png" alt-text="Снимок экрана: пункт Synchronize with Database (Синхронизировать с базой данных) в области SQL Server Metadata Explorer (Обозреватель метаданных SQL Server).":::

1. Перенесите данные. В области **Db2 Metadata Explorer** (Обозреватель метаданных Db2) щелкните правой кнопкой мыши схему или объект, которые вы хотите перенести, и выберите **Migrate Data** (Миграция данных). Также можно выбрать вкладку **Migrate Data** (Миграция данных) в правом верхнем углу.
 
   Чтобы перенести данные всей базы данных, установите флажок рядом с ее именем. Чтобы перенести данные из отдельных таблиц, разверните базу данных, разверните узел **Tables** (Таблицы) и установите флажок рядом с нужной таблицей. Чтобы не переносить данные из определенной таблицы, снимите флажок.

   :::image type="content" source="media/db2-to-sql-server/migrate-data.png" alt-text="Снимок экрана: ссылки для переноса данных.":::

1. Укажите сведения о подключении для экземпляров Db2 и SQL Server. 
1. После завершения миграции изучите **отчет о переносе данных**.  

   :::image type="content" source="media/db2-to-sql-server/data-migration-report.png" alt-text="Снимок экрана: отчет о переносе данных.":::

1. Подключитесь к экземпляру SQL Server с помощью [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) и проверьте результаты миграции, просмотрев данные и схему. 

   :::image type="content" source="media/db2-to-sql-server/compare-schema-in-ssms.png" alt-text="Снимок экрана: сервер управления SQL Server":::.

## <a name="post-migration"></a>После миграции 

После успешного завершения этапа *миграции* необходимо выполнить ряд дополнительных задач, чтобы обеспечить бесперебойную и эффективную работу всех компонентов.

### <a name="remediate-applications"></a>Исправление приложений 

После переноса данных в целевую среду все приложения, которые раньше использовали источник, должны переключиться на использование целевого объекта миграции. Для этого в некоторых случаях потребуется внести изменения в приложения.

### <a name="perform-tests"></a>Выполнение тестов

Тестирование переноса базы данных включает следующие действия:

1. **Разработка проверочных тестов.** Чтобы протестировать перенос базы данных, необходимо использовать SQL-запросы. Следует создать проверочные запросы, которые будут выполняться в исходной и в целевой базах данных. Проверочные запросы должны охватывать всю определенную ранее область.

1. **Настройка тестовой среды.** Тестовая среда должна содержать копию исходной и целевой баз данных. Не забудьте изолировать тестовую среду.

1. **Выполнение проверочных тестов.** Выполните проверочные тесты в исходной и целевой базах данных, а затем проанализируйте результаты.

1. **Выполнение тестов производительности.** Запустите тесты производительности для исходной и целевой баз данных, а затем проанализируйте и сравните результаты.


## <a name="migration-assets"></a>Ресурсы, посвященные миграции 

Дополнительную помощь по этому сценарию миграции можно получить в приведенных ниже ресурсах. Они разработаны как вспомогательные материалы по реализации реальных проектов миграции.

| Заголовок | Описание |
| --- | --- |
| [Модель и средство оценки рабочей нагрузки данных](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool) | Предоставляет рекомендации по оптимальным целевым платформам, готовности к переходу в облако и уровням исправления для приложений или баз данных с учетом определенных рабочих нагрузок. Обеспечивает простое и быстрое вычисление и создание отчетов, ускоряя оценку больших объемов ресурсов, предоставляя автоматизированный и унифицированный процесс принятия решений относительно целевой платформы. |
| [Пакет обнаружения и оценки ресурсов данных IBM Db2 zOS](https://github.com/Microsoft/DataMigrationTeam/tree/master/Db2%20zOS%20Data%20Assets%20Discovery%20and%20Assessment%20Package) | После выполнения скрипта SQL в базе данных результаты можно экспортировать в файл в файловой системе. Поддерживается несколько форматов файлов, в том числе CSV, что позволяет записывать результаты во внешние средства, такие как электронные таблицы. Этот метод может быть полезен, если требуется возможность с легкостью делиться результатами с командами, у которых нет Workbench.|
| [Скрипты и артефакты инвентаризации IBM Db2 LUW](https://github.com/Microsoft/DataMigrationTeam/tree/master/IBM%20Db2%20LUW%20Inventory%20Scripts%20and%20Artifacts) | Содержит SQL-запрос, который обращается к системным таблицам IBM Db2 LUW версии 11.1 и предоставляет количество объектов в разбивке по схемам и типам объектов, приблизительную оценку необработанных данных в каждой схеме и размер таблиц в каждой схеме с результатами, хранящимися в формате CSV. |
| [Инструкции по установке IBM Db2 LUW pureScale в Azure](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/Db2%20PureScale%20on%20Azure.pdf) | Отправная точка для плана реализации IBM Db2. Ваши бизнес-требования могут быть другими, но базовый шаблон будет таким же. Этот шаблон архитектуры также может использоваться для приложений OLAP в Azure. |

Эти ресурсы разработали специалисты по разработке данных SQL. Основная задача этой команды — включить и ускорить комплексную модернизацию проектов миграции платформы данных на платформу данных Microsoft Azure.

## <a name="next-steps"></a>Дальнейшие действия

После переноса ознакомьтесь с [руководством по проверке и оптимизации после миграции](../../../relational-databases/post-migration-validation-and-optimization-guide.md). 

Матрицу служб и средств, предоставляемых корпорацией Майкрософт и сторонними разработчиками для оказания помощи с разными сценариями переноса баз данных и данных, а также для специализированных задач, см. в статье [Службы и инструменты для переноса данных](/azure/dms/dms-tools-matrix).

Другие рекомендации по миграции см. в [руководствах по переносу баз данных в Azure](https://datamigration.microsoft.com/). 

Видео по миграции см. в разделе [Обзор процесса миграции](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/).

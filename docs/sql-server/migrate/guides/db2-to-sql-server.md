---
title: Руководство по миграции. SQL Server DB2
description: Следуйте этому руководству, чтобы перенести сервер DB2 на SQL Server.
ms.custom: ''
ms.date: 08/17/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- processors [SQL Server], supported
- number of processors supported
- maximum number of processors supported
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 016d0e1a48e9f14356cae9dd4915fedd2b45374b
ms.sourcegitcommit: ae474d21db4f724523e419622ce79f611e956a22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2020
ms.locfileid: "92258011"
---
# <a name="migration-guide-db2-to-sql-server"></a>Руководство по миграции. SQL Server DB2
[!INCLUDE[sqlserver](../../../includes/applies-to-version/sql-asdb-asdbmi.md)]

В этом руководстве по миграции представлены инструкции по переносу пользовательских баз данных из DB2 в SQL Server с помощью Помощника по миграции SQL Server для DB2. 

Другие руководства по миграции см. в разделе [Перенос базы данных](https://datamigration.microsoft.com/). 


## <a name="prerequisites"></a>Предварительные требования

Чтобы перенести базу данных DB2 в SQL Server, необходимо:

- проверить, что исходная среда поддерживается;
- [Помощник по миграции SQL Server (SSMA) для DB2](https://www.microsoft.com/download/details.aspx?id=54254).



## <a name="pre-migration"></a>Подготовка к миграции

После выполнения необходимых условий можно приступать к поиску топологии среды и оценить возможность миграции. 

### <a name="assess"></a>Оценка 

Создайте оценку с помощью Помощника по миграции SQL Server (SSMA). 

Чтобы создать оценку, выполните следующие действия.

1. Откройте Помощник по миграции Microsoft SQL Server (SSMA) для DB2. 
1. Выберите **Файл** , а затем пункт **Новый проект** . 
1. Укажите имя проекта, расположение для сохранения проекта, а затем выберите из раскрывающегося списка целевой объект миграции SQL Server. Щелкните **ОК** . 

   :::image type="content" source="media/db2-to-sql-server/new-project.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::


1. Введите сведения о подключении к DB2 в диалоговом окне **Подключение к DB2** . 

   :::image type="content" source="media/db2-to-sql-server/connect-to-db2.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::


1. Щелкните правой кнопкой мыши схему DB2, которую необходимо перенести, и выберите **Создать отчет** . При этом будет создан HTML-отчет. Кроме того, можно выбрать **Создать отчет** на панели навигации после выбора схемы. 

   :::image type="content" source="media/db2-to-sql-server/create-report.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения." в каталоге SSMAProjects.

   Например: `drive:\<username>\Documents\SSMAProjects\MyDB2Migration\report\report_<date>`. 

   :::image type="content" source="media/db2-to-sql-server/report.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::


### <a name="validate-data-types"></a>Обновление типов данных

Проверьте сопоставления типов данных по умолчанию и измените их в зависимости от требований, если это необходимо. Для этого выполните следующие действия. 

1. В главном меню выберите **Сервис** . 
1. Выберите **Параметры проекта** . 
1. Перейдите на вкладку **Сопоставления типов** . 

   :::image type="content" source="media/db2-to-sql-server/type-mapping.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::

1. Сопоставление типов для каждой таблицы можно изменить, выбрав таблицу в **обозревателе метаданных DB2** . 

### <a name="convert-schema"></a>Преобразовать схему 

Чтобы преобразовать схему, выполните следующие действия.

1. (Необязательно) Добавьте динамические или нерегламентированные запросы в инструкции. Щелкните узел правой кнопкой мыши и выберите **Добавить инструкции** . 
1. Выберите **Подключиться к SQL Server** . 
    1. Введите сведения о подключении для подключения к экземпляру SQL Server. 
    1. Выберите подключение к существующей базе данных на целевом сервере или введите новое имя, чтобы создать новую базу данных на целевом сервере. 
    1. Выберите **Подключиться** . 

   :::image type="content" source="media/db2-to-sql-server/connect-to-sql-server.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::


1. Щелкните правой кнопкой мыши схему и выберите **Преобразовать схему** . Кроме того, можно выбрать пункт **Преобразовать схему** на верхней панели навигации после выбора схемы. 

   :::image type="content" source="media/db2-to-sql-server/convert-schema.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::

1. После завершения преобразования сравните и изучите структуру схемы, чтобы вычислить потенциальные проблемы и устранить их в соответствии с рекомендациями. 

   :::image type="content" source="media/db2-to-sql-server/compare-review-schema-structure.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::

1. Сохраните проект локально для исправления схемы в автономном режиме. В меню **Файл** выберите команду **Сохранить проект** . 


## <a name="migrate"></a>Миграция

После того как вы завершите оценку баз данных и устраните несоответствия, следующим шагом является выполнение процесса миграции.

Чтобы опубликовать схему и перенести данные, выполните следующие действия.

1. Опубликуйте схему: щелкните правой кнопкой мыши базу данных в узле **Базы данных** в **обозревателе метаданных SQL Server** и выберите **Синхронизировать с базой данных** .

   :::image type="content" source="media/db2-to-sql-server/synchronize-with-database.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::

1. Перенесите данные: щелкните правой кнопкой мыши схему в **обозревателе метаданных DB2** и выберите **Перенести данные** . 

   :::image type="content" source="media/db2-to-sql-server/migrate-data.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::

1. Укажите сведения о подключении для экземпляров DB2 и SQL Server. 
1. Просмотрите **отчет о переносе данных** . 

   :::image type="content" source="media/db2-to-sql-server/data-migration-report.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::

1. Подключитесь к экземпляру SQL Server с помощью SQL Server Management Studio и проверьте его, просмотрев данные и схему. 

   :::image type="content" source="media/db2-to-sql-server/compare-schema-in-ssms.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::

## <a name="post-migration"></a>Действия после миграции 

После успешного завершения этапа миграции необходимо выполнить ряд задач, чтобы обеспечить бесперебойную и эффективную работу всех компонентов.

### <a name="remediate-applications"></a>Исправление приложений 

После переноса данных в целевую среду все приложения, которые раньше использовали источник, должны приступить к приему целевого объекта. Для этого в некоторых случаях потребуется внести изменения в приложения.

### <a name="perform-tests"></a>Выполнение тестов

Тестирование переноса базы данных включает следующие действия.

1. **Разработка проверочных тестов** . Чтобы протестировать перенос базы данных, необходимо использовать SQL-запросы. Необходимо создать запросы проверки, которые будут выполняться как в исходной, так и в целевой базах данных. Запросы на проверку должны охватывать определенную область.
1. **Настройка тестовой среды** . Тестовая среда должна содержать копию исходной и целевой баз данных. Не забудьте изолировать тестовую среду.
1. **Выполнение проверочных тестов** . Выполните проверочные тесты для источника и целевого объекта, а затем проанализируйте результаты.
1. **Выполнение тестов производительности** . Запустите тест производительности для источника и целевого объекта, а затем проанализируйте и сравните результаты.

   > [!NOTE]
   > Чтобы получить помощь в разработке и выполнении проверочных тестов, выполняемых после переноса, используйте решение по обеспечению качества данных, предоставляемое нашим партнером [QuerySurge](https://www.querysurge.com/company/partners/microsoft). 

## <a name="migration-assets"></a>Ресурсы, посвященные миграции 

Дополнительные сведения см. на следующих ресурсах, которые были разработаны с учетом поддержки реального сотрудничества проекта миграции.

|Ресурс  |Описание  |
|---------|---------|
|[Модель и средство оценки рабочей нагрузки данных](https://github.com/Microsoft/DataMigrationTeam/tree/master/Data%20Workload%20Assessment%20Model%20and%20Tool)| Это средство предоставляет предлагаемые "оптимальные" целевые платформы, готовность к переходу в облако и уровень исправления приложения/базы данных для конкретной рабочей нагрузки. Оно обеспечивает простое и быстрое вычисление и создание отчетов, которое помогает ускорить оценку больших объемов, предоставляя, автоматизируя и унифицируя процесс принятия решения относительно целевой платформы.|
|[Пакет обнаружения и оценки ресурсов данных DB2 zOS](https://github.com/Microsoft/DataMigrationTeam/tree/master/DB2%20zOS%20Data%20Assets%20Discovery%20and%20Assessment%20Package)|После выполнения скрипта SQL в базе данных результаты можно экспортировать в файл в файловой системе. Поддерживается несколько форматов файлов, в том числе CSV, что позволяет записывать результаты во внешние средства, такие как электронные таблицы. Этот метод может быть полезен, если требуется возможность с легкостью делиться результатами с командами, у которых нет Workbench.|
|[Сценарии и артефакты инвентаризации IBM DB2 LUW](https://github.com/Microsoft/DataMigrationTeam/tree/master/IBM%20DB2%20LUW%20Inventory%20Scripts%20and%20Artifacts)|Этот ресурс включает SQL-запрос, который обращается к системным таблицам IBM DB2 LUW версии 11.1 и предоставляет количество объектов по схеме и типу объектов, приблизительную оценку "необработанных данных" в каждой схеме и размер таблиц в каждой схеме с результатами, хранящимися в формате CSV.|
|[Инструкции по установке DB2 LUW pure scale в Azure](https://github.com/Microsoft/DataMigrationTeam/blob/master/Whitepapers/DB2%20PureScale%20on%20Azure.pdf)|Это руководством служит отправной точкой для плана реализации DB2. Ваши бизнес-требования будут другими, однако базовый шаблон остается без изменений. Этот шаблон архитектуры также может использоваться для приложений OLAP в Azure.|

Эти ресурсы были разработаны в рамках программы Data SQL Ninja, которая спонсируется группой разработчиков Azure Data Group. Основная часть программы Data SQL Ninja заключается в разрешении и ускорении сложной модернизации и реализация перехода на платформу данных Microsoft Azure. Если вы считаете, что ваша организация заинтересована в участии в программе Data SQL Ninja, обратитесь в службу поддержки своей учетной записи и попросите отправить заявку.

## <a name="partners"></a>Участники

Следующие партнеры могут также предоставить альтернативные методы миграции: 

:::row:::
   :::column span="":::
      [:::image type="content" source="media/db2-to-sql-server/blitzz-logo.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::](https://www.blitzz.io/product)
   :::column-end:::
   :::column span="":::
      [:::image type="content" source="media/db2-to-sql-server/blueprint-logo.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::](https://bpcs.com/what-we-do)
   :::column-end:::
   :::column span="":::
      [:::image type="content" source="media/db2-to-sql-server/cognizant-logo.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::](https://www.cognizant.com/partners/microsoft)
   :::column-end:::   
:::row-end:::
:::row:::
   :::column span="":::
      [:::image type="content" source="media/db2-to-sql-server/dxc-logo.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::](https://www.dxc.technology/application_services/offerings/139843/142343-application_services_for_microsoft_azure)
   :::column-end:::
   :::column span="":::
      [:::image type="content" source="media/db2-to-sql-server/hvr-logo.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::](https://www.hvr-software.com/solutions/azure-data-integration/)
   :::column-end:::
   :::column span="":::
      [:::image type="content" source="media/db2-to-sql-server/infosys-logo.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::](https://www.infosys.com/services/)
   :::column-end:::   
:::row-end:::
:::row:::
   :::column span="":::
     [:::image type="content" source="media/db2-to-sql-server/ispirer-logo.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::](https://www.ispirer.com/blog/migration-to-the-microsoft-technology-stack)
   :::column-end:::
   :::column span="":::
      [:::image type="content" source="media/db2-to-sql-server/querysurge-logo.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::](https://www.querysurge.com/company/partners/microsoft)
   :::column-end:::
   :::column span="":::
     [:::image type="content" source="media/db2-to-sql-server/scalability-experts-logo.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::](http://www.scalabilityexperts.com/products/index.html)
   :::column-end:::   
:::row-end:::
:::row:::
   :::column span="":::
     [:::image type="content" source="media/db2-to-sql-server/wipro-logo.png" alt-text="Укажите сведения о проекте и нажмите кнопку ОК для сохранения.":::](https://www.wipro.com/analytics/)
   :::column-end:::
:::row-end:::

## <a name="next-steps"></a>Дальнейшие действия

После миграции ознакомьтесь с [руководством по проверке и оптимизации после миграции](../../../relational-databases/post-migration-validation-and-optimization-guide.md). 

Матрица Майкрософт, а также сторонние службы и средства, доступные для помощи в различных сценариях перемещения баз данных и данных, а также специальных задачах см. в разделе [Службы и инструменты для переноса данных](/azure/dms/dms-tools-matrix).

Другие руководства по миграции см. в разделе [Перенос базы данных](https://datamigration.microsoft.com/). 

Видеоматериалы см. в следующих статьях:
- [Работа с руководством по переносу баз данных](https://azure.microsoft.com/resources/videos/how-to-use-the-azure-database-migration-guide/)
- [Обзор процесса миграции](https://azure.microsoft.com/resources/videos/overview-of-migration-and-recommended-tools-services/)

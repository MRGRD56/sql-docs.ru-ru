---
description: Источник ADO NET
title: Источник ADO NET | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.adonetsource.f1
- sql13.dts.designer.adonetsource.connection.f1
- sql13.dts.designer.adonetsource.columns.f1
- sql13.dts.designer.adonetsource.erroroutput.f1
helpviewer_keywords:
- ADO.NET source
- sources [Integration Services], ADO.NET
- sources [Integration Services], DataReader
- .NET Framework [Integration Services]
- DataReader source
ms.assetid: 2a2f1750-2cda-4dda-9dca-623a96a6b3c0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b10ca4ddff49785c1b5b6e33d7510985fd801c85
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92194866"
---
# <a name="ado-net-source"></a>Источник ADO NET

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Источник ADO NET использует данные поставщика .NET и делает данные доступными для потока данных.  
  
 Вы можете использовать источник ADO NET для подключения к [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]. Соединение с базой данных [!INCLUDE[ssSDS](../../includes/sssds-md.md)] с помощью OLE DB не поддерживается. Дополнительные сведения о [!INCLUDE[ssSDS](../../includes/sssds-md.md)] см. в статье об [общих рекомендациях и ограничениях Базы данных SQL Azure](/previous-versions/azure/ee336245(v=azure.100)).  
  
## <a name="data-type-support"></a>Поддержка типов данных  
 Источник преобразует все типы данных, которые не сопоставлены с конкретными типами данных служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , в тип данных служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] DT_NTEXT. Преобразованию подвергаются даже данные типа **System.Object**.  
  
 Тип данных DT_NTEXT можно изменить на тип DT_WSTR, а DT_WSTR на DT_NTEXT. Типы данных меняются установкой свойства **DataType** в диалоговом окне **Расширенный редактор** источника ADO NET. Дополнительные сведения см. в статье [Common Properties](./set-the-properties-of-a-data-flow-component.md).  
  
 Тип данных DT_NTEXT можно также преобразовать в типы DT_BYTES и DT_STR с помощью преобразования «Конвертация данных» после источника ADO NET. Дополнительные сведения см. в статье [Data Conversion Transformation](../../integration-services/data-flow/transformations/data-conversion-transformation.md).  
  
 В службах [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]типы данных для обозначения дат (DT_DBDATE, DT_DBTIME2, DT_DBTIMESTAMP2 и DT_DBTIMESTAMPOFFSET) соответствуют определенным типам данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Источник ADO NET можно настроить на преобразование типов данных даты, используемых [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , в типы, используемые службами [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Чтобы настроить источник ADO NET на преобразование типов данных «date», задайте для свойства **Версия системы типов** диспетчера соединений значение [!INCLUDE[vstecado](../../includes/vstecado-md.md)] или **Последняя**. (Свойство **Версия системы типов** находится на странице **Все** диалогового окна **Диспетчер соединений** . Чтобы открыть диалоговое окно **Диспетчер соединений** , щелкните правой кнопкой мыши диспетчер соединений [!INCLUDE[vstecado](../../includes/vstecado-md.md)] и выберите команду **Изменить**.)  
  
> [!NOTE]  
>  Если свойство **Версия системы типов** диспетчера соединений [!INCLUDE[vstecado](../../includes/vstecado-md.md)] равно **SQL Server 2005**, система преобразует типы данных даты [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в DT_WSTR.  
  
 Система преобразует определяемые пользователем типы данных в объекты BLOB служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , если поставщик в диспетчере соединений [!INCLUDE[vstecado](../../includes/vstecado-md.md)] указан как поставщик данных .NET для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SqlClient). При преобразовании определяемых пользователем типов данных система применяет следующие правила.  
  
-   Если данные не являются большим определяемым пользователем типом, система преобразует данные в тип DT_BYTES.  
  
-   Если данные не являются большим определяемым пользователем типом и свойство **Длина** столбца базы данных равно -1 или его величина превышает 8000 байт, система преобразует эти данные в тип DT_IMAGE.  
  
-   Если данные являются большим определяемым пользователем типом, система преобразует их в тип DT_IMAGE.  
  
    > [!NOTE]  
    >  Если источник ADO NET не настроен для использования вывода ошибок на выходе, система передает данные столбцу DT_IMAGE в виде потока фрагментами по 8000 байт. Если источник ADO NET настроен для использования вывода ошибок на выходе, система передает весь массив байтов столбцу DT_IMAGE. Дополнительные сведения о настройке компонентов для использования вывода ошибок на выходе см. в разделе [Обработка ошибок в данных](../../integration-services/data-flow/error-handling-in-data.md).  
  
 Дополнительные сведения о типах данных служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , поддерживаемых преобразованиях и сопоставлении типов данных в различных базах данных, в том числе в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в разделе [Типы данных служб Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
 Дополнительные сведения о сопоставлении типов данных [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] с управляемыми типами данных см. в разделе [Работа с типами данных в потоке данных](../../integration-services/extending-packages-custom-objects/data-flow/working-with-data-types-in-the-data-flow.md).  
  
## <a name="ado-net-source-troubleshooting"></a>Устранение неполадок источника ADO NET  
 Можно протоколировать вызовы, сделанные источником ADO NET к внешним поставщикам данных. Эта возможность ведения журнала может быть использована для устранения неполадок загрузки данных из внешнего источника, выполняемой источником ADO NET. Чтобы протоколировать вызовы источника ADO NET к внешнему поставщику данных, необходимо разрешить ведение журнала пакета и выбрать событие **Диагностика** на уровне пакета. Дополнительные сведения см. в разделе [Инструменты устранения неполадок при выполнении пакетов](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
## <a name="ado-net-source-configuration"></a>Настройка источника ADO NET  
 Источник ADO NET настраивается предоставлением инструкции SQL, которая определяет результирующий набор. Например, источник ADO NET, который подключается к базе данных [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] и использует инструкцию SQL `SELECT * FROM Production.Product` , извлекает все строки из таблицы **Production.Product** и предоставляет набор данных для нисходящего компонента.  
  
> [!NOTE]  
>  Когда инструкция SQL используется для вызова хранимой процедуры, возвращающей результаты из временной таблицы, используйте параметр WITH RESULT SETS для определения метаданных набора результатов.  
  
> [!NOTE]  
>  Если при использовании инструкции SQL для выполнения хранимой процедуры происходит сбой пакета со следующей ошибкой, эту ошибку можно исправить путем добавления инструкции **SET FMTONLY OFF** перед инструкцией EXEC.  
>   
>  **Столбец <имя_столбца> не найден в источнике данных.**  
  
 Источник ADO NET использует диспетчер соединений [!INCLUDE[vstecado](../../includes/vstecado-md.md)] для подключения к источнику данных, а диспетчер соединений указывает поставщика .NET. Дополнительные сведения см. в статье [ADO.NET Connection Manager](../../integration-services/connection-manager/ado-net-connection-manager.md).  
  
 Источник ADO NET имеет один обычный выход и один выход ошибок.  
  
 Значения свойств можно задавать с помощью конструктора [!INCLUDE[ssIS](../../includes/ssis-md.md)] или программными средствами.  
  
 Дополнительные сведения о свойствах, которые вы можете задать в диалоговом окне **Расширенный редактор** или программными средствами, см. в следующих разделах.  
  
-   [Общие свойства](./set-the-properties-of-a-data-flow-component.md)  
  
-   [Пользовательские свойства ADO NET](../../integration-services/data-flow/ado-net-custom-properties.md)  
  
 Дополнительные сведения о настройке свойств см. в разделе [Установление свойств компонента потока данных](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="ado-net-source-editor-connection-manager-page"></a>Редактор источника данных «ADO.NET» (страница «Диспетчер соединений»)
  Используйте страницу **Диспетчер соединений** диалогового окна **Редактор назначения «ADO.NET»** для выбора соединения [!INCLUDE[vstecado](../../includes/vstecado-md.md)] применительно к источнику. На этой странице также можно выбрать таблицу или представление базы данных.  
  
 Дополнительные сведения об источниках данных «ADO.NET» см. в разделе [ADO NET Source](../../integration-services/data-flow/ado-net-source.md).  
  
 **Открытие страницы «Диспетчер соединений»**  
  
1.  В среде [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]откройте пакет [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , содержащий источник данных «ADO.NET».  
  
2.  На вкладке **Поток данных** дважды щелкните источник данных "ADO.NET".  
  
3.  В окне **Редактор источника «ADO.NET»** нажмите кнопку **Диспетчер соединений**.  
  
### <a name="static-options"></a>Статические параметры  
 **Диспетчер соединений ADO.NET**  
 Выберите из списка существующий диспетчер соединений или создайте новое соединение, нажав кнопку **Создать**.  
  
 **Создать**  
 Создайте новый диспетчер соединений с помощью диалогового окна **Настройка диспетчера соединений ADO.NET** .  
  
 **Режим доступа к данным**  
 Укажите метод выбора данных из источника.  
  
|Параметр|Описание|  
|------------|-----------------|  
|Таблица или представление|Выборка данных из таблицы или представления в источнике данных [!INCLUDE[vstecado](../../includes/vstecado-md.md)] .|  
|Команда SQL|Выборка данных из источника данных [!INCLUDE[vstecado](../../includes/vstecado-md.md)] с использованием SQL-запроса.|  
  
 **Предварительный просмотр**  
 Осуществляйте предварительный просмотр результатов в диалоговом окне **Просмотр данных** . В окне**Предварительный просмотр** может отображаться до 200 строк.  
  
> [!NOTE]  
>  При предварительном просмотре столбцы с определяемым пользователем типом данных CLR не содержат данных. Вместо этого отображаются значения \<value too big to display> или System.Byte[]. Первый вариант отображается во время доступа к источнику данных с помощью поставщика [!INCLUDE[vstecado](../../includes/vstecado-md.md)] , а последний — при использовании поставщика данных для собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="data-access-mode-dynamic-options"></a>Динамические параметры режима доступа к данным  
  
#### <a name="data-access-mode--table-or-view"></a>Режим доступа к данным = Таблица или представление  
 **Имя таблицы или представления**  
 Выберите имя таблицы или представления из списка доступных в источнике данных.  
  
#### <a name="data-access-mode--sql-command"></a>Режим доступа к данным — команда SQL  
 **Текст команды SQL**  
 Введите текст SQL-запроса, постройте запрос, нажав кнопку **Создать запрос**, или выберите файл, содержащий текст запроса, нажав кнопку **Обзор**.  
  
 **Создать запрос**  
 Воспользуйтесь диалоговым окном **Построитель запросов** для визуального конструирования SQL-запроса.  
  
 **Обзор**  
 Воспользуйтесь диалоговым окном **Открыть** для выбора файла, содержащего текст SQL-запроса.  
  
## <a name="ado-net-source-editor-columns-page"></a>Редактор источника «ADO.NET» (страница «Столбцы»)
  Страница **Столбцы** диалогового окна **Редактор источника "ADO.NET"** используется для сопоставления выходного столбца с каждым внешним (исходным) столбцом.  
  
 Дополнительные сведения об источниках данных «ADO.NET» см. в разделе [ADO NET Source](../../integration-services/data-flow/ado-net-source.md).  
  
 **Открытие страницы «Столбцы»**  
  
1.  В среде [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]откройте пакет [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , содержащий источник данных «ADO.NET».  
  
2.  На вкладке **Поток данных** дважды щелкните источник данных "ADO.NET".  
  
3.  В окне **Редактор источника «ADO.NET»** нажмите кнопку **Столбцы**.  
  
### <a name="options"></a>Параметры  
 **Доступные внешние столбцы**  
 Просмотр списка доступных внешних столбцов источника данных. В этой таблице нельзя добавлять или удалять столбцы.  
  
 **Внешний столбец**  
 Просмотрите внешние столбцы (столбцы в источнике) в том порядке, в котором они отображаются при настройке компонентов, получающих данные из этого источника.  
  
 **Выходной столбец**  
 Введите уникальное имя для каждого выходного столбца. По умолчанию используется имя выбранного внешнего (исходного) столбца, однако можно выбрать любое уникальное описательное имя. Это имя будет отображаться в конструкторе служб [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
## <a name="ado-net-source-editor-error-output-page"></a>Редактор источника данных «ADO.NET» (страница «Вывод ошибок»)
  Страница **Вывод ошибок** диалогового окна **Редактор источника «ADO.NET»** используется для выбора параметров обработки ошибок, а также для установки свойств выходных столбцов ошибок.  
  
 Дополнительные сведения об источниках данных «ADO.NET» см. в разделе [ADO NET Source](../../integration-services/data-flow/ado-net-source.md).  
  
 **Открытие страницы «Вывод ошибок»**  
  
1.  В среде [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]откройте пакет [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , содержащий источник данных «ADO.NET».  
  
2.  На вкладке **Поток данных** дважды щелкните источник данных "ADO.NET".  
  
3.  В окне **Редактор источника «ADO.NET»** нажмите кнопку **Вывод ошибок**.  
  
### <a name="options"></a>Параметры  
 **Ввод-вывод**  
 Просмотр имени источника данных.  
  
 **Столбец**  
 Просмотрите внешние (исходные) столбцы, выбранные на странице **Диспетчер подключений** диалогового окна **Редактор источника "ADO.NET"** .  
  
 **Error**  
 Задайте действие, которое необходимо выполнить при возникновении ошибки: пропустить ошибку, перенаправить строку или вызвать сбой компонента.  
  
 **См. также:** [Обработка ошибок в данных](../../integration-services/data-flow/error-handling-in-data.md)  
  
 **Усечение**  
 Укажите, что нужно сделать при усечении: пропустить ошибку, перенаправить строку или вызвать сбой компонента.  
  
 **Описание**  
 Просмотреть описание ошибки.  
  
 **Присвоить указанное значение выбранным ячейкам**  
 Укажите действие, которое необходимо применить ко всем выбранным ячейкам при возникновении ошибки или усечения: пропустить ошибку, перенаправить строку или вызвать сбой компонента.  
  
 **Применить**  
 Применить параметр обработки ошибок к выбранным ячейкам.  
  
## <a name="see-also"></a>См. также:  
 [Назначение DataReader](../../integration-services/data-flow/datareader-destination.md)   
 [Назначение "ADO.NET"](../../integration-services/data-flow/ado-net-destination.md)   
 [Поток данных](../../integration-services/data-flow/data-flow.md)  
  

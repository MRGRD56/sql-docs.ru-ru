---
title: Тип соединения PowerPivot | Документация Майкрософт
description: В этой статье о типе соединения Power Pivot вы узнаете, как создать источник данных.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: a104c3c7-f118-4d02-9a0f-6859f1469d11
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c83a33e56bca1cba9fab1c5e3ef873e9aa312f8a
ms.sourcegitcommit: 783b35f6478006d654491cb52f6edf108acf2482
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91891734"
---
# <a name="power-pivot-connection-type-ssrs"></a>Тип соединения PowerPivot (SSRS)
  Для извлечения данных из книги [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , опубликованной в коллекции [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] на сайте SharePoint, можно использовать модуль обработки данных служб SQL Server Analysis Services.  
  
 Используйте сведения в этом разделе для создания источника данных. Пошаговые инструкции см. в разделе [Добавление и проверка подключения к данным (построитель отчетов и службы SSRS)](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
## <a name="prerequisites"></a>Предварительные требования  
 Источник данных [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] должен быть опубликован в коллекции [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] на сайте SharePoint.  
  
 Для подключения к книге [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] из построителя отчетов необходимо установить на рабочей станции библиотеку SQL Server 2008 R2 ADOMD.NET. Эта клиентская библиотека устанавливается вместе с [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для Excel, но, если используется компьютер, на котором нет этого приложения, необходимо скачать и установить ADOMD.NET со страницы [Пакет дополнительных компонентов SQL Server 2008 R2](https://www.microsoft.com/download/details.aspx?id=44272).  
  
## <a name="data-source-type"></a>Тип источника данных  
 Используйте тип источника данных отчета **Microsoft SQL Server Analysis Services**.  
  
## <a name="connection-string"></a>Строка подключения  
 Строкой подключения является URL-адрес для книги [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)], опубликованной на сайте SharePoint в коллекции [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] или другой библиотеке, например `https://contoso-srv/subsite/PowerPivotLibrary/ContosoSales.xlsx`.  
  
## <a name="credentials"></a>Учетные данные  
 Укажите учетные данные, необходимые для доступа к книге [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] и сайту SharePoint, например данные для проверки подлинности Windows (встроенная безопасность). См. сведения о [создании строк подключения к данным (построитель отчетов и SSRS)](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) и [определении учетных данных и сведений о подключении для источников данных отчета](specify-credential-and-connection-information-for-report-data-sources.md).  
  
## <a name="queries"></a>Запросы  
 После подключения к источнику данных [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] используйте графический конструктор запросов многомерных выражений для формирования запроса путем просмотра и выбора из базовых структур данных. После создания запроса запустите его, чтобы просмотреть выбранный образец данные на панели результатов.  
  
 Для определения полей набора данных конструктор запросов анализирует запрос. Можно также вручную изменить коллекцию полей набора данных в области **Данные отчета** . Дополнительные сведения см. в разделе [Добавление, изменение и обновление полей в области данных отчета (построитель отчетов и службы SSRS)](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).  
  
## <a name="filters"></a>Фильтры  
 На панели «Фильтры» укажите измерения и элементы, которые необходимо включать или не включать в результаты запроса.  
  
## <a name="parameters"></a>Параметры  
 На панели «Фильтры» установите параметр **Параметры** для фильтра, чтобы автоматически создавать параметр отчета с доступными значениями, соответствующими условиям фильтра.  
  
## <a name="remarks"></a>Remarks  
 При открытии построителя отчетов из книги [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] в коллекции [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] сводные таблицы, сводные диаграммы, срезы и другие функции макета и аналитические функции из книги [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] в отчете повторно не создаются. Вместо этого пустой отчет включает предварительно настроенный источник данных, указывающий на данные в книге [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Конструирование отчетов с помощью книги [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] может потребовать много усилий и времени в зависимости от числа срезов, фильтров и таблиц или диаграмм, которые нужно повторно создавать в отчете. Удобнее создать представление данных, которое требуется использовать в отчете, независимо от проекта [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Данные в книге [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] имеют высокую степень сжатия. Данные, извлекаемые из книги [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для отчета, не сжимаются. Используйте конструктор запросов, чтобы указать фильтры и параметры, ограничивающие объем данных в отчете.  
  
 В отличие от куба служб Analysis Services, модель [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] не имеет иерархий. Чтобы предоставить схожую функциональность связанным срезам в книге, необходимо создать каскадные параметры в отчете. Дополнительные сведения см. в разделах [Добавление каскадных параметров в отчет (построитель отчетов и службы SSRS)](../../reporting-services/report-design/add-cascading-parameters-to-a-report-report-builder-and-ssrs.md).  
  
 В некоторых случаях может потребоваться изменить выражения, чтобы привести их в соответствие со значениями базовых данных из модели [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Может потребоваться изменить выражения, чтобы преобразовать данные в нужный тип данных или чтобы добавить или удалить агрегатную функцию. Например, чтобы преобразовать тип данных из String в Integer, можно использовать функцию `=CInt`. Перед публикацией отчета всегда проверяйте, отображаются ли в отчете ожидаемые значения данных из модели [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Изображения отчета в коллекции [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] создаются только при выполнении следующих условий:  
  
-   Отчет и книга [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , из которой берутся данные, должны храниться вместе в одной галерее [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
-   Отчет содержит только данные [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] из источника данных [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
## <a name="see-also"></a>См. также:  
 [Пользовательский интерфейс конструктора запросов многомерных выражений служб Analysis Services (построитель отчетов)](/previous-versions/sql/)   
 [Выражения (построитель отчетов и службы SSRS)](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  

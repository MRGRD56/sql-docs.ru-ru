---
description: Конструкторы визуальных инструментов для баз данных
title: Конструкторы визуальных инструментов для баз данных
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- data sources [SQL Server]
- View Designer
- Visual Database Tools [SQL Server]
- Database Diagram Designer
- Query Designer [SQL Server]
- design tools [Visual Database Tools]
- Table Designer
- Visual Database Tools [SQL Server], designers
- Properties window [Visual Database Tools]
ms.assetid: bd0ca68e-6f69-42dd-bcb5-ce511673769c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 129de2adfa1b3b033551d3b41db4eac8f34891fe
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338776"
---
# <a name="visual-database-tool-designers"></a>Конструкторы визуальных инструментов для баз данных
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Визуальные инструменты для баз данных представляют собой сочетание средств проектирования, которые можно использовать для работы с источниками данных. С их помощью можно создавать запросы, конструировать и менять структуру базы данных и обновлять данные. В число этих средств входят конструктор диаграмм баз данных, конструктор таблиц и конструктор запросов и представлений.  
  
## <a name="properties-window"></a>Окно «Свойства»  
Аналогичное окно доступно не только в визуальных инструментах для баз данных, но именно здесь можно вносить многие изменения. В этом окне показаны свойства текущего выбранного элемента (например, таблицы), которые можно изменять (все, начиная от имени свойства и заканчивая параметрами сортировки столбца). Некоторые свойства можно просматривать в окне «Свойства», но менять их нужно другими средствами.  
  
## <a name="database-diagram-designer"></a>конструктор диаграмм баз данных  
Конструктор диаграмм баз данных открывает окно, где можно визуально создавать и изменять таблицы и связи в базе данных, а также просматривать их.  
  
Чтобы открыть конструктор диаграмм баз данных, откройте уже существующую диаграмму или в обозревателе объектов щелкните правой кнопкой мыши узел "База данных" и в раскрывающемся списке выберите **Создать диаграмму базы данных** .  
  
После открытия конструктора в главном меню появится меню **Диаграмма базы данных** . Это меню является точкой доступа к специальным возможностям конструктора.  
  
> [!NOTE]  
> Конструктор работает с базами данных Microsoft SQL Server.  
>   
> Данная версия визуальных инструментов для баз данных не поддерживает Microsoft SQL Server версии 7 и более ранние версии.  
  
## <a name="table-designer"></a>конструктор таблиц  
Конструктор таблиц — визуальное средство проектирования и визуализации отдельных таблиц базы данных Microsoft SQL Server, к которой подключен пользователь.  
  
Окно конструктора делится на две панели. В верхней области показана таблица, в каждой строке которой описывается один столбец таблицы базы данных. Для каждого столбца отображаются его главные атрибуты: имя столбца, тип данных и допустимость значения NULL.  
  
В нижней области конструктора таблиц на вкладке «Свойства столбца» отображаются дополнительные атрибуты любого выбранного в верхней таблице столбца.  
  
Щелкнув правой кнопкой мыши в таблице конструктора можно получить доступ к диалоговым окнам, в которых можно создавать и менять связи, ограничения, индексы и ключи таблицы базы данных.  
  
Чтобы открыть конструктор таблиц, откройте уже существующую таблицу или щелкните правой кнопкой мыши узел **Таблицы** в обозревателе объектов и в раскрывающемся списке выберите **Добавить новую таблицу** .  
  
После открытия конструктора в главном меню появится меню «Конструктор таблиц». Это меню является точкой доступа к специальным возможностям конструктора.  
  
> [!NOTE]  
> Конструктор работает с базами данных Microsoft SQL Server.  
>   
> Данная версия визуальных инструментов для баз данных не поддерживает Microsoft SQL Server версии 7 и более ранние версии.  
  
## <a name="query-and-view-designer"></a>конструктор запросов и представлений  
Конструктор запросов и представлений фактически представляет собой два средства, работающих схожим образом. К некоторым из их основных отличий относится следующее.  
  
-   Представления сохраняются в базе данных, а запросы сохраняются в проекте базы данных среды Visual Studio.  
  
-   Конструктор запросов работает практически с любыми источниками данных, а конструктор представлений поддерживает только SQL Server.  
  
-   Конструктор запросов позволяет проектировать инструкции языка манипулирования данными SELECT, INSERT, UPDATE и DELETE, а представления могут содержать только инструкции SELECT.  
  
### <a name="view-designer"></a>Конструктор представлений  
Конструктор представлений позволяет проектировать и наглядно отображать существующие представления или создавать новые в базе данных Microsoft SQL Server, к которой подключен пользователь.  
  
Окно конструктора содержит четыре панели: панель диаграмм, панель критериев, панель «SQL» и панель результатов. Дополнительные сведения о каждой из этих панелей см. в разделе [Инструменты конструктора запросов и представлений (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/query-and-view-designer-tools-visual-database-tools.md).  
  
Чтобы открыть конструктор представлений, откройте уже существующее представление или щелкните правой кнопкой мыши узел **Представление** в обозревателе объектов и в раскрывающемся списке выберите **Добавить новое представление**.  
  
После открытия конструктора в главном меню появится меню **Конструктор запросов** . Это меню является точкой доступа к специальным возможностям конструктора.  
  
> [!NOTE]  
> Конструктор работает с базами данных Microsoft SQL Server.  
>   
> Данная версия визуальных инструментов для баз данных не поддерживает Microsoft SQL Server версии 7 и более ранние версии.  
  
## <a name="see-also"></a>См. также:  
[Конструирование диаграмм баз данных (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/design-database-diagrams-visual-database-tools.md)  
[Проектирование таблиц (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/design-tables-visual-database-tools.md)  
[Разделы по конструированию запросов и представлений (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  

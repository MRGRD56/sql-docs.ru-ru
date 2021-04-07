---
title: Библиотеки классов пользовательского элемента отчета | Документы Майкрософт
description: Узнайте больше о библиотеках классов пользовательских элементов отчета и используйте примеры кода, чтобы понять, как использовать эти классы.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- custom report items, RDL
- RDL [Reporting Services], custom report items
ms.assetid: f18c5d8f-1d6b-4f0b-8657-c14896c2ce0d
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d5d58066e16788dcba313bba82e77504c8394a75
ms.sourcegitcommit: 0b37eb7aef2f358f80867cd13830dd6683da8d85
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2021
ms.locfileid: "105981130"
---
# <a name="custom-report-item-class-libraries"></a>Библиотеки классов пользовательского элемента отчета
  Пользовательские элементы отчета используют классы из пространства имен **Microsoft.ReportDesigner**. Классы, используемые для реализации пользовательского элемента отчета, можно разделить на две основные группы: уникальные классы, созданные для поддержки инфраструктуры пользовательских элементов отчетов, и управляемые классы-оболочки, инкапсулирующие функциональные возможности соответствующих элементов языка определения отчетов. Образец кода для использования этих классов см. на странице [Образцы продуктов служб SQL Server Reporting](https://go.microsoft.com/fwlink/?LinkId=177889).  
  
## <a name="custom-report-item-infrastructure-classes"></a>Классы инфраструктуры пользовательского элемента отчета  
 Для реализации пользовательского элемента отчета используются следующие классы.  
  
> [!NOTE]  
>  Приводимые далее в таблицах списки неполны; в них перечислены только наиболее часто используемые методы и свойства для каждого класса.  
  
### <a name="microsoftreportdesignercustomreportitemdesigner"></a>Microsoft.ReportDesigner.CustomReportItemDesigner  
 Это главный класс пользовательского элемента отчета. Главный класс реализации пользовательского элемента отчета должен быть производным от этого класса.  
  
#### <a name="public-properties"></a>Открытые свойства  
  
|Свойство|Описание|  
|-|-|  
|**имя**;|Имя пользовательского элемента отчета.|  
|**Тип**|Тип пользовательского элемента отчета.|  
|**CustomData**|Объект <xref:Microsoft.ReportingServices.RdlObjectModel.CustomData>, инкапсулирующий свойства данных пользовательского элемента отчета, заданные во время разработки.|  
|**CustomProperties**|Коллекция пользовательских свойств пользовательского элемента отчета.|  
|**Height**|Высота элемента управления пользовательского элемента отчета.|  
|**Width**|Ширина элемента управления пользовательского элемента отчета.|  
|**Отчет**|Контейнер для свойств уровня отчета, таких как список наборов данных отчета.|  
|**AltReportItem**|Альтернативный объект — элемент отчета, который будет использоваться там, где не поддерживается элемент управления времени выполнения для пользовательского элемента отчета.|  
|**Стиль**|Свойства стиля для пользовательского элемента отчета.|  
|**Adornment**|Окно дополнения, используемое для интерактивного редактирования элемента управления.|  
|**Сайт**|**ISite** компонента.|  
|**DesignerVerbCollection**|Набор пользовательских команд, доступных через контекстное меню элемента управления.|  
  
#### <a name="public-methods"></a>Открытые методы  
  
|Метод|Описание|  
|-|-|  
|**BeginEdit**|Активизирует интерактивное редактирование элемента управления.|  
|**DoDefaultAction**|Вызывается двойным щелчком или нажатием клавиши ВВОД в элементе управления.|  
|**EndEdit**|Деактивирует интерактивное редактирование элемента управления.|  
|**GetService**|Возвращает объект, представляющий собой службу.|  
|**InitializeNewComponent**|Вызывается при создании нового пользовательского элемента отчета.|  
|**Invalidate**|Перерисовывает всю область элемента управления.|  
|**OnDragEnter**<br /><br /> **OnDragDrop**|Вызывается при перетаскивании объекта на элемент управления.|  
|**OnPaint**|Вызывается в ответ на событие **Paint**.|  
  
### <a name="microsoftreportdesignercustomreportitemattribute"></a>Microsoft.ReportDesigner.CustomReportItemAttribute  
 Атрибут используется для выяснения типа пользовательского элемента отчета. Имя должно соответствовать значению атрибута \<**Name**> элемента **ReportItem** в файле конфигурации конструктора отчетов.  
  
#### <a name="public-methods"></a>Открытые методы  
  
|Метод|Описание|  
|-|-|  
|**CustomReportItemAttribute**|Создает объект CustomReportItemAttribute.|  
  
### <a name="microsoftreportdesignerlocalizednameattribute"></a>Microsoft.ReportDesigner.LocalizedNameAttribute  
 Этот атрибут позволяет указать отображаемое имя, предназначенное для конструктора пользовательского элемента отчета.  
  
#### <a name="public-methods"></a>Открытые методы  
  
|Метод|Описание|  
|-|-|  
|**LocalizedNameAttribute**|Создает объект LocalizedNameAttribute.|  
  
### <a name="microsoftreportdesigneradornment"></a>Microsoft.ReportDesigner.Adornment  
 Класс **Adornment** используется компонентом времени разработки пользовательского элемента отчета для предоставления областей за пределами основного прямоугольника области конструктора. Эти области могут обрабатывать события пользовательского интерфейса, такие как щелчки кнопкой мыши и операции перетаскивания.  
  
#### <a name="public-methods"></a>Открытые методы  
  
|Метод|Описание|  
|-|-|  
|**OnShow**|Вызывается при активации **Adornment**.|  
|**OnHide**|Вызывается при деактивации **Adornment**.|  
|**Paint**|Вызывается в ответ на событие **Paint**.|  
|**OnDragEnter**<br /><br /> **OnDragOver**<br /><br /> **OnDragLeave**<br /><br /> **OnDragDrop**|Вызывается при перетаскивании объекта в **Adornment**.|  
  
### <a name="microsoftreportdesigneradornerservice"></a>Microsoft.ReportDesigner.AdornerService  
 Этот класс используется для предоставления коллекции служб отображения, используемых пользовательским элементом отчета для поддержки объектов **Adornment** для компонента времени разработки пользовательского элемента отчета.  
  
#### <a name="public-properties"></a>Открытые свойства  
  
|Свойство|Описание|  
|-|-|  
|**AdornerWindowBounds**|Границы окна Adorner.|  
|**AdornerWindowRegion**|Область окна Adorner.|  
|**AdornerWindowGraphics**|Графический контекст окна Adorner.|  
  
#### <a name="public-methods"></a>Открытые методы  
  
|Метод|Описание|  
|-|-|  
|**ComponentRectInDesignerFrame**|Возвращает границы компонента, преобразованные в координаты экрана конструктора.|  
|**InvalidateAdorner**|Делает недействительным окно Adorner.|  
|**PointToAdorner**|Возвращает точку в экранных координатах, преобразованных в координаты окна Adorner.|  
  
### <a name="microsoftreportdesignerexpressioneditor"></a>Microsoft.ReportDesigner.ExpressionEditor  
 Этот класс можно использовать из элемента управления времени разработки пользовательского элемента отчета для вызова редактора выражений.  
  
#### <a name="public-methods"></a>Открытые методы  
  
|Метод|Описание|  
|-|-|  
|**EditValue**|Вызывает редактор выражений, инициализированный значением данного объекта.|  
  
### <a name="microsoftreportdesignerifieldsdataobject"></a>Microsoft.ReportDesigner.IFieldsDataObject  
 Этот класс представляет собой коллекцию полей служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и используется для поддержки событий перетаскивания в среде разработки. Наследует от **IReportItemDataObject**.  
  
#### <a name="public-properties"></a>Открытые свойства  
  
|Свойство|Описание|  
|-|-|  
|**DataSetName**|Имя набора данных, содержащего поля, которые предназначены для перетаскивания.|  
|**Fields**|Коллекция полей (**Microsoft.ReportDesigner.Field**) для удаления.|  
  
## <a name="see-also"></a>См. также:  
 [Язык определения отчетов (службы SSRS)](../../reporting-services/reports/report-definition-language-ssrs.md)   
 [Создание компонента времени выполнения пользовательского элемента отчета](../../reporting-services/custom-report-items/creating-a-custom-report-item-run-time-component.md)   
 [Создание компонента времени разработки пользовательского элемента отчета](../../reporting-services/custom-report-items/creating-a-custom-report-item-design-time-component.md)  
  
  

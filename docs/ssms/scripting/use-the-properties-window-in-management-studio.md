---
title: Использование окна «Свойства» в среде Management Studio
description: Узнайте, как использовать окно свойств для просмотра сведений об элементах SQL Server Management Studio, таких как соединение, и об объектах базы данных.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- viewing properties
- Properties window [SQL Server Management Studio]
- complex properties [SQL Server Management Studio]
ms.assetid: 903d4aca-f57c-43d9-a893-702eceaa7004
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2bd8af54c0178ac13f27dcb3ae23fc8e72b712a8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346087"
---
# <a name="use-the-properties-window-in-management-studio"></a>Использование окна «Свойства» в среде Management Studio
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Окно свойств описывает состояние элемента в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], например соединение или оператор Showplan, и сведения об объектах базы данных, таких как таблицы, представления и конструкторы.  
  
 Окно свойств можно использовать для просмотра свойств текущего соединения. Многие свойства в окне свойств доступны только для чтения, однако могут быть изменены другими средствами среды [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Например, свойство «База данных» запроса в окне свойств доступно только для чтения, но может изменяться на панели инструментов.  
  
### <a name="to-view-properties-using-the-properties-window"></a>Просмотр свойств с помощью окна свойств  
  
1.  Если окна свойств нет на экране, выберите **Окно "Свойства"** в меню **Вид** или нажмите клавишу F4.  
  
2.  Выберите объект, который необходимо просмотреть.  
  
3.  Найдите в окне свойств соответствующее свойство.  
  
### <a name="to-view-connection-properties-of-a-query-window"></a>Просмотр свойств соединения окна запроса  
  
1.  Если окна свойств нет на экране, выберите **Окно "Свойства"** в меню **Вид** или нажмите клавишу F4.  
  
2.  В окне свойств можно увидеть все свойства соединения.  
  
### <a name="to-view-the-properties-of-a-showplan-operator"></a>Просмотр свойств инструкции оператора Showplan  
  
1.  В меню **Запрос** выберите пункт **Включить действительный план выполнения**.  
  
2.  В редакторе SQL-запросов введите текст запроса и выполните его.  
  
3.  Если окна свойств нет на экране, выберите **Окно "Свойства"** в меню **Вид** или нажмите клавишу F4.  
  
4.  На вкладке **План выполнения** редактора SQL-запросов нажмите значки инструкций для просмотра сведений об инструкциях в окне свойств.  
  
## <a name="see-also"></a>См. также:  
 [Окно "Свойства" (среда Management Studio)](../properties-window-management-studio.md)  
  

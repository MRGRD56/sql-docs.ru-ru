---
title: Диалоговое окно «Быстрая проверка»
description: Узнайте, как использовать диалоговое окно "Быстрая проверка" при отладке кода для быстрого просмотра типа данных и значения одного выражения Transact-SQL (например, переменной).
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vs.debug.quickwatch
helpviewer_keywords:
- QuickWatch Dialog [Transact-SQL]
ms.assetid: d6bbb373-1452-41f2-bdc5-86ae689c3dc0
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a958b77c4ad0bec747ec6a39bc25781eb96156cb
ms.sourcegitcommit: 6d53ecfdc463914f045c20eda96da39dec22acca
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/26/2020
ms.locfileid: "88900441"
---
# <a name="transact-sql-debugger---quickwatch-dialog-box"></a>Отладчик Transact-SQL, диалоговое окно "Быстрая проверка"

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Используйте диалоговое окно **Быстрая проверка** , чтобы быстро просмотреть тип данных и значение одного из выражений [!INCLUDE[tsql](../../includes/tsql-md.md)] , такого как переменная или параметр, при отладке кода [!INCLUDE[tsql](../../includes/tsql-md.md)] . Чтобы просмотреть несколько выражений, можно также добавить выражение в окне **Контрольные значения** .  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="task-list"></a>Список задач

 **Доступ к диалоговому окну «Быстрая проверка»**  
  
-   В меню **Отладка** выберите пункт **Быстрая проверка**.  
  
 **Просмотр информации о выражении**  
  
1.  В списке **Выражение** введите или выберите необходимое выражение. Поддерживаются следующие выражения [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
    -   Переменные.  
  
    -   Параметры.  
  
    -   Системные функции, имена которых начинаются с @@.  
  
    -   Выражения, построенные путем применения операторов к одной или нескольким переменным, параметрам или системным функциям, например @IntegerCounter+1 или FirstName+LastName.  
  
    -   Инструкции Transact-SQL, возвращающие только одно значение, например SELECT CharacterCol FROM MyTable WHERE PrimaryKey = 1.  
  
2.  Нажмите кнопку **Пересчет**.  
  
 **Добавление выражения быстрого просмотра в окно «Просмотр значений»**  
  
-   Нажмите кнопку **Добавить контрольное значение**.  
  
 **Изменение значения выражения быстрой проверки**  
  
-   Щелкните правой кнопкой мыши выражение и выберите команду **Изменить значение**.  
  
## <a name="options"></a>Параметры  
 **Список выражений**  
 Отображается выражение, выбранное в настоящее время. Раскрывающийся список содержит набор выражений, которые можно выбрать для отображения. В списке содержатся выражения, доступные в пределах кадра стека, выбранного в настоящее время в окне **Стек вызова** . Чтобы отобразить другое выражение, введите выражение или выберите его из списка. Отладчик [!INCLUDE[tsql](../../includes/tsql-md.md)] поддерживает следующие выражения: переменные, параметры и системные функции, которые имеют имена, начинающиеся с @@.  
  
 **Сетка значений**  
 Отображаются свойства выражения, которое просматривается в настоящее время.  
  
 **имя**;  
 Является просматриваемым выражением [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 **Значение**  
 Отображается значение, которое в настоящее время присвоено выражению. Если в настоящее время выражение не имеет значения, отображается пустое поле.  
  
 Если длина выражения больше ширины столбца **Значение** , полное значение отображается в подсказке при перемещении указателя на ячейку **Значение** для этого выражения.  
  
 Значок лупы в ячейке **Значение** указывает, что доступен визуализатор отладчика [!INCLUDE[tsql](../../includes/tsql-md.md)] . В этом списке можно указать **Визуализатор текста**, **Визуализатор XML**или **Визуализатор HTML**. Чтобы выполнить запуск визуализатора отладчика, щелкните значок лупы. В отладчике [!INCLUDE[tsql](../../includes/tsql-md.md)] откроется диалоговое окно, в котором данные будут отображены в формате, соответствующем типу этих данных.  
  
 **Тип**  
 Отображает тип данных выражения.  
  
## <a name="see-also"></a>См. также:  
 [Отладчик Transact-SQL](../../relational-databases/scripting/transact-sql-debugger.md)   
 [Сведения отладчика Transact-SQL](../../relational-databases/scripting/transact-sql-debugger-information.md)   
 [окно просмотра значений](../../relational-databases/scripting/transact-sql-debugger-watch-window.md)   
 [окно локальных переменных](../../relational-databases/scripting/transact-sql-debugger-locals-window.md)   
 [Окно стека вызовов](../../relational-databases/scripting/transact-sql-debugger-call-stack-window.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  

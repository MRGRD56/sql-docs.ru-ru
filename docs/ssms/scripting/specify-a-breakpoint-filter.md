---
title: Задание фильтра точек останова
description: Сведения о том, как реализовать фильтр точек останова, чтобы ограничить действие точки останова только отладкой, выполняемой на определенных компьютерах, в определенных процессах операционной системы и потоках.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, breakpoint filter
ms.assetid: 7bf1dddd-7b0b-4c47-8a7b-28a5569b4fa5
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d6e2a3540ea758fa394ad0ebcdcd28777bf4237b
ms.sourcegitcommit: 6d53ecfdc463914f045c20eda96da39dec22acca
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/26/2020
ms.locfileid: "88901594"
---
# <a name="specify-a-breakpoint-filter"></a>Задание фильтра точек останова

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Фильтр для точек останова ограничивает действие точки останова определенными компьютерами, процессами операционной системы и потоками. Фильтры точек останова обычно используются при отладке параллельных приложений.

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]
  
##  <a name="filter-considerations"></a><a name="BKMK_ActionConsiderations"></a> Вопросы применения фильтров

Обычно фильтры точек останова используют вместе с отладчиком [!INCLUDE[tsql](../../includes/tsql-md.md)] , потому что скрипты и хранимые процедуры [!INCLUDE[tsql](../../includes/tsql-md.md)] не являются параллельными приложениями.  
  
#### <a name="to-specify-a-breakpoint-filter"></a>Задание фильтра точки останова  
  
1.  В окне редактора щелкните метку точки останова правой кнопкой мыши и выберите в контекстном меню пункт **Фильтр** .  
  
     -или-  
  
     В окне **Точки останова** щелкните метку точки останова правой кнопкой мыши и выберите в контекстном меню пункт **Фильтр** .  
  
2.  В диалоговом окне **Фильтры точки останова** используйте флажок **Фильтр** для указания компьютеров по имени или процессов и потоков операционной системы по имени или идентификационному номеру.  
  
    -   **MachineName** — это компьютер, на котором запущен экземпляр Database Engine.  
  
    -   **ProcessID**и **ProcessName** — это процесс операционной системы, в котором исполняется экземпляр компонента Database Engine.  
  
    -   **ThreadID** и **ThreadName** — это поток операционной системы, в котором выполняется пакет, процедура или функция [!INCLUDE[tsql](../../includes/tsql-md.md)] на экземпляре Database Engine.  
  
3.  Нажмите кнопку **ОК** , чтобы внести изменения, либо кнопку **Отмена** , чтобы выйти без их применения.  
  
## <a name="see-also"></a>См. также:  
 [Задание условия точки останова](../../relational-databases/scripting/specify-a-breakpoint-condition.md)   
 [Настройка счетчика числа попаданий](../../relational-databases/scripting/specify-a-hit-count.md)   
 [Задание действия в точке останова](../../relational-databases/scripting/specify-a-breakpoint-action.md)  

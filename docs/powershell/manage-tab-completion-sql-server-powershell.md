---
title: Управление завершением по нажатию клавиши Tab (SQL Server PowerShell)
description: Узнайте, как управлять завершением по нажатию клавиши TAB в Windows PowerShell, правильно используя три переменные в модулях SQL Server PowerShell.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 4fd55fc24889e84757fb1d78f5e9cd5c8dbb7212
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838021"
---
# <a name="manage-tab-completion-with-sql-server-powershell"></a>Управление завершением по нажатию клавиши TAB с SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

 Оснастки PowerShell [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] предоставляют три переменные для управления завершением Windows PowerShell по нажатию клавиши TAB: **$SqlServerMaximumTabCompletion**, **$SqlServerMaximumChildItems** и **$SqlServerIncludeSystemObjects**. Функция завершения по клавише TAB позволяет сократить объем вводимого текста, поскольку возвращает таблицы элементов, имена которых начинаются с набранной строки.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Функция завершения по клавише TAB среды Windows PowerShell дает возможность после ввода части имени пути или командлета нажать клавишу TAB, чтобы получить список элементов, имена которых согласуются с уже набранным текстом. Затем можно выбрать нужный элемент из списка, не набирая остальную часть его имени.  

При работе с базой данных, содержащей много объектов, списки завершения по клавише TAB могут стать очень большими. Кроме того, для некоторых типов объектов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , например представлений, предусмотрено большое число системных объектов.  

В оснастках [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] впервые представлены три системные переменные, которые позволяют управлять объемом данных, выводимых функцией завершения по клавише TAB и командлетом **Get-ChildItem**.

## <a name="sqlservermaximumtabcompletion--n"></a>$SqlServerMaximumTabCompletion =** *n*

Указывает максимальное число объектов, включаемых в список завершения по клавише TAB. Если нажать клавишу TAB в узле пути, для которого существует более *n* подходящих объектов, выводимый список завершения будет усечен до *n* объектов. *n* имеет тип integer. 0 — значение по умолчанию, которое означает, что число перечисляемых объектов не ограничено.  

## <a name="sqlservermaximumchilditems--n"></a>$SqlServerMaximumChildItems =** *n*

Указывает максимальное количество объектов, отображаемых командлетом **Get-ChildItem**. Если командлет **Get-ChildItem** выполняется в узле пути, для которого существует более *n* объектов, список будет усечен до *n* объектов. *n* имеет тип integer. 0 — значение по умолчанию, которое означает, что число перечисляемых объектов не ограничено.  

## <a name="sqlserverincludesystemobjects---true--false-"></a>$SqlServerIncludeSystemObjects =** { **$True** | **$False** }

Если указано значение **$True**, функция завершения по клавише TAB и командлет **Get-ChildItem** отображают системные объекты. Если значение равно **$False**, системные объекты не отображаются. Значение по умолчанию — **$False**.  

## <a name="set-the-sql-server-tab-completion-variables"></a>Установка переменных функции завершения по клавише TAB для SQL Server

Задайте новое значение для любой переменной, значение которой необходимо заменить на отличное от применяемого по умолчанию.  

### <a name="example-powershell"></a>Пример (PowerShell)

В следующем примере задаются все три переменные и выводятся их значения:  

```powershell
$SqlServerMaximumTabCompletion = 20  
$SqlServerMaximumChildItems = 10  
$SqlServerIncludeSystemObjects = $False  
dir variable:sqlserver*  
```

## <a name="see-also"></a>См. также:

- [Установка компонентов SQL Server PowerShell](download-sql-server-ps-module.md)
- [SQL Server PowerShell](sql-server-powershell.md)
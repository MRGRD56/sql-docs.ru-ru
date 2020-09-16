---
title: Управление завершением по нажатию клавиши Tab (SQL Server PowerShell) | Документация Майкрософт
description: Узнайте, как управлять завершением по нажатию клавиши TAB в Windows PowerShell, правильно используя три переменные в модулях SQL Server PowerShell.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: 6296848a-890f-4ad3-8d9f-92ed6a79aa00
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b60741f9e35b9910f9650d1ce7dcb920ba22c08b
ms.sourcegitcommit: a9f16d7819ed0e2b7ad8f4a7d4d2397437b2bbb2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2020
ms.locfileid: "88714132"
---
# <a name="manage-tab-completion-sql-server-powershell"></a>Управление завершением по нажатию клавиши Tab (SQL Server PowerShell)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

 Оснастки PowerShell [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] предоставляют три переменные для управления завершением Windows PowerShell по нажатию клавиши TAB: **$SqlServerMaximumTabCompletion**, **$SqlServerMaximumChildItems** и **$SqlServerIncludeSystemObjects**. Функция завершения по клавише TAB позволяет сократить объем вводимого текста, поскольку возвращает таблицы элементов, имена которых начинаются с набранной строки.  

> [!NOTE]
> Существует два модуля SQL Server PowerShell — **SqlServer** и **SQLPS**. Модуль **SQLPS** входит в состав установки SQL Server (для обеспечения обратной совместимости), но больше не обновляется. Самым актуальным модулем PowerShell является модуль **SqlServer**. Модуль **SqlServer** содержит обновленные версии командлетов в **SQLPS**, а также новые командлеты для поддержки последних функций SQL.  
> Предыдущие версии модуля **SqlServer** *входили* в состав среды SQL Server Management Studio (SSMS), но только с SSMS версий 16.x. Для работы PowerShell с SSMS 17.0 и более поздних версий необходимо установить модуль **SqlServer** из коллекции PowerShell.
> Сведения об установке модуля **SqlServer** см. в статье [Установка компонентов SQL Server PowerShell](download-sql-server-ps-module.md).
  
Функция завершения по клавише TAB среды Windows PowerShell дает возможность после ввода части имени пути или командлета нажать клавишу TAB, чтобы получить список элементов, имена которых согласуются с уже набранным текстом. Затем можно выбрать нужный элемент из списка, не набирая остальную часть его имени.  
  
При работе с базой данных, содержащей много объектов, списки завершения по клавише TAB могут стать очень большими. Кроме того, для некоторых типов объектов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , например представлений, предусмотрено большое число системных объектов.  
  
В оснастках [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] впервые представлены три системные переменные, которые позволяют управлять объемом данных, выводимых функцией завершения по клавише TAB и командлетом **Get-ChildItem**.  
  
 **$SqlServerMaximumTabCompletion =** *n*  
 Указывает максимальное число объектов, включаемых в список завершения по клавише TAB. Если нажать клавишу TAB в узле пути, для которого существует более *n* подходящих объектов, выводимый список завершения будет усечен до *n*объектов. *n* имеет тип integer. 0 — значение по умолчанию, которое означает, что число перечисляемых объектов не ограничено.  
  
 **$SqlServerMaximumChildItems =** *n*  
 Указывает максимальное количество объектов, отображаемых командлетом **Get-ChildItem**. Если командлет **Get-ChildItem** выполняется в узле пути, для которого существует более *n* объектов, список будет усечен до *n*объектов. *n* имеет тип integer. 0 — значение по умолчанию, которое означает, что число перечисляемых объектов не ограничено.  
  
 **$SqlServerIncludeSystemObjects =** { **$True** |  **$False** }  
 Если указано значение **$True**, функция завершения по клавише TAB и командлет **Get-ChildItem**отображают системные объекты. Если значение равно **$False**, системные объекты не отображаются. Значение по умолчанию — **$False**.  
  
## <a name="set-the-sql-server-tab-completion-variables"></a>Установка переменных функции завершения по клавише TAB для SQL Server  
 Задайте новое значение для любой переменной, значение которой необходимо заменить на отличное от применяемого по умолчанию.  
  
### <a name="example-powershell"></a>Пример (PowerShell)  
 В следующем примере задаются все три переменные и выводятся их значения:  
  
```  
$SqlServerMaximumTabCompletion = 20  
$SqlServerMaximumChildItems = 10  
$SqlServerIncludeSystemObjects = $False  
dir variable:sqlserver*  
```  
  
## <a name="see-also"></a>См. также:  
 [SQL Server PowerShell](sql-server-powershell.md)  
  
  

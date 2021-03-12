---
title: Запуск Windows PowerShell из среды SQL Server Management Studio
description: Узнайте, как запустить сеанс Windows PowerShell из обозревателя объектов в SQL Server Management Studio с предустановленным путем к выбранному вами расположению объектов.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 03/14/2017
ms.openlocfilehash: 9fd9b7039680b05515d1f70102408394b5443c9c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839479"
---
# <a name="run-windows-powershell-from-sql-server-management-studio"></a>Запуск Windows PowerShell из среды SQL Server Management Studio

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Сеансы Windows PowerShell можно запустить из **обозревателя объектов** в SQL Server Management Studio (SSMS). SSMS запускает Windows PowerShell, загружает модуль **SqlServer** и задает контекст пути для связанного узла в дереве **обозревателя объектов**.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Когда вы указываете, что PowerShell необходимо вызвать для объекта в **обозревателе объектов**, SQL Server Management Studio начинает сеанс Windows PowerShell, в котором были загружены и зарегистрированы оснастки SQL Server PowerShell. Путем для сеанса становится расположение объекта, выбранного правой кнопкой мыши в обозревателе объектов.

Например, если щелкнуть правой кнопкой мыши объект базы данных AdventureWorks в обозревателе объектов, а затем выбрать **Запустить PowerShell**, путь Windows PowerShell будет выглядеть так:

```powershell
SQLSERVER:\SQL\MyComputer\MyInstance\Databases\AdventureWorks2012>  
```

## <a name="run-powershell"></a>Запуск PowerShell

### <a name="to-run-powershell-from-sql-server-management-studio"></a>Запуск PowerShell из среды SQL Server Management Studio

1. Откройте **обозреватель объектов**.

2. Перейдите к узлу для объекта, с которым выполняется работа.

3. Щелкните объект правой кнопкой мыши и выберите команду **Запустить PowerShell**.

## <a name="permissions"></a>Разрешения

При открытии из среды SQL Server Management Studio PowerShell не запускается с правами администратора, и это может стать причиной того, что некоторые действия не будут выполнены, например вызовы WMI.

## <a name="see-also"></a>См. также:

- [SQL Server PowerShell](sql-server-powershell.md)
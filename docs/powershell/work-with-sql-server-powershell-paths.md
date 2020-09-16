---
title: Работа с путями SQL Server PowerShell | Документация Майкрософт
description: Сведения о том, как управлять информацией и получать ее с помощью командлетов либо методов и свойств объекта, определяемого путем поставщика.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: f31d8e2c-8d59-4fee-ac2a-324668e54262
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 15dae50a965c6e30c9531a76b0cb7d6867ee49ec
ms.sourcegitcommit: a9f16d7819ed0e2b7ad8f4a7d4d2397437b2bbb2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2020
ms.locfileid: "88714322"
---
# <a name="work-with-sql-server-powershell-paths"></a>Работа с  путями SQL Server PowerShell
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

После перехода к узлу в пути поставщика компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] можно выполнять действия или с помощью методов и свойств извлекать информацию из управляющих объектов компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] , связанных с этим узлом.  
  
> [!NOTE]
> Существует два модуля SQL Server PowerShell — **SqlServer** и **SQLPS**. Модуль **SQLPS** входит в состав установки SQL Server (для обеспечения обратной совместимости), но больше не обновляется. Самым актуальным модулем PowerShell является модуль **SqlServer**. Модуль **SqlServer** содержит обновленные версии командлетов в **SQLPS**, а также новые командлеты для поддержки последних функций SQL.  
> Предыдущие версии модуля **SqlServer** *входили* в состав среды SQL Server Management Studio (SSMS), но только с SSMS версий 16.x. Для работы PowerShell с SSMS 17.0 и более поздних версий необходимо установить модуль **SqlServer** из коллекции PowerShell.
> Сведения об установке модуля **SqlServer** см. в статье [Установка компонентов SQL Server PowerShell](download-sql-server-ps-module.md).

  
После того как выбран узел в пути поставщика компонента [!INCLUDE[ssDE](../includes/ssde-md.md)], можно выполнять действия двух типов.  
  
-   Можно запускать командлеты Windows PowerShell, работающие с узлами, такие как **Rename-Item**.  
  
-   Можно вызывать методы из соответствующей модели управляющих объектов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , например SMO. Например, если перейти в пути к узлу Databases, то можно использовать методы и свойства класса <xref:Microsoft.SqlServer.Management.Smo.Database> .  
  
 Поставщик [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] используется для управления объектами в экземпляре компонента [!INCLUDE[ssDE](../includes/ssde-md.md)]. Он не предназначен для работы с данными в базах данных. Если выбрана таблица или представление, нельзя использовать поставщик для выбора, вставки, обновления или удаления данных. Чтобы запросить или изменить данные в таблицах и представлениях из среды Windows PowerShell, воспользуйтесь командлетом **Invoke-Sqlcmd** . Дополнительные сведения см. в разделе [Invoke-Sqlcmd, командлет](invoke-sqlcmd-cmdlet.md).  
  
##  <a name="listing-methods-and-properties"></a><a name="ListPropMeth"></a> список методов и свойств  
 **список методов и свойств**  
  
 Командлет **Get-Member** используется для просмотра методов и свойств, доступных для определенных объектов или классов объектов.  
  
### <a name="examples-listing-methods-and-properties"></a>Примеры: список методов и свойств  
 В этом примере задается переменная Windows PowerShell для класса <xref:Microsoft.SqlServer.Management.Smo.Database> модели SMO и перечисляются методы и свойства:  
  
```  
$MyDBVar = New-Object Microsoft.SqlServer.Management.SMO.Database  
$MyDBVar | Get-Member -Type Methods  
$MyDBVar | Get-Member -Type Properties  
```  
  
 Командлет **Get-Member** также можно использовать для вывода методов и свойств, связанных с конечным узлом пути Windows PowerShell.  
  
 В этом примере выполняется переход к узлу Databases на диске SQLSERVER и перечисление свойства коллекции.  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
Get-Item . | Get-Member -Type Properties  
```  
  
 В этом примере выполняется переход к узлу AdventureWorks2012 по пути SQLSERVER: и перечисление свойства объектов.  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012  
Get-Item . | Get-Member -Type Properties  
```  
  
##  <a name="using-methods-and-properties"></a><a name="UsePropMeth"></a> использование методов и свойств  
 **Использование методов и свойств SMO**  
  
 Для выполнения действий с объектами из пути поставщика компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] можно использовать методы и свойства объектов SMO.  
  
### <a name="examples-using-methods-and-properties"></a>Примеры: использование методов и свойств  
 В этом примере свойство **Schema** объекта SMO служит для получения списка таблиц из схемы Sales в базе данных AdventureWorks2012:  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Tables  
Get-ChildItem | where {$_.Schema -eq "Sales"}  
```  
  
 В этом примере с помощью метода **Script** объекта SMO создается скрипт, содержащий инструкции **CREATE VIEW** , необходимые для повторного создания представлений в базе данных AdventureWorks2012:  
  
```  
Remove-Item C:\PowerShell\CreateViews.sql  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Views  
foreach ($Item in Get-ChildItem) { $Item.Script() | Out-File -Filepath C:\PowerShell\CreateViews.sql -append }  
```  
  
 В этом примере используется метод **Create** модели SMO, чтобы создать базу данных, а затем используется свойство **State** , чтобы показать, существует ли эта база данных:  
  
```  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
$MyDBVar = New-Object Microsoft.SqlServer.Management.SMO.Database  
$MyDBVar.Parent = (Get-Item ..)  
$MyDBVar.Name = "NewDB"  
$MyDBVar.Create()  
$MyDBVar.State  
```  
  
## <a name="see-also"></a>См. также:  
 [SQL Server PowerShell, поставщик](sql-server-powershell-provider.md)   
 [Перемещение путей SQL Server PowerShell](navigate-sql-server-powershell-paths.md)   
 [Преобразование универсальных имен ресурса в пути поставщика SQL Server](https://docs.microsoft.com/powershell/module/sqlserver/Convert-UrnToPath)   
 [SQL Server PowerShell](sql-server-powershell.md)  
  
  

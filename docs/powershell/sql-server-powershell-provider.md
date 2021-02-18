---
title: Поставщик SQL Server PowerShell | Документация Майкрософт
description: Узнайте о поставщике SQL Server для Windows PowerShell, который предоставляет доступ к объектам SQL Server с помощью путей, аналогичных путям файловой системы.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
helpviewer_keywords:
- PowerShell [SQL Server], provider
- PowerShell [SQL Server], SQL Server PowerShell Provider
- Providers [PowerShell]
- SMO [SQL Server], PowerShell
- PowerShell [SQL Server], SMO
- SQL Server Management Objects, PowerShell
ms.assetid: b97acc43-fcd2-4ae5-b218-e183bab916f9
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 07/31/2019
ms.openlocfilehash: 5e62ed5e7482ca6fc1d80eb6e7f19ca1e0da4e7f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350165"
---
# <a name="sql-server-powershell-provider"></a>SQL Server PowerShell, поставщик

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Поставщик [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] для Windows PowerShell отображает иерархию объектов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] в виде путей, аналогичных путям файловой системы. Можно определить расположение объекта с помощью путей, а затем использовать методы, доступные в моделях объектов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] SMO, для выполнения действия с объектами.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="benefits-of-the-sql-server-powershell-provider"></a>Преимущества поставщика SQL Server PowerShell

Пути, реализуемые поставщиком [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , позволяют легко просматривать все объекты в экземпляре SQL Server в интерактивном режиме. Предусмотрена возможность переходить по этим путям с использованием псевдонимов Windows PowerShell по аналогии с переходом по путям файловой системы с помощью команд.  
  
## <a name="the-sql-server-powershell-hierarchy"></a>Иерархия SQL Server в PowerShell

Продукты, в которых модели данных или модели объектов можно представить в иерархическом виде, используют для представления таких иерархий поставщики Windows PowerShell. Иерархия отображается при помощи диска и структуры пути, похожей на ту, которая используется в файловой системе Windows.  
  
 Каждый поставщик Windows PowerShell реализует один или несколько дисков. Каждый диск является корневым узлом в иерархии связанных объектов. В поставщике [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell реализован диск «SQLSERVER:». Поставщик также определяет набор основных папок для диска SQLSERVER:. Каждая папка и вложенные в нее папки представляют набор объектов, к которым можно получить доступ с помощью модели управляющих объектов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Если выделена вложенная папка в пути, начинающемся с одной из этих папок, можно использовать методы из связанной объектной модели для выполнения действий с объектом, который представлен вложенной папкой. Папки Windows PowerShell, реализуемые поставщиком [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)], перечислены в следующей таблице.  
  
|Папка|Пространство имен объектной модели SQL Server|Объекты|  
|------------|---------------------------------------|-------------|  
|`SQLSERVER:\SQL`|<xref:Microsoft.SqlServer.Management.Smo><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.Agent><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.Broker><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.Mail>|Объекты базы данных, такие как таблицы, представления и хранимые процедуры.|  
|`SQLSERVER:\SQLPolicy`|<xref:Microsoft.SqlServer.Management.Dmf><br /><br /> <xref:Microsoft.SqlServer.Management.Facets>|Объекты управления на основе политик, такие как политики и аспекты.|  
|`SQLSERVER:\SQLRegistration`|<xref:Microsoft.SqlServer.Management.RegisteredServers><br /><br /> <xref:Microsoft.SqlServer.Management.Smo.RegSvrEnum>|Зарегистрированные объекты серверов, такие как группы серверов и зарегистрированные серверы.|  
|`SQLSERVER:\Utility`|<xref:Microsoft.SqlServer.Management.Utility>|Вспомогательные объекты, такие как управляемые экземпляры компонента [!INCLUDE[ssDE](../includes/ssde-md.md)].|  
|`SQLSERVER:\DAC`|[Microsoft.SqlServer.Management.Dac](/previous-versions/sql/sql-server-2012/ee212127(v=sql.110))|Объекты приложения уровня данных, такие как пакеты DAC, и операции, такие как развертывание DAC.|  
|`SQLSERVER:\DataCollection`|<xref:Microsoft.SqlServer.Management.Collector>|Объекты сборщика данных, такие как наборы элементов сбора и хранилища конфигураций.|  
|`SQLSERVER:\SSIS`|<xref:Microsoft.SqlServer.Management.IntegrationServices>|[!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , как проекты, пакеты и среды.|  
|`SQLSERVER:\XEvent`|<xref:Microsoft.SqlServer.Management.XEvent>|Расширенные события SQL Server|
|`SQLSERVER:\DatabaseXEvent`|[Microsoft.SqlServer.Management.XEventDbScoped](/dotnet/api/microsoft.sqlserver.management.xeventdbscoped)|Расширенные события SQL Server|
|`SQLSERVER:\SQLAS`|<xref:Microsoft.AnalysisServices>|[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] , такие как кубы, агрегаты и измерения.|  
  
 Например, папку SQLSERVER:\SQL можно использовать, чтобы начинать пути, которые могут представлять любой объект, поддерживаемый объектной моделью SMO. Начальная область пути SQLSERVER:\SQL — SQLSERVER:\SQL\\*ComputerName*\\*InstanceName*. Узлы после имени экземпляра поочередно указывают коллекции объектов (такие как *Базы данных* или *Представления*) и имена объектов (наподобие AdventureWorks2012). Схемы не представляются в качестве классов объектов. Если указывается узел для объекта верхнего уровня в схеме, такого как таблица или представление, необходимо указать имя объекта в формате *ИмяСхемы.ИмяОбъекта*.  
  
 В следующем примере показан путь к таблице "Vendor" в схеме "Purchasing" базы данных AdventureWorks2012 на экземпляре по умолчанию компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] на локальном компьютере.  
  
```powershell
SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012\Tables\Purchasing.Vendor  
```
  
 Дополнительные сведения об иерархии модели объектов SMO см. в разделе [SMO Object Model Diagram](../relational-databases/server-management-objects-smo/smo-object-model-diagram.md).  
  
 Узлы коллекций в пути связаны с классом коллекций в связанной объектной модели. Узлы имен объектов связаны с классом объектов в связанной модели объектов, как показано в следующей таблице.  
  
|путь|Класс SMO|  
|----------|---------------|  
|`SQLSERVER:\SQL\MyComputer\DEFAULT\Databases`|<xref:Microsoft.SqlServer.Management.Smo.DatabaseCollection>|  
|`SQLSERVER:\SQL\MyComputer\DEFAULT\Databases\AdventureWorks2012`|<xref:Microsoft.SqlServer.Management.Smo.Database>|  
  
## <a name="sql-server-provider-tasks"></a>Задачи поставщика SQL Server  
  
|Описание задачи|Статья|  
|----------------------|-----------|  
|Описано, как использовать командлеты Windows PowerShell для перехода по узлам пути и получения в каждом узле списка объектов этого узла.|[Перемещение путей SQL Server PowerShell](navigate-sql-server-powershell-paths.md)|  
|Описано, как использовать методы и свойства объектов SMO для получения отчета и выполнения работы над объектом, представленным узлом пути. Кроме того, описано, как получить список методов и свойств объектов SMO для этого узла.|[Работа с путями SQL Server PowerShell](work-with-sql-server-powershell-paths.md)|  
|Описывает, как преобразовать универсальное имя ресурса объекта SMO в путь поставщика SQL Server.|[Преобразование URNs в пути поставщика SQL Server](/powershell/module/sqlserver/Convert-UrnToPath)|  
|Описано, как открывать соединения проверки подлинности SQL Server с использованием поставщика [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . По умолчанию поставщик использует соединения проверки подлинности Windows, установленные с помощью учетных данных той учетной записи Windows, которая используется в сеансе Windows PowerShell.|[Управление проверкой подлинности в PowerShell ядра СУБД ](manage-authentication-in-database-engine-powershell.md)|  
  
## <a name="next-steps"></a>Дальнейшие действия

[SQL Server PowerShell](sql-server-powershell.md)
---
title: Перемещение по путям PowerShell (SQL Server) | Документация Майкрософт
description: Узнайте, как использовать командлеты PowerShell для навигации по структуре пути, которая представляет иерархию объектов, поддерживаемых поставщиком PowerShell.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: d68aca48-d161-45ed-9f4f-14122ed30218
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c823a1ff344fb453229330a0657d0604432397c0
ms.sourcegitcommit: a9f16d7819ed0e2b7ad8f4a7d4d2397437b2bbb2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2020
ms.locfileid: "88714242"
---
# <a name="navigate-sql-server-powershell-paths"></a>Перемещение путей SQL Server PowerShell
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Поставщик компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] PowerShell представляет набор объектов в экземпляре SQL Server в структуре, аналогичной пути к файлу. Командлеты Windows PowerShell можно использовать для навигации по пути поставщика и для создания нестандартных дисков, укорачивающих путь, который требуется ввести.  

> [!NOTE]
> Существует два модуля SQL Server PowerShell — **SqlServer** и **SQLPS**. Модуль **SQLPS** входит в состав установки SQL Server (для обеспечения обратной совместимости), но больше не обновляется. Самым актуальным модулем PowerShell является модуль **SqlServer**. Модуль **SqlServer** содержит обновленные версии командлетов в **SQLPS**, а также новые командлеты для поддержки последних функций SQL.  
> Предыдущие версии модуля **SqlServer** *входили* в состав среды SQL Server Management Studio (SSMS), но только с SSMS версий 16.x. Для работы PowerShell с SSMS 17.0 и более поздних версий необходимо установить модуль **SqlServer** из коллекции PowerShell.
> Сведения об установке модуля **SqlServer** см. в статье [Установка компонентов SQL Server PowerShell](download-sql-server-ps-module.md).
  
В Windows PowerShell предусмотрены командлеты для навигации по структуре пути, которая представляет иерархию объектов, поддерживаемых поставщиком PowerShell. После перехода к нужному узлу можно использовать другие командлеты для выполнения основных операций с текущим объектом. Поскольку командлеты используются часто, они обладают краткими каноническими псевдонимами. Также существует один набор псевдонимов, сопоставляющий командлеты с похожими командами командной строки, и другой набор для команд оболочки UNIX.  
  
 Поставщик [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] реализует подмножество командлетов поставщика, приведенных в следующей таблице.  
  
|командлет|Канонический псевдоним|Псевдоним командной строки|Псевдоним оболочки UNIX|Описание|  
|------------|---------------------|---------------|----------------------|-----------------|  
|**Get-Location**|**gl**|**pwd**|**pwd**|Возвращает текущий узел.|  
|**Set-Location**|**sl**|**cd, chdir**|**cd, chdir**|Изменяет текущий узел.|  
|**Get-ChildItem**|**gci**|**dir**|**ls**|Перечисляет объекты, хранящиеся в текущем узле.|  
|**Get-Item**|**gi**|||Возвращает свойства текущего элемента.|  
|**Rename-Item**|**rni**|**rn**|**ren**|Переименовывает объект.|  
|**Remove-Item**|**ri**|**del, rd**|**rm, rmdir**|Удаляет объект.|  
  
> [!IMPORTANT]  
>  Некоторые идентификаторы [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (имена объектов) содержат символы, которые не поддерживаются в именах путей Windows PowerShell. Дополнительные сведения об использовании имен, содержащих такие символы, см. в разделе [SQL Server Identifiers in PowerShell](sql-server-identifiers-in-powershell.md).  
  
### <a name="sql-server-information-returned-by-get-childitem"></a>SQL Server сведения, возвращаемые командлетом Get-ChildItem  
 Данные, возвращаемые командлетом **Get-ChildItem** (или его псевдонимами **dir** и **ls** ), зависят от текущего расположения на диске SQLSERVER.  
  
|Положение на пути|Результаты выполнения Get-ChildItem|  
|-------------------|----------------------------|  
|SQLSERVER:\SQL|Возвращает имя локального компьютера. Если соединения с экземплярами компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] на других компьютерах устанавливалось с помощью объектов SMO или инструментария WMI, также будут приведены имена этих компьютеров.|  
|SQLSERVER:\SQL\\*ИмяКомпьютера*|Список экземпляров компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] на компьютере.|  
|SQLSERVER:\SQL\\*ИмяКомпьютера*\\*ИмяЭкземпляра*|Список типов объектов верхнего уровня в экземпляре, таких как «Конечные точки», «Сертификаты» и «Базы данных».|  
|Узел класса объектов, например «Базы данных»|Список объектов этого типа, например список баз данных: master, model, AdventureWorks20008R2.|  
|Узел имени объекта, например AdventureWorks2012|Список типов объектов, содержащихся в этом объекте. Например, для базы данных будет выведен список типов объектов, таких как таблицы и представления.|  
  
 По умолчанию командлет **Get-ChildItem** не выводит системные объекты. Воспользуйтесь параметром *Force* для просмотра таких системных объектов, как объекты в схеме **sys**  
  
### <a name="custom-drives"></a>Определение нестандартных дисков  
 Windows PowerShell позволяет определять виртуальные диски, которые называются дисками PowerShell. Они сопоставляются начальным узлам в указании пути. Обычно они используются в качестве краткой записи для часто используемых путей. Пути диска SQLSERVER: могут оказаться длинными, занимать много места в окне Windows PowerShell и требовать много времени на ввод с клавиатуры. Если планируется выполнить большой объем работы для некоторого узла пути, для него можно определить нестандартный диск Windows PowerShell.  
  
## <a name="use-powershell-cmdlet-aliases"></a>Использование псевдонимов командлетов PowerShell  
 **Использование псевдонима командлета**  
  
-   Вместо полного имени командлета введите более короткий псевдоним или псевдоним, сопоставленный с известной командной строки.  
  
### <a name="alias-example-powershell"></a>Пример псевдонима (PowerShell)  
 Например, чтобы получить список экземпляров [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , доступных путем перехода к папке SQLSERVER:\SQL и запроса списка дочерних элементов для папки, можно использовать один из следующих наборов командлетов или псевдонимов.  
  
```  
## Shows using the full cmdet name.  
Set-Location SQLSERVER:\SQL  
Get-ChildItem  
  
## Shows using canonical aliases.  
sl SQLSERVER:\SQL  
gci  
  
## Shows using command prompt aliases.  
cd SQLSERVER:\SQL  
dir  
  
## Shows using Unix shell aliases.  
cd SQLSERVER:\SQL  
ls  
```  
  
## <a name="use-get-childitem"></a>Использование Get-ChildItem  
 **Получение сведений с помощью командлета Get-ChildItem**  
  
1.  Перейдите на узел, для которого хотите получить список потомков.  
  
2.  Выполните командлет Get-Childitem для получения списка.  
  
### <a name="get-childitem-example-powershell"></a>Пример командлета Get-ChildItem пример (PowerShell)  
 Эти примеры показывают сведения, возвращаемые командлетом Get-ChildItem для различных узлов на пути поставщика SQL Server.  
  
```  
## Return the current computer and any computer  
## to which you have made a SQL or WMI connection.  
Set-Location SQLSERVER:\SQL  
Get-ChildItem  
  
## List the instances of the Database Engine on the local computer.  
  
Set-Location SQLSERVER:\SQL\localhost  
Get-ChildItem  
  
## Lists the categories of objects available in the  
## default instance on the local computer.  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT  
Get-ChildItem  
  
## Lists the databases from the local default instance.  
## The force parameter is used to include the system databases.  
Set-Location SQLSERVER:\SQL\localhost\DEFAULT\Databases  
Get-ChildItem -force  
```  
  
## <a name="create-a-custom-drive"></a>Создание пользовательского диска  
 **Создание и использование пользовательского диска**  
  
1.  Используйте **New-PSDrive** для определения пользовательского диска. Используйте параметр **Root** для указания пути, представляемого именем пользовательского диска.  
  
2.  Используйте имя пользовательского диска в таких командлетах перехода по пути, как **Set-Location**.  
  
### <a name="custom-drive-example-powershell"></a>Пример пользовательского диска (PowerShell)  
 В приведенном ниже примере создается виртуальный диск AWDB, сопоставленный с узлом для развернутой копии образца базы данных AdventureWorks2012. Виртуальный диск затем используется для перехода к таблице в базе данных.  
  
```  
## Create a new virtual drive.  
New-PSDrive -Name AWDB -Root SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012  
  
## Use AWDB: to navigate to a specific table.  
Set-Location AWDB:\Tables\Purchasing.Vendor  
```  
  
## <a name="see-also"></a>См. также:  
 [SQL Server PowerShell, поставщик](sql-server-powershell-provider.md)   
 [Работа с  путями SQL Server PowerShell](work-with-sql-server-powershell-paths.md)   
 [SQL Server PowerShell](sql-server-powershell.md)  
  
  

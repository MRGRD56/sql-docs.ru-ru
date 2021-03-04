---
title: Перемещение путей SQL Server PowerShell
description: Узнайте, как использовать командлеты PowerShell для навигации по структуре пути, которая представляет иерархию объектов, поддерживаемых поставщиком PowerShell.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: ef6e6b71d43c73f92f5e73cc7f5faf84230c2c26
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838014"
---
# <a name="navigate-sql-server-powershell-paths"></a>Перемещение путей SQL Server PowerShell

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Поставщик компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] PowerShell представляет набор объектов в экземпляре SQL Server в структуре, аналогичной пути к файлу. Командлеты Windows PowerShell можно использовать для навигации по пути поставщика и для создания нестандартных дисков, укорачивающих путь, который требуется ввести.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

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
> Некоторые идентификаторы [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (имена объектов) содержат символы, которые не поддерживаются в именах путей Windows PowerShell. Дополнительные сведения об использовании имен, содержащих такие символы, см. в разделе [SQL Server Identifiers in PowerShell](sql-server-identifiers-in-powershell.md).  

## <a name="sql-server-information-returned-by-get-childitem"></a>SQL Server сведения, возвращаемые командлетом Get-ChildItem  

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

- Вместо полного имени командлета введите более короткий псевдоним или псевдоним, сопоставленный с известной командной строки.  

### <a name="alias-example-powershell"></a>Пример псевдонима (PowerShell)  

Например, чтобы получить список экземпляров [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , доступных путем перехода к папке SQLSERVER:\SQL и запроса списка дочерних элементов для папки, можно использовать один из следующих наборов командлетов или псевдонимов.  
  
```powershell  
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
  
```powershell  
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
  
```powershell  
## Create a new virtual drive.  
New-PSDrive -Name AWDB -Root SQLSERVER:\SQL\localhost\DEFAULT\Databases\AdventureWorks2012  
  
## Use AWDB: to navigate to a specific table.  
Set-Location AWDB:\Tables\Purchasing.Vendor  
```
  
## <a name="see-also"></a>См. также:

- [Поставщик SQL Server PowerShell](sql-server-powershell-provider.md)
- [Работа с путями SQL Server PowerShell](work-with-sql-server-powershell-paths.md)
- [SQL Server PowerShell](sql-server-powershell.md)

---
description: Вызов методов
title: Вызов методов | Документация Майкрософт
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- methods [SMO]
- calling methods
- SQL Server Management Objects, method calling
- SMO [SQL Server], method calling
ms.assetid: c88d5c5f-9ff0-4f84-b2b6-24c6b90fa15e
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ef9906590a509f05a6ef23153e273a98ff687d89
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770562"
---
# <a name="calling-methods"></a>Вызов методов
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Методы выполняют специальные задачи, связанные с объектом, например устанавливают **Checkpoint** в базе данных или запрашивают перечисляемый список имен входа для экземпляра [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Методы выполняют операции с объектом. Они могут принимать параметры и часто возвращают значение. Возвращаемое значение может быть простым типом данных, сложным объектом или структурой, содержащей большое число членов.  
  
 Для проверки успешного выполнения метода используется обработка исключений. Дополнительные сведения см. в статье [Handling SMO Exceptions](../../../relational-databases/server-management-objects-smo/create-program/handling-smo-exceptions.md).  
  
## <a name="examples"></a>Примеры  
Чтобы использовать какой-либо из представленных примеров кода, нужно выбрать среду, шаблон и язык программирования, с помощью которых будет создаваться приложение. Дополнительные сведения см.  [в статье Создание проекта Visual C&#35; SMO в Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
 
  
## <a name="using-a-simple-smo-method-in-visual-basic"></a>Использование простого метода SMO на языке Visual Basic  
 В этом примере метод <xref:Microsoft.SqlServer.Management.Smo.Database.Create%2A> не принимает параметры и не возвращает значение.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Define a Database object variable by supplying the parent server and the database name arguments in the constructor.
Dim db As Database
db = New Database(srv, "Test_SMO_Database")
'Call the Create method to create the database on the instance of SQL Server. 
db.Create()
```
  
## <a name="using-a-simple-smo-method-in-visual-c"></a>Использование простого метода SMO на языке Visual C#  
 В этом примере метод <xref:Microsoft.SqlServer.Management.Smo.Database.Create%2A> не принимает параметры и не возвращает значение.  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Define a Database object variable by supplying the parent server and the database name arguments in the constructor.   
Database db;   
db = new Database(srv, "Test_SMO_Database");   
//Call the Create method to create the database on the instance of SQL Server.   
db.Create();   
}
```  
  
## <a name="using-an-smo-method-with-a-parameter-in-visual-basic"></a>Использование метода SMO с параметром на языке Visual Basic  
 Объект <xref:Microsoft.SqlServer.Management.Smo.Table> вызывает метод <xref:Microsoft.SqlServer.Management.Smo.Table.RebuildIndexes%2A>. Этому методу требуется числовой параметр, задающий **FillFactor**.  
  
```VBNET
Dim srv As Server  
srv = New Server  
Dim tb As Table  
tb = srv.Databases("AdventureWorks2012").Tables("Employee", "HumanResources")  
tb.RebuildIndexes(70)  
```  
  
## <a name="using-an-smo-method-with-a-parameter-in-visual-c"></a>Использование метода SMO с параметром на языке Visual C#  
 Объект <xref:Microsoft.SqlServer.Management.Smo.Table> вызывает метод <xref:Microsoft.SqlServer.Management.Smo.Table.RebuildIndexes%2A>. Этому методу требуется числовой параметр, задающий `FillFactor`.  
  
```csharp  
{   
Server srv = default(Server);   
srv = new Server();   
Table tb = default(Table);   
tb = srv.Databases("AdventureWorks2012").Tables("Employee", "HumanResources");   
tb.RebuildIndexes(70);   
}   
```  
  
## <a name="using-an-enumeration-method-that-returns-a-datatable-object-in-visual-basic"></a>Использование метода перечисления, который возвращает объект DataTable, на языке Visual Basic  
 В этом разделе описывается вызов метода перечисления и обработка данных в возвращаемом объекте <xref:System.Data.DataTable>.  
  
 Метод <xref:Microsoft.SqlServer.Management.Smo.Server.EnumCollations%2A> возвращает объект <xref:System.Data.DataTable>, который требует дальнейшего перехода для доступа ко всем имеющимся сведениям о параметрах сортировки в экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```VBNET
'Connect to the local, default instance of SQL Server.  
Dim srv As Server  
srv = New Server  
'Call the EnumCollations method and return collation information to DataTable variable.  
Dim d As DataTable  
'Select the returned data into an array of DataRow.  
d = srv.EnumCollations  
'Iterate through the rows and display collation details for the instance of SQL Server.  
Dim r As DataRow  
Dim c As DataColumn  
For Each r In d.Rows  
    Console.WriteLine("==")  
    For Each c In r.Table.Columns  
        Console.WriteLine(c.ColumnName + " = " + r(c).ToString)  
    Next  
Next  
```  
  
## <a name="using-an-enumeration-method-that-returns-a-datatable-object-in-visual-c"></a>Использование метода перечисления, который возвращает объект DataTable, на языке Visual C#  
 В этом разделе описывается вызов метода перечисления и обработка данных в возвращаемом объекте <xref:System.Data.DataTable>.  
  
 Метод <xref:Microsoft.SqlServer.Management.Smo.Server.EnumCollations%2A> возвращает системный объект <xref:System.Data.DataTable>. Объект <xref:System.Data.DataTable> требует дальнейшего перехода для доступа ко всем имеющимся сведениям о параметрах сортировки в экземпляре [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```csharp  
//Connect to the local, default instance of SQL Server.   
{   
Server srv = default(Server);   
srv = new Server();   
//Call the EnumCollations method and return collation information to DataTable variable.   
DataTable d = default(DataTable);   
//Select the returned data into an array of DataRow.   
d = srv.EnumCollations;   
//Iterate through the rows and display collation details for the instance of SQL Server.   
DataRow r = default(DataRow);   
DataColumn c = default(DataColumn);   
foreach ( r in d.Rows) {   
  Console.WriteLine("=========");   
  foreach ( c in r.Table.Columns) {   
    Console.WriteLine(c.ColumnName + " = " + r(c).ToString);   
  }   
}   
}   
```  
  
## <a name="constructing-an-object-in-visual-basic"></a>Создание объекта на языке Visual Basic  
 Конструктор любого объекта можно вызвать с помощью оператора **New** . Конструктор объектов <xref:Microsoft.SqlServer.Management.Smo.Database> перегружен, и версия конструктора объектов <xref:Microsoft.SqlServer.Management.Smo.Database>, приведенная в этом образце, принимает два параметра: родительский объект <xref:Microsoft.SqlServer.Management.Smo.Server>, которому принадлежит база данных, и строку, представляющую имя новой базы данных.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Declare and define a Database object by supplying the parent server and the database name arguments in the constructor.
Dim d As Database
d = New Database(srv, "Test_SMO_Database")
'Create the database on the instance of SQL Server.
d.Create()
Console.WriteLine(d.Name)
```
  
## <a name="constructing-an-object-in-visual-c"></a>Создание объекта на языке Visual C#  
 Конструктор любого объекта можно вызвать с помощью оператора **New** . Конструктор объектов <xref:Microsoft.SqlServer.Management.Smo.Database> перегружен, и версия конструктора объектов <xref:Microsoft.SqlServer.Management.Smo.Database>, приведенная в этом образце, принимает два параметра: родительский объект <xref:Microsoft.SqlServer.Management.Smo.Server>, которому принадлежит база данных, и строку, представляющую имя новой базы данных.  
  
```csharp  
{   
Server srv;   
srv = new Server();   
Table tb;   
tb = srv.Databases("AdventureWorks2012").Tables("Employee", "HumanResources");   
tb.RebuildIndexes(70);   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Declare and define a Database object by supplying the parent server and the database name arguments in the constructor.   
Database d;   
d = new Database(srv, "Test_SMO_Database");   
//Create the database on the instance of SQL Server.   
d.Create();   
Console.WriteLine(d.Name);   
}  
```  
  
## <a name="copying-an-smo-object-in-visual-basic"></a>Копирование объекта SMO на языке Visual Basic  
 В этом примере кода метод <xref:Microsoft.SqlServer.Management.Common.ServerConnection.Copy%2A> используется для создания копии объекта <xref:Microsoft.SqlServer.Management.Smo.Server>. Объект <xref:Microsoft.SqlServer.Management.Smo.Server> представляет соединение с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```VBNET  
'Connect to the local, default instance of SQL Server.
Dim srv1 As Server
srv1 = New Server()
'Modify the default database and the timeout period for the connection.
srv1.ConnectionContext.DatabaseName = "AdventureWorks2012"
srv1.ConnectionContext.ConnectTimeout = 30
'Make a second connection using a copy of the ConnectionContext property and verify settings.
Dim srv2 As Server
srv2 = New Server(srv1.ConnectionContext.Copy)
Console.WriteLine(srv2.ConnectionContext.ConnectTimeout.ToString)
```
  
## <a name="copying-an-smo-object-in-visual-c"></a>Копирование объекта SMO на языке Visual C#  
 В этом примере кода метод <xref:Microsoft.SqlServer.Management.Common.ServerConnection.Copy%2A> используется для создания копии объекта <xref:Microsoft.SqlServer.Management.Smo.Server>. Объект <xref:Microsoft.SqlServer.Management.Smo.Server> представляет соединение с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv1;   
srv1 = new Server();   
//Modify the default database and the timeout period for the connection.   
srv1.ConnectionContext.DatabaseName = "AdventureWorks2012";   
srv1.ConnectionContext.ConnectTimeout = 30;   
//Make a second connection using a copy of the ConnectionContext property and verify settings.   
Server srv2;   
srv2 = new Server(srv1.ConnectionContext.Copy);   
Console.WriteLine(srv2.ConnectionContext.ConnectTimeout.ToString);   
}  
```  
  
## <a name="monitoring-server-processes-in-visual-basic"></a>Наблюдение за процессами сервера на языке Visual Basic  
 С помощью методов перечисления можно получить сведения о типе текущего состояния экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . В этом примере кода метод <xref:Microsoft.SqlServer.Management.Smo.Server.EnumProcesses%2A> используется для получения сведений о текущих процессах. В нем также показана работа со столбцами и строками в возвращаемом объекте <xref:System.Data.DataTable>.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Call the EnumCollations method and return collation information to DataTable variable.
Dim d As DataTable
'Select the returned data into an array of DataRow.
d = srv.EnumProcesses
'Iterate through the rows and display collation details for the instance of SQL Server.
Dim r As DataRow
Dim c As DataColumn
For Each r In d.Rows
    Console.WriteLine("============================================")
    For Each c In r.Table.Columns
        Console.WriteLine(c.ColumnName + " = " + r(c).ToString)
    Next
Next
```
  
## <a name="monitoring-server-processes-in-visual-c"></a>Наблюдение за процессами сервера на языке Visual C#  
 С помощью методов перечисления можно получить сведения о типе текущего состояния экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . В этом примере кода метод <xref:Microsoft.SqlServer.Management.Smo.Server.EnumProcesses%2A> используется для получения сведений о текущих процессах. В нем также показана работа со столбцами и строками в возвращаемом объекте <xref:System.Data.DataTable>.  
  
```csharp  
//Connect to the local, default instance of SQL Server.   
{   
Server srv = default(Server);   
srv = new Server();   
//Call the EnumCollations method and return collation information to DataTable variable.   
DataTable d = default(DataTable);   
//Select the returned data into an array of DataRow.   
d = srv.EnumProcesses;   
//Iterate through the rows and display collation details for the instance of SQL Server.   
DataRow r = default(DataRow);   
DataColumn c = default(DataColumn);   
foreach ( r in d.Rows) {   
  Console.WriteLine("=====");   
  foreach ( c in r.Table.Columns) {   
    Console.WriteLine(c.ColumnName + " = " + r(c).ToString);   
  }   
}   
}   
```  
  
## <a name="see-also"></a>См. также  
 <xref:Microsoft.SqlServer.Management.Smo.Server>   
 <xref:Microsoft.SqlServer.Management.Common.ServerConnection>  
  
  

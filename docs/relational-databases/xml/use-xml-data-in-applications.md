---
title: Использование данных XML в приложениях | Документация Майкрософт
description: Сведения о параметрах, доступных при работе с типом данных XML в приложениях.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- parameters [XML in SQL Server]
- XML [SQL Server], ADO
- columns [XML in SQL Server], ADO.NET
- ADO [XML in SQL Server]
- columns [XML in SQL Server], SQL Server Native Client
- xml data type [SQL Server], ADO
- SQLNCLI, XML
- xml data type [SQL Server], SQL Server Native Client
- SQL Server Native Client, XML
- ADO.NET [XML in SQL Server]
- XML [SQL Server], ADO.NET
- columns [XML in SQL Server], ADO
- xml data type [SQL Server], ADO.NET
- XML [SQL Server], SQL Server Native Client
ms.assetid: 5dabf7e0-c6df-451d-a070-4661f84607fd
author: rothja
ms.author: jroth
ms.openlocfilehash: 23b91982b037cc5c94055cc76e286099dd299cb0
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487109"
---
# <a name="use-xml-data-in-applications"></a>Использование XML-данных в приложениях
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В этом подразделе описываются параметры, доступные при работе с типом данных **xml** в приложениях. Он содержит следующие сведения:  
  
-   Обработка XML в столбцах типа **xml** с помощью ADO и собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Обработка XML в столбцах типа **xml** с помощью ADO.NET  
  
-   Обработка типа **xml** в параметрах с помощью ADO.NET  
  
## <a name="handling-xml-from-an-xml-type-column-by-using-ado-and-sql-server-native-client"></a>Обработка XML в столбцах типа xml с помощью ADO и собственного клиента SQL Server  
 Чтобы при помощи компонентов MDAC получить доступ к типам и возможностям, появившимся в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], в строке соединения ADO необходимо указать свойство инициализации, DataTypeCompatibility.  
  
 Например, в следующем образце скрипта Visual Basic Scripting Edition (VBScript) выдается результат запроса столбца **, содержащего данные типа** xml `Demographics`, из таблицы `Sales.Store` образца базы данных `AdventureWorks2012` . В частности, запрос производит поиск значения столбца для строки, в которой значение `CustomerID` равно `3`.  
  
```  
Const DS = "MyServer"  
Const DB = "AdventureWorks2012"  
  
Set objConn = CreateObject("ADODB.Connection")  
Set objRs = CreateObject("ADODB.Recordset")  
  
CommandText = "SELECT Demographics" & _  
              " FROM Sales.Store" & _  
              " INNER JOIN Sales.Customer" & _  
              " ON Sales.Store.BusinessEntityID = sales.customer.StoreID" & _  
              " WHERE Sales.Customer.CustomerID = 3" & _  
              " OR Sales.Customer.CustomerID = 4"  
  
ConnectionString = "Provider=SQLNCLI11" & _  
                   ";Data Source=" & DS & _  
                   ";Initial Catalog=" & DB & _  
                   ";Integrated Security=SSPI;" & _  
                   "DataTypeCompatibility=80"  
  
'Connect to the data source.  
objConn.Open ConnectionString  
  
'Execute command through the connection and display  
Set objRs = objConn.Execute(CommandText)  
  
Dim rowcount  
rowcount = 0  
Do While Not objRs.EOF  
   rowcount = rowcount + 1  
   MsgBox "Row " & rowcount & _  
           vbCrLf & vbCrLf & objRs(0)  
   objRs.MoveNext  
Loop  
  
'Clean up.  
objRs.Close  
objConn.Close  
Set objRs = Nothing  
Set objConn = Nothing  
```  
  
 В этом примере показано, как настроить свойство совместимости типов данных. По умолчанию, при использовании собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] это свойство принимает значение 0. Если этому свойству присвоить значение 80, то поставщик собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] представляет столбцы типа данных **xml** и определяемого пользователем типа в виде типов данных [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] . то есть DBTYPE_WSTR и DBTYPE_BYTES соответственно.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должен быть установлен на клиентском компьютере, а в строке соединения он должен быть указан в качестве поставщика данных: "`Provider=SQLNCLI11;...`".  
  
#### <a name="to-test-this-example"></a>Проверка этого примера  
  
1.  Убедитесь, что на компьютере клиента установлен собственный клиент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и доступны компоненты MDAC 2.6.0 или более поздней версии.  
  
     Дополнительные сведения см. в статье [Программирование SQL Server Native Client](../../relational-databases/native-client/sql-server-native-client-programming.md).  
  
2.  Убедитесь, что образец базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] установлен в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Для этого примера требуется образец базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
3.  Скопируйте код, приведенный ранее в этом подразделе, и вставьте его в текстовый редактор или редактор кода. Сохраните файл под именем HandlingXmlDataType.vbs.  
  
4.  Измените скрипт так, как необходимо для вашей установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , и сохраните изменения.  
  
     В частности, вместо `MyServer` необходимо указать либо `(local)` , либо действительное имя сервера, на котором установлен [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
5.  Запустите файл скрипта HandlingXmlDataType.vbs на выполнение.  
  
 Результат должен примерно соответствовать следующему:  
  
```  
Row 1  
  
<StoreSurvey xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/StoreSurvey">  
  <AnnualSales>1500000</AnnualSales>  
  <AnnualRevenue>150000</AnnualRevenue>  
  <BankName>Primary International</BankName>  
  <BusinessType>OS</BusinessType>  
  <YearOpened>1974</YearOpened>  
  <Specialty>Road</Specialty>  
  <SquareFeet>38000</SquareFeet>  
  <Brands>3</Brands>  
  <Internet>DSL</Internet>  
  <NumberEmployees>40</NumberEmployees>  
</StoreSurvey>  
  
Row 2  
  
<StoreSurvey xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/StoreSurvey">  
  <AnnualSales>300000</AnnualSales>  
  <AnnualRevenue>30000</AnnualRevenue>  
  <BankName>United Security</BankName>  
  <BusinessType>BM</BusinessType>  
  <YearOpened>1976</YearOpened>  
  <Specialty>Road</Specialty>  
  <SquareFeet>6000</SquareFeet>  
  <Brands>2</Brands>  
  <Internet>DSL</Internet>  
  <NumberEmployees>5</NumberEmployees>  
</StoreSurvey>  
```  
  
## <a name="handling-xml-from-an-xml-type-column-by-using-adonet"></a>Обработка XML-данных в столбцах типа xml с помощью ADO.NET  
 Для работы со столбцами, содержащими данные типа **xml**, с помощью ADO.NET и [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] можно использовать стандартную реализацию класса **SqlCommand**. Например, столбец с данными типа **xml** и его значения могут быть запрошены таким же образом, как и любые столбцы SQL, с помощью **SqlDataReader**. Но если необходимо работать со столбцами типа **xml** как с XML-данными, то сначала необходимо связать содержимое с типом **XmlReader** .  
  
 Дополнительные сведения и примеры кода см. в разделе "Значения XML-столбцов в модуле чтения данных" в документации по пакету [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnlong](../../includes/dnprdnlong-md.md)] SDK.  
  
## <a name="handling-an-xml-type-column-in-parameters-by-using-adonet"></a>Обработка столбцов типа xml в параметрах с помощью ADO.NET  
 Для обработки XML-данных, переданных в качестве параметра в ADO.NET и [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], значение можно представить в виде экземпляра данных типа **SqlXml** . Специальная обработка не производится, поскольку для столбцов типа **xml** в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] допустимы такие же значения параметров, как и для любых других столбцов и типов данных, например **string** или **integer**.  
  
 Дополнительные сведения и примеры кода см. в разделе "XML-значения в качестве параметров" в документации по пакету [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnlong](../../includes/dnprdnlong-md.md)] SDK.  
  
## <a name="see-also"></a>См. также:  
 [Данные XML (SQL Server)](../../relational-databases/xml/xml-data-sql-server.md)  
  
  

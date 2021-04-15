---
title: Использование результатов FOR XML в коде приложений | Документация Майкрософт
description: Сведения о том, как использовать результаты SQL-запроса с предложениями FOR XML в приложении.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- FOR XML clause, application code usage
- XML [SQL Server], FOR XML clause
- ASP.NET [SQL Server]
- .NET Framework [SQL Server], FOR XML data
- ADO [SQL Server]
- XML data islands [SQL Server]
- data islands [SQL Server]
ms.assetid: 41ae67bd-ece9-49ea-8062-c8d658ab4154
author: rothja
ms.author: jroth
ms.openlocfilehash: fa42eca0883961376dbab3707b61c7d31135459e
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487161"
---
# <a name="use-for-xml-results-in-application-code"></a>Использование результатов FOR XML в коде приложений
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  При помощи предложения FOR XML в SQL-запросах можно получать и даже преобразовывать результаты запросов в формат XML. Если приложение способно обрабатывать XML-данные, эта возможность позволяет выполнять следующие задачи:  
  
-   Запрашивать из таблиц SQL экземпляры значений [данных XML (SQL Server)](../../relational-databases/xml/xml-data-sql-server.md)  
  
-   Применять [TYPE Directive in FOR XML Queries](../../relational-databases/xml/type-directive-in-for-xml-queries.md) для возврата результата запроса, содержащего текст или двоичные данные, в формате XML  
  
 В этом подразделе содержатся примеры, демонстрирующие перечисленные возможности.  
  
## <a name="retrieving-for-xml-data-with-ado-and-xml-data-islands"></a>Получение данных FOR XML при использовании ADO и островов XML-данных  
 Результаты, возвращаемые запросами FOR XML, могут быть сохранены объектом **Stream** ADO или другим объектом, поддерживающим COM-интерфейс **IStream** (например, объектами ASP **Request** и **Response** ).  
  
 Например, следующий код ASP отображает результаты запроса к столбцу Demographics типа **xml** таблицы Sales.Store в образце в базе данных AdventureWorks. В частности, запрос производит поиск значения столбца для строки, в которой значение CustomerID равно 3.  
  
```  
<!-- BeginRecordAndStreamVBS -->  
<%@ LANGUAGE = VBScript %>  
<!-- %  Option Explicit      % -->  
<!-- 'Request.ServerVariables("SERVER_NAME") & ";" & _ -->  
<HTML>  
<HEAD>  
<META NAME="GENERATOR" Content="Microsoft Developer Studio"/>  
<META HTTP-EQUIV="Content-Type" content="text/html"; charset="iso-8859-1">  
<TITLE>FOR XML Query Example</TITLE>  
<STYLE>  
   BODY  
   {  
      FONT-FAMILY: Tahoma;  
      FONT-SIZE: 8pt;  
      OVERFLOW: auto  
   }  
   H3  
   {  
      FONT-FAMILY: Tahoma;  
      FONT-SIZE: 8pt;  
      OVERFLOW: auto  
   }  
</STYLE>  
  
<!-- #include file="adovbs.inc" -->  
<%  
   Response.Write "<H3>Server-side processing</H3>"  
   Response.Write "Page Generated @ " & Now() & "<BR/>"  
   Dim adoConn  
   Set adoConn = Server.CreateObject("ADODB.Connection")  
   Dim sConn  
   sConn = "Provider=SQLOLEDB;Data Source=(local);" & _  
            "Initial Catalog=AdventureWorks;Integrated Security=SSPI;"  
   Response.write "Connect String = " & sConn & "<BR/>"  
   adoConn.ConnectionString = sConn  
   adoConn.CursorLocation = adUseClient  
   adoConn.Open  
   Response.write "ADO Version = " & adoConn.Version & "<BR/>"  
   Response.write "adoConn.State = " & adoConn.State & "<BR/>"  
   Dim adoCmd  
   Set adoCmd = Server.CreateObject("ADODB.Command")  
   Set adoCmd.ActiveConnection = adoConn  
   Dim sQuery  
   sQuery = "<ROOT xmlns:sql='urn:schemas-microsoft-com:xml-sql'><sql:query>SELECT Demographics from Sales.Store WHERE CustomerID = 3 FOR XML AUTO</sql:query></ROOT>"  
   Response.write "Query String = " & sQuery & "<BR/>"  
   Dim adoStreamQuery  
   Set adoStreamQuery = Server.CreateObject("ADODB.Stream")  
   adoStreamQuery.Open  
   adoStreamQuery.WriteText sQuery, adWriteChar  
   adoStreamQuery.Position = 0  
   adoCmd.CommandStream = adoStreamQuery  
   adoCmd.Dialect = "{5D531CB2-E6Ed-11D2-B252-00C04F681B71}"  
   Response.write "Pushing XML to client for processing "  & "<BR/>"  
   adoCmd.Properties("Output Stream") = Response  
   Response.write "<XML ID='MyDataIsle'>"  
   adoCmd.Execute , , 1024  
   Response.write "</XML>"  
%>  
<SCRIPT language="VBScript" For="window" Event="onload">  
   Dim xmlDoc  
   Set xmlDoc = MyDataIsle.XMLDocument  
   Dim root  
   Set root = xmlDoc.documentElement.childNodes.Item(0).childNodes.Item(0).childNodes.Item(0)  
   For each child in root.childNodes  
      dim OutputXML  
      OutputXML = document.all("log").innerHTML  
      document.all("log").innerHTML = OutputXML & "<LI><B>" & child.nodeName &  ":</B>  " & child.Text  & "</LI>"  
   Next  
   MsgBox xmlDoc.xml  
</SCRIPT>  
</HEAD>  
<BODY>  
   <H3>Client-side processing of XML Document MyDataIsle</H3>  
   <UL id=log>  
   </UL>  
</BODY>  
</HTML>  
<!-- EndRecordAndStreamVBS -->  
```  
  
 Этот пример ASP-страницы содержит работающий на стороне сервера скрипт VBScript, выполняющий запрос FOR XML через ADO и возвращающий результат в виде острова XML-данных с именем MyDataIsle. Затем этот остров XML-данных возвращается браузеру и подвергается дополнительной обработке на стороне клиента После этого на стороне клиента дополнительный код скрипта VBScript используется для обработки содержимого острова XML-данных. После этого этапа обработки содержимое отображается в виде результирующего DHTML и открывается окно сообщения, где отображается обработанное содержимое острова XML-данных.  
  
#### <a name="to-test-this-example"></a>Проверка этого примера  
  
1.  Убедитесь, что установлены службы IIS и образец базы данных AdventureWorks для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Данному примеру требуются службы IIS версии 5.0 или более поздней, с включенной поддержкой ASP. Кроме того, необходимо установить образец базы данных AdventureWorks.  
  
2.  Скопируйте код приведенного выше примера и вставьте его в редактор текста или XML. Сохраните файл под именем RetrieveResults.asp в корневой каталог документов IIS. Обычно это каталог «C:\Inetpub\wwwroot».  
  
3.  Откройте страницу ASP в окне браузера, указав следующий URL-адрес. Замените «MyServer» на «localhost» или на действительное имя сервера, на котором установлены службы IIS и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    ```  
    https://MyServer/RetrieveResults.asp  
    ```  
  
 Результаты, сформированные в виде страниц HTML, будут выглядеть следующим образом:  
  
##### <a name="server-side-processing"></a>Обработка на стороне сервера  
 Page Generated \@ 3/11/2006 3:36:02 PM  
  
 Connect String = Provider=SQLOLEDB;Data Source=MyServer;Initial Catalog=AdventureWorks;Integrated Security=SSPI;  
  
 ADO Version = 2.8  
  
 adoConn.State = 1  
  
 Query String = SELECT Demographics from Sales.Store WHERE CustomerID = 3 FOR XML AUTO  
  
 Передача кода XML для обработки на клиенте.  
  
##### <a name="client-side-processing-of-xml-document-mydataisle"></a>Обработка XML-документа MyDataIsle на стороне клиента  
  
-   **AnnualSales:** 1500000  
  
-   **AnnualRevenue:** 150000  
  
-   **BankName:** Primary International  
  
-   **BusinessType:** OS  
  
-   **YearOpened:** 1974  
  
-   **Specialty:** Дорога  
  
-   **SquareFeet:** 38000  
  
-   **Brands:** 3  
  
-   **Internet:** DSL  
  
-   **NumberEmployees:** 40  
  
 Затем окно сообщения VBScript отобразит следующее содержимое исходного, нефильтрованного массива XML-данных, который был возвращен по запросу FOR XML.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Sales.Store>  
    <Demographics>  
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
    </Demographics>  
  </Sales.Store>  
</ROOT>  
```  
  
## <a name="retrieving-for-xml-data-with-aspnet-and-the-net-framework"></a>Получение данных FOR XML при использовании ASP.NET и .NET Framework  
 Как и в предыдущем примере, код ASP.NET отображает результаты запроса столбца Demographics типа **xml** из таблицы Sales.Store в образце базы данных AdventureWorks. Как и в предыдущем случае, запрос ищет значение этого столбца, соответствующее строке, в которой CustomerID равен значению 3.  
  
 В этом примере применяются следующие управляемые интерфейсы прикладных программ (API) Microsoft .NET Framework для возврата и подготовки просмотра результатов запроса FOR XML:  
  
1.  Соединение с SQL Server производится через **SqlConnection** по строке соединения, содержащейся в символьной переменной, strConn.  
  
2.  После этого **SqlDataAdapter** , используя соединение SQL и указанную строку SQL-запроса, выполняет запрос FOR XML.  
  
3.  После выполнения запроса вызывается метод **SqlDataAdapter.Fill** , который передает экземпляр **DataSet,** MyDataSet, чтобы заполнить набор данных результатами выполнения запроса FOR XML.  
  
4.  Затем вызывается метод **DataSet.GetXml** , который возвращает результаты запроса в виде строки, отображаемой на странице HTML, формируемой на сервере.  
  
    ```  
    <%@ Page Language="VB" %>  
    <HTML>  
    <HEAD>  
    <TITLE>FOR XML Query Example</TITLE>  
    <STYLE>  
       BODY  
       {  
          FONT-FAMILY: Tahoma;  
          FONT-SIZE: 8pt;  
          OVERFLOW: auto  
       }  
       H3  
       {  
          FONT-FAMILY: Tahoma;  
          FONT-SIZE: 8pt;  
          OVERFLOW: auto  
       }  
    </STYLE>  
    </HEAD>  
    <BODY>  
    <%  
    Dim s as String  
    s = "<H3>Server-side processing</H3>" & _  
        "Page Generated @ " & Now() & "<BR/>"  
  
    Dim SQL As String   
    SQL = "SELECT Demographics from Sales.Store WHERE CustomerID = 3 FOR XML AUTO"  
  
    Dim strConn As String   
    strConn = "Server=(local);Database=AdventureWorks;Integrated Security=SSPI;"  
  
    Dim MySqlConn As New System.Data.SqlClient.SqlConnection(strConn)  
    Dim MySqlAdapter As New System.Data.SqlClient.SqlDataAdapter(SQL,MySqlConn)  
    Dim MyDataSet As New System.Data.DataSet  
  
    MySqlConn.Open()  
    s = s & "<P>SqlConnection opened.</P>"   
  
    MySqlAdapter.Fill(MyDataSet)  
    s = s & "<P>" & MyDataSet.GetXml  & "</P>"  
  
    MySqlConn.Close()  
    s = s & "<P>SqlConnection closed.</P>"   
  
    Message.InnerHtml=s  
    %>  
    <SPAN id="Message" runat=server />  
    </BODY>  
    </HTML>  
    ```  
  
#### <a name="to-test-this-example"></a>Проверка этого примера  
  
1.  Убедитесь, что установлены службы IIS и образец базы данных AdventureWorks для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Данному примеру требуются службы IIS версии 5.0 или более поздней, с включенной поддержкой ASP.NET. Кроме того, необходимо установить образец базы данных AdventureWorks.  
  
2.  Скопируйте код приведенного выше примера и вставьте его в редактор текста или XML. Сохраните файл под именем RetrieveResults.aspx в корневой каталог документов IIS. Обычно это каталог «C:\Inetpub\wwwroot».  
  
3.  Откройте страницу ASP.NET в окне браузера, указав следующий URL-адрес. Замените «MyServer» на «localhost» или на действительное имя сервера, на котором установлены службы IIS и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    ```  
    https://MyServer/RetrieveResults.aspx  
    ```  
  
 Результаты, сформированные в виде страниц HTML, будут выглядеть следующим образом:  
  
##### <a name="server-side-processing"></a>Обработка на стороне сервера  
  
```  
Page Generated @ 3/11/2006 3:36:02 PM  
  
SqlConnection opened.  
  
<Sales.Store><Demographics><StoreSurvey xmlns="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/StoreSurvey"><AnnualSales>1500000</AnnualSales><AnnualRevenue>150000</AnnualRevenue><BankName>Primary International</BankName><BusinessType>OS</BusinessType><YearOpened>1974</YearOpened><Specialty>Road</Specialty><SquareFeet>38000</SquareFeet><Brands>3</Brands><Internet>DSL</Internet><NumberEmployees>40</NumberEmployees></StoreSurvey></Demographics></Sales.Store>  
  
SqlConnection closed.  
```  
  
> [!NOTE]  
>  Поддержка типа данных **xml** в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] позволяет реализовать требование, согласно которому результат запроса FOR XML должен быть возвращен в качестве типа данных **xml**, а не типов данных string или image, с помощью задания [директивы TYPE](../../relational-databases/xml/type-directive-in-for-xml-queries.md). Если в запросе FOR XML указана директива TYPE, она предоставляет программный доступ к результатам FOR XML, как описано в разделе [Использование XML-данных в приложениях](../../relational-databases/xml/use-xml-data-in-applications.md).  
  
## <a name="see-also"></a>См. также:  
 [FOR XML (SQL Server)](../../relational-databases/xml/for-xml-sql-server.md)  
  
  

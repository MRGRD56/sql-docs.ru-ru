---
description: Пример метода CancelUpdate (VBScript)
title: Пример метода CancelUpdate (VBScript) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- CancelUpdate method [ADO], VBScript example
ms.assetid: c23912f0-1288-4727-8fb4-f643b8811cf7
author: rothja
ms.author: jroth
ms.openlocfilehash: f97b0fd1dfea16e9215d3c8a0f187de174905223
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053325"
---
# <a name="cancelupdate-method-example-vbscript"></a>Пример метода CancelUpdate (VBScript)
> [!IMPORTANT]
>  Начиная с Windows 8 и Windows Server 2012, компоненты RDS больше не включены в операционную систему Windows (Дополнительные сведения см. в статье о совместимости Windows 8 и [Windows server 2012 Cookbook](https://www.microsoft.com/download/details.aspx?id=27416) ). Клиентские компоненты RDS будут удалены в следующей версии Windows. Избегайте использования этого компонента в новых разработках и запланируйте изменение существующих приложений, в которых он применяется. Приложения, использующие RDS, должны переноситься в [службу данных WCF](/dotnet/framework/wcf/).  
  
 Чтобы протестировать этот пример, вырежьте и вставьте этот код между \<Body> \</Body> тегами и в обычном HTML-документе и назовите его **канцелупдатевбс. ASP**. Сценарий ASP определит ваш Интернет сервер. Необходимо изменить имя сервера, чтобы оно отражало собственную настройку. Просто измените значение в строке подключения с MyServer на имя установки SQL Server.  
  
```  
<!-- BeginCancelUpdateVBS -->  
<%@Language=VBScript%>  
  
<%'Option Explicit%>  
<% 'use the following META tag instead of adovbs.inc%>  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
<HTML>  
<HEAD>  
<META NAME="GENERATOR" Content="Microsoft Visual Studio 6.0">  
<TITLE></TITLE>  
</HEAD>  
<BODY>  
<CENTER>  
<H1>Remote Data Service</H1>  
<H2>SubmitChanges and CancelUpdate Methods</H2>  
  
<%  ' to integrate/test this code replace the Server property value and   
    ' the Data Source value in the Connect property with appropriate values%>  
  
<HR>  
<OBJECT ID=RDS classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" HEIGHT=1 WIDTH=1></OBJECT>  
<SCRIPT Language="VBScript">  
  
     'set RDS properties for control just created  
  
    RDS.Server = "https://<%=Request.ServerVariables("SERVER_NAME")%>"  
    RDS.SQL = "Select * from Employees"  
    RDS.Connect = "Provider='sqloledb';Integrated Security='SSPI';Initial Catalog='Northwind';"  
    RDS.Refresh  
</SCRIPT>  
  
<TABLE DATASRC=#RDS>  
<THEAD>  
<TR ID="ColHeaders">  
   <TH>ID</TH>  
   <TH>FName</TH>  
   <TH>LName</TH>  
   <TH>Title</TH>  
   <TH>Hire Date</TH>  
   <TH>Birth Date</TH>  
   <TH>Extension</TH>  
   <TH>Home Phone</TH>  
</TR>  
</THEAD>  
<TBODY>  
<TR>  
   <TD> <INPUT DATAFLD="EmployeeID" size=4> </TD>  
   <TD> <INPUT DATAFLD="FirstName" size=10> </TD>  
   <TD> <INPUT DATAFLD="LastName" size=10> </TD>  
   <TD> <INPUT DATAFLD="Title" size=10> </TD>  
   <TD> <INPUT DATAFLD="HireDate" size=10> </TD>  
   <TD> <INPUT DATAFLD="BirthDate" size=10> </TD>  
   <TD> <INPUT DATAFLD="Extension" size=10> </TD>  
   <TD> <INPUT DATAFLD="HomePhone" size=8> </TD>  
</TR>  
</TBODY>  
</TABLE>  
<HR>  
<INPUT TYPE=button NAME="SubmitChange" VALUE="Submit Changes">  
<INPUT TYPE=button NAME="CancelChange" VALUE="Cancel Update">  
<BR>  
<H4>Alter a current entry on the grid. Move off that Row. <BR>  
Submit the Changes to your DBMS or cancel the updates. </H4>  
</CENTER>  
<SCRIPT Language="VBScript">  
  
Sub SubmitChange_OnClick  
  
    msgbox "Changes will be made"  
    RDS.SubmitChanges     
    RDS.Refresh  
  
End Sub  
  
Sub CancelChange_OnClick  
  
    msgbox "Changes will be cancelled"  
    RDS.CancelUpdate  
    RDS.Refresh  
  
End Sub  
-->  
</SCRIPT>  
  
</BODY>  
</HTML>  
<!-- EndCancelUpdateVBS -->  
```  
  
## <a name="see-also"></a>См. также:  
 [Метод CancelUpdate (ADO)](../ado-api/cancelupdate-method-ado.md)
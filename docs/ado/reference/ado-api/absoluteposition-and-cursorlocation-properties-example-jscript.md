---
description: Примеры свойств примеры AbsolutePosition и CursorLocation (JScript)
title: Примеры свойств примеры AbsolutePosition и CursorLocation (JScript) | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- JScript
helpviewer_keywords:
- AbsolutePosition property [ADO], JScript example
- CursorLocation property [ADO], JScript example
ms.assetid: bff98617-a6ba-4f41-9c5f-915161e3ea31
author: rothja
ms.author: jroth
ms.openlocfilehash: 36c914ecdcf216c3f11f6e4afae3efbfb167c18e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159286"
---
# <a name="absoluteposition-and-cursorlocation-properties-example-jscript"></a>Примеры свойств примеры AbsolutePosition и CursorLocation (JScript)
В этом примере показано, как свойство [примеры AbsolutePosition](./absoluteposition-property-ado.md) может отслеживать ход выполнения цикла, который перечисляет все записи [набора записей](./recordset-object-ado.md). Свойство [CursorLocation](./cursorlocation-property-ado.md) используется для включения свойства **примеры AbsolutePosition** путем установки курсора на клиентский курсор. Вырежьте и вставьте следующий код в Блокнот или другой текстовый редактор и сохраните его как **абсолутепоситионжс. ASP**.  
  
```  
<!-- BeginAbsolutePositionJS -->  
<%@LANGUAGE="JScript" %>  
<%// use this meta tag instead of adojavas.inc%>  
<!--METADATA TYPE="typelib" uuid="00000205-0000-0010-8000-00AA006D2EA4" -->  
  
<html>  
  
<head>  
<title>AbsolutePosition and CursorLocation Properties Example (JScript)</title>  
<style>  
<!--  
BODY {  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;  
   BACKGROUND-COLOR:white;  
   COLOR:black;  
    }  
.thead2 {  
   background-color: #800000;   
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
   color: white;  
   }  
.tbody {   
   text-align: center;  
   background-color: #f7efde;  
   font-family: 'Verdana','Arial','Helvetica',sans-serif;   
   font-size: x-small;  
    }  
-->  
</style>  
</head>  
  
<body>  
<h1>AbsolutePosition and CursorLocation Properties Example (JScript)</h1>  
<%  
    // connection and recordset variables  
    var strCnxn = "Provider='sqloledb';Data Source=" + Request.ServerVariables("SERVER_NAME") + ";" +  
            "Initial Catalog='Northwind';Integrated Security='SSPI';";  
    var rsEmployee = Server.CreateObject("ADODB.Recordset");  
        // display string  
    var strMessage;          
  
    try  
    {  
        // Open a recordset on the Employee table using  
        // a client-side cursor to enable AbsolutePosition property.  
        rsEmployee.CursorLocation = adUseClient;  
        rsEmployee.Open("employees", strCnxn, adOpenStatic, adLockOptimistic, adCmdTable);  
  
        // Write beginning of table to the document.  
        Response.Write('<table border="0" align="center">');  
        Response.Write('<tr class="thead2">');  
        Response.Write("<th>AbsolutePosition</th><th>Name</th><th>Hire Date</th></tr>");  
  
        while (!rsEmployee.EOF)  
        {  
            strMessage = "";  
  
            // Start a new table row.  
            strMessage = '<tr class="tbody">';  
  
            // First column in row contains AbsolutePosition value.  
            strMessage += "<td>" + rsEmployee.AbsolutePosition + " of " + rsEmployee.RecordCount + "</td>"  
  
            // First and last name are in first column.  
            strMessage += "<td>" + rsEmployee.Fields("FirstName") + " ";  
            strMessage += rsEmployee.Fields("LastName") + " " + "</td>";  
  
            // Hire date in second column.  
            strMessage += "<td>" + rsEmployee.Fields("HireDate") + "</td>";  
  
            // End the row.  
            strMessage += "</tr>";  
  
            // Write line to document.  
            Response.Write(strMessage);  
  
            // Get next record.  
            rsEmployee.MoveNext;  
        }  
  
        // Finish writing document.  
        Response.Write("</table>");  
    }  
    catch (e)  
    {  
        Response.Write(e.message);  
    }  
    finally  
    {  
        // 'clean up  
        if (rsEmployee.State == adStateOpen)  
            rsEmployee.Close;  
        rsEmployee = null;  
    }  
%>  
  
</html>  
<!-- EndAbsolutePositionJS -->  
```  
  
## <a name="see-also"></a>См. также:  
 [Свойство примеры AbsolutePosition (ADO)](./absoluteposition-property-ado.md)   
 [Свойство CursorLocation (ADO)](./cursorlocation-property-ado.md)   
 [Объект Recordset (ADO)](./recordset-object-ado.md)
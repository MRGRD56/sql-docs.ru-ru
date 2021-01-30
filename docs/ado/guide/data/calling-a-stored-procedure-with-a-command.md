---
description: Вызов хранимой процедуры с использованием команды
title: Вызов хранимой процедуры с помощью команды | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- calling stored procedures [ADO]
- stored procedures [ADO]
- commands [ADO]
ms.assetid: 685f7652-2271-4ede-b552-2eeb8c756b4c
author: rothja
ms.author: jroth
ms.openlocfilehash: 5c66c4bdd347aaa8d01ab80d3a11ae67ce683b47
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075866"
---
# <a name="calling-a-stored-procedure-with-a-command"></a>Вызов хранимой процедуры с использованием команды
Для вызова хранимой процедуры можно использовать команду. Пример кода в конце этого раздела относится к хранимой процедуре в образце базы данных Northwind, именуемой Кустордерсордерс, которая определяется следующим образом.  
  
```  
CREATE PROCEDURE CustOrdersOrders @CustomerID nchar(5) AS  
SELECT OrderID, OrderDate, RequiredDate, ShippedDate  
FROM Orders  
WHERE CustomerID = @CustomerID  
ORDER BY OrderID  
```  
  
 Дополнительные сведения об определении и вызове хранимых процедур см. в документации по SQL Server.  
  
 Эта хранимая процедура аналогична команде, используемой в [параметрах объекта команды](./command-object-parameters.md). Он принимает параметр идентификатора клиента и возвращает сведения о заказах этого клиента. В следующем примере кода эта хранимая процедура используется в качестве источника для **набора записей** ADO.  
  
 Использование хранимой процедуры позволяет получить доступ к другой возможности ADO: метод **обновления** коллекции **параметров** . С помощью этого метода ADO может автоматически заполнять всю информацию о параметрах, необходимых команде во время выполнения. Использование этой методики приводит к снижению производительности, поскольку ADO должен запрашивать у источника данных сведения о параметрах.  
  
 Существуют другие важные различия между следующим образцом кода и кодом в [параметрах объекта Command](./command-object-parameters.md), где параметры были указаны вручную. Во-первых, этот код не устанавливает свойство **Prepared** в **значение true** , поскольку оно является SQL Server хранимой процедурой и предкомпилировано по определению. Во-вторых, свойство **CommandType** объекта **Command** изменилось на **адкмдсторедпрок** во втором примере, чтобы информировать ADO о том, что команда была хранимой процедурой.  
  
 Наконец, во втором примере при задании значения параметр должен называться индексом, поскольку имя параметра во время разработки может быть неизвестно. Если известно имя параметра, можно задать для нового свойства [NamedParameters](../../reference/ado-api/namedparameters-property-ado.md) объекта **Command** значение true и указать имя свойства. Может возникнуть вопрос, почему первым параметром, упомянутым в хранимой процедуре ( @CustomerID ), является 1, а не 0 ( `objCmd(1) = "ALFKI"` ). Это происходит потому, что параметр 0 содержит возвращаемое значение из SQL Server хранимой процедуры.  
  
```  
'BeginAutoParamCmd  
    On Error GoTo ErrHandler:  
  
    Dim objConn As New ADODB.Connection  
    Dim objCmd As New ADODB.Command  
    Dim objParm1 As New ADODB.Parameter  
    Dim objRs As New ADODB.Recordset  
  
    ' Set CommandText equal to the stored procedure name.  
    objCmd.CommandText = "CustOrdersOrders"  
    objCmd.CommandType = adCmdStoredProc  
  
    ' Connect to the data source.  
    Set objConn = GetNewConnection  
    objCmd.ActiveConnection = objConn  
  
    ' Automatically fill in parameter info from stored procedure.  
    objCmd.Parameters.Refresh  
  
    ' Set the param value.  
    objCmd(1) = "ALFKI"  
  
    ' Execute once and display...  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    ' ...then set new param value, re-execute command, and display.  
    objCmd(1) = "CACTU"  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    'clean up  
    objRs.Close  
    objConn.Close  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
    Set objParm1 = Nothing  
    Exit Sub  
  
ErrHandler:  
    'clean up  
    If objRs.State = adStateOpen Then  
        objRs.Close  
    End If  
  
    If objConn.State = adStateOpen Then  
        objConn.Close  
    End If  
  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
    Set objParm1 = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
'EndAutoParamCmd  
  
'BeginNewConnection  
Private Function GetNewConnection() As ADODB.Connection  
    Dim oCn As New ADODB.Connection  
    Dim sCnStr As String  
  
    sCnStr = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _  
             "Integrated Security='SSPI';Initial Catalog='Northwind';"  
    oCn.Open sCnStr  
  
    If oCn.State = adStateOpen Then  
        Set GetNewConnection = oCn  
    End If  
  
End Function  
'EndNewConnection  
```  
  
## <a name="see-also"></a>См. также:  
 [Статья базы знаний 117500](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/185125)

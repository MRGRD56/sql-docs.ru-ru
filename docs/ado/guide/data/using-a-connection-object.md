---
description: Использование объекта Connection
title: Использование объекта подключения | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [ADO]
ms.assetid: 4b34f971-5699-43e7-9b15-137d334fa66e
author: rothja
ms.author: jroth
ms.openlocfilehash: 41652f73868380d4902c7c6815a7ee53868c0969
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91724865"
---
# <a name="using-a-connection-object"></a>Использование объекта Connection
Перед открытием объекта **соединения** необходимо определить определенные сведения об источнике данных и типе соединения. Большая часть этих сведений удерживается параметром *ConnectionString* [метода Open](../../../ado/reference/ado-api/open-method-ado-connection.md) для объекта **Connection** или [свойством ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) объекта **Connection** . Строка подключения состоит из списка пар "аргумент-значение", разделенных точкой с запятой, со значениями, заключенными в одинарные кавычки. Пример:  
  
```  
Dim sConn As String  
sConn = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _  
             "Initial Catalog='Northwind';Integrated Security='SSPI';"  
```  
  
> [!NOTE]
>  В строке подключения можно также указать имя источника данных ODBC (DSN) или файл связи данных (UDL). Дополнительные сведения об именах DSN см. в разделе [Управление источниками данных](../../../odbc/admin/managing-data-sources.md) в справочнике программиста ODBC. Дополнительные сведения о UDL см. в разделе [Общие сведения об API канала передачи данных](/previous-versions/windows/desktop/ms718102(v=vs.85)) в справочнике по OLE DB программиста.  
  
 Как правило, соединение устанавливается путем вызова метода **Connection. Open** с соответствующей *строкой подключения* в качестве параметра. Пример показан в следующем Visual Basic фрагменте кода:  
  
```  
Dim oConn As ADODB.Connection  
Dim oRs As ADODB.Recordset  
Dim sConn As String  
Dim sSQL as String  
  
' Open a connection.  
Set oConn = New ADODB.Connection  
.Open   
  
' Make a query over the connection.  
sSQL = "SELECT ProductID, ProductName, CategoryID, UnitPrice " & _  
             "FROM Products"  
Set oRs = New ADODB.Recordset  
oRs.Open sSQL, , adOpenStatic, adLockBatchOptimistic, adCmdText  
  
MsgBox oRs.RecordCount  
  
' Close the connection.  
oConn.Close  
Set oConn = Nothing  
  
```  
  
 Здесь **or. Open** принимает переменную объекта **Connection** (*оконн*) в качестве значения ее параметра *ActiveConnection* . Кроме того, свойство **Connection. CursorLocation** предполагает, что значение по умолчанию — **адусесервер**. Сравните это с примером [HelloData](../../../ado/guide/data/hellodata-a-simple-ado-application.md) в предыдущем разделе. Следующая инструкция приведет к ошибкам во время выполнения.  
  
```  
oRs.MarshalOptions = adMarshalModifiedOnly  
' Disconnect the Recordset.  
Set oRs.ActiveConnection = Nothing  
```
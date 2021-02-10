---
description: Пример метода Refresh коллекции Procedures (Visual Basic)
title: Пример метода обновления процедур (Visual Basic) | Документация Майкрософт
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
- Refresh method [ADOX], Visual Basic example
ms.assetid: 499679bd-287b-487d-bdfb-3803abffec1c
author: rothja
ms.author: jroth
ms.openlocfilehash: 289cee12f65511cf64a68837043eacb37d2f2763
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054849"
---
# <a name="procedures-refresh-method-example-vb"></a>Пример метода Refresh коллекции Procedures (Visual Basic)
В следующем коде показано, как обновить коллекцию [процедур](./procedures-collection-adox.md) [каталога](./catalog-object-adox.md). Это необходимо, прежде чем можно будет получить доступ к объектам [процедуры](./procedure-object-adox.md) из **каталога** .  
  
```  
' BeginProceduresRefreshVB  
Sub Main()  
    On Error GoTo ProcedureRefreshError  
  
    Dim cat As New ADOX.Catalog  
  
    ' Open the Catalog  
    cat.ActiveConnection = _  
        "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Refresh the Procedures collection  
    cat.Procedures.Refresh  
  
    'Clean up  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Exit Sub  
  
ProcedureRefreshError:  
    Set cat = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndProceduresRefreshVB  
```  
  
## <a name="see-also"></a>См. также:  
 [Объект каталога (ADOX)](./catalog-object-adox.md)   
 [Коллекция процедур (ADOX)](./procedures-collection-adox.md)   
 [Метод Refresh (ADO)](../ado-api/refresh-method-ado.md)
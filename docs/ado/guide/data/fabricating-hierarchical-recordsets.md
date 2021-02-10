---
description: Составление иерархических наборов записей
title: Упорядочение иерархических наборов записей | Документация Майкрософт
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Recordset fabrication [ADO]
- hierarchical Recordsets [ADO]
- fabricating hierarchical Recordsets [ADO]
- data shaping [ADO], hierarchical Recordsets
ms.assetid: a584e642-a4a3-418e-bc20-3aff81a5625a
author: rothja
ms.author: jroth
ms.openlocfilehash: 836c1e0091166485f68c04226c632e9999038469
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033120"
---
# <a name="fabricating-hierarchical-recordsets"></a>Составление иерархических наборов записей
В следующем примере показано, как создать иерархический набор записей без базового источника данных, используя грамматику формирования данных для определения столбцов для родительских, дочерних и внучатый **наборов записей**.  
  
 Для формирования иерархического **набора записей** необходимо указать [службу формирования данных Майкрософт для OLE DB (поставщик служб ADO)](../appendixes/microsoft-data-shaping-service-for-ole-db-ado-service-provider.md) (мсдаташапе), а в параметре строки подключения метода [Open](../../reference/ado-api/open-method-ado-connection.md) объекта [Connection](../../reference/ado-api/connection-object-ado.md) можно указать значение None для поставщика данных. Дополнительные сведения см. в разделе [необходимые поставщики для формирования данных](./required-providers-for-data-shaping.md).  
  
```  
Dim cn As New ADODB.Connection  
Dim rsCustomers As New ADODB.Recordset  
  
cn.Open "Provider=MSDataShape;Data Provider=NONE;"  
  
strShape = _  
"SHAPE APPEND NEW adInteger AS CustID," & _  
            " NEW adChar(25) AS FirstName," & _  
            " NEW adChar(25) AS LastName," & _  
            " NEW adChar(12) AS SSN," & _  
            " NEW adChar(50) AS Address," & _  
         " ((SHAPE APPEND NEW adChar(80) AS VIN_NO," & _  
                        " NEW adInteger AS CustID," & _  
                        " NEW adChar(20) AS BodyColor, " & _  
                     " ((SHAPE APPEND NEW adChar(80) AS VIN_NO," & _  
                                    " NEW adChar(20) AS Make, " & _  
                                    " NEW adChar(20) AS Model," & _  
                                    " NEW adChar(4) AS Year) " & _  
                        " AS VINS RELATE VIN_NO TO VIN_NO))" & _  
            " AS Vehicles RELATE CustID TO CustID) "  
  
rsCustomers.Open strShape, cn, adOpenStatic, adLockOptimistic, -1  
```  
  
 Как только **набор записей** будет создан, он может быть заполнен, обработан или сохранен в файле.  
  
## <a name="see-also"></a>См. также:  
 [Доступ к строкам в иерархическом наборе записей](./accessing-rows-in-a-hierarchical-recordset.md)   
 [Грамматика формальной фигуры](./formal-shape-grammar.md)   
 [Необходимые поставщики для формирования данных](./required-providers-for-data-shaping.md)   
 [Предложение APPEND для фигур](./shape-append-clause.md)   
 [Общие сведения о командах формирования данных](./shape-commands-in-general.md)
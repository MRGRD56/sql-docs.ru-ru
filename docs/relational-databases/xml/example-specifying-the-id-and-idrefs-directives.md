---
title: Пример Указание директив ID и IDREFS | Документация Майкрософт
description: Узнайте, как указать в SQL-запросе директивы ID и IDREFS, которые могут включать ссылки внутри документа.
ms.custom: fresh2019may
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- IDREFS directive
- ID directive
ms.assetid: 99b9f0d8-ecbb-4225-859f-881066c09785
author: rothja
ms.author: jroth
ms.openlocfilehash: 6da50deb87e9fa4b06119b06ca1f701c1075ddc7
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488572"
---
# <a name="example-specifying-the-id-and-idrefs-directives"></a>Пример Указание директив ID и IDREFS

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Атрибут элемента может быть указан в качестве атрибута с типом **ID** , а атрибут **IDREFS** может быть использован для ссылки на него. Этим включаются связи внутри документа, которые похожи на связи первичного и внешнего ключей в реляционных базах данных.  
  
 Этот пример иллюстрирует, как директивы **ID** и **IDREFS** могут быть использованы для создания атрибутов с типами **ID** и **IDREFS** . Так как идентификаторы не могут быть целочисленными, значения ID в этом примере преобразуются. Другими словами, они подвергаются приведению типа. Префиксы используются для значений ID.  
  
 Предположим, что требуется создать следующий XML:  
  
```xml
<Customer CustomerID="C1" SalesOrderIDList=" O11 O22 O33..." >
    <SalesOrder SalesOrderID="O11" OrderDate="..." />  
    <SalesOrder SalesOrderID="O22" OrderDate="..." />  
    <SalesOrder SalesOrderID="O33" OrderDate="..." />  
    ...  
</Customer>  
```  
  
Атрибут `SalesOrderIDList` элемента `<Customer>` является многозначным атрибутом, который ссылается на атрибут `SalesOrderID` элемента `<SalesOrder>`. Для установления этой связи атрибут `SalesOrderID` должен быть объявлен с типом `ID`, а атрибут `SalesOrderIDList` элемента `<Customer>` — с типом `IDREFS`. Так как заказчик может оставить несколько заказов, используется тип `IDREFS` .
  
 Элементы типа **IDREFS** также имеют более одного значения. Поэтому следует использовать отдельные предложения выборки, которые будут повторно использовать одни и те же сведения столбцов тега, родителя и ключевого столбца. Предложение `ORDER BY` обеспечивает отображение последовательностей строк, определяющих значения **IDREFS** , сгруппированных по родительскому элементу.  
  
 Далее запрос, который создает желаемый XML. В запросе используются директивы `ID` и `IDREFS` для перезаписи типов в именах столбцов (`SalesOrder!2!SalesOrderID!ID`, `Customer!1!SalesOrderIDList!IDREFS`).  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT  1 as Tag,  
        0 as Parent,  
        C.CustomerID   [Customer!1!CustomerID],  
        NULL           [Customer!1!SalesOrderIDList!IDREFS],
        NULL           [SalesOrder!2!SalesOrderID!ID],  
        NULL           [SalesOrder!2!OrderDate]  
FROM   Sales.Customer C   

UNION ALL   
SELECT  1 as Tag,  
        0 as Parent,  
        C.CustomerID,  
        'O-'+CAST(SalesOrderID as varchar(10)),   
        NULL,  
        NULL  
FROM   Sales.Customer AS C  
INNER JOIN Sales.SalesOrderHeader AS SOH  
    ON  C.CustomerID = SOH.CustomerID  

UNION ALL  
SELECT 2 as Tag,  
       1 as Parent,  
        C.CustomerID,  
        NULL,  
        'O-'+CAST(SalesOrderID as varchar(10)),  
        OrderDate  
FROM   Sales.Customer AS C  
INNER JOIN Sales.SalesOrderHeader AS SOH  
    ON  C.CustomerID = SOH.CustomerID
ORDER BY [Customer!1!CustomerID] ,
         [SalesOrder!2!SalesOrderID!ID],  
         [Customer!1!SalesOrderIDList!IDREFS]  
FOR XML EXPLICIT;  
```  
  
## <a name="see-also"></a>См. также:  
 [Использование режима EXPLICIT с предложением FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
  
  

---
title: Указание столбца с подстановочным знаком (SQLXML) | Документация Майкрософт
description: Узнайте, как имена столбцов, указанные в качестве подстановочных знаков, влияют на результаты запроса XQuery.
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- names [SQL Server], columns with
ms.assetid: d9551df1-5bb4-4c0b-880a-5bb049834884
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
ms.openlocfilehash: 915bd2824f6e3a706587769413c0f116df859f06
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96125030"
---
# <a name="columns-with-a-name-specified-as-a-wildcard-character"></a>Столбцы с именем, заданным в виде символа-шаблона

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Если указанное имя столбца представляет собой подстановочный знак (\*), содержимое этого столбца вставляется, как будто имя столбца не указано. Если этот столбец не является столбцом типа **xml** , содержимое столбца вставляется в качестве текстового узла так, как это показано в следующем примере:  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT E.BusinessEntityID "@EmpID",   
       FirstName "*",   
       MiddleName "*",   
       LastName "*"  
FROM   HumanResources.Employee AS E  
  INNER JOIN Person.Person AS P  
    ON E.BusinessEntityID = P.BusinessEntityID  
WHERE E.BusinessEntityID=1  
FOR XML PATH;  
```  
  
 Результат:  

```xml
<row EmpID="1">KenJSánchez</row>
```

 Если столбец имеет тип - **xml** , вставляется соответствующее дерево XML. Например, в следующем запросе столбец, содержащий XML-данные, возвращенные в результате запроса на языке XQuery к столбцу Instructions, имеет имя «*».  
  
```sql
SELECT   
       ProductModelID,  
       Name,  
       Instructions.query('declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"  
                /MI:root/MI:Location   
              ') as "*"  
FROM Production.ProductModel  
WHERE ProductModelID=7  
FOR XML PATH;   
```  
  
 Результат. Запрос на языке XQuery возвращает XML-данные, которые вставляются без закрытия элемента.  

```xml
<row>
  <ProductModelID>7</ProductModelID>
  <Name>HL Touring Frame</Name>
  <MI:Location LocationID="10">...</MI:Location>
  <MI:Location LocationID="20">...</MI:Location>
  ...
</row>
```

## <a name="see-also"></a>См. также:  
 [Использование режима PATH совместно с FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md)  
  
  

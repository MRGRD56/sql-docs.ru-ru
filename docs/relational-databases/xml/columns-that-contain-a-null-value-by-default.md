---
title: Столбцы, по умолчанию содержащие значение NULL | Документация Майкрософт
description: Сведения о том, как работать со столбцами, содержащими значение NULL по умолчанию, используя ключевую фразу ELEMENTS XSINIL в SQL
ms.custom: fresh2019may
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- columns [XML in SQL Server], null default value
ms.assetid: 9381c07f-6887-4a62-9730-32661f9aa87c
author: rothja
ms.author: jroth
ms.openlocfilehash: c4336c3da8ecabf39c5b4ae592892cc184e56981
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488995"
---
# <a name="columns-that-contain-a-null-value-by-default"></a>Столбцы, по умолчанию содержащие значение NULL

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

По умолчанию значение NULL в столбце сопоставляется с отсутствием атрибута, узла или элемента. Это поведение по умолчанию можно переопределить с помощью ключевой фразы ELEMENTS XSINIL. Эта фраза запрашивает элементный XML-код. Это означает, что значения NULL явно указаны в возвращаемых результатах. Эти элементы не будут иметь значение.

Фраза ELEMENTS XSINIL показана в следующем примере Transact-SQL SELECT.

```sql
SELECT EmployeeID as "@EmpID",   
       FirstName  as "EmpName/First",   
       MiddleName as "EmpName/Middle",   
       LastName   as "EmpName/Last"  
FROM   HumanResources.Employee E, Person.Contact C  
WHERE  E.EmployeeID = C.ContactID  
  AND  E.EmployeeID=1
FOR XML PATH, ELEMENTS XSINIL;
```  
  
 Ниже показан результат. Обратите внимание, что, если ключевое слово XSINIL не указано, элемент <`Middle`> будет отсутствовать.  
  
```xml
<row xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" EmpID="1">  
  <EmpName>  
    <First>Gustavo</First>  
    <Middle xsi:nil="true" />  
    <Last>Achong</Last>  
  </EmpName>  
</row>  
```  
  
## <a name="see-also"></a>См. также:  
 [Использование режима PATH совместно с FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md)  
  
  

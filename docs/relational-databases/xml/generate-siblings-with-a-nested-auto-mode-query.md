---
title: Формирование одноуровневых элементов с помощью вложенного запроса в режиме AUTO | Документация Майкрософт
description: Сведения о создании элементов XML того же уровня с помощью вложенного запроса в режиме AUTO в качестве альтернативы использованию режима EXPLICIT.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- queries [XML in SQL Server], nested AUTO mode
- nested AUTO mode query
ms.assetid: 748d9899-589d-4420-8048-1258e9e67c20
author: rothja
ms.author: jroth
ms.openlocfilehash: 8edc13548f6a07785cf3523b8cc5c09f5d84bcb5
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107488209"
---
# <a name="generate-siblings-with-a-nested-auto-mode-query"></a>Формирование одноуровневых элементов с помощью вложенного запроса в режиме AUTO
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В следующем примере показано, как создавать элементы с общим родителем при помощи вложенного запроса в режиме AUTO. Единственный способ создать такой XML — использовать режим EXPLICIT. Однако пользоваться этим способом не всегда удобно.  
  
## <a name="example"></a>Пример  
 Этот запрос создает XML, который предоставляет сведения о заказах на продажу. Это включает следующие действия.  
  
-   Данные заголовка заказа на продажу, `SalesOrderID`, `SalesPersonID`и `OrderDate`. [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] сохраняет эти данные в таблице `SalesOrderHeader` .  
  
-   Дополнительные сведения о заказе на продажу. Она включает один или несколько заказанных продуктов, цену за штуку и заказанное количество. Эти сведения хранятся в таблице `SalesOrderDetail` .  
  
-   Сведения о менеджере по продажам. Сведения о менеджере по продажам, принявшем заказ. В таблице `SalesPerson` хранятся идентификаторы `SalesPersonID`. Чтобы найти имя менеджера по продажам, в этом запросе данную таблицу необходимо соединить с таблицей `Employee` .  
  
 Два отдельных запроса `SELECT`, показанных ниже, создают XML-документ с небольшими различиями в формате данных.  
  
 Первый запрос создает XML, в котором <`SalesPerson`> и <`SalesOrderHeader`> появляются как потомки общего родителя <`SalesOrder`>:  
  
```  
SELECT   
      (SELECT top 2 SalesOrderID, SalesPersonID, CustomerID,  
         (select top 3 SalesOrderID, ProductID, OrderQty, UnitPrice  
           from Sales.SalesOrderDetail  
            WHERE  SalesOrderDetail.SalesOrderID =   
                   SalesOrderHeader.SalesOrderID  
            FOR XML AUTO, TYPE)  
        FROM  Sales.SalesOrderHeader  
        WHERE SalesOrderHeader.SalesOrderID = SalesOrder.SalesOrderID  
        for xml auto, type),  
        (SELECT *   
         FROM  (SELECT SalesPersonID, EmployeeID  
              FROM Sales.SalesPerson, HumanResources.Employee  
              WHERE SalesPerson.SalesPersonID = Employee.EmployeeID) As   
                     SalesPerson  
         WHERE  SalesPerson.SalesPersonID = SalesOrder.SalesPersonID  
       FOR XML AUTO, TYPE)  
FROM (SELECT SalesOrderHeader.SalesOrderID, SalesOrderHeader.SalesPersonID  
      FROM Sales.SalesOrderHeader, Sales.SalesPerson  
      WHERE SalesOrderHeader.SalesPersonID = SalesPerson.SalesPersonID  
     ) as SalesOrder  
ORDER BY SalesOrder.SalesOrderID  
FOR XML AUTO, TYPE  
```  
  
 В предшествующем запросе самая внешняя инструкция `SELECT` выполняет следующие действия.  
  
-   Запрашивает набор строк `SalesOrder`, указанный в предложении `FROM`. Результат — XML с одним или несколькими элементами <`SalesOrder`>.  
  
-   Указывает режим `AUTO` и директиву `TYPE` . `AUTO` результат запроса преобразуется в XML, а директива `TYPE` возвращает результат типа **xml** .  
  
-   Содержит две вложенные инструкции `SELECT` , разделенные запятой. Первая вложенная инструкция `SELECT` получает сведения о заказе на продажу, заголовок и дополнительные сведения, а вторая инструкция `SELECT` получает сведения о менеджере по продажам.  
  
    -   Инструкция `SELECT` , получающая `SalesOrderID`, `SalesPersonID`и `CustomerID` , содержит вложенную инструкцию `SELECT ... FOR XML` (с режимом `AUTO` и директивой `TYPE` ), возвращающую подробные данные о заказе на продажу.  
  
 Инструкция `SELECT` , которая получает сведения о менеджере по продажам, запрашивает набор строк `SalesPerson`, созданный в предложении `FROM` . Для работы запросов `FOR XML` необходимо указать имя для анонимного набора строк, создаваемого в предложении `FROM` . В данном случае указано имя `SalesPerson`.  
  
 Частичный результат:  
  
```  
<SalesOrder>  
  <Sales.SalesOrderHeader SalesOrderID="43659" SalesPersonID="279" CustomerID="676">  
    <Sales.SalesOrderDetail SalesOrderID="43659" ProductID="776" OrderQty="1" UnitPrice="2024.9940" />  
    <Sales.SalesOrderDetail SalesOrderID="43659" ProductID="777" OrderQty="3" UnitPrice="2024.9940" />  
    <Sales.SalesOrderDetail SalesOrderID="43659" ProductID="778" OrderQty="1" UnitPrice="2024.9940" />  
  </Sales.SalesOrderHeader>  
  <SalesPerson SalesPersonID="279" EmployeeID="279" />  
</SalesOrder>  
...  
```  
  
 Следующий запрос формирует те же сведения о заказе, за исключением результирующего XML; <`SalesPerson`> представляет собой элемент, имеющий общего родителя с <`SalesOrderDetail`>:  
  
```  
<SalesOrder>  
    <SalesOrderHeader ...>  
          <SalesOrderDetail .../>  
          <SalesOrderDetail .../>  
          ...  
          <SalesPerson .../>  
    </SalesOrderHeader>  
  
</SalesOrder>  
<SalesOrder>  
  ...  
</SalesOrder>  
```  
  
 Запрос является таковым:  
  
```  
SELECT SalesOrderID, SalesPersonID, CustomerID,  
             (select top 3 SalesOrderID, ProductID, OrderQty, UnitPrice  
              from Sales.SalesOrderDetail  
              WHERE SalesOrderDetail.SalesOrderID = SalesOrderHeader.SalesOrderID  
              FOR XML AUTO, TYPE),  
              (SELECT *   
               FROM  (SELECT SalesPersonID, EmployeeID  
                    FROM Sales.SalesPerson, HumanResources.Employee  
                    WHERE SalesPerson.SalesPersonID = Employee.EmployeeID) As SalesPerson  
               WHERE  SalesPerson.SalesPersonID = SalesOrderHeader.SalesPersonID  
         FOR XML AUTO, TYPE)  
FROM Sales.SalesOrderHeader  
WHERE SalesOrderID=43659 or SalesOrderID=43660  
FOR XML AUTO, TYPE  
```  
  
 Результат:  
  
```  
<Sales.SalesOrderHeader SalesOrderID="43659" SalesPersonID="279" CustomerID="676">  
  <Sales.SalesOrderDetail SalesOrderID="43659" ProductID="776" OrderQty="1" UnitPrice="2024.9940" />  
  <Sales.SalesOrderDetail SalesOrderID="43659" ProductID="777" OrderQty="3" UnitPrice="2024.9940" />  
  <Sales.SalesOrderDetail SalesOrderID="43659" ProductID="778" OrderQty="1" UnitPrice="2024.9940" />  
  <SalesPerson SalesPersonID="279" EmployeeID="279" />  
</Sales.SalesOrderHeader>  
<Sales.SalesOrderHeader SalesOrderID="43660" SalesPersonID="279" CustomerID="117">  
  <Sales.SalesOrderDetail SalesOrderID="43660" ProductID="762" OrderQty="1" UnitPrice="419.4589" />  
  <Sales.SalesOrderDetail SalesOrderID="43660" ProductID="758" OrderQty="1" UnitPrice="874.7940" />  
  <SalesPerson SalesPersonID="279" EmployeeID="279" />  
</Sales.SalesOrderHeader>  
```  
  
 Директива `TYPE` возвращает результат запроса как тип данных **xml** , поэтому можно выполнять к результирующему XML запросы с использованием различных методов типа данных **xml** . Дополнительные сведения см. в разделе [Методы типа данных xml](../../t-sql/xml/xml-data-type-methods.md). В приведенном далее запросе обратите внимание на следующее.  
  
-   Предшествующий запрос добавлен в предложении `FROM` . Результат запроса возвращается в виде таблицы. Следует обратить внимание, что добавлен псевдоним `XmlCol` .  
  
-   Предложение `SELECT` определяет запрос XQuery к `XmlCol` , возвращаемому в предложении `FROM` . Метод **query()** типа данных **xml** используется для определения запроса XQuery. Дополнительные сведения см. в разделе [Метод query&#40;&#41;&#40;тип данных xml&#41;](../../t-sql/xml/query-method-xml-data-type.md).  
  
    ```  
    SELECT XmlCol.query('<Root> { /* } </Root>')  
    FROM (  
    SELECT SalesOrderID, SalesPersonID, CustomerID,  
                 (select top 3 SalesOrderID, ProductID, OrderQty, UnitPrice  
                  from Sales.SalesOrderDetail  
                  WHERE SalesOrderDetail.SalesOrderID = SalesOrderHeader.SalesOrderID  
                  FOR XML AUTO, TYPE),  
                  (SELECT *   
                   FROM  (SELECT SalesPersonID, EmployeeID  
                        FROM Sales.SalesPerson, HumanResources.Employee  
                        WHERE SalesPerson.SalesPersonID = Employee.EmployeeID) As SalesPerson  
                   WHERE  SalesPerson.SalesPersonID = SalesOrderHeader.SalesPersonID  
             FOR XML AUTO, TYPE)  
    FROM Sales.SalesOrderHeader  
    WHERE SalesOrderID='43659' or SalesOrderID='43660'  
    FOR XML AUTO, TYPE ) as T(XmlCol)  
    ```  
  
## <a name="see-also"></a>См. также:  
 [Использование вложенных запросов FOR XML](../../relational-databases/xml/use-nested-for-xml-queries.md)  
  
  

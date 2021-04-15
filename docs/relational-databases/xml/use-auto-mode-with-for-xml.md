---
title: Использование режима AUTO для FOR XML | Документация Майкрософт
description: Узнайте, как использовать режим AUTO с предложением FOR XML для возврата результатов запроса в виде вложенных XML-элементов.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- FOR XML clause, AUTO mode
- ELEMENTS option
- FOR XML AUTO mode
- AUTO FOR XML mode
ms.assetid: 7140d656-1d42-4f01-a533-5251429f4450
author: rothja
ms.author: jroth
ms.openlocfilehash: 33782432e7fef693cec31c13e631d1c84826a033
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107487240"
---
# <a name="use-auto-mode-with-for-xml"></a>Использование с AUTO Mode для FOR XML
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Как описано в статье [FOR XML (SQL Server)](../../relational-databases/xml/for-xml-sql-server.md), в режиме AUTO результаты запросов возвращаются в виде вложенных XML-элементов. Такой механизм не обеспечивает достаточное управление структурой XML, формируемой из результатов запроса. Запросы в режиме AUTO полезны, если необходимо формировать простые иерархии. При этом [использование режима EXPLICIT совместно с предложением FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md) и [использование режима PATH совместно с FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md) дают больше контроля и гибкости при выборе формы XML из результатов запроса.  
  
 Каждая таблица в предложении FROM, из которой по крайней мере один столбец присутствует в предложении SELECT, представляется как элемент XML. Столбцы, перечисляемые в предложении SELECT, сопоставляются атрибутам или подчиненным элементам, если в предложении FOR XML указан необязательный аргумент ELEMENTS.  
  
 XML-иерархия (порядок вложенности элементов) в результирующих XML-данных основана на порядке таблиц, определяемых столбцами, которые указаны в предложении SELECT. Поэтому важен порядок, в котором в предложении SELECT указываются имена столбцов. Первая, самая левая идентифицируемая таблица образует верхний элемент в результирующем XML-документе. Вторая слева таблица, идентифицируемая столбцами в инструкции SELECT, образует элемент, подчиненный верхнему элементу и т. д.  
  
 Если столбец, имя которого указывается в предложении SELECT, принадлежит таблице, уже идентифицируемой столбцом, заданным до этого в предложении SELECT, вместо открытия нового уровня иерархии этот столбец добавляется как атрибут уже созданного элемента. Если указан аргумент ELEMENTS, столбец добавляется как атрибут.  
  
 В качестве примера рассмотрим выполнение следующего запроса:  
  
```  
SELECT Cust.CustomerID,   
       OrderHeader.CustomerID,  
       OrderHeader.SalesOrderID,   
       OrderHeader.Status,  
       Cust.CustomerType  
FROM Sales.Customer Cust, Sales.SalesOrderHeader OrderHeader  
WHERE Cust.CustomerID = OrderHeader.CustomerID  
ORDER BY Cust.CustomerID  
FOR XML AUTO  
```  
  
 Частичный результат:  
  
```  
<Cust CustomerID="1" CustomerType="S">  
  <OrderHeader CustomerID="1" SalesOrderID="43860" Status="5" />  
  <OrderHeader CustomerID="1" SalesOrderID="44501" Status="5" />  
  <OrderHeader CustomerID="1" SalesOrderID="45283" Status="5" />  
  <OrderHeader CustomerID="1" SalesOrderID="46042" Status="5" />  
</Cust>  
...  
```  
  
 В предложении SELECT следует отметить следующее.  
  
-   Идентификатор CustomerID ссылается на таблицу «Cust». Поэтому создается элемент <`Cust`>, а CustomerID добавляется как его атрибут.  
  
-   Далее три столбца, OrderHeader.CustomerID, OrderHeader.SaleOrderID и OrderHeader.Status ссылаются на таблицу OrderHeader. Поэтому элемент <`OrderHeader`> добавляется как подчиненный элемент элемента <`Cust`>, и три столбца добавляются как атрибуты <`OrderHeader`>.  
  
-   Далее, столбец Cust.CustomerType вновь ссылается на таблицу «Cust», которая уже была идентифицирована столбцом Cust.CustomerID. Поэтому никакого нового элемента не создается. Вместо этого атрибут CustomerType добавляется к созданному до этого элементу <`Cust`>.  
  
-   Запрос задает псевдонимы для имен таблиц. Эти псевдонимы представляются как имена соответствующих элементов.  
  
-   Для группирования всех дочерних элементов одного родителя необходимо предложение ORDER BY.  
  
 Следующий запрос аналогичен предыдущему, за исключением того, что предложение SELECT задает столбцы в таблице OrderHeader перед столбцами в таблице «Cust». Поэтому первым создается элемент <`OrderHeader`>, а к нему добавляется дочерний элемент <`Cust`>.  
  
```  
select OrderHeader.CustomerID,  
       OrderHeader.SalesOrderID,   
       OrderHeader.Status,  
       Cust.CustomerID,   
       Cust.CustomerType  
from Sales.Customer Cust, Sales.SalesOrderHeader OrderHeader  
where Cust.CustomerID = OrderHeader.CustomerID  
for xml auto  
```  
  
 Частичный результат:  
  
```  
<OrderHeader CustomerID="1" SalesOrderID="43860" Status="5">  
  <Cust CustomerID="1" CustomerType="S" />  
</OrderHeader>  
...  
```  
  
 Если в предложение FOR XML добавляется аргумент ELEMENTS, происходит возврат кода XML, основанного на элементах.  
  
```  
SELECT Cust.CustomerID,   
       OrderHeader.CustomerID,  
       OrderHeader.SalesOrderID,   
       OrderHeader.Status,  
       Cust.CustomerType  
FROM Sales.Customer Cust, Sales.SalesOrderHeader OrderHeader  
WHERE Cust.CustomerID = OrderHeader.CustomerID  
ORDER BY Cust.CustomerID  
FOR XML AUTO, ELEMENTS  
```  
  
 Частичный результат:  
  
```  
<Cust>  
  <CustomerID>1</CustomerID>  
  <CustomerType>S</CustomerType>  
  <OrderHeader>  
    <CustomerID>1</CustomerID>  
    <SalesOrderID>43860</SalesOrderID>  
    <Status>5</Status>  
  </OrderHeader>  
   ...  
</Cust>  
...  
```  
  
 В этом запросе при создании элементов \<Cust> значение идентификатора CustomerID из одной строки сравнивается со значением этого идентификатора из следующей строки, так как CustomerID является первичным ключом таблицы. Если CustomerID не идентифицирован для таблицы как первичный ключ, во всех столбцах (CustomerID и CustomerType в этом запросе) проводится сравнение каждой строки со следующей строкой. Если значения строк различаются, в XML добавляется новый элемент \<Cust>.  
  
 Если сравниваемые столбцы имеют тип данных **text**, **ntext**, **image** или **xml**, при сравнении значений соответствующих строк использование FOR XML предполагает, что значения различаются, поэтому они не сравниваются, даже если могут быть одинаковыми. Причиной этого является то, что сравнение больших объектов не поддерживается. Элементы добавляются в результат для каждой выбранной строки. Обратите внимание на то, что столбцы типа **(n)varchar(max)** и **varbinary(max)** сравниваются.  
  
 Если столбец в предложении SELECT не может быть сопоставлен ни с одной таблицей, идентифицируемой в предложении FROM, как в случае столбца со статистическими выражениями или вычисляемого столбца, столбец добавляется в XML-документ на самый глубокий уровень вложенности, если он присутствует в списке. Если такой столбец является первым столбцом в предложении SELECT, столбец добавляется к верхнему элементу.  
  
 Если в предложении SELECT задается символ-шаблон «*», вложенность определяется таким же образом, как это описано выше, на основании порядка, в котором строки возвращаются механизмом запроса.  
  
## <a name="in-this-section"></a>в этом разделе  
 В следующих подразделах представлены дополнительные сведения о режиме AUTO.  
  
-   [Использование параметра BINARY BASE64](../../relational-databases/xml/use-the-binary-base64-option.md)  
  
-   [Эвристические методы режима AUTO, используемые при формировании возвращаемого XML-кода](../../relational-databases/xml/auto-mode-heuristics-in-shaping-returned-xml.md)  
  
-   [Примеры. Использование режима AUTO](../../relational-databases/xml/examples-using-auto-mode.md)  
  
## <a name="see-also"></a>См. также:  
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [FOR XML (SQL Server)](../../relational-databases/xml/for-xml-sql-server.md)  
  
  

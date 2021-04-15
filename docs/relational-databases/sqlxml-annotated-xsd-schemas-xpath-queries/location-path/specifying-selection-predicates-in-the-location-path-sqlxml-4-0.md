---
title: Задать предикаты выбора в пути расположения (SQLXML)
description: Узнайте, как указать предикаты выбора в выражении пути расположения запроса XPath (SQLXML 4,0) фильтрует набор узлов, к которому выполняется запрос.
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- XPath queries [SQLXML], predicates
- predicates [SQLXML]
- position-based filtering [SQLXML]
- XPath queries [SQLXML], location paths
- filtering [SQLXML]
- location path for XPath query
ms.assetid: dbef4cf4-a89b-4d7e-b72b-4062f7b29a80
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 06bcd85b4f3b25d0a01e5b82e1f6858049772b2e
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491854"
---
# <a name="specifying-selection-predicates-in-the-location-path-sqlxml-40"></a>Указание предикатов выбора в пути доступа (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Предикат фильтрует набор узлов по отношению к оси (аналогично предложению WHERE в инструкции SELECT). Предикат указывается в квадратных скобках. Для каждого узла в фильтруемом наборе узлов выражение предиката вычисляется с этим узлом в качестве узла контекста, а количество узлов в наборе определяет размер контекста. Если для данного узла выражение предиката дает значение TRUE, то узел включается в результирующий набор узлов.  
  
 XPath также позволяет выполнять фильтрацию в зависимости от позиции. Выражение предиката, результатом оценки которого является число, выбирает этот исходный узел. Например, путь доступа `Customer[3]` возвращает третьего клиента. Такие числовые предикаты не поддерживаются. Поддерживаются только предикаты, которые возвращают логический результат.  
  
> [!NOTE]  
>  Дополнительные сведения об ограничениях этой реализации XPath и различиях между ней и спецификацией W3C см. [в статье Введение в использование XPath запросов &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/introduction-to-using-xpath-queries-sqlxml-4-0.md).  
  
## <a name="selection-predicate-example-1"></a>Предикат выбора: Пример 1  
 Следующее выражение XPath (путь расположения) выбирает из текущего контекстного узла все **\<Customer>** дочерние элементы, имеющие атрибут **CustomerID** со значением ALFKI:  
  
```  
/child::Customer[attribute::CustomerID="ALFKI"]  
```  
  
 В этом запросе XPath `child` и `attribute` являются именами осей. `Customer` является тестом узла (значение TRUE `Customer` , если является **\<element node>** , поскольку **\<element>** является типом основного узла для `child` оси). `attribute::CustomerID="ALFKI"` является предикатом. В предикате `attribute` является осью и `CustomerID` является тестом узла (значение true, если **CustomerID** является атрибутом контекстного узла, поскольку **\<attribute>** является типом основного узла оси **атрибутов** ).  
  
 Запрос XPath также можно задать с использованием сокращенного синтаксиса:  
  
```  
/Customer[@CustomerID="ALFKI"]  
```  
  
## <a name="selection-predicate-example-2"></a>Предикат выбора: пример 2  
 Следующее выражение XPath (путь расположения) выбирает из текущего контекстного узла все **\<Order>** внуками, имеющие атрибут **SalesOrderID** со значением 1:  
  
```  
/child::Customer/child::Order[attribute::SalesOrderID="1"]  
```  
  
 В этом выражении XPath `child` и `attribute` являются именами осей. `Customer`, `Order` и `SalesOrderID` являются проверками узла. `attribute::OrderID="1"` является предикатом.  
  
 Запрос XPath также можно задать с использованием сокращенного синтаксиса:  
  
```  
/Customer/Order[@SalesOrderID="1"]  
```  
  
## <a name="selection-predicate-example-3"></a>Предикат выбора: пример 3  
 Следующее выражение XPath (путь расположения) выбирает из текущего контекстного узла все **\<Customer>** дочерние элементы, имеющие один или несколько **\<ContactName>** дочерних элементов:  
  
```  
child::Customer[child::ContactName]  
```  
  
 В этом примере предполагается, что **\<ContactName>** является дочерним элементом **\<Customer>** элемента в XML-документе, который называется *сопоставлением с использованием элементов* в схеме XSD с заметками.  
  
 В этом выражении XPath `child` является именем оси. `Customer` — Это проверка узла (значение TRUE `Customer` , если является **\<element>** узлом, поскольку **\<element>** является типом основного узла для `child` оси). `child::ContactName` является предикатом. В предикате `child` является осью и `ContactName` является тестом узла (значение true, если `ContactName` является **\<element>** узлом).  
  
 Это выражение возвращает только **\<Customer>** дочерние элементы узла контекста, которые имеют **\<ContactName>** дочерние элементы.  
  
 Запрос XPath также можно задать с использованием сокращенного синтаксиса:  
  
```  
Customer[ContactName]  
```  
  
## <a name="selection-predicate-example-4"></a>Предикат выбора: пример 4  
 Следующее выражение XPath выбирает **\<Customer>** дочерние элементы узла контекста, не имеющие **\<ContactName>** дочерних элементов.  
  
```  
child::Customer[not(child::ContactName)]  
```  
  
 В этом примере предполагается, что **\<ContactName>** является дочерним элементом **\<Customer>** элемента в XML-документе, а поле ContactName не является обязательным в базе данных.  
  
 В этом примере `child` является осью. `Customer` является тестом узла (значение TRUE, если `Customer` является \<element> узлом). `not(child::ContactName)` является предикатом. В предикате `child` является осью и `ContactName` является тестом узла (значение true, если `ContactName` является \<element> узлом).  
  
 Запрос XPath также можно задать с использованием сокращенного синтаксиса:  
  
```  
Customer[not(ContactName)]  
```  
  
## <a name="selection-predicate-example-5"></a>Предикат выбора: пример 5  
 Следующее выражение XPath выбирает из текущего контекстного узла все **\<Customer>** дочерние элементы, имеющие атрибут **CustomerID** :  
  
```  
child::Customer[attribute::CustomerID]  
```  
  
 В этом примере `child` — это ось, а `Customer` — Проверка узла (значение true, если `Customer` является \<element> узлом). `attribute::CustomerID` является предикатом. В предикате `attribute` — это ось, а `CustomerID` — ПРЕДИКАТ (true, если `CustomerID` является **\<attribute>** узлом).  
  
 Запрос XPath также можно задать с использованием сокращенного синтаксиса:  
  
```  
Customer[@CustomerID]  
```  
  
## <a name="selection-predicate-example-6"></a>Предикат выбора: пример 6  
 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 включает поддержку запросов XPath, которые содержат в предикате перекрестное произведение, как показано в следующем примере:  
  
```  
Customer[Order/@OrderDate=Order/@ShipDate]  
```  
  
 Этот запрос выбирает всех клиентов с элементом `Order`, для которого `OrderDate` равен `ShipDate``Order`.  
  
## <a name="see-also"></a>См. также  
 [Общие сведения о схемах XSD с заметками &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md)   
 [Форматирование XML на стороне клиента &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/formatting/client-side-xml-formatting-sqlxml-4-0.md)  
  
  

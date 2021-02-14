---
title: Сопоставление типов последовательности | Документация Майкрософт
description: Узнайте, как сопоставить тип последовательности, возвращаемый выражением XQuery, с конкретным типом.
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- sequence type matching [XQuery]
- XQuery, sequence type matching
ms.assetid: 8c56fb69-ca04-4aba-b55a-64ae216c492d
author: rothja
ms.author: jroth
ms.openlocfilehash: 11c0eb693468b980db88995713dc6af99180c373
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352821"
---
# <a name="type-system---sequence-type-matching"></a>Система типов — сопоставление типов последовательности
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Значением выражения языка XQuery всегда является последовательность из нуля или более элементов. Элемент может быть атомарным значением или узлом. Тип элементов последовательности соответствует типу результатов, возвращаемых выражением запроса. Пример:  
  
-   Если значение выражения является атомарным, может понадобиться выяснить, имеет ли оно тип integer, decimal или string.  
  
-   Если значение выражения является XML-узлом, может понадобиться выяснить, является ли оно комментарием, инструкцией по обработке или текстовым узлом.  
  
-   Может возникнуть необходимость выяснить, является ли результат выражения XML-элементом или узлом атрибута с заданным именем и типом.  
  
 Для сопоставления типов элементов последовательности можно использовать логический оператор `instance of`. Дополнительные сведения о `instance of` выражении см. в разделе [SequenceType expressions &#40;XQuery&#41;](../xquery/sequencetype-expressions-xquery.md).  
  
## <a name="comparing-the-atomic-value-type-returned-by-an-expression"></a>Сравнение типов атомарных значений, возвращенных выражением  
 Определить тип значения в последовательности может понадобиться в том случае, если выражение возвращает последовательность атомарных значений. В следующем примере показано использование синтаксиса типа последовательности для определения типа атомарного значения, возвращаемого выражением.  
  
### <a name="example-determining-whether-a-sequence-is-empty"></a>Пример: определение, является ли последовательность пустой  
 Тип последовательности **Empty ()** можно использовать в выражении типа последовательности, чтобы определить, является ли последовательность, возвращаемая указанным выражением, пустой последовательностью.  
  
 В следующем примере XML-схема <позволяет `root` пуст элемент>:  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" nillable="true" type="byte"/>  
</schema>'  
GO  
```  
  
 Теперь, если типизированный XML-экземпляр указывает значение для элемента <`root`>, `instance of empty()` возвращает значение false.  
  
```  
DECLARE @var XML(SC1)  
SET @var = '<root>1</root>'  
-- The following returns False  
SELECT @var.query('data(/root[1]) instance of  empty() ')  
GO  
```  
  
 Если `root` элемент> <пуст в экземпляре, его значение является пустой последовательностью и возвращается значение `instance of empty()` true.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xsi:nil="true" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" />'  
SELECT @var.query('data(/root[1]) instance of  empty() ')  
GO  
```  
  
### <a name="example-determining-the-type-of-an-attribute-value"></a>Пример. определение типа значения атрибута  
 Иногда требуется выяснить тип последовательности, возвращаемой выражением, до начала обработки. Например, узел может быть определен в XML-схеме с типом union. В следующем примере XML-схема в коллекции определяет атрибут `a` как имеющий объединенный тип данных, значение которого может иметь десятичный или строковый тип данных.  
  
```  
-- Drop schema collection if it exists.  
-- DROP XML SCHEMA COLLECTION SC.  
-- GO  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
  <element name="root">  
    <complexType>  
       <sequence/>  
         <attribute name="a">  
            <simpleType>  
               <union memberTypes="decimal string"/>  
            </simpleType>  
         </attribute>  
     </complexType>  
  </element>  
</schema>'  
GO  
```  
  
 Перед обработкой типизированного XML-экземпляра может понадобиться определить тип данных значения атрибута `a`. В следующем примере значение атрибута `a` имеет десятичный тип данных. Следовательно, выражение `, instance of xs:decimal` возвращает True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root a="2.5"/>'  
SELECT @var.query('data((/root/@a)[1]) instance of xs:decimal')  
GO  
```  
  
 Теперь изменим тип значения атрибута `a` на строковый. Выражение `instance of xs:string` возвратит значение True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root a="Hello"/>'  
SELECT @var.query('data((/root/@a)[1]) instance of xs:string')  
GO  
```  
  
### <a name="example-cardinality-in-sequence-expressions"></a>Пример: количество элементов в выражениях последовательности  
 В данном примере показывается влияние количества элементов в выражении последовательности. Следующая схема XML определяет <`root`> элемента, который имеет тип Byte и является нулевым.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" nillable="true" type="byte"/>  
</schema>'  
GO  
```  
  
 В следующем примере выражение `instance of` возвращает значение True, так как результатом выполнения выражения является одиночный элемент типа byte.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root>111</root>'  
SELECT @var.query('data(/root[1]) instance of  xs:byte ')   
GO  
```  
  
 Если <`root`> элемент nil, его значение будет пустой последовательностью. Поэтому выражение `/root[1]` возвращает пустую последовательность. Следовательно, выражение `instance of xs:byte` возвращает значение False. Обратите внимание, что количество элементов по умолчанию в данном случае равно 1.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xsi:nil="true" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"></root>'  
SELECT @var.query('data(/root[1]) instance of  xs:byte ')   
GO  
-- result = false  
```  
  
 Если количество элементов указывается с добавлением признака вхождения (`?`), выражение последовательности возвращает значение True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xsi:nil="true" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"></root>'  
SELECT @var.query('data(/root[1]) instance of  xs:byte? ')   
GO  
-- result = true  
```  
  
 Обратите внимание, что проверка типа в выражении последовательности проходит в два этапа:  
  
1.  Вначале определяется, совпадает ли тип выражения с указанным типом.  
  
2.  При совпадении типов выражений производится проверка совпадения количества возвращенных выражением элементов с указанным значением признака вхождения.  
  
 Если обе проверки дают положительные результаты, выражение `instance of` возвращает значение True.  
  
### <a name="example-querying-against-an-xml-type-column"></a>Пример. запрос к столбцу типа XML  
 В следующем примере запрос задается для столбца инструкций типа **XML** в [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] базе данных. Это типизированный XML-столбец, так как имеется связанная с ним схема. Схема XML определяет целочисленный атрибут `LocationID`. Поэтому в выражении последовательности функция `instance of xs:integer?` возвращает значение true.  
  
```  
SELECT Instructions.query('   
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
data(/AWMI:root[1]/AWMI:Location[1]/@LocationID) instance of xs:integer?') as Result   
FROM Production.ProductModel   
WHERE ProductModelID = 7  
```  
  
## <a name="comparing-the-node-type-returned-by-an-expression"></a>Сравнение типов узлов, возвращаемых выражением  
 Определить тип узла в последовательности может понадобиться в том случае, если выражение возвращает последовательность узлов. В следующем примере показано использование синтаксиса типа последовательности для определения типа узла, возвращаемого выражением. Можно использовать следующие типы последовательности:  
  
-   **Item ()** — соответствует любому элементу в последовательности.  
  
-   **node ()** — определяет, является ли последовательность узлом.  
  
-   **Processing-инструкция ()** определяет, возвращает ли выражение инструкцию по обработке.  
  
-   **comment ()** — определяет, возвращает ли выражение комментарий.  
  
-   **document-node ()** — определяет, возвращает ли выражение узел документа.  
  
 Следующий пример иллюстрирует эти типы последовательностей.  
  
### <a name="example-using-sequence-types"></a>Пример. Использование типов последовательностей  
 В этом примере выполняется несколько запросов к нетипизированной xml-переменной. В этих запросах демонстрируется использование указанных типов последовательности.  
  
```  
DECLARE @var XML  
SET @var = '<?xml-stylesheet href="someValue" type="text/xsl" ?>  
<root>text node  
  <!-- comment 1 -->   
  <a>Data a</a>  
  <!-- comment  2 -->  
</root>'  
```  
  
 В первом запросе выражение возвращает типизированное значение элемента <`a`>. Во втором запросе выражение возвращает элемент <`a`>. Оба результата являются элементами. Поэтому оба запроса возвращают значение True.  
  
```  
SELECT @var.query('data(/root[1]/a[1]) instance of item()')  
SELECT @var.query('/root[1]/a[1] instance of item()')  
```  
  
 Все выражения XQuery в следующих трех запросах возвращают дочерний узел element элемента <`root`>. Поэтому выражение типа последовательности `instance of node()` возвращает значение True, а два других выражения (`instance of text()` и `instance of document-node()`) — значение False.  
  
```  
SELECT @var.query('(/root/*)[1] instance of node()')  
SELECT @var.query('(/root/*)[1] instance of text()')  
SELECT @var.query('(/root/*)[1] instance of document-node()')   
```  
  
 В следующем запросе `instance of document-node()` выражение возвращает значение true, так как родительский `root` элемент> элемента <является узлом документа.  
  
```  
SELECT @var.query('(/root/..)[1] instance of document-node()') -- true  
```  
  
 В следующем запросе выражение получает первый узел XML-экземпляра. Так как он является узлом инструкции по обработке, выражение `instance of processing-instruction()` возвращает значение True.  
  
```  
SELECT @var.query('(/node())[1] instance of processing-instruction()')  
```  
  
### <a name="implementation-limitations"></a>Ограничения реализации  
 Особые ограничения:  
  
-   **документ-node ()** с синтаксисом типа содержимого не поддерживается.  
  
-   синтаксис **инструкции обработки (Name)** не поддерживается.  
  
## <a name="element-tests"></a>Проверки элемента  
 Проверка элемента используется для сопоставления узла элемента, возвращаемого выражением, с узлом элемента с указанным именем и типом. Можно использовать следующие проверки элемента:  
  
```  
element ()  
element(ElementName)  
element(ElementName, ElementType?)   
element(*, ElementType?)  
```  
  
## <a name="attribute-tests"></a>Проверки атрибута  
 В ходе проверки атрибута выясняется, является ли атрибут, возвращенный выражением, узлом. Можно использовать следующие проверки атрибутов.  
  
 `attribute()`  
  
 `attribute(AttributeName)`  
  
 `attribute(AttributeName, AttributeType)`  
  
## <a name="test-examples"></a>Примеры проверки  
 В представленных ниже примерах приведены ситуации, в которых можно использовать проверки элемента и атрибута.  
  
### <a name="example-a"></a>Пример A  
 Следующая XML-схема определяет `CustomerType` сложный тип, в котором `firstName` элементы <> и <`lastName`> являются необязательными. В указанном XML-экземпляре может понадобиться определить, существует ли запись, содержащая фамилию определенного покупателя.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
targetNamespace="myNS" xmlns:ns="myNS">  
  <complexType name="CustomerType">  
     <sequence>  
        <element name="firstName" type="string" minOccurs="0"   
                  nillable="true" />  
        <element name="lastName" type="string" minOccurs="0"/>  
     </sequence>  
  </complexType>  
  <element name="customer" type="ns:CustomerType"/>  
</schema>  
'  
GO  
DECLARE @var XML(SC)  
SET @var = '<x:customer xmlns:x="myNS">  
<firstName>SomeFirstName</firstName>  
<lastName>SomeLastName</lastName>  
</x:customer>'  
```  
  
 В следующем запросе используется `instance of element (firstName)` выражение для определения того, является ли первый дочерний элемент <`customer`> элементом, имя которого <`firstName`>. Если является, выражение возвращает значение True.  
  
```  
SELECT @var.query('declare namespace x="myNS";   
     (/x:customer/*)[1] instance of element (firstName)')  
GO  
```  
  
 Если удалить `firstName` элемент <> из экземпляра, запрос возвратит значение false.  
  
 Также можно использовать следующее:  
  
-   Синтаксис типа последовательности `element(ElementName, ElementType?)`, представленный в следующем запросе. Указанный запрос относится к узлу элемента с именем `firstName` и типом `xs:string`.  
  
    ```  
    SELECT @var.query('declare namespace x="myNS";   
    (/x:customer/*)[1] instance of element (firstName, xs:string?)')  
    ```  
  
-   Синтаксис типа последовательности `element(*, type?)`, представленный в следующем запросе. Указанный запрос извлекает узел элементов, имеющий тип `xs:string`, независимо от его имени.  
  
    ```  
    SELECT @var.query('declare namespace x="myNS"; (/x:customer/*)[1] instance of element (*, xs:string?)')  
    GO  
    ```  
  
### <a name="example-b"></a>Пример Б  
 В следующем примере показано, как определить, является ли узел, возвращенный выражением, элементом узла с указанным именем. Он использует тест **element ()** .  
  
 В следующем примере два элемента <`Customer`> в запрашиваемом экземпляре XML имеют два разных типа, `CustomerType` и `SpecialCustomerType` . Предположим, что необходимо определить тип `Customer` элемента <>, возвращаемого выражением. Следующая коллекция XML-схем определяет типы данных `CustomerType` и `SpecialCustomerType`.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
          targetNamespace="myNS"  xmlns:ns="myNS">  
  <complexType name="CustomerType">  
    <sequence>  
      <element name="firstName" type="string"/>  
      <element name="lastName" type="string"/>  
    </sequence>  
  </complexType>  
  <complexType name="SpecialCustomerType">  
     <complexContent>  
       <extension base="ns:CustomerType">  
        <sequence>  
            <element name="Age" type="int"/>  
        </sequence>  
       </extension>  
     </complexContent>  
    </complexType>  
   <element name="customer" type="ns:CustomerType"/>  
</schema>  
'  
GO  
```  
  
 Эта коллекция схем XML используется для создания типизированной переменной **XML** . Экземпляр XML, назначенный этой переменной, имеет два <`customer`> элементов двух разных типов. Первый элемент имеет тип данных `CustomerType`, а второй элемент — `SpecialCustomerType`.  
  
```  
DECLARE @var XML(SC)  
SET @var = '  
<x:customer xmlns:x="myNS">  
   <firstName>FirstName1</firstName>  
   <lastName>LastName1</lastName>  
</x:customer>  
<x:customer xsi:type="x:SpecialCustomerType" xmlns:x="myNS" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
   <firstName> FirstName2</firstName>  
   <lastName> LastName2</lastName>  
   <Age>21</Age>  
</x:customer>'  
```  
  
 В следующем запросе выражение `instance of element (*, x:SpecialCustomerType ?)` возвращает значение False, так как оно возвращает первый элемент результата, не имеющий тип данных `SpecialCustomerType`.  
  
```  
SELECT @var.query('declare namespace x="myNS";   
    (/x:customer)[1] instance of element (*, x:SpecialCustomerType ?)')  
```  
  
 Если изменить выражение предыдущего запроса и получить второй <`customer`> элемент ( `/x:customer)[2]` ), то `instance of` возвратит значение true.  
  
### <a name="example-c"></a>Пример В  
 В данном примере используется проверка атрибута. Следующая XML-схема определяет сложный тип CustomerType с атрибутами CustomerID и Age. Атрибут Age является необязательным. Для конкретного экземпляра XML может потребоваться определить, существует ли атрибут Age в `customer` элементе <>.  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
       targetNamespace="myNS" xmlns:ns="myNS">  
<complexType name="CustomerType">  
  <sequence>  
     <element name="firstName" type="string" minOccurs="0"   
               nillable="true" />  
     <element name="lastName" type="string" minOccurs="0"/>  
  </sequence>  
  <attribute name="CustomerID" type="integer" use="required" />  
  <attribute name="Age" type="integer" use="optional" />  
 </complexType>  
 <element name="customer" type="ns:CustomerType"/>  
</schema>  
'  
GO  
```  
  
 Результатом следующего запроса является значение True, так как имеется узел атрибута с именем `Age` в экземпляре XML, к которому производится запрос. В данном выражении используется проверка атрибута `attribute(Age)`. Так как атрибуты не упорядочены, то в запросе вначале извлекаются все атрибуты с помощью выражения FLWOR, а затем производится их проверка с помощью выражения `instance of`. В примере сначала создается коллекция XML-схем для создания типизированной переменной **XML** .  
  
```  
DECLARE @var XML(SC)  
SET @var = '<x:customer xmlns:x="myNS" CustomerID="1" Age="22" >  
<firstName>SomeFName</firstName>  
<lastName>SomeLName</lastName>  
</x:customer>'  
SELECT @var.query('declare namespace x="myNS";   
FOR $i in /x:customer/@*  
RETURN  
    IF ($i instance of attribute (Age)) THEN  
        "true"  
        ELSE  
        ()')     
GO  
  
```  
  
 Если из экземпляра удалить необязательный атрибут `Age`, предыдущий запрос возвращает значение False.  
  
 Для проверки атрибута необходимо указать его имя и ввести (`attribute(name,type)`).  
  
```  
SELECT @var.query('declare namespace x="myNS";   
FOR $i in /x:customer/@*  
RETURN  
    IF ($i instance of attribute (Age, xs:integer)) THEN  
        "true"  
        ELSE  
        ()')  
```  
  
 Кроме того, можно указать `attribute(*, type)` синтаксис типа последовательности. Если тип атрибута соответствует указанному типу вне зависимости от его имени, узел атрибута считается удовлетворяющим запросу.  
  
### <a name="implementation-limitations"></a>Ограничения реализации  
 Особые ограничения:  
  
-   В тесте элемента за именем типа должен следовать индикатор вхождения (**?**).  
  
-   **элемент (ElementName, TypeName)** не поддерживается.  
  
-   **элемент ( \* , TypeName)** не поддерживается.  
  
-   **элемент Schema-Element ()** не поддерживается.  
  
-   **атрибут Schema-Attribute (AttributeName)** не поддерживается.  
  
-   Явное выполнение запроса для **xsi: Type** или **xsi: nil** не поддерживается.  
  
## <a name="see-also"></a>См. также:  
 [Введите System &#40;XQuery&#41;](../xquery/type-system-xquery.md)  
  
  

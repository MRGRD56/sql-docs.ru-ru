---
title: Конструкция XML (XQuery) | Документация Майкрософт
description: Узнайте, как создавать XML-структуры в XQuery с помощью прямых и вычисленных конструкторов.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- white space [XQuery]
- computed constructor
- construct XML structures [XQuery]
- constructors [XQuery]
- prolog
- direct constructor [SQL Server]
- XML [SQL Server], construction
- XQuery, XML construction
ms.assetid: a6330b74-4e52-42a4-91ca-3f440b3223cf
author: rothja
ms.author: jroth
ms.openlocfilehash: 1c171dab70a909edb197f5a498d2207bcb2e555b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352320"
---
# <a name="xml-construction-xquery"></a>Построение XML (XQuery)
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  В языке XQuery можно использовать **прямые** и **вычисленные** КОНСТРУКТОРЫ для создания XML-структур в запросе.  
  
> [!NOTE]  
>  Между **прямым** и **вычисленным** конструкторами нет различий.  
  
## <a name="using-direct-constructors"></a>Использование конструкторов Direct  
 При использовании конструкторов direct в построении XML указывается синтаксис, подобный XML. В следующих примерах иллюстрируется построение XML с помощью конструкторов direct.  
  
### <a name="constructing-elements"></a>Построение элементов  
 В используемой нотации XML можно создавать элементы. В следующем примере используется выражение конструктора прямого элемента и создается \<ProductModel> элемент. Созданный элемент содержит три дочерних элемента.  
  
-   Текстовый узел.  
  
-   Два узла Element \<Summary> и \<Features> .  
  
    -   \<Summary>Элемент имеет один дочерний текстовый узел со значением "часть описания".  
  
    -   \<Features>Элемент имеет три дочерних элемента Node,, \<Color> \<Weight> и \<Warranty> . Каждый из этих узлов имеет по одному текстовому дочернему элементу и значения «Red», «25» и «2 years parts and labor» соответственно.  
  
```sql
declare @x xml;  
set @x='';  
select @x.query('<ProductModel ProductModelID="111">;  
This is product model catalog description.  
<Summary>Some description</Summary>  
<Features>  
  <Color>Red</Color>  
  <Weight>25</Weight>  
  <Warranty>2 years parts and labor</Warranty>  
</Features></ProductModel>')  
  
```  
  
 Результирующий XML-документ:  
  
```xml
<ProductModel ProductModelID="111">  
  This is product model catalog description.  
  <Summary>Some description</Summary>  
  <Features>  
    <Color>Red</Color>  
    <Weight>25</Weight>  
    <Warranty>2 years parts and labor</Warranty>  
  </Features>  
</ProductModel>  
```  
  
 Хотя показанное в данном примере построение элементов из неизменных выражений полезно само по себе, настоящая мощь этой функции языка XQuery раскрывается при построении XML-документов с динамическим извлечением данных из базы данных. Для указания выражений запроса можно использовать фигурные скобки. В результирующем XML-документе выражение заменяется своим значением. Например, следующий запрос конструирует `NewRoot` элемент <> с одним дочерним элементом (<`e`>). Значение элемента <`e`> вычисляются путем указания выражения пути внутри фигурных скобок ("{...}").  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
SELECT @x.query('<NewRoot><e> { /root } </e></NewRoot>');  
```  
  
 Фигурные скобки выступают в качестве токенов переключения контекста и переключают запрос с построения XML на оценку запроса. В данном случае производится оценка выражения пути XQuery внутри фигурных скобок, `/root`, и вместо выражения подставляются результаты.  
  
 Результат:  
  
```xml
<NewRoot>  
  <e>  
    <root>5</root>  
  </e>  
</NewRoot>  
```  
  
 Следующий запрос похож на предыдущий. Однако выражение в фигурных скобках указывает функцию **Data ()** для получения атомарного значения `root` элемента <> и присваивает его сконструированному элементу, <`e`>.  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
DECLARE @y xml;  
SET @y = (SELECT @x.query('  
                           <NewRoot>  
                             <e> { data(/root) } </e>  
                           </NewRoot>' ));  
SELECT @y;  
```  
  
 Результат:  
  
```xml
<NewRoot>  
  <e>5</e>  
</NewRoot>  
```  
  
 Если в качестве части текста желательно использовать фигурные скобки вместо токенов переключения контекста, то последние можно изолировать с помощью фигурных скобок «}}» или «{{», как показано в следующем примере:  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
DECLARE @y xml;  
SET @y = (SELECT @x.query('  
<NewRoot> Hello, I can use {{ and  }} as part of my text</NewRoot>'));  
SELECT @y;  
```  
  
 Результат:  
  
```xml
<NewRoot> Hello, I can use { and  } as part of my text</NewRoot>  
```  
  
 Следующий запрос — еще один пример построения элементов с помощью прямого конструктора элементов. Кроме того, значение `FirstLocation` элемента <> получается путем выполнения выражения в фигурных скобках. Выражение запроса возвращает шаги изготовления к первому цеху из столбца Instructions таблицы Production.ProductModel.  
  
```sql
SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        <FirstLocation>  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM Production.ProductModel  
WHERE ProductModelID=7;  
```  
  
 Результат:  
  
```xml
<FirstLocation>  
  <AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">  
      Insert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>.   
  </AWMI:step>  
  <AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">  
      Attach <AWMI:tool>Trim Jig TJ-26</AWMI:tool> to the upper and lower right corners of the aluminum sheet.   
  </AWMI:step>  
   ...  
</FirstLocation>  
```  
  
#### <a name="element-content-in-xml-construction"></a>Содержимое элемента в конструкции XML  
 В следующем примере показано использование выражений при построении содержимого элемента с помощью прямого конструктора элементов. В этом примере прямой конструктор элементов указывает одно выражение. Для этого выражения в результирующем XML-документе создается один текстовый узел.  
  
```sql
declare @x xml;  
set @x='  
<root>  
  <step>This is step 1</step>  
  <step>This is step 2</step>  
  <step>This is step 3</step>  
</root>';  
select @x.query('  
<result>  
 { for $i in /root[1]/step  
    return string($i)  
 }  
</result>');  
  
```  
  
 Последовательность элементарного значения, рассчитанная при оценке выражения, добавляется в текстовый узел с пробелом между соседними элементарными значениями, как это показано в результате. Построенный элемент содержит один дочерний элемент. Это текстовый узел, который хранит значение, показанное в результате.  
  
```xml
<result>This is step 1 This is step 2 This is step 3</result>  
```  
  
 Если вместо одного выражения задать три отдельных выражения, формирующих три текстовых узла, в результирующем XML-документе происходит объединение соседних текстовых узлов в один узел методом сцепления.  
  
```sql
declare @x xml;  
set @x='  
<root>  
  <step>This is step 1</step>  
  <step>This is step 2</step>  
  <step>This is step 3</step>  
</root>';  
select @x.query('  
<result>  
 { string(/root[1]/step[1]) }  
 { string(/root[1]/step[2]) }  
 { string(/root[1]/step[3]) }  
</result>');  
```  
  
 Построенный узел элементов содержит один дочерний элемент. Это текстовый узел, который хранит значение, показанное в результате.  
  
```xml
<result>This is step 1This is step 2This is step 3</result>  
```  
  
### <a name="constructing-attributes"></a>Построение атрибутов  
 При построении элемента с использованием прямого конструктора элементов можно также задать атрибуты элемента с помощью синтаксиса, подобного XML, как показано в этом примере:  
  
```sql
declare @x xml;  
set @x='';  
select @x.query('<ProductModel ProductModelID="111">;  
This is product model catalog description.  
<Summary>Some description</Summary>  
</ProductModel>')  
```  
  
 Результирующий XML-документ:  
  
```xml
<ProductModel ProductModelID="111">  
  This is product model catalog description.  
  <Summary>Some description</Summary>  
</ProductModel>  
```  
  
 Сконструированный элемент <`ProductModel`> имеет атрибут ProductModelID и следующие дочерние узлы:  
  
-   Текстовый узел `This is product model catalog description.`  
  
-   Узел элемента <`Summary`>. Этот узел содержит один дочерний текстовый узел, `Some description`.  
  
 При построении атрибута его значение можно указать с помощью выражения в фигурных скобках. В этом случае результат выражения возвращается в качестве значения атрибута.  
  
 В следующем примере функция **Data ()** не является строго необходимой. Поскольку значение выражения присваивается атрибуту, **данные ()** неявно применяются для получения типизированного значения указанного выражения.  
  
```sql
DECLARE @x xml;  
SET @x='<root>5</root>';  
DECLARE @y xml;  
SET @y = (SELECT @x.query('<NewRoot attr="{ data(/root) }" ></NewRoot>'));  
SELECT @y;  
```  
  
 Результат:  
  
```xml
<NewRoot attr="5" />  
```  
  
 Ниже приведен еще один пример, в котором указаны выражения для построения атрибутов LocationID и SetupHrs. Оценка этих выражений производится по XML-документу, содержащемуся в столбце Instruction. Типизированное значение выражения присваивается атрибутам.  
  
```sql
SELECT Instructions.query('  
    declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        <FirstLocation   
         LocationID="{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
         SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7;  
```  
  
 Частичный результат:  
  
```xml
<FirstLocation LocationID="10" SetupHours="0.5" >  
  <AWMI:step ...   
  </AWMI:step>  
  ...  
</FirstLocation>  
```  
  
#### <a name="implementation-limitations"></a>Ограничения реализации  
 Существуют следующие ограничения:  
  
-   Не поддерживается использование выражений с несколькими атрибутами или смешанными атрибутами (строка и выражение языка XQuery). Например, как показано в следующем запросе, создается XML-документ, в котором `Item` является константой, а значение `5` получено при оценке выражения запроса:  
  
    ```xml
    <a attr="Item 5" />  
    ```  
  
     Следующий запрос возвращает ошибку, потому что в нем строка константы смешана с выражением ({/x}), а это не поддерживается:  
  
    ```sql
    DECLARE @x xml  
    SET @x ='<x>5</x>'  
    SELECT @x.query( '<a attr="Item {/x}"/>' )   
    ```  
  
     В этом случае имеются варианты, перечисленные ниже.  
  
    -   Сформировать значение атрибута путем объединения двух элементарных значений. Эти элементарные значения включены последовательно в значение атрибута с пробелом между элементарными значениями:  
  
        ```sql
        SELECT @x.query( '<a attr="{''Item'', data(/x)}"/>' )   
        ```  
  
         Результат:  
  
        ```xml
        <a attr="Item 5" />  
        ```  
  
    -   Используйте [функцию concat](../xquery/functions-on-string-values-concat.md) , чтобы объединить два строковых аргумента в результирующее значение атрибута:  
  
        ```sql
        SELECT @x.query( '<a attr="{concat(''Item'', /x[1])}"/>' )   
        ```  
  
         В этом случае между двумя строковыми значениями пробелы отсутствуют. Если требуется добавить пробелы между двумя значениями, необходимо задать их явно.  
  
         Результат:  
  
        ```xml
        <a attr="Item5" />  
        ```  
  
-   Не поддерживается использование нескольких выражений в качестве значения атрибута. Например, следующий запрос возвращает ошибку:  
  
    ```sql
    DECLARE @x xml  
    SET @x ='<x>5</x>'  
    SELECT @x.query( '<a attr="{/x}{/x}"/>' )  
    ```  
  
-   Не поддерживаются разнородные последовательности. При попытке присвоения значению атрибута разнородной последовательности произойдет ошибка, как показано в следующем примере. В этом примере в качестве значения атрибута указывается разнородная последовательность, строковый элемент и элемент <`x`>.  
  
    ```sql
    DECLARE @x xml  
    SET @x ='<x>5</x>'  
    select @x.query( '<a attr="{''Item'', /x }" />')  
    ```  
  
     При применении функции **Data ()** запрос работает, поскольку он получает атомарное значение выражения, `/x` которое объединяется со строкой. Ниже приведена последовательность элементарных значений:  
  
    ```sql
    SELECT @x.query( '<a attr="{''Item'', data(/x)}"/>' )   
    ```  
  
     Результат:  
  
    ```xml
    <a attr="Item 5" />  
    ```  
  
-   Узел атрибутов принудительно упорядочивается во время сериализации, а не при статической проверке типов. Например, следующий запрос завершится ошибкой, потому что запрос пытается добавить атрибут после узла без атрибутов.  
  
    ```sql
    select convert(xml, '').query('  
    element x { attribute att { "pass" }, element y { "Element text" }, attribute att2 { "fail" } }  
    ')  
    go  
    ```  
  
     Результат этого запроса будет таким:  
  
    ```  
    XML well-formedness check: Attribute cannot appear outside of element declaration. Rewrite your XQuery so it returns well-formed XML.  
    ```  
  
### <a name="adding-namespaces"></a>Добавление пространств имен  
 При построении XML-документа с помощью прямых конструкторов имена создаваемого элемента и атрибута можно определить с помощью префикса пространства имен. Префикс можно связать с пространством имен следующими способами:  
  
-   с помощью атрибута объявления пространства имен;  
  
-   с помощью предложения WITH XMLNAMESPACES;  
  
-   в прологе запроса на языке XQuery.  
  
#### <a name="using-a-namespace-declaration-attribute-to-add-namespaces"></a>Добавление пространств имен с помощью атрибута объявления пространства имен  
 В следующем примере атрибут объявления пространства имен используется в конструкции элемента <`a`> для объявления пространства имен по умолчанию. Конструкция дочернего элемента <`b`> отменяет объявление пространства имен по умолчанию, объявленного в родительском элементе.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
  <a xmlns="a">  
    <b xmlns=""/>  
  </a>' )   
```  
  
 Результат:  
  
```xml
<a xmlns="a">  
  <b xmlns="" />  
</a>  
```  
  
 Пространству имен можно назначить префикс. Префикс указывается в конструкции элемента <`a`>.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
  <x:a xmlns:x="a">  
    <b/>  
  </x:a>' )  
```  
  
 Результат:  
  
```xml
<x:a xmlns:x="a">  
  <b />  
</x:a>  
```  
  
 Можно отменить объявление пространства имен, используемого по умолчанию в конструкции XML, однако нельзя отменить объявление префикса пространства имен. Следующий запрос возвращает ошибку, поскольку нельзя отменить объявление префикса, как указано в конструкции элемента <`b`>.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
  <x:a xmlns:x="a">  
    <b xmlns:x=""/>  
  </x:a>' )  
```  
  
 Новое построенное пространство имен доступно для использования внутри запроса. Например, следующий запрос объявляет пространство имен при конструировании элемента, <`FirstLocation`> и задает префикс в выражениях для значений атрибутов LocationID и сетуфрс.  
  
```sql
SELECT Instructions.query('  
        <FirstLocation xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"  
         LocationID="{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
         SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7  
```  
  
 Обратите внимание, что при создании префикса пространства имен этим способом произойдет замена любых ранее существовавших объявлений пространства имен для этого префикса. Например, объявление пространства имен, `AWMI="https://someURI"` в прологе запроса, переопределяется объявлением пространства имен в `FirstLocation` элементе <>.  
  
```sql
SELECT Instructions.query('  
declare namespace AWMI="https://someURI";  
        <FirstLocation xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"  
         LocationID="{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
         SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
           { /AWMI:root/AWMI:Location[1]/AWMI:step }  
        </FirstLocation>   
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7  
```  
  
#### <a name="using-a-prolog-to-add-namespaces"></a>Использование пролога для добавления пространств имен  
 В следующем примере показан порядок добавления пространств имен в построенный XML-документ. Пространство имен по умолчанию объявляется в прологе запроса.  
  
```sql
declare @x xml  
set @x ='<x>5</x>'  
select @x.query( '  
           declare default element namespace "a";  
            <a><b xmlns=""/></a>' )  
```  
  
 Обратите внимание, что при построении элемента <`b`> атрибут объявления пространства имен указывается с пустой строкой в качестве значения. Это отменяет объявление пространства имен, произведенное по умолчанию в родительском элементе.  
  

Результат:  

```xml
<a xmlns="a">  
  <b xmlns="" />  
</a>  
```  
  
### <a name="xml-construction-and-white-space-handling"></a>Построение XML и управление пробелами  
 Содержимое элемента в конструкции XML может содержать пробелы. Эти символы обрабатываются следующим образом.  
  
-   Символы пробела в URI пространства имен рассматриваются как тип XSD **anyURI**. В частности, такая обработка заключается в следующем.  
  
    -   Все начальные и конечные пробелы удаляются.  
  
    -   Внутренние последовательности пробельных символов заменяются одиночными пробелами.  
  
-   Символы перехода на новую строку внутри содержимого атрибутов заменяются пробелами. Все остальные пробелы остаются неизменными.  
  
-   Пробелы внутри элементов не изменяются.  
  
 В следующем примере показана обработка пробельных символов в конструкции XML:  
  
```sql
-- line feed is repaced by space.  
declare @x xml  
set @x=''  
select @x.query('  
  
declare namespace myNS="   https://       
 abc/  
xyz  
  
";  
<test attr="    my   
test   attr   
value    " >  
  
<a>  
  
This     is  a  
  
test  
  
</a>  
</test>  
') as XML_Result  
  
```  
  
 Результат:  
  
```xml
-- result  
<test attr="<test attr="    my test   attr  value    "><a>  
  
This     is  a  
  
test  
  
</a></test>  
"><a>  
  
This     is  a  
  
test  
  
</a></test>  
```  
  
### <a name="other-direct-xml-constructors"></a>Другие прямые конструкторы XML  
 В конструкторах для обработки инструкций и комментариев XML используется тот же синтаксис, что и в соответствующих конструкциях XML. Имеется также поддержка вычисляемых конструкторов для текстовых узлов, но они используются в основном в языке XML DML для построения текстовых узлов.  
  
 **Примечание** . Пример использования конструктора явных текстовых узлов см. в конкретном примере в статье [insert &#40;XML DML&#41;](../t-sql/xml/insert-xml-dml.md).  
  
 В следующем запросе созданный XML-документ содержит элемент, два атрибута, комментарий и инструкцию по обработке. Обратите внимание, что перед> <используется запятая `FirstLocation` , так как создается последовательность.  
  
```sql
SELECT Instructions.query('  
  declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   <?myProcessingInstr abc="value" ?>,   
   <FirstLocation   
        WorkCtrID = "{ (/AWMI:root/AWMI:Location[1]/@LocationID)[1] }"  
        SetupHrs = "{ (/AWMI:root/AWMI:Location[1]/@SetupHours)[1] }" >  
       <!-- some comment -->  
       <?myPI some processing instructions ?>  
       { (/AWMI:root/AWMI:Location[1]/AWMI:step) }  
    </FirstLocation>   
') as Result   
FROM Production.ProductModel  
where ProductModelID=7;  
  
```  
  
 Частичный результат:  
  
```xml
<?myProcessingInstr abc="value" ?>  
<FirstLocation WorkCtrID="10" SetupHrs="0.5">  
  <!-- some comment -->  
  <?myPI some processing instructions ?>  
  <AWMI:step xmlns:AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">I  
  nsert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>.   
  </AWMI:step>  
    ...  
</FirstLocation>  
  
```  
  
## <a name="using-computed-constructors"></a>Использование вычисляемых конструкторов  
 . В данном случае указываются ключевые слова, которые идентифицируют тип узла, подлежащий построению. Поддерживается использование только следующих ключевых слов:  
  
-   element  
  
-   Атрибут  
  
-   text  
  
 В узлах элементов и атрибутов эти ключевые слова используются перед именем узла и перед заключенным в фигурные скобки выражением, которое формирует содержимое этого узла. В приведенном ниже примере создается этот XML-документ:  
  
```xml
<root>  
  <ProductModel PID="5">Some text <summary>Some Summary</summary></ProductModel>  
</root>  
```  
  
 Этот запрос использует вычисляемые конструкторы для формирования XML-документа:  
  
```sql
declare @x xml  
set @x=''  
select @x.query('element root   
               {   
                  element ProductModel  
     {  
attribute PID { 5 },  
text{"Some text "},  
    element summary { "Some Summary" }  
 }  
               } ')  
  
```  
  
 Выражение запроса может быть задано выражением, формирующим содержимое узла.  
  
```sql
declare @x xml  
set @x='<a attr="5"><b>some summary</b></a>'  
select @x.query('element root   
               {   
                  element ProductModel  
     {  
attribute PID { /a/@attr },  
text{"Some text "},  
    element summary { /a/b }  
 }  
               } ')  
```  
  
 Обратите внимание, что вычисляемые конструкторы элементов и атрибутов, определенные в спецификации языка XQuery, позволяют вычислять имена узлов. При использовании прямых конструкторов в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] имена узлов, такие как element и attribute, должны быть указаны в виде постоянных литералов. Таким образом, различий между прямыми конструкторами и вычисляемыми конструкторами элементов и атрибутов нет.  
  
 В следующем примере содержимое для созданных узлов получается из инструкций по производству XML, хранящихся в столбце Instructions типа данных **XML** в таблице ProductModel.  
  
```sql
SELECT Instructions.query('  
  declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   element FirstLocation   
     {  
        attribute LocationID { (/AWMI:root/AWMI:Location[1]/@LocationID)[1] },  
        element   AllTheSteps { /AWMI:root/AWMI:Location[1]/AWMI:step }  
     }  
') as Result   
FROM  Production.ProductModel  
where ProductModelID=7  
```  
  
 Частичный результат:  
  
```xml
<FirstLocation LocationID="10">  
  <AllTheSteps>  
    <AWMI:step> ... </AWMI:step>  
    <AWMI:step> ... </AWMI:step>  
    ...  
  </AllTheSteps>  
</FirstLocation>    
```  
  
## <a name="additional-implementation-limitations"></a>Дополнительные ограничения реализации  
 Вычисляемые конструкторы атрибутов не могут использоваться для объявления нового пространства имен. Кроме того, в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] не поддерживаются следующие вычисляемые конструкторы:  
  
-   вычисляемые конструкторы узлов документов;  
  
-   вычисляемые конструкторы инструкций обработки;  
  
-   вычисляемые конструкторы комментариев.  
  
## <a name="see-also"></a>См. также:  
 [Выражения языка XQuery](../xquery/xquery-expressions.md)  
  
  

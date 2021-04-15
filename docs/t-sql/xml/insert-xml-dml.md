---
description: insert (XML DML)
title: insert (XML DML)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- XML DML [SQL Server]
- inserting nodes
- insert keyword [XML DML]
- insert XML DML statement
ms.assetid: 0c95c2b3-5cc2-4c38-9e25-86493096c442
author: rothja
ms.author: jroth
ms.openlocfilehash: 6f1900598b70f656e7ae2441840fce8988d66d0a
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491301"
---
# <a name="insert-xml-dml"></a>insert (XML DML)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Вставляет один или несколько узлов, идентифицируемых выражением *Expression1*, в качестве дочерних узлов или одноуровневых элементов с общим родителем для узла, идентифицируемого выражением *Expression2*.  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql 
insert Expression1 (  
{AS first | AS last} INTO | AFTER | BEFORE  
Expression2  
)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *Expression1*  
 Идентифицирует один или несколько вставляемых узлов. Это может быть постоянный экземпляр XML; ссылка на типизированный тип данных XML той же коллекции XML-схем, к которой применяется метод изменения; нетипизированный тип данных XML, использующий изолированную функцию **sql:column()** /**sql:variable()** ; или выражение XQuery. Результатом выражения может быть узел, в том числе текстовый, или упорядоченная последовательность узлов. Корневой узел (/) не может быть результатом выражения. Если результатом выражения является значение или последовательность значений, то такие значения вставляются в качестве одиночного текстового узла, в котором все значения разделены пробелами. Если несколько узлов указаны как постоянные, они заключаются в скобки и разделяются запятыми. Разнородные последовательности, например, последовательность элементов, атрибутов или значений, вставлять нельзя. Если результатом выражения *Expression1* является пустая последовательность, вставка не выполняется, и никакие ошибки не возвращаются.  
  
 into  
 Узлы, определяемые выражением *Expression1*, вставляются в качестве прямых потомков (дочерних узлов) узла, определяемого выражением *Expression2*. Если у узла в выражении *Expression2* уже есть один или более дочерних узлов, то необходимо использовать ключевые слова **as first** или **as last**, чтобы указать, где следует добавить новый узел. Они означают, соответственно, добавление в начало или в конец списка потомков. Ключевые слова **as first** и **as last** при вставке атрибутов игнорируются.  
  
 after  
 Узлы, определяемые выражением *Expression1*, вставляются в качестве элементов с общим родителем непосредственно после узла, определяемого выражением *Expression2*. При вставке атрибутов не может использоваться ключевое слово **after**. Например, его нельзя использовать для вставки конструктора атрибута или для возвращения атрибута из запроса XQuery.  
  
 before  
 Узлы, определяемые выражением *Expression1*, вставляются в качестве элементов с общим родителем непосредственно перед узлом, определяемым выражением *Expression2*. При вставке атрибутов не может использоваться ключевое слово **before**. Например, его нельзя использовать для вставки конструктора атрибута или для возвращения атрибута из запроса XQuery.  
  
 *Expression2*  
 Идентифицирует узел. Вставка узлов, определяемых выражением *Expression1*, выполняется относительно узла, определяемого выражением *Expression2*. Это может быть выражение XQuery, возвращающее ссылку на узел, существующий в документе, к которому в данный момент производится обращение. Если возвращается более одного узла, то операция вставки завершается неудачно. Если результатом выражения *Expression2* является пустая последовательность, то вставка не производится и ошибки не возвращаются. Если выражение *Expression2* статически не является одноэлементным множеством, то возвращается статическая ошибка. Выражение *Expression2* не может представлять собой инструкцию по обработке, комментарий или атрибут. Обратите внимание, что выражение *Expression2* должно представлять собой ссылку на существующий узел в документе, а не на конструируемый узел.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-inserting-element-nodes-into-the-document"></a>A. Вставка узлов элементов в документ  
 В следующем примере показана вставка элементов в документ. Сначала XML-документ присваивается переменной типа **xml**. Затем с помощью нескольких XML-инструкций DML **insert** демонстрируется вставка узлов элементов в документ. После каждой операции вставки с помощью инструкции SELECT отображается результат.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;         
SET @myDoc = '<Root>         
    <ProductDescription ProductID="1" ProductName="Road Bike">         
        <Features>         
        </Features>         
    </ProductDescription>         
</Root>'  ;       
SELECT @myDoc;     
-- insert first feature child (no need to specify as first or as last)         
SET @myDoc.modify('         
INSERT <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>   
INTO (/Root/ProductDescription/Features)[1]') ;  
SELECT @myDoc ;        
-- insert second feature. We want this to be the first in sequence so use 'as first'         
SET @myDoc.modify('         
INSERT <Warranty>1 year parts and labor</Warranty>          
AS first         
INTO (/Root/ProductDescription/Features)[1]         
')  ;       
SELECT @myDoc  ;       
-- insert third feature child. This one is the last child of <Features> so use 'as last'         
SELECT @myDoc         
SET @myDoc.modify('         
INSERT <Material>Aluminium</Material>          
AS last         
INTO (/Root/ProductDescription/Features)[1]         
')         
SELECT @myDoc ;        
-- Add fourth feature - this time as a sibling (and not a child)         
-- 'after' keyword is used (instead of as first or as last child)         
SELECT @myDoc  ;       
SET @myDoc.modify('         
INSERT <BikeFrame>Strong long lasting</BikeFrame>   
AFTER (/Root/ProductDescription/Features/Material)[1]         
')  ;       
SELECT @myDoc;  
GO  
```  
  
 Заметьте, что в разнообразных выражениях путей в данном примере в качестве требования к статическому вводу указывается «[1]». Это позволяет гарантированно указать отдельный целевой узел.  
  
### <a name="b-inserting-multiple-elements-into-the-document"></a>Б. Вставка нескольких элементов в документ  
 В следующем примере документу сначала назначается переменная типа **xml**. Затем последовательность из двух элементов, представляющих функции продукта, назначается второй переменной типа **xml**. Эта последовательность затем вставляется в первую переменную.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc = N'<Root>             
<ProductDescription ProductID="1" ProductName="Road Bike">             
    <Features> </Features>             
</ProductDescription>             
</Root>';  
DECLARE @newFeatures xml;  
SET @newFeatures = N'<Warranty>1 year parts and labor</Warranty>            
<Maintenance>3 year parts and labor extended maintenance is available</Maintenance>';           
-- insert new features from specified variable            
SET @myDoc.modify('             
insert sql:variable("@newFeatures")             
into (/Root/ProductDescription/Features)[1] ')             
SELECT @myDoc;  
GO  
```  
  
### <a name="c-inserting-attributes-into-a-document"></a>В. Вставка атрибутов в документ  
 В следующем примере показана вставка атрибутов в документ. Сначала документ заносится в переменную типа **xml**. Затем используется серия XML-инструкций DML **insert** для вставки атрибутов в документ. После каждой операции вставки с помощью инструкции SELECT отображается результат.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;            
SET @myDoc =   
'<Root>             
    <Location LocationID="10" >             
        <step>Manufacturing step 1 at this work center</step>             
        <step>Manufacturing step 2 at this work center</step>             
    </Location>             
</Root>' ;  
SELECT @myDoc;          
-- insert LaborHours attribute             
SET @myDoc.modify('             
insert attribute LaborHours {".5" }             
into (/Root/Location[@LocationID=10])[1] ');           
SELECT @myDoc;          
-- insert MachineHours attribute but its value is retrived from a sql variable @Hrs             
DECLARE @Hrs FLOAT;            
SET @Hrs =.2;          
SET @myDoc.modify('             
insert attribute MachineHours {sql:variable("@Hrs") }             
into   (/Root/Location[@LocationID=10])[1] ');            
SELECT @myDoc;             
-- insert sequence of attribute nodes (note the use of ',' and ()              
-- around the attributes.             
SET @myDoc.modify('             
insert (              
           attribute SetupHours {".5" },             
           attribute SomeOtherAtt {".2"}             
        )             
INTO (/Root/Location[@LocationID=10])[1] ');             
SELECT @myDoc;  
GO  
```  
  
### <a name="d-inserting-a-comment-node"></a>Г. Вставка узла с комментарием  
 В этом запросе XML-документ сначала присваивается переменной типа **xml**. Затем для вставки узла с комментарием после первого элемента <`step`> используется язык XML DML.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;             
SET @myDoc =   
'<Root>             
    <Location LocationID="10" >             
        <step>Manufacturing step 1 at this work center</step>             
        <step>Manufacturing step 2 at this work center</step>             
    </Location>             
</Root>' ;           
SELECT @myDoc;             
SET @myDoc.modify('             
insert <!-- some comment -->             
after (/Root/Location[@LocationID=10]/step[1])[1] ');            
SELECT @myDoc;  
GO  
```  
  
### <a name="e-inserting-a-processing-instruction"></a>Д. Вставка инструкции по обработке  
 В этом запросе XML-документ сначала присваивается переменной типа **xml**. Затем используется ключевое слово языка XML DML для вставки инструкции по обработке в начало документа.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc =   
'<Root>   
    <Location LocationID="10" >   
        <step>Manufacturing step 1 at this work center</step>   
        <step>Manufacturing step 2 at this work center</step>   
    </Location>   
</Root>' ;  
SELECT @myDoc ;  
SET @myDoc.modify('   
insert <?Program = "Instructions.exe" ?>   
before (/Root)[1] ') ;  
SELECT @myDoc ;  
GO  
```  
  
### <a name="f-inserting-data-using-a-cdata-section"></a>Е. Вставка данных с помощью раздела CDATA  
 При вставке текста, содержащего недопустимые в XML символы, например < или >, можно использовать разделы CDATA для вставки данных, как показано в следующем запросе. В запросе указывается раздел CDATA, но добавляется он в качестве текстового узла, в котором недопустимые символы преобразуются в сущности. Например, `<` сохраняется как `&lt;`.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;             
SET @myDoc =   
'<Root>             
    <ProductDescription ProductID="1" ProductName="Road Bike">             
        <Features> </Features>             
    </ProductDescription>             
</Root>' ;            
SELECT @myDoc ;            
SET @myDoc.modify('             
insert <![CDATA[ <notxml> as text </notxml> or cdata ]]>   
into  (/Root/ProductDescription/Features)[1] ') ;   
SELECT @myDoc ;  
GO  
```  
  
 Запрос вставляет текстовый узел в элемент <`Features`>:  
  
```  
<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features> &lt;notxml@gt; as text &lt;/notxml&gt; or cdata </Features>  
</ProductDescription>  
</Root>       
```  
  
### <a name="g-inserting-text-node"></a>Ж. Вставка текстового узла  
 В этом запросе XML-документ сначала присваивается переменной типа **xml**. Затем для вставки текстового узла в качестве первого дочернего узла элемента <`Root`> используется язык XML DML. Для указания текста используется текстовый конструктор.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  
</Features>  
</ProductDescription>  
</Root>'  
SELECT @myDoc;  
SET @myDoc.modify('  
 insert text{"Product Catalog Description"}   
 as first into (/Root)[1]  
');  
SELECT @myDoc;  
```  
  
### <a name="h-inserting-a-new-element-into-an-untyped-xml-column"></a>З. Вставка нового элемента в нетипизированный XML-столбец  
 В следующем примере язык XML DML применяется для обновления экземпляра XML, хранящегося в столбце типа **xml**:  
  
```sql
USE AdventureWorks;  
GO  
CREATE TABLE T (i INT, x XML);  
GO  
INSERT INTO T VALUES(1,'<Root>  
    <ProductDescription ProductID="1" ProductName="Road Bike">  
        <Features>  
            <Warranty>1 year parts and labor</Warranty>  
            <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
        </Features>  
    </ProductDescription>  
</Root>');  
GO  
-- insert a new element  
UPDATE T  
SET x.modify('insert <Material>Aluminium</Material> as first  
  into   (/Root/ProductDescription/Features)[1]  
');  
GO  
```  
  
 Опять же, при вставке узла элемента <`Material`> выражение пути должно возвращать одну цель. Это указывается явно путем добавления [1] в конце выражения.  
  
```sql
-- check the update  
SELECT x.query(' //ProductDescription/Features')  
FROM T;  
GO  
```  
  
### <a name="i-inserting-based-on-an-if-condition-statement"></a>И. Вставка на основе инструкции с условием  
 В следующем примере в XML-инструкции DML **insert** в качестве части выражения Expression1 указывается условие IF. Если условие возвращает значение True, атрибут добавляется в элемент <`WorkCenter`>.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc =   
'<Root>  
    <Location LocationID="10" LaborHours="1.2" >  
        <step>Manufacturing step 1 at this work center</step>  
    <step>Manufacturing step 2 at this work center</step>  
    </Location>  
</Root>';  
SELECT @myDoc  
SET @myDoc.modify('  
insert  
if (/Root/Location[@LocationID=10])  
then attribute MachineHours {".5"}  
else ()  
    as first into   (/Root/Location[@LocationID=10])[1] ');  
SELECT @myDoc;  
GO  
```  
  
 Ниже приведен похожий пример, отличающийся только тем, что XML-инструкция DML **insert** вставляет элемент в документ, если условие возвращает значение True. То есть в случае, если элемент <`WorkCenter`> содержит два или менее дочерних элементов <`step`>.  
  
```sql
USE AdventureWorks;  
GO  
DECLARE @myDoc XML;  
SET @myDoc =   
'<Root>  
    <Location LocationID="10" LaborHours="1.2" >  
        <step>Manufacturing step 1 at this work center</step>  
        <step>Manufacturing step 2 at this work center</step>  
    </Location>  
</Root>';  
SELECT @myDoc;  
SET @myDoc.modify('  
insert  
if (count(/Root/Location/step) <= 2)  
then element step { "This is a new step" }  
else ()  
    as last into   (/Root/Location[@LocationID=10])[1] ');  
SELECT @myDoc;  
GO  
```  
  
 Результат:  
  
```  
<Root>  
 <WorkCenter WorkCenterID="10" LaborHours="1.2">  
  <step>Manufacturing step 1 at this work center</step>  
  <step>Manufacturing step 2 at this work center</step>  
  <step>This is a new step</step>  
 </WorkCenter>  
```  
  
### <a name="j-inserting-nodes-in-a-typed-xml-column"></a>К. Вставка узлов в типизированный XML-столбец  
 В данном примере выполняется вставка элемента и атрибута в производственные инструкции в формате XML, которые хранятся в типизированном столбце типа **xml**.  
  
 В примере сначала создается таблица (T) с типизированным столбцом типа **xml** в базе данных AdventureWorks. Затем копируются производственные инструкции экземпляра XML из столбца Instructions в таблице ProductModel в таблицу T. После этого вставленные данные применяются к XML в таблице T.  
  
```sql
USE AdventureWorks;  
GO            
DROP TABLE T;  
GO             
CREATE TABLE T(
  ProductModelID INT PRIMARY KEY,    
  Instructions XML (Production.ManuInstructionsSchemaCollection));  
GO  
INSERT T              
    SELECT ProductModelID, Instructions             
    FROM Production.ProductModel             
    WHERE ProductModelID=7;  
GO             
SELECT Instructions             
FROM T;  
-- now insertion begins             
--1) insert a new manu. Location. The <Root> specified as              
-- expression 2 in the insert() must be singleton.      
UPDATE T   
SET Instructions.modify('   
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
insert <MI:Location LocationID="1000" >   
           <MI:step>New instructions go here</MI:step>   
         </MI:Location>   
as first   
into   (/MI:root)[1]   
') ;  
  
SELECT Instructions             
FROM T ;  
-- 2) insert attributes in the new <Location>             
UPDATE T             
SET Instructions.modify('             
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";             
insert attribute LaborHours { "1000" }             
into (/MI:root/MI:Location[@LocationID=1000])[1] ');   
GO             
SELECT Instructions             
FROM T ;  
GO             
--cleanup             
DROP TABLE T ;  
GO             
```  
  
## <a name="see-also"></a>См. также:  
 [Сравнение типизированного и нетипизированного XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Создание экземпляров XML-данных](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [методов типа данных xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Язык модификации XML-данных (XML DML)](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  

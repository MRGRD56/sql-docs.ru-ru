---
description: Метод nodes() (тип данных xml)
title: Метод nodes() (тип данных xml)
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- nodes() method
- nodes method
ms.assetid: 7267fe1b-2e34-4213-8bbf-1c953822446c
author: rothja
ms.author: jroth
ms.openlocfilehash: 61673f5a3471e8c4af8aeca36eaa8eb4984f13ab
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491267"
---
# <a name="nodes-method-xml-data-type"></a>Метод nodes() (тип данных xml)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Метод **nodes()** незаменим в тех случаях, когда экземпляр типа данных **xml** необходимо разделить на реляционные данные. Он позволяет идентифицировать узлы, которые будут ставиться в соответствие новой строке.  
  
У каждого экземпляра типа данных **xml** имеется неявно заданный узел контекста. Для сохраняемого в столбце или переменной экземпляра XML таким узлом является узел документов. Узел документа является неявным узлом, который находится на верхнем уровне каждого экземпляра типа данных **xml**.  
  
Метод **nodes()** возвращает набор строк, содержащий логические копии исходных экземпляров XML. В таких логических копиях контекстным узлом каждого экземпляра строки устанавливается один из узлов, идентифицируемых выражением запроса. Таким образом последующие запросы могут перемещаться относительно этих контекстных узлов.  
  
Из этих наборов строк могут извлекаться несколько значений. Например, метод **value()** может быть применен к набору строк, возвращенному методом **nodes()** , для извлечения нескольких значений из исходного экземпляра XML. Метод **value()** , примененный к экземпляру XML, возвращает только одно значение.  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
nodes (XQuery) as Table(Column)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*XQuery*  
Строковый литерал, выражение XQuery. Если выражение запроса конструирует узлы, эти сконструированные узлы представлены в результирующем наборе строк. Если результатом выражения запроса является пустая последовательность, набор строк также будет пустым. Если статическим результатом выражения запроса является последовательность, которая вместо узлов содержит атомарные значения, возникает статическая ошибка.  
  
*Таблица*(*столбец*)  
Имя таблицы и имя столбца для результирующего набора строк.  
  
## <a name="remarks"></a>Remarks  
В качестве примера предположим, что имеется следующая таблица:  
  
```sql
T (ProductModelID INT, Instructions XML)  
```  
  
В таблице хранится следующее руководство по изготовлению. Здесь показан только фрагмент этого документа. Обратите внимание, что в документе указываются три места производства.  
  
```
<root>  
  <Location LocationID="10"...>  
     <step>...</step>  
     <step>...</step>  
      ...  
  </Location>  
  <Location LocationID="20" ...>  
       ...  
  </Location>  
  <Location LocationID="30" ...>  
       ...  
  </Location>  
</root>  
```  
  
Вызов метода `nodes()` с выражением запроса `/root/Location` возвращает набор из трех строк, каждая из которых содержит логическую копию исходного XML-документа с контекстным элементом, присвоенным одному из узлов `<Location>`:  
  
```
Product  
ModelID      Instructions  
----------------------------------  
1      <root><Location LocationID="10" ... />  
             <Location LocationID="20" ... />  
             <Location LocationID="30" .../></root>  
1      <root><Location LocationID="10" ... />  
             <Location LocationID="20" ... />  
             <Location LocationID="30" .../></root>  
1      <root><Location LocationID="10" ... />  
             <Location LocationID="20" ... />  
             <Location LocationID="30" .../></root>  
```  
  
Затем для набора строк можно выполнять запросы при помощи методов типа данных **xml**. Следующий запрос извлекает поддерево элемента контекста для каждой сформированной строки:  
  
```sql
SELECT T2.Loc.query('.')  
FROM T  
CROSS APPLY Instructions.nodes('/root/Location') AS T2(Loc)   
```  
  
Результат:  
  
```
ProductModelID  Instructions  
----------------------------------  
1        <Location LocationID="10" ... />  
1        <Location LocationID="20" ... />  
1        <Location LocationID="30" .../>  
```  
  
Возвращаемый набор строк сохраняет информацию о типе. Методы типа данных **xml** (**query()** , **value()** , **exist()** и **nodes()** ) могут применяться к результату, возвращенному методом **nodes()** . Однако вы не можете использовать метод **modify()** , чтобы изменить экземпляр XML.  
  
Кроме того, контекстный узел в наборе строк не может быть материализован. Это означает, что вы не можете использовать его в инструкции SELECT. Однако он может использоваться в выражениях IS NULL и COUNT(*).  
  
Сценарии применения метода **nodes()** и инструкции [OPENXML (Transact-SQL)](../../t-sql/functions/openxml-transact-sql.md) совпадают. Тем самым обеспечивается представление набора строк XML. Однако при применении метода **nodes()** в таблице, содержащей несколько строк XML-документов, необязательно использовать курсоры.  
  
Набор строк, возвращаемый методом **nodes()** , является неименованным набором строк. Поэтому он должен быть явно именован с помощью механизма псевдонимов.  
  
Функция **nodes()** не может быть применена непосредственно к результатам определенной пользователем функции. Чтобы использовать функцию **nodes()** с результатом скалярной функции, определенной пользователем, вы можете:
 
- назначить переменной результат функции, определенной пользователем;
- воспользоваться производной таблицей и присвоить псевдоним столбца возвращаемому значению определенной пользователем функции, после чего применить инструкцию `CROSS APPLY`, чтобы выбрать данные из псевдонима.  
  
В следующем примере показан один из способов использования инструкции `CROSS APPLY` для выборки из результата пользовательской функции.  
  
```sql
USE AdventureWorks;  
GO  
  
CREATE FUNCTION XTest()  
RETURNS XML  
AS  
BEGIN  
RETURN '<document/>';  
END;  
GO  
  
SELECT A2.B.query('.')  
FROM  
(SELECT dbo.XTest()) AS A1(X)   
CROSS APPLY X.nodes('.') A2(B);  
GO  
  
DROP FUNCTION XTest;  
GO  
```  
  
## <a name="examples"></a>Примеры  
  
### <a name="using-nodes-method-against-a-variable-of-xml-type"></a>Применение метода nodes() к переменной типа данных xml  
В этом примере рассматривается XML-документ с элементом верхнего уровня <`Root`> и тремя дочерними элементами <`row`>. Запрос использует метод `nodes()` для разделения контекстных узлов, по одному для каждого элемента <`row`>. Метод `nodes()` возвращает набор из трех строк. Каждая строка имеет логическую копию исходного XML, с контекстными узлами, идентифицирующими разные элементы <`row`> исходного документа.  
  
Запрос возвращает контекстный узел из каждой строки:  
  
```sql
DECLARE @x XML   
SET @x='<Root>  
    <row id="1"><name>Larry</name><oflw>some text</oflw></row>  
    <row id="2"><name>moe</name></row>  
    <row id="3" />  
</Root>'  
SELECT T.c.query('.') AS result  
FROM   @x.nodes('/Root/row') T(c)  
GO  
```  
  
В следующем примере метод запроса возвращает элемент контекста и его содержимое:  
  
```
<row id="1"><name>Larry</name><oflw>some text</oflw></row>  
<row id="2"><name>moe</name></row>  
<row id="3"/>  
```  
  
Применение родительского метода доступа на контекстных узлах возвращает элемент <`Root`> для всех трех узлов:  
  
```sql
SELECT T.c.query('..') AS result  
FROM   @x.nodes('/Root/row') T(c)  
GO  
```  
  
Результат:  
  
```
<Root>  
    <row id="1"><name>Larry</name><oflw>some text</oflw></row>  
    <row id="2"><name>moe</name></row>  
    <row id="3" />  
</Root>  
<Root>  
    <row id="1"><name>Larry</name><oflw>some text</oflw></row>  
    <row id="2"><name>moe</name></row>  
    <row id="3" />  
</Root>  
<Root>  
    <row id="1"><name>Larry</name><oflw>some text</oflw></row>  
    <row id="2"><name>moe</name></row>  
    <row id="3" />  
</Root>  
```  
  
### <a name="specifying-the-nodes-method-against-a-column-of-xml-type"></a>Задание метода nodes() для столбца типа данных xml  
В этом примере использованы инструкции по производству велосипедов, которые хранятся в столбце Instructions типа **xml** в таблице **ProductModel**.  
  
В следующем примере метод `nodes()` применяется к столбцу `Instructions` типа **xml** в таблице `ProductModel`.  
  
Метод `nodes()` устанавливает элементы <`Location`> как контекстные узлы, задавая путь `/MI:root/MI:Location`. Результирующий набор строк включает в себя логические копии исходного документа, одну для каждого узла <`Location`> в документе, с контекстным узлом, установленным в элемент <`Location`>. В качестве результата функция `nodes()` выдает набор контекстных узлов <`Location`>.  
  
Метод `query()`, применяемый к этому набору строк, запрашивает `self::node` и возвращает элемент `<Location>` для каждой строки.  
  
В этом примере запрос устанавливает каждый элемент <`Location`> как контекстный узел в руководстве по изготовлению определенной производственной модели. Вы можете использовать эти контекстные узлы, чтобы извлечь такие значения:  
  
- Найти идентификаторы LocationID в каждом элементе <`Location`>  
  
- Получить шаги производства (дочерние элементы <`step`>) в каждом элементе <`Location`>  
  
Этот запрос возвращает контекстный элемент, у которого в методе `'.'` используется сокращенный синтаксис `self::node()` для `query()`.  
  
Следует отметить следующее.
  
- Метод `nodes()` применяется к столбцу Instructions и возвращает набор строк `T (C)`. Этот набор строк содержит логические копии исходного руководства по изготовлению с `/root/Location` в качестве контекстного элемента.  
  
- Инструкция CROSS APPLY применяет метод `nodes()` к каждой строке в таблице `Instructions` и возвращает только строки, образующие результирующий набор.  
  
    ```sql  
    SELECT C.query('.') as result  
    FROM Production.ProductModel  
    CROSS APPLY Instructions.nodes('  
    declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
    /MI:root/MI:Location') as T(C)  
    WHERE ProductModelID=7  
    ```  
  
  Частичный результат:  
  
    ```
    <MI:Location LocationID="10"  ...>  
       <MI:step ... />  
          ...  
    </MI:Location>  
    <MI:Location LocationID="20"  ... >  
        <MI:step ... />  
          ...  
    </MI:Location>  
    ...  
    ```  
  
### <a name="applying-nodes-to-the-rowset-returned-by-another-nodes-method"></a>Применение метода nodes() к набору строк, возвращенному другим методом nodes()  
В следующем коде из столбца `Instructions` таблицы `ProductModel` запрашиваются XML-документы, составляющие руководство по изготовлению. Запрос возвращает набор строк, содержащий идентификатор производственной модели, места производства и шаги производства.  
  
Следует отметить следующее.  
  
- Метод `nodes()` применяется к столбцу `Instructions` и возвращает набор строк `T1 (Locations)`, Этот набор строк содержит логические копии исходного руководства по изготовлению с `/root/Location` в качестве контекстного элемента.  
  
- Метод `nodes()` применяется к набору строк `T1 (Locations)` и возвращает набор строк `T2 (steps)`. Этот набор строк содержит логические копии исходного руководства по изготовлению с `/root/Location/step` в качестве контекстного элемента.  
  
```sql
SELECT ProductModelID, Locations.value('./@LocationID','int') AS LocID,  
steps.query('.') AS Step         
FROM Production.ProductModel         
CROSS APPLY Instructions.nodes('         
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";         
/MI:root/MI:Location') AS T1(Locations)         
CROSS APPLY T1.Locations.nodes('         
declare namespace MI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";         
./MI:step ') AS T2(steps)         
WHERE ProductModelID=7         
GO         
```  
  
Результат:  
  
```
ProductModelID LocID Step         
----------------------------         
7      10   <step ... />         
7      10   <step ... />         
...         
7      20   <step ... />         
7      20   <step ... />         
7      20   <step ... />         
...         
```  
  
В запросе префикс `MI` объявляется два раза. Вместо этого можно воспользоваться `WITH XMLNAMESPACES`, чтобы объявить этот префикс один раз и использовать его в запросе:  
  
```sql
WITH XMLNAMESPACES (  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions' AS MI)  
  
SELECT ProductModelID, Locations.value('./@LocationID','int') AS LocID,  
steps.query('.') AS Step         
FROM Production.ProductModel         
CROSS APPLY Instructions.nodes('         
/MI:root/MI:Location') AS T1(Locations)         
CROSS APPLY T1.Locations.nodes('         
./MI:step ') as T2(steps)         
WHERE ProductModelID=7         
GO    
```  
  
## <a name="see-also"></a>См. также:  
[Добавление пространств имен в запросы с WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
[Создание экземпляров XML-данных](../../relational-databases/xml/create-instances-of-xml-data.md)   
[Методы для типа данных XML](../../t-sql/xml/xml-data-type-methods.md)  
  
  

---
description: query() (тип данных xml)
title: query() (тип данных xml)
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- query method
- query() method
ms.assetid: f48f6f7b-219f-463a-bf36-bc10f21afaeb
author: rothja
ms.author: jroth
ms.openlocfilehash: aa45cbfb4e629304d3a64c36da530807f7338798
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491232"
---
# <a name="query-method-xml-data-type"></a>query() (тип данных xml)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Определяет запрос XQuery для экземпляра типа данных **xml**. Результат имеет тип данных **xml**. Метод возвращает экземпляр нетипизированного XML.  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
query ('XQuery')  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
XQuery  
Это строка, выражение XQuery, выполняющее в экземпляре XML запросы к узлам — элементам и атрибутам.  
  
## <a name="examples"></a>Примеры  
В этом подразделе приведены примеры использования метода query() типа данных **xml**.  
  
### <a name="a-using-the-query-method-against-an-xml-type-variable"></a>A. Использование метода query() для переменной типа xml  
В следующем примере объявляется переменная **\@myDoc** типа данных **xml**, и этой переменной присваивается экземпляр XML. Далее методом **query()** для документа определяется запрос XQuery.  
  
Запрос извлекает дочерний элемент <`Features`> элемента <`ProductDescription`>:  
  
```sql
DECLARE @myDoc XML  
SET @myDoc = '<Root>  
<ProductDescription ProductID="1" ProductName="Road Bike">  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>  
</ProductDescription>  
</Root>'  
SELECT @myDoc.query('/Root/ProductDescription/Features')  
```  
  
Далее представлен результат.  
  
```  
<Features>  
  <Warranty>1 year parts and labor</Warranty>  
  <Maintenance>3 year parts and labor extended maintenance is available</Maintenance>  
</Features>        
```  
  
### <a name="b-using-the-query-method-against-an-xml-type-column"></a>Б. Использование метода query() для столбца типа XML  
В следующем примере метод **query()** используется для задания запроса XQuery к столбцу **CatalogDescription** типа данных **xml** в базе данных **AdventureWorks**:  
  
```sql
SELECT CatalogDescription.query('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
<Product ProductModelID="{ /PD:ProductDescription[1]/@ProductModelID }" />  
') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('  
declare namespace PD="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
declare namespace wm="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain";  
     /PD:ProductDescription/PD:Features/wm:Warranty ') = 1  
```  
  
Обратите внимание на следующие элементы из предыдущего запроса:  
  
-   Столбец CatalogDescription является типизированным столбцом **xml**. Это означает, что с ним связана коллекция схем. В [прологе XQuery](../../xquery/modules-and-prologs-xquery-prolog.md) ключевое слово **namespace** определяет префикс, который в дальнейшем используется в тексте запроса.  
  
-   Метод **query()** строит XML, элемент <`Product`>, имеющий атрибут **ProductModelID**, и значение атрибута **ProductModelID** извлекается из базы данных. Дополнительные сведения о конструировании XML см. в разделе [XML-конструкция (XQuery)](../../xquery/xml-construction-xquery.md).  
  
-   [Метод exist() (тип данных XML)](../../t-sql/xml/exist-method-xml-data-type.md) в предложении WHERE находит только те строки, которые в XML содержат элемент <`Warranty`>. Опять же, ключевое слово **namespace** определяет два префикса пространства имен.  
  
Далее представлен частичный результат.  
  
```  
<Product ProductModelID="19"/>   
<Product ProductModelID="23"/>   
...  
```  
  
Заметим, что оба метода — и query(), и exist() — объявляют префикс PD. В этих случаях для первоначального определения префиксов и использования их в запросе можно использовать WITH XMLNAMESPACES.  
  
```sql
WITH XMLNAMESPACES 
(  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD,  
   'https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain' AS WM
)  
SELECT CatalogDescription.query('<Product ProductModelID="{ /PD:ProductDescription[1]/@ProductModelID }" />')
       AS Result  
FROM Production.ProductModel  
WHERE CatalogDescription.exist('/PD:ProductDescription/PD:Features/WM:Warranty ') = 1;
```  
  
## <a name="see-also"></a>См. также:  
 [Добавление пространств имен в запросы с WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Сравнение типизированного и нетипизированного XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Создание экземпляров XML-данных](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [методов типа данных xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Язык модификации XML-данных (XML DML)](../../t-sql/xml/xml-data-modification-language-xml-dml.md)  
  
  

---
title: Namespace-функция URI (XQuery) | Документация Майкрософт
description: Узнайте, как использовать функцию Namespace-URI в XQuery для возврата URI пространства имен указанного QName.
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- fn:namespace-uri function
- namespace-uri function
ms.assetid: 9b48d216-26c8-431d-9ab4-20ab187917f4
author: rothja
ms.author: jroth
ms.openlocfilehash: 4a9734dd4efd2f4dcedd539218a0a7d989211749
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353287"
---
# <a name="functions-on-nodes---namespace-uri"></a>Функции с узлами — namespace-uri
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  Возвращает URI пространства имен QName, указанного в *$arg* , как xs: String.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
fn:namespace-uri() as xs:string  
fn:namespace-uri($arg as node()?) as xs:string  
```  
  
## <a name="arguments"></a>Аргументы  
 *$arg*  
 Имя узла, для которого будет получена часть URI-кода пространства имен.  
  
## <a name="remarks"></a>Remarks  
  
-   Если аргумент опускается, значением по умолчанию является узел контекста.  
  
-   В SQL Server **fn: Namespace-URI ()** без аргумента может использоваться только в контексте контекстно-зависимого предиката. Точнее, она может использоваться только внутри квадратных скобок ([ ]).  
  
-   Если *$arg* является пустой последовательностью, возвращается строка нулевой длины.  
  
-   Если *$arg* является узлом элемента или атрибута, чья развернутая-QName не находится в пространстве имен, функция возвращает строку нулевой длины.  
  
## <a name="examples"></a>Примеры  
 В этом разделе приведены примеры запросов XQuery к экземплярам XML, хранящимся в различных столбцах типа **XML** в базе данных AdventureWorks.  
  
### <a name="a-retrieve-namespace-uri-of-a-specific-node"></a>A. Получение URI-адреса пространства имен определенного узла  
 Следующий запрос обращается к нетипизированному экземпляру XML. В выражении запроса выражение `namespace-uri(/ROOT[1])` получает часть URI-кода пространства имен указанного узла.  
  
```  
set @x='<ROOT><a>111</a></ROOT>'  
SELECT @x.query('namespace-uri(/ROOT[1])')  
```  
  
 Так как указанное имя QName не содержит части URI-кода пространства имен, а только часть локального имени, результатом будет строка нулевой длины.  
  
 Следующий запрос задается для типизированного **XML-** столбца инструкций. Выражение `namespace-uri(/AWMI:root[1]/AWMI:Location[1])` ВОЗВРАЩАЕТ URI пространства имен первого <`Location`> дочернего элемента для `root` элемента <>.  
  
```  
SELECT Instructions.query('  
declare namespace AWMI="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions" ;  
     namespace-uri(/AWMI:root[1]/AWMI:Location[1])') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Результат:  
  
```  
https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions  
```  
  
### <a name="b-using-namespace-uri-without-argument-in-a-predicate"></a>Б. Использование функции namespace-uri() без аргумента в предикате  
 Следующий запрос указывается в типизированном столбце xml типа CatalogDescription. Выражение возвращает все узлы элементов, чей URI-код пространства имен является адресом `https://www.adventure-works.com/schemas/OtherFeatures`. Функция Namespace-**URI ()** указана без аргумента и использует контекстный узел.  
  
```  
SELECT CatalogDescription.query('  
declare namespace p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
   /p1:ProductDescription//*[namespace-uri() = "https://www.adventure-works.com/schemas/OtherFeatures"]  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=19  
```  
  
 Частичный результат:  
  
```  
<p1:wheel xmlns:p1="https://www.adventure-works.com/schemas/OtherFeatures">High performance wheels.</p1:wheel>  
<p2:saddle xmlns:p2="https://www.adventure-works.com/schemas/OtherFeatures">  
  <p3:i xmlns:p3="http://www.w3.org/1999/xhtml">Anatomic design</p3:i> and made from durable leather for a full-day of riding in comfort.</p2:saddle>  
...  
```  
  
 Пространство имен URI в предыдущем запросе можно изменить на адрес `https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain`. Затем будут получены все дочерние узлы элемента <`ProductDescription`>, чей URI-адрес пространства имен расширенного имени QName — `https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelWarrAndMain` .  
  
### <a name="implementation-limitations"></a>Ограничения реализации  
 Существуют следующие ограничения:  
  
-   Функция **Namespace-URI ()** возвращает экземпляры типа xs: String вместо xs: anyURI.  
  
## <a name="see-also"></a>См. также:  
 [Функции на узлах](./xquery-functions-against-the-xml-data-type.md)   
 [Функция локального имени &#40;XQuery&#41;](../xquery/functions-on-nodes-local-name.md)  
  

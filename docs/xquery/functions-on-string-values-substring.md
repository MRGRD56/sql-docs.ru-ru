---
title: Функция substring (XQuery) | Документация Майкрософт
description: Сведения о функции XQuery Function substring (), возвращающей указанную часть исходной строки.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.reviewer: ''
ms.technology: xml
ms.topic: language-reference
dev_langs:
- XML
helpviewer_keywords:
- substring function [XQuery]
- fn:substring function
ms.assetid: 2b3b8651-de51-46dc-af82-c86c45eac871
author: rothja
ms.author: jroth
ms.openlocfilehash: 7457341a5489485973869e46c2ba721b86352217
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345014"
---
# <a name="functions-on-string-values---substring"></a>Функции со строковыми значениями — substring
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]

  Возвращает часть значения *$sourceString*, начиная с позиции, указанной значением *$startingLoc,* и продолжится для числа символов, указанного значением *$length*.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
fn:substring($sourceString as xs:string?,  
                          $startingLoc as xs:decimal?) as xs:string?  
  
fn:substring($sourceString as xs:string?,  
                          $startingLoc as xs:decimal?,  
                          $length as xs:decimal?) as xs:string?  
```  
  
## <a name="arguments"></a>Аргументы  
 *$sourceString*  
 Исходная строка.  
  
 *$startingLoc*  
 Начальная позиция в исходной строке, с которой начинается подстрока. Если это значение отрицательно или равно нулю, символы возвращаются с начала строки. Если значение превышает длину *$sourceString*, возвращается строка нулевой длины.  
  
 *$length*  
 Количество получаемых символов [дополнительно]. Если не указано, возвращаются все символы из расположения, указанного в *$startingLoc* вплоть до конца строки.  
  
## <a name="remarks"></a>Remarks  
 Версия этой функции с тремя аргументами возвращает символы строки `$sourceString`, позиции которых `$p` удовлетворяют следующим условиям:  
  
 `fn:round($startingLoc) <= $p < fn:round($startingLoc) + fn:round($length)`  
  
 Значение *$length* может быть больше числа символов в значении *$sourceString* после начальной должности. В этом случае подстрока возвращает символы до конца *$sourceString*.  
  
 Первый символ строки расположен в позиции 1.  
  
 Если значение *$sourceString* является пустой последовательностью, оно обрабатывается как строка нулевой длины. В противном случае, если либо *$startingLoc* , либо *$length* является пустой последовательностью, возвращается пустая последовательность.  
  
## <a name="supplementary-characters-surrogate-pairs"></a>Дополнительные символы (суррогатные пары)  
 Поведение суррогатных пар в функциях XQuery зависит от уровня совместимости базы данных и, в некоторых случаях, от URI-кода пространства имен по умолчанию для функций. Дополнительные сведения см. в разделе "функции XQuery, поддерживающие суррогаты" в разделе [критические изменения ядро СУБД функций в SQL Server 2016](../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md). См. также раздел [уровень совместимости ALTER database &#40;&#41;Transact-SQL ](../t-sql/statements/alter-database-transact-sql-compatibility-level.md) и [Параметры сортировки и поддержка Юникода](../relational-databases/collations/collation-and-unicode-support.md).  
  
## <a name="implementation-limitations"></a>Ограничения реализации  
 SQL Server требует, чтобы параметры *$startingLoc* и *$length* были типа xs: Decimal вместо xs: Double.  
  
 SQL Server позволяет *$startingLoc* и *$length* быть пустой последовательностью, поскольку пустая последовательность является возможным значением в результате возникновения динамических ошибок, сопоставленных с ().  
  
## <a name="examples"></a>Примеры  
 В этом разделе приведены примеры запросов XQuery к экземплярам XML, хранящимся в различных столбцах типа **XML** в [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] базе данных.  
  
### <a name="a-using-the-substring-xquery-function-to-retrieve-partial-summary-product-model-descriptions"></a>A. Использование функции XQuery substring() для получения частичного резюме о модели продукта  
 Запрос получает первые 50 символов текста, описывающего модель продукта, `Summary` элемент <> в документе.  
  
```  
WITH XMLNAMESPACES ('https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS pd)  
SELECT ProductModelID, CatalogDescription.query('  
    <Prod>{ substring(string((/pd:ProductDescription/pd:Summary)[1]), 1, 50) }</Prod>  
 ') as Result  
FROM Production.ProductModel  
where CatalogDescription.exist('/pd:ProductDescription')  = 1;  
```  
  
 Обратите внимание на следующие данные из предыдущего запроса:  
  
-   Функция **String ()** возвращает строковое значение `Summary` элемента<>. Эта функция используется, поскольку `Summary` элемент <> содержит как текст, так и вложенные элементы (элементы форматирования HTML), и потому что эти элементы будут пропущены и получен весь текст.  
  
-   Функция **substring ()** извлекает первые 50 символов из строкового значения, полученного **строкой ()**.  
  
 Частичный результат:  
  
```  
ProductModelID Result  
-------------- ----------------------------------------------------  
19      <Prod>Our top-of-the-line competition mountain bike.</Prod>   
23      <Prod>Suitable for any type of riding, on or off-roa</Prod>  
...  
```  
  
## <a name="see-also"></a>См. также:  
 [Функции XQuery для типа данных xml](../xquery/xquery-functions-against-the-xml-data-type.md)  
  
  

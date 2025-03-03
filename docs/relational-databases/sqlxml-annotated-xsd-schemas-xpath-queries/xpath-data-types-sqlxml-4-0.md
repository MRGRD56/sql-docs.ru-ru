---
title: Типы данных XPath (SQLXML)
description: Сведения о типах данных XPath в SQLXML 4,0 и их сравнении с типами данных Microsoft SQL Server и схем XML (XSD).
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- mapping XDR types to XPath types [SQLXML]
- data types [XPath]
- arithmetic operators
- mapping data types [SQLXML]
- relational operators [SQLXML]
- node-set [SQLXML]
- data types [SQLXML], XPath
- XPath operators [SQLXML]
- XDR data type [SQLXML]
- equality operators [SQLXML]
- XPath conversions [SQLXML]
- converting data types [SQLXML]
- Boolean operators
- XPath queries [SQLXML], data types
- XPath data types [SQLXML]
- operators [SQLXML]
ms.assetid: a90374bf-406f-4384-ba81-59478017db68
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e4f212129c020661eeb9a207e7b5bbe2c02ec15a
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107491443"
---
# <a name="xpath-data-types-sqlxml-40"></a>Типы данных XPath (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], XPath и XML Schema (XSD) имеют очень разные типы данных. Например, в XPath отсутствуют целочисленные типы данных и тип данных для обозначения даты, а в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и XSD таких типов множество. Типы данных XSD определяют время с точностью до наносекунды, а [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] только до одной трехсотой доли секунды. Поэтому не всегда возможно сопоставить один тип другому. Дополнительные сведения о сопоставлении [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типов данных с типами данных XSD см. [в разделе приведение типов данных и аннотация SQL: datatype &#40;&#41;SQLXML 4,0 ](../../relational-databases/sqlxml-annotated-xsd-schemas-using/data-type-coercions-and-the-sql-datatype-annotation-sqlxml-4-0.md).  
  
 XPath имеет три типа данных: **строка**, **число** и **логическое значение**. **Числовым** типом данных всегда является IEEE 754 с плавающей запятой двойной точности. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Тип данных **float (53)** является ближайшим к **числу** XPath. Однако тип **float (53)** не соответствует стандарту IEEE 754. В частности, этот тип не содержит ни значения NaN (не число), ни значения бесконечности. Попытка преобразовать нечисловую строку в **число** и попытку деления на ноль приведет к ошибке.  
  
## <a name="xpath-conversions"></a>Преобразования в XPath  
 При использовании запроса XPath, например, `OrderDetail[@UnitPrice > "10.0"]`, явные и неявные преобразования типов данных могут различными неочевидными способами изменить значение запроса. Поэтому важно знать, как реализована система типов данных в XPath. Спецификация языка XPath, язык XML Path (XPath) версии 1,0 W3C предложенная рекомендация 8 1999 октября, находится на веб-сайте W3C по адресу http://www.w3.org/TR/1999/PR-xpath-19991008.html .  
  
 Операторы XPath делятся на четыре категории:  
  
-   Логические операторы (и, или)  
  
-   Операторы отношения ( \<, > , \<=, > =)  
  
-   Операторы равенства (=, !=)  
  
-   Арифметические операторы (+, -, *, div, mod)  
  
 Операторы разных категорий по-разному преобразуют операнды. При необходимости операторы XPath неявно преобразуют операнды. Арифметические операторы преобразуют свои операнды в **число** и приводят к числу числовых значений. Логические операторы преобразуют свои операнды в **логические** значения и приводят к логическому значению. Результатом выполнения реляционных операторов и операторов равенства является логическое значение. Однако правила преобразования, которыми пользуются операторы, зависят от первоначальных типов операндов, как показано в таблице.  
  
|Операнд|Реляционный оператор|Оператор равенства|  
|-------------|-------------------------|-----------------------|  
|Оба операнда представляют собой наборы узлов.|Значение TRUE, если и только в том случае, если в одном наборе есть узел и узел во втором наборе, чтобы сравнение их **строковых** значений было истинным.|То же.|  
|Один из них является набором узлов, другой **строкой**.|Значение TRUE, если и только если в наборе узлов имеется узел, который при преобразовании в **число** имеет значение true для сравнения с **строкой** , преобразованной в **число** .|Значение TRUE, если и только если в наборе узлов имеется узел, для которого при преобразовании в **строку** сравнение со **СТРОКОЙ** имеет значение true.|  
|Один из них — набор узлов, другой **номер**.|Значение TRUE, если и только если в наборе узлов имеется узел, для которого при преобразовании в **число** выполняется сравнение с **числом** true.|То же.|  
|Один из них является набором узлов, а другой **логическим**.|Значение TRUE, если и только если в наборе узлов имеется узел, для которого при преобразовании в **логическое значение** , а затем в **число**, сравнение с **логическим** значением, преобразованным в **Number** , равно true.|Значение TRUE, если и только если в наборе узлов имеется узел, для которого при преобразовании в **логическое** значение выполняется сравнение с **логическим** значением true.|  
|Ни один из них не представляет собой набор узлов.|Преобразуйте оба операнда в **Number** , а затем сравните.|Преобразует оба операнда к одному типу, а затем сравнивает их. Преобразовать в **логическое значение** , если либо имеет **логическое** значение, либо **число** , если значение равно **числу**; в противном случае преобразуйте в **строку**.|  
  
> [!NOTE]  
>  Поскольку операторы отношения XPath всегда преобразуют свои операнды в **число**, сравнение **строк** невозможно. Чтобы включить сравнения дат, SQL Server 2000 предлагает этот вариант для спецификации XPath: когда реляционный оператор сравнивает **строку** со **строкой**, набор узлов **для строки или** набор узлов, возвращающий строки, в набор узлов со строковыми значениями, выполняется сравнение **строк** (не сравнение **чисел** ).  
  
## <a name="node-set-conversions"></a>Преобразования наборов узлов  
 Преобразования наборов узлов не всегда интуитивно понятны. Набор узлов преобразуется в **строку** , принимая строковое значение только первого узла в наборе. Набор узлов преобразуется в **число** , преобразуя его в **строку**, а затем преобразуя **строку** в **число**. Набор узлов преобразуется в **логическое значение** путем проверки его существования.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не производит выборку по положению в наборах узлов — например, запрос XPath `Customer[3]` указывает на третьего заказчика; такой тип выборки по положению не поддерживается в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Таким образом, преобразование "набор узлов в **строку** " или "набор узлов в **число** ", как описано в спецификации XPath, не реализовано. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует семантику «любой» там, где спецификация XPath использует семантику «первый». Например, на основе спецификации W3C XPath запрос XPath `Order[OrderDetail/@UnitPrice > 10.0]` выбирает эти заказы с помощью первого элемента **OrderDetail** , у которого **Цена** больше 10,0. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] этот запрос XPath выбирает эти заказы с любым значением **OrderDetail** , имеющим значение **UnitPrice** больше 10,0.  
  
 Преобразование в **Boolean** создает тест существования; Таким образом, запрос XPath `Products[@Discontinued=true()]` эквивалентен выражению SQL "Products. uncontinueed не null", а не выражение SQL "Products. uncontinueed = 1". Чтобы сделать запрос эквивалентным последнему выражению SQL, сначала преобразуйте набор узлов в тип, отличный от **логического** , например **Number**. Например, `Products[number(@Discontinued) = true()]`.  
  
 Большинство операторов реализовано так, что они возвращают TRUE, если их результат равен TRUE хотя бы для одного (любого) узла набора, поэтому они возвращают FALSE, если набор узлов пуст. Таким образом, если набор узлов A пуст, оба оператора `A = B` и `A != B` вернут FALSE, а `not(A=B)` и `not(A!=B)` — TRUE.  
  
 Как правило, атрибут или элемент, сопоставляемый со столбцом, существует, если значение этого столбца в базе данных не равно **null**. Элементы, сопоставляемые со строками, существуют, если есть хотя бы один из их дочерних элементов.  
  
> [!NOTE]  
>  Элементы с заметками **— константа** всегда существует. Следовательно, предикаты XPath не могут использоваться для элементов **с константой** .  
  
 Если набор узлов преобразуется в **строку** или **число**, его тип XDR (если таковой имеется) проверяется в схеме с заметками, и этот тип используется для определения требуемого преобразования.  
  
## <a name="mapping-xdr-data-types-to-xpath-data-types"></a>Сопоставление типов данных XDR типам данных XPath  
 Тип данных XPath узла является производным от типа данных XDR в схеме, как показано в следующей таблице (для наглядного назначения используется узел **EmployeeID** ).  
  
|Тип данных XDR|Эквивалентный<br /><br /> тип данных XPath|Использованное преобразование SQL Server|  
|-------------------|------------------------------------|--------------------------------|  
|Nonebin.base64bin.hex|Н/Д|NoneEmployeeID|  
|Логическое|Логическое|CONVERT(bit, EmployeeID)|  
|number, int, float,i1, i2, i4, i8,r4, r8ui1, ui2, ui4, ui8|number|CONVERT(float(53), EmployeeID)|  
|id, idref, idrefsentity, entities, enumerationnotation, nmtoken, nmtokens, chardate, Timedate, Time.tz, string, uri, uuid|строка|CONVERT(nvarchar(4000), EmployeeID, 126)|  
|fixed14.4|н/д (в XPath нет типа данных, эквивалентного типу fixed14.4 XDR)|CONVERT(money, EmployeeID)|  
|Дата|строка|LEFT(CONVERT(nvarchar(4000), EmployeeID, 126), 10)|  
|time<br /><br /> time.tz|строка|SUBSTRING(CONVERT(nvarchar(4000), EmployeeID, 126), 1 + CHARINDEX(N'T', CONVERT(nvarchar(4000), EmployeeID, 126)), 24)|  
  
 Преобразования даты и времени предназначены для работы, если значение хранится в базе данных с использованием [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типа данных **DateTime** или **строки**. Обратите внимание, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] тип данных **DateTime** не **использует часовой пояс** и имеет меньшую точность, чем тип данных XML **time** . Чтобы включить тип данных **TimeZone** или дополнительную точность, храните данные в, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используя **строковый** тип.  
  
 При преобразовании узла из типа данных XDR в тип данных XPath иногда требуются дополнительные преобразования (из одного типа XPath в другой тип XPath). В качестве примера рассмотрим следующий запрос XPath:  
  
```  
(@m + 3) = 4  
```  
  
 Если @m имеет **фиксированный** тип данных XDR 14,4, преобразование из типа данных XDR в тип данных XPath выполняется с помощью следующих средств:  
  
```  
CONVERT(money, m)  
```  
  
 В этом преобразовании узел `m` преобразуется из **фиксированного 14,4** в **money**. Однако для прибавления 3 требуется дополнительное преобразование:  
  
```  
CONVERT(float(CONVERT(money, m))  
```  
  
 Выражение XPath вычисляется следующим образом:  
  
```  
CONVERT(float(CONVERT(money, m)) + CONVERT(float(53), 3) = CONVERT(float(53), 3)  
```  
  
 Как показано в следующей таблице, это то же самое преобразование, которое применялось для других выражений XPath (например, литералов или составных выражений).  
  
|   | Х неизвестен | X является строкой | X является числом | X является логическим |
| - | ------------ | ----------- | ----------- | ------------ |
| **string(X)** |CONVERT (nvarchar(4000), X, 126)|-|CONVERT (nvarchar(4000), X, 126)|CASE WHEN X THEN N'true' ELSE N'false' END|  
| **number(X)** |CONVERT (float(53), X)|CONVERT (float(53), X)|-|CASE WHEN X THEN 1 ELSE 0 END|  
| **boolean(X)** |-|LEN (X) > 0|X != 0|-|  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-convert-a-data-type-in-an-xpath-query"></a>A. Преобразование типа данных в запросе XPath  
 В следующем запросе XPath, указанном для аннотированной схемы XSD, запрос выбирает все узлы **Employee** с атрибутом **EmployeeID** со значением e-1, где "E-" — это префикс, указанный с помощью аннотации **SQL: id-prefix** .  
  
 `Employee[@EmployeeID="E-1"]`  
  
 Предикат в запросе эквивалентен следующему выражению SQL:  
  
 `N'E-' + CONVERT(nvarchar(4000), Employees.EmployeeID, 126) = N'E-1'`  
  
 Поскольку **значение EmployeeID** — одно из значений типа данных (**IDREF**, **IDREFS**, **NMTOKEN**, **NMTOKENS** и т. д.) в схеме XSD, **код** **EmployeeID** преобразуется в тип данных **String** XPath с использованием правил преобразования, описанных выше.  
  
 `CONVERT(nvarchar(4000), Employees.EmployeeID, 126)`  
  
 К строке добавляется префикс «E-», а результат затем сравнивается с `N'E-1'`.  
  
### <a name="b-perform-several-data-type-conversions-in-an-xpath-query"></a>Б. Несколько преобразований типов данных в запросе XPath  
 Рассмотрим этот запрос XPath, заданный для схемы XSD с заметками: `OrderDetail[@UnitPrice * @OrderQty > 98]`  
  
 Этот запрос XPath возвращает все элементы, которые **\<OrderDetail>** соответствуют предикату `@UnitPrice * @OrderQty > 98` . Если **UnitPrice** помечена **фиксированным** типом данных 14,4 в схеме с заметками, то этот предикат эквивалентен выражению SQL:  
  
 `CONVERT(float(53), CONVERT(money, OrderDetail.UnitPrice)) * CONVERT(float(53), OrderDetail.OrderQty) > CONVERT(float(53), 98)`  
  
 В ходе преобразований значений в запросе XPath сначала тип данных XDR преобразуется в тип данных XPath. Поскольку тип данных XSD для **UnitPrice** является **фиксированным 14,4**, как описано в предыдущей таблице, это первое используемое преобразование:  
  
```  
CONVERT(money, OrderDetail.UnitPrice))   
```  
  
 Поскольку арифметические операторы преобразуют свои операнды в тип данных XPath **Number** , применяется второе преобразование (из одного типа данных XPath в другой тип данных XPath), в котором значение преобразуется в тип **float (53)** (**float (53)** близко к типу данных XPath **Number** ):  
  
```  
CONVERT(float(53), CONVERT(money, OrderDetail.UnitPrice))   
```  
  
 Если атрибут **OrderQty** не имеет типа данных XSD, то **OrderQty** преобразуется в **числовой** тип данных XPath в одном преобразовании:  
  
```  
CONVERT(float(53), OrderDetail.OrderQty)  
```  
  
 Аналогичным образом значение 98 преобразуется в тип данных XPath **Number** :  
  
```  
CONVERT(float(53), 98)  
```  
  
> [!NOTE]  
>  Если используемый в схеме тип данных XSD несовместим с базовым типом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в базе данных или нужное преобразование типа данных XPath невозможно выполнить, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может вернуть ошибку. Например, если атрибут **EmployeeID** снабжен аннотацией с **префиксом ID** , то XPath `Employee[@EmployeeID=1]` выдает ошибку, поскольку **EmployeeID** имеет аннотацию **идентификатора** и не может быть преобразована в **число**.  
  
  

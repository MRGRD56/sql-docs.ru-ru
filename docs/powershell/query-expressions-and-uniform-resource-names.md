---
title: Выражения запросов и универсальные имена ресурсов
description: Сведения о выражениях запроса, которые перечисляют один объект или несколько в иерархии объектной модели, и об унифицированных именах ресурсов (URN), однозначно идентифицирующих отдельный объект.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
helpviewer_keywords:
- query expressions
- unique resource names
- URN
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: e1955201f60f58c6513928c7185dbbdad16cf445
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839491"
---
# <a name="query-expressions-and-uniform-resource-names"></a>Выражения запросов и универсальные имена ресурсов

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

В моделях объектов SMO [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] и оснастках [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] PowerShell используется два типа строк выражений, которые похожи на выражения XPath. Выражения запроса — это строки, которые указывают набор условий, используемых для перечисления одного или нескольких объектов в иерархии объектной модели. Универсальное имя ресурса (URN) — это конкретный тип строки выражения запроса, который уникально определяет один объект.  

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="syntax"></a>Синтаксис  

```powershell
  
Object1[<FilterExpression1>]/ ... /ObjectN[<FilterExpressionN>]  
  
<FilterExpression>::=  
<PropertyExpression> [and <PropertyExpression>][...n]  
  
<PropertyExpression>::=  
      @BooleanPropertyName=true()  
 | @BooleanPropertyName=false()  
 | contains(@StringPropertyName, 'PatternString')  
  | @StringPropertyName='String'  
 | @DatePropertyName=datetime('DateString')  
 | is_null(@PropertyName)  
 | not(<PropertyExpression>)  
```  

## <a name="arguments"></a>Аргументы

*Объект*  
Указывает тип объекта, который представлен в узле строки выражения. Каждый объект представляет класс коллекции из этих пространств имен объектной модели SMO.  

<xref:Microsoft.SqlServer.Management.Smo>  

<xref:Microsoft.SqlServer.Management.Smo.Agent>  

<xref:Microsoft.SqlServer.Management.Smo.Broker>  

<xref:Microsoft.SqlServer.Management.Smo.Mail>  

<xref:Microsoft.SqlServer.Management.Dmf>  

<xref:Microsoft.SqlServer.Management.Facets>  

<xref:Microsoft.SqlServer.Management.RegisteredServers>  

<xref:Microsoft.SqlServer.Management.Smo.RegSvrEnum>  

Например, укажите имя Server для класса **ServerCollection** и имя Database для класса **DatabaseCollection** .  

\@*PropertyName*  
Указывает имя одного из свойств класса, связанного с объектом, который задан в *Object*. Имя свойства должно начинаться с префикса \@. Например, укажите \@IsAnsiNull для свойства **IsAnsiNull** класса **Database**.  
  
\@*BooleanPropertyName*=true()  
Перечисляет все объекты, где указанное логическое свойство имеет значение TRUE.  
  
\@*BooleanPropertyName*=false()  
Перечисляет все объекты, где указанное логическое свойство имеет значение FALSE.  
  
contains(\@*StringPropertyName*, '*PatternString*')  
Перечисляет все объекты, указанное строковое свойство которых содержит хотя бы одно вхождение набора символов, который указан в '*PatternString*'.  
  
\@*StringPropertyName*='*PatternString*'  
Перечисляет все объекты, значение указанного строкового свойства которых точно такое же, как комбинация символов, указанная в '*PatternString*'.  
  
\@*DatePropertyName*= datetime('*DateString*')  
Перечисляет все объекты, значение указанного свойства даты которых соответствует дате, указанной в '*DateString*'. Значение *DateString* должно иметь формат гггг-мм-дд чч:ми:сс.ммм.  
  
|Компонент DateString|Описание|  
|-|-|  
|гггг|Год из четырех цифр.|  
|ММ|Месяц из двух цифр (от 01 до 12).|  
|дд|День из двух цифр (от 01 до 31).|  
|hh|Час из двух цифр в 24-часовом формате (от 01 до 23).|  
|ми|Минута из двух цифр (от 01 до 59).|  
|сс|Секунда из двух цифр (от 01 до 59).|  
|ммм|Количество миллисекунд (от 001 до 999).|  
  
 Даты, указанные в этом формате, можно вычислять с любым форматом даты, хранящимся в [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 is_null(\@*PropertyName*)  
 Перечисляет все объекты, где указанное свойство имеет значение NULL.  
  
 not(\<*PropertyExpression*>)  
 Инвертирует значение вычисленного выражения *PropertyExpression*, перечисляя все объекты, не соответствующие условию, заданному в *PropertyExpression*. Например, not(contains(\@Name, 'xyz')) перечисляет все объекты, в именах которых нет строки xyz.  
  
## <a name="remarks"></a>Remarks  

Выражения запроса — это строки, которые перечисляют узлы в иерархии моделей SMO. У каждого узла есть критерий фильтра, задающий условие для определения того, какие объекты в этом узле будут перечисляться. Выражения запроса моделируются на языке выражений XPath. Выражения запроса представляют собой небольшое подмножество выражений XPath, кроме того, в них есть некоторые выражения, которых нет в XPath. Выражения XPath — это строки, которые указывают набор критериев, используемых для перечисления одного или нескольких тегов в XML-документе. Дополнительные сведения об XPath см. в разделе [Язык W3C XPath](http://www.w3.org/TR/xpath20/).  

Выражения запроса должны начинаться с абсолютной ссылки на объект сервера. Относительные выражения, начинающиеся с символа /, не допустимы. Последовательность объектов, указанных в выражении запроса, должна соответствовать иерархии коллекции объектов в связанной модели объекта. Например, выражение запроса, которое ссылается на объекты в пространстве имен Microsoft.SqlServer.Management.Smo, должно начинаться с узла сервера, за которым идет узел базы данных, и так далее.  

Если критерий *\<FilterExpression>* для объекта не указан, перечисляются все объекты в этом узле.  
  
## <a name="uniform-resource-names-urn"></a>Универсальные имена ресурсов (URN)  

Имена URN представляют собой подмножество выражений запроса. Каждое имя URN является полной ссылкой на один объект. В обычном имени URN свойство «Имя» используется для определения одного объекта в каждом узле. Например, данное имя URN ссылается на определенный столбец:  
  
```powershell
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012']/Table[@Name='SalesPerson' and @Schema='Sales']/Column[@Name='SalesPersonID']  
```
  
## <a name="examples"></a>Примеры  
  
### <a name="a-enumerating-objects-using-false"></a>A. Перечисление объектов при помощи функции false()  
 Приведенное ниже выражение запроса перечисляет все базы данных в экземпляре по умолчанию в **MyComputer** , атрибут **AutoClose** которых имеет значение false.  
  
```  
Server[@Name='MYCOMPUTER']/Database[@AutoClose=false()]  
```  
  
### <a name="b-enumerating-objects-using-contains"></a>Б. Перечисление объектов при помощи функции contains  
 Следующее выражение запроса перечисляет все базы данных, в которых учитывается регистр и в имени которых имеется символ «m».  
  
```  
Server[@Name='MYCOMPUTER']/Database[@CaseSensitive=false() and contains(@Name, 'm')]   
```  
  
### <a name="c-enumerating-objects-using-not"></a>В. Перечисление объектов при помощи функции not  
 Следующее выражение запроса перечисляет все таблицы базы данных [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] , которые не находятся в схеме **Production** и содержат в имени таблицы слово "History":  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012']/Table[not(@Schema='Production') and contains(@Name, 'History')]  
```  
  
### <a name="d-not-supplying-a-filter-expression-for-the-final-node"></a>Г. Отсутствие критерия фильтра для итогового узла  
 Следующее выражение запроса перечисляет все столбцы в таблице **AdventureWorks2012.Sales.SalesPerson** :  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012"]/Table[@Schema='Sales' and @Name='SalesPerson']/Columns  
```  
  
### <a name="e-enumerating-objects-using-datetime"></a>Д. Перечисление объектов при помощи функции datetime  
 Следующее выражение запроса перечисляет все таблицы, созданные в базе данных [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] в определенное время:  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012"]/Table[@CreateDate=datetime('2008-03-21 19:49:32.647')]  
```  
  
### <a name="f-enumerating-objects-using-is_null"></a>Е. Перечисление объектов при помощи функции is_null  
 Следующее выражение запроса перечисляет все таблицы в базе данных [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] , значение свойства «Дата последнего изменения» в которых не равно NULL:  
  
```  
Server[@Name='MYCOMPUTER']/Database[@Name='AdventureWorks2012"]/Table[Not(is_null(@DateLastModified))]  
```  
  
## <a name="see-also"></a>См. также:

- [Invoke-PolicyEvaluation, командлет](/powershell/module/sqlserver/Invoke-PolicyEvaluation)
- [Подсистема аудита SQL Server (компонент Database Engine)](../relational-databases/security/auditing/sql-server-audit-database-engine.md)
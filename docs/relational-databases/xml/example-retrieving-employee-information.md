---
title: Пример Получение сведений о сотрудниках | Документация Майкрософт
description: Просмотрите пример получения сведений о сотрудниках из базы данных в виде файла XML с помощью запроса FOR XML EXPLICIT.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- EXPLICIT mode
ms.assetid: 63cd6569-2600-485b-92b4-1f6ba09db219
author: rothja
ms.author: jroth
ms.openlocfilehash: 89136f9e67022c619c1d9ad4e78e4196fe131a68
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490923"
---
# <a name="example-retrieving-employee-information"></a>Пример Получение сведений о сотрудниках
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В этом примере извлекаются идентификаторы и имена всех работников. В базе данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] значение employeeID может быть получено из столбца BusinessEntityID таблицы Employee. Имена работников могут быть получены из таблицы Person. Столбец BusinessEntityID можно использовать для соединения этих таблиц.  
  
 Предположим, что требуется произвести преобразование FOR XML EXPLICIT, чтобы сформировать XML так, как показано далее:  
  
```  
<Employee EmpID="1" >  
  <Name FName="Ken" LName="Sánchez" />  
</Employee>  
...  
```  
  
 Так как существует два уровня в иерархии, следует написать два запроса `SELECT` и применить предложение UNION ALL. Это первый запрос, который извлекает значения для элемента <`Employee`> и его атрибутов. Этот запрос присваивает значение `1` атрибуту `Tag` элемента <`Employee`>, а атрибуту `Parent` значение NULL, так как это элемент верхнего уровня.  
  
```  
SELECT 1    as Tag,  
       NULL as Parent,  
       E.BusinessEntityID AS [Employee!1!EmpID],  
       NULL       as [Name!2!FName],  
       NULL       as [Name!2!LName]  
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID;  
```  
  
 Это второй запрос. Он извлекает значения для элемента <`Name`>. Присваивает значение `2` атрибуту `Tag` элемента <`Name`> и значение `1` атрибуту `Parent`, определяющее в качестве родителя элемент <`Employee`>.  
  
```  
SELECT 2 as Tag,  
       1 as Parent,  
       E.BusinessEntityID,  
       FirstName,   
       LastName   
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID;  
```  
  
 Объединив эти запросы с помощью инструкции `UNION AL`, примените директиву `FOR XML EXPLICIT`и укажите необходимое предложение `ORDER BY` . Сначала нужно отсортировать набор строк по значению `BusinessEntityID` , потом по имени, так что значения NULL в именах будут отображаться первыми. Выполняя следующий запрос без предложения FOR XML, можно увидеть сформированную универсальную таблицу.  
  
 Это окончательный запрос:  
  
```  
SELECT 1    as Tag,  
       NULL as Parent,  
       E.BusinessEntityID as [Employee!1!EmpID],  
       NULL       as [Name!2!FName],  
       NULL       as [Name!2!LName]  
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID  
UNION ALL  
SELECT 2 as Tag,  
       1 as Parent,  
       E.BusinessEntityID,  
       FirstName,   
       LastName   
FROM   HumanResources.Employee AS E  
INNER JOIN Person.Person AS P  
ON  E.BusinessEntityID = P.BusinessEntityID  
ORDER BY [Employee!1!EmpID],[Name!2!FName]  
FOR XML EXPLICIT;  
```  
  
 Частичный результат:  
  
```
<Employee EmpID="1">
  <Name FName="Ken" LName="Sánchez" />
</Employee>
<Employee EmpID="2">
  <Name FName="Terri" LName="Duffy" />
</Employee>
...
```
  
 Первая инструкция `SELECT` указывает имена для столбцов в результирующем наборе строк. Эти имена образуют две группы столбцов. Группа со значением `Tag` , равным `1` , в имени столбца задает `Employee` в качестве элемента и `EmpID` в качестве атрибута. Другая группа столбцов со значением `Tag`, равным `2`, в имени столбца задает <`Name`> в качестве элемента, а `FName` и `LName` в качестве атрибутов.  
  
 Следующая таблица показывает частичный набор строк, сформированный запросом:  
  
Тег | Parent | Employee!1!EmpID | Name!2!FName | Name!2!LName
-|-|-|-|-
1 | NULL | 1 | NULL | NULL 
2 | 1 | 1 | Ken | Sánchez 
1 | NULL | 2 | NULL | NULL 
2 | 1 | 2 | Terri | Duffy 
1 | NULL | 3 | NULL | NULL 
2 | 1 | 3 | Roberto | Tamburello 
... | ... | ... | ... | ...
  
 Вот как строки универсальной таблицы обрабатываются для создания результирующего дерева XML:  
  
 Первая строка задает для атрибута `Tag` значение `1`. В результате получается группа столбцов, у которых имеется значение `Tag` , равное `1` , `Employee!1!EmpID`. Этот столбец задает в качестве имени элемент `Employee` . После этого создается элемент <`Employee`> с атрибутами `EmpID`. Соответствующие значения столбца присваиваются этим атрибутам.  
  
 Вторая строка имеет атрибут `Tag` со значением `2`. В результате получается группа столбцов, у которых атрибут `Tag` имеет значение `2` в имени столбца, `Name!2!FName`, `Name!2!LName`. Эти имена столбцов задают в качестве имени элемента `Name` . После этого создается элемент <`Name`> с атрибутами `FName` и `LName`. После этого соответствующие значения столбца присваиваются этим атрибутам. Эта строка задает `1` в качестве `Parent`. Этот дочерний элемент добавляется к предыдущему элементу <`Employee`>.  
  
 Процесс повторяется для оставшихся строк набора строк. Обратите внимание на важность порядка строк в универсальной таблице, чтобы FOR XML EXPLICIT, обработав набор строк по порядку, мог создать желаемый XML.  
  
## <a name="see-also"></a>См. также:  
 [Использование режима EXPLICIT с предложением FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)  
  
  

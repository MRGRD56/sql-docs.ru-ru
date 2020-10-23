---
description: MultiLineString
title: MultiLineString | Документация Майкрософт
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- MultiLineString geometry subtype [SQL Server]
- geometry subtypes [SQL Server]
ms.assetid: 95deeefe-d6c5-4a11-b347-379e4486e7b7
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6caab752b3edc7c56d127fb3a378a9a5a69a18b7
ms.sourcegitcommit: a5398f107599102af7c8cda815d8e5e9a367ce7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "92006295"
---
# <a name="multilinestring"></a>MultiLineString
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
   Тип данных **MultiLineString** представляет собой коллекцию экземпляров **geometry** или **geographyLineString**.  
  
## <a name="multilinestring-instances"></a>Экземпляры MultiLineString  
 На рисунке ниже приведены примеры экземпляров **MultiLineString** .  
  
 ![Примеры объектов MultiLineString типа geometry](../../relational-databases/spatial/media/multilinestring.gif "Примеры объектов MultiLineString типа geometry")  
  
 На рисунке представлены:  
  
-   Изображение 1 представляет простой экземпляр **MultiLineString** , граница которого определяется четырьмя конечными точками двух его элементов **LineString** .  
  
-   Изображение 2 представляет простой экземпляр **MultiLineString** , поскольку пересекаются только конечные точки элементов **LineString** . Граница образована двумя неперекрывающимися конечными точками.  
  
-   Изображение 3 представляет непростой экземпляр **MultiLineString** , поскольку имеется пересечение внутренней части одного из элементов **LineString** этого экземпляра. Границей данного экземпляра **MultiLineString** являются четыре конечные точки.  
  
-   На рисунке 4 представлен отличный от простого незамкнутый экземпляр объекта **MultiLineString** .  
  
-   Изображение 5 представляет простой, незамкнутый экземпляр **MultiLineString**. Экземпляр является незамкнутым, поскольку незамкнуты его элементы **LineStrings** . Экземпляр является простым, поскольку внутренние стороны экземпляров **LineStrings** не пересекаются.  
  
-   Изображение 6 представляет простой, замкнутый экземпляр **MultiLineString** . Экземпляр является замкнутым, поскольку все его элементы замкнуты. Экземпляр является простым, поскольку внутренние области его элементов не пересекаются.  
  
### <a name="accepted-instances"></a>Правильные экземпляры  
 Чтобы экземпляр **MultiLineString** был принят, он должен быть пустым или содержать только принимаемые экземпляры **LineString**. Дополнительные сведения о принимаемых экземплярах **LineString** см. в разделе [LineString](../../relational-databases/spatial/linestring.md). В следующих примерах показаны принятые экземпляры **MultiLineString** .  
  
```sql  
DECLARE @g1 geometry = 'MULTILINESTRING EMPTY';  
DECLARE @g2 geometry = 'MULTILINESTRING((1 1, 3 5), (-5 3, -8 -2))';  
DECLARE @g3 geometry = 'MULTILINESTRING((1 1, 5 5), (1 3, 3 1))';  
DECLARE @g4 geometry = 'MULTILINESTRING((1 1, 3 3, 5 5),(3 3, 5 5, 7 7))';  
```  
  
Приведенный ниже пример вызывает исключение `System.FormatException` , так как второй экземпляр **LineString** недействителен.  
  
```sql  
DECLARE @g geometry = 'MULTILINESTRING((1 1, 3 5),(-5 3))';  
```  
  
### <a name="valid-instances"></a>Допустимые экземпляры  
Чтобы экземпляр **MultiLineString** был действителен, он должен соответствовать следующим критериям.  
  
1.  Все экземпляры, составляющие экземпляр **MultiLineString** , должны быть действительными экземплярами **LineString** .  
  
2.  Никакие два экземпляра **LineString** , составляющие экземпляр **MultiLineString** , не могут перекрывать сами себя на интервале. Экземпляры **LineString** могут взаимодействовать или соприкасаться только с собой или с другими экземплярами **LineString** в конечном числе точек.  

В следующем примере показаны три действительных экземпляра **MultiLineString** и один экземпляр **MultiLineString** , который недействителен.  
  
```sql  
DECLARE @g1 geometry = 'MULTILINESTRING EMPTY';  
DECLARE @g2 geometry = 'MULTILINESTRING((1 1, 3 5), (-5 3, -8 -2))';  
DECLARE @g3 geometry = 'MULTILINESTRING((1 1, 5 5), (1 3, 3 1))';  
DECLARE @g4 geometry = 'MULTILINESTRING((1 1, 3 3, 5 5),(3 3, 5 5, 7 7))';  
SELECT @g1.STIsValid(), @g2.STIsValid(), @g3.STIsValid(), @g4.STIsValid();  
```  
  
`@g4` является недопустимым, так как второй экземпляр **LineString** пересекается с первым экземпляром **LineString** в интервале. Они соприкасаются в бесконечном количестве точек.  
  
## <a name="examples"></a>Примеры  
В следующем примере создается простой экземпляр `geometry``MultiLineString` , содержащий два элемента `LineString` со значением SRID 0.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('MULTILINESTRING((0 2, 1 1), (1 0, 1 1))');  
```  
  
Чтобы создать экземпляр с другим значением SRID, используйте функции `STGeomFromText()` или `STMLineStringFromText()`. Можно также использовать функцию `Parse()` , а затем изменить SRID, как показано в следующем примере.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('MULTILINESTRING((0 2, 1 1), (1 0, 1 1))');  
SET @g.STSrid = 13;  
```  
  
## <a name="see-also"></a>См. также  
 [STLength (тип данных geometry)](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)   
 [STIsClosed (тип данных geometry)](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)   
 [LineString](../../relational-databases/spatial/linestring.md)   
 [Пространственные данные (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  

---
description: Создание, проектирование и создание запросов к экземплярам типа данных geography
title: Создание и конструирование географических экземпляров и отправка запросов к ним | Документация Майкрософт
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- geography data type [SQL Server]
- geodetic data type [SQL Server]
- geography data type [SQL Server], about geography data type
ms.assetid: b585851e-d15b-411f-adeb-aeabeb777c0b
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b6bbe58d7e21c5ef44c8e830daf4c69f754dcdfb
ms.sourcegitcommit: a177a1e17200400a70f1d61b737481c83249e9a3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2021
ms.locfileid: "107584074"
---
# <a name="create-construct-and-query-geography-instances"></a>Создание, проектирование и создание запросов к экземплярам типа данных geography
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
   Тип пространственных данных **geography** представляет данные в системе координат круглой земли. Этот тип реализован как тип данных среды CLR .NET в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **geography** хранит данные эллипсоидальной (сферической) Земли, такие как координаты широты и долготы GPS.  
  
 Тип **geography** является стандартным и доступен в каждой базе данных. В таблице можно создать столбцы типа **geography** и обращаться с данными **geography** так же, как с данными других предусмотренных в системе типов.  
  
##  <a name="creating-or-constructing-a-new-geography-instance"></a><a name="creating"></a> Создание или построение нового экземпляра географического объекта  
  
###  <a name="creating-a-new-geography-instance-from-an-existing-instance"></a><a name="existing"></a> Создание нового экземпляра географического объекта из существующего экземпляра  
 Тип данных **geography** содержит множество встроенных методов, с помощью которых можно создавать новые объекты **geography** на основе существующих.  
  
 **Создание буфера вокруг географического объекта**  
 [STBuffer (тип данных geography)](../../t-sql/spatial-geography/stbuffer-geography-data-type.md)  
  
 **Создание буфера вокруг географического объекта с допуском**  
 [BufferWithTolerance (тип данных geography)](../../t-sql/spatial-geography/bufferwithtolerance-geography-data-type.md)  
  
 **Создание географического объекта из пересечения двух географических объектов**  
 [STIntersection (тип данных geography)](../../t-sql/spatial-geography/stintersection-geography-data-type.md)  
  
 **Создание географического объекта из объединения двух географических объектов**  
 [STUnion (тип данных geography)](../../t-sql/spatial-geography/stunion-geography-data-type.md)  
  
 **Создание географического объекта из точек, в которых один объект не пересекается с другим**  
 [STDifference (тип данных geography)](../../t-sql/spatial-geography/stdifference-geography-data-type.md)  
  
###  <a name="constructing-a-geography-instance-from-well-known-text-input"></a><a name="wkt"></a> Построение экземпляра географического объекта на основе входных данных в формате Well-Known Text  
 Тип данных **geography** предоставляет несколько встроенных методов, позволяющих создать экземпляр типа geography на основе представления Open Geospatial Consortium (OGC) WKT. Стандарт WKT представляет собой текстовую строку, позволяющую осуществлять обмен географическими данными в текстовой форме.  
  
 **Создание экземпляра географического объекта любого типа на основе входных данных в формате WKT**  
 [STGeomFromText (тип данных geography)](../../t-sql/spatial-geography/stgeomfromtext-geography-data-type.md)  
  
 [Parse (тип данных geography)](../../t-sql/spatial-geography/parse-geography-data-type.md)  
  
 **Создание экземпляра географического объекта Point на основе входных данных в формате WKT**  
 [STPointFromText (тип данных geography)](../../t-sql/spatial-geography/stpointfromtext-geography-data-type.md)  
  
 **Создание экземпляра географического объекта MultiPoint на основе входных данных в формате WKT**  
 [STMPointFromText (тип данных geography)](../../t-sql/spatial-geography/stmpointfromtext-geography-data-type.md)  
  
 **Создание экземпляра географического объекта LineString на основе входных данных в формате WKT**  
 [STLineFromText &#40;тип данных geography&#41;](../../t-sql/spatial-geography/stlinefromtext-geography-data-type.md) 
  
 **Создание экземпляра географического объекта MultiLineString на основе входных данных в формате WKT**  
 [STMLineFromText (тип данных geography)](../../t-sql/spatial-geography/stmlinefromtext-geography-data-type.md)  
  
 **Создание экземпляра географического объекта Polygon на основе входных данных в формате WKT**  
 [STPolyFromText (тип данных geography)](../../t-sql/spatial-geography/stpolyfromtext-geography-data-type.md)  
  
 **Создание экземпляра географического объекта MultiPolygon на основе входных данных в формате WKT**  
 [STMPolyFromText (тип данных geography)](../../t-sql/spatial-geography/stmpolyfromtext-geography-data-type.md)  
  
 **Создание экземпляра географического объекта GeometryCollection на основе входных данных в формате WKT**  
 [STGeomCollFromText (тип данных geography)](../../t-sql/spatial-geography/stgeomcollfromtext-geography-data-type.md)  
  
###  <a name="constructing-a-geography-instance-from-well-known-binary-input"></a><a name="wkb"></a> Построение экземпляра географического объекта на основе входных данных в формате Well-Known Binary  
 WKB представляет собой описанный консорциумом OGC двоичный формат, позволяющий осуществлять обмен данными типа **Geography** между клиентскими приложениями и базой данных SQL. С помощью следующих функций создаются экземпляры географических объектов на основе входных данных WKB:  
  
 **Создание экземпляра географического объекта любого типа на основе входных данных в формате WKB**  
 [STGeomFromWKB (тип данных geography)](../../t-sql/spatial-geography/stgeomfromwkb-geography-data-type.md)  
  
 **Создание экземпляра географического объекта Point на основе входных данных в формате WKB**  
 [STPointFromWKB (тип данных geography)](../../t-sql/spatial-geography/stpointfromwkb-geography-data-type.md)  
  
 **Создание экземпляра географического объекта MultiPoint на основе входных данных в формате WKB**  
 [STMPointFromWKB (тип данных geography)](../../t-sql/spatial-geography/stmpointfromwkb-geography-data-type.md)  
  
 **Создание экземпляра географического объекта LineString на основе входных данных в формате WKB**  
 [STLineFromWKB (тип данных geography)](../../t-sql/spatial-geography/stlinefromwkb-geography-data-type.md)  
  
 **Создание экземпляра географического объекта MultiLineString на основе входных данных в формате WKB**  
 [STMLineFromWKB (тип данных geography)](../../t-sql/spatial-geography/stmlinefromwkb-geography-data-type.md)  
  
 **Создание экземпляра географического объекта Polygon на основе входных данных в формате WKB**  
 [STPolyFromWKB (тип данных geography)](../../t-sql/spatial-geography/stpolyfromwkb-geography-data-type.md)  
  
 **Создание экземпляра географического объекта MultiPolygon на основе входных данных в формате WKB**  
 [STMPolyFromWKB (тип данных geography)](../../t-sql/spatial-geography/stmpolyfromwkb-geography-data-type.md)  
  
 **Создание экземпляра географического объекта GeometryCollection на основе входных данных в формате WKB**  
 [STGeomCollFromWKB (тип данных geography)](../../t-sql/spatial-geography/stgeomcollfromwkb-geography-data-type.md)STGeomCollFromWKB (тип данных geography)  
  
###  <a name="constructing-a-geography-instance-from-gml-text-input"></a><a name="gml"></a> Построение экземпляра географического объекта на основе входных данных в формате GML Text  
 Тип данных **geometry** предоставляет метод, с помощью которого создается экземпляр **geography** на основе GML, XML-представления экземпляров **geography** . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает подмножество GML.  
  
 Дополнительные сведения о языке GML см. в спецификации OGC: [Спецификации OGC, географический язык разметки.](https://go.microsoft.com/fwlink/?LinkId=93629)  
  
 **Создание экземпляра географического объекта любого типа на основе входных данных в формате GML**  
 [GeomFromGML (тип данных geography)](../../t-sql/spatial-geography/geomfromgml-geography-data-type.md)  
  
##  <a name="returning-well-known-text-and-well-known-binary-from-a-geography-instance"></a><a name="returning"></a> Получение данных в формате Well-Known Text и Well-Known Binary из экземпляра географического объекта  
 Можно использовать следующие методы для получения данных экземпляра **geography** в формате WKT или формате WKB:  
  
 **Возврат WKT-представления экземпляра географического объекта**  
 [STAsText (тип данных geography)](../../t-sql/spatial-geography/stastext-geography-data-type.md)  
  
 [ToString (тип данных geography)](../../t-sql/spatial-geography/tostring-geography-data-type.md)  
  
 **Получение представления экземпляра географического объекта в формате WKT, включая все значения Z и M**  
 [AsTextZM (тип данных geography)](../../t-sql/spatial-geography/astextzm-geography-data-type.md)  
  
 **Возврат WKB-представления экземпляра географического объекта**  
 [STAsBinary (тип данных geography)](../../t-sql/spatial-geography/stasbinary-geography-data-type.md)  
  
 **Возврат GML-представления экземпляра географического объекта**  
 [AsGml (тип данных geography)](../../t-sql/spatial-geography/asgml-geography-data-type.md)  
  
##  <a name="querying-the-properties-and-behaviors-of-geography-instances"></a><a name="query"></a> Выполнение запроса к свойствам и методам экземпляров географических объектов  
 Все экземпляры **geometry** обладают рядом свойств, получить которые можно с помощью методов, предоставляемых в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . В следующих разделах определяются свойства и поведение географических типов, а также методы запросов к каждому из них.  
  
###  <a name="validity-instance-type-and-geometrycollection-information"></a><a name="valid"></a> Допустимость, тип экземпляра и сведения GeometryCollection  
 Когда экземпляр **geography** создан, можно использовать следующие методы, чтобы получить типа экземпляра или, если это экземпляр **GeometryCollection** , конкретный экземпляр **geography** .  
  
 **Возврат типа географического экземпляра**  
 [STGeometryType (тип данных geography)](../../t-sql/spatial-geography/stgeometrytype-geography-data-type.md)  
  
 **Определение принадлежности географического экземпляра к заданному типу**  
 [InstanceOf (тип данных geography)](../../t-sql/spatial-geography/instanceof-geography-data-type.md)  
  
 **Проверка соответствия формата экземпляра географического объекта его типу**  
 [STNumGeometries (тип данных geography)](../../t-sql/spatial-geography/stnumgeometries-geography-data-type.md)  
  
 **Возврат конкретного географического экземпляра из экземпляра GeometryCollection**  
 [STGeometryN (тип данных geography)](../../t-sql/spatial-geography/stgeometryn-geography-data-type.md)STGeometryN (тип данных geography)  
  
###  <a name="number-of-points"></a><a name="number"></a> Число точек  
 Все непустые экземпляры **geography** состоят из *точек*. Данные точки представляют координаты широты и долготы, соответствующие месту создания экземпляров **geography** . В типе данных **geography** предусмотрены многочисленные встроенные методы запросов к точкам экземпляра.  
  
 **Получение числа точек, образующих экземпляр**  
 [STNumPoints (тип данных geography)](../../t-sql/spatial-geography/stnumpoints-geography-data-type.md)  
  
 **Получение выбранной точки в экземпляре**  
 [STPointN (тип данных geometry)](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md)  
  
 **Получение начальной точки экземпляра**  
 [STStartPoint (тип данных geography)](../../t-sql/spatial-geography/ststartpoint-geography-data-type.md)  
  
 **Получение конечной точки экземпляра**  
 [STEndpoint (тип данных geography)](../../t-sql/spatial-geography/stendpoint-geography-data-type.md)  
  
###  <a name="dimension"></a><a name="dimension"></a> Измерение  
 Непустой объект **geography** может иметь 0, 1 или 2 измерения. Объекты **geography** , имеющие 0 измерений, например **Point** и **MultiPoint**, не имеют ни длины, ни площади. Одномерные объекты, такие как **LineString, CircularString**, **CompoundCurve** и **MultiLineString**, имеют длину. Двухмерные экземпляры, такие как **Polygon, CurvePolygon** и **MultiPolygon**, имеют длину и площадь. В отчете пустых экземпляров указывается измерение -1, а в отчетах **GeometryCollection** — максимальное измерение содержимого.  
  
 **Получение измерения экземпляра**  
 [STDimension (тип данных geography)](../../t-sql/spatial-geography/stdimension-geography-data-type.md)  
  
 **Получение длины экземпляра**  
 [STLength (тип данных geography)](../../t-sql/spatial-geography/stlength-geography-data-type.md)  
  
 **Получение площади экземпляра**  
 [STArea (тип данных geography)](../../t-sql/spatial-geography/starea-geography-data-type.md)  
  
###  <a name="empty"></a><a name="empty"></a> Пустой  
 _Пустым_ называется экземпляр объекта **geography**, не содержащий ни одной точки. Длина пустых экземпляров **LineString, CircularString**, **CompoundCurve** и **MultiLineString** равна 0. Площадь пустых экземпляров **Polygon, CurvePolygon** и **MultiPolygon** равна 0.  
  
 **Проверка, является ли экземпляр пустым**  
 [STIsEmpty (тип данных geography)](../../t-sql/spatial-geography/stisempty-geography-data-type.md)  
  
###  <a name="closure"></a><a name="closure"></a> Замыкание  
 _Замкнутый_ экземпляр **geography** — это фигура, начальная и конечная точки которой совпадают. Экземпляры **Polygon** считаются замкнутыми. Экземпляры **Point** не замкнуты.  
  
 Кольцо — это простой замкнутый экземпляр **LineString** .  
  
 **Определение, является ли экземпляр замкнутым**  
 [STIsClosed (тип данных geography)](../../t-sql/spatial-geography/stisclosed-geography-data-type.md)  
  
 **Получение количества колец экземпляра объекта Polygon**  
 [NumRings (тип данных geography)](../../t-sql/spatial-geography/numrings-geography-data-type.md)  
  
 **Получение указанного кольца какого-либо географического объекта**  
 [RingN (тип данных geography)](../../t-sql/spatial-geography/ringn-geography-data-type.md)  
  
###  <a name="spatial-reference-id-srid"></a><a name="srid"></a> Идентификатор пространственной ссылки (SRID)  
 Идентификатор пространственной ссылки (SRID) представляет собой идентификатор, указывающий на систему эллиптических координат, в которой находится экземпляр **geography** . Сравнение двух экземпляров **geography** с различными идентификаторами SRID невозможно.  
  
 **Задание или возврат идентификатора SRID экземпляра**  
 [STSrid (тип данных geography)](../../t-sql/spatial-geography/stsrid-geography-data-type.md)  
  
 Это свойство можно изменять.  
  
##  <a name="determining-relationships-between-geography-instances"></a><a name="rel"></a> Определение связей между экземплярами географических объектов  
 Тип данных **geography** предоставляет множество встроенных методов, с помощью которых можно определить связи между двумя объектами типа **geography** .  
  
 **Определение возможного наличия одинакового набора точек в двух объектах**  
 [STEquals (тип данных geometry)](../../t-sql/spatial-geometry/stequals-geometry-data-type.md)  
  
 **Определение отсутствия перекрытия двух объектов**  
 [STDisjoint (тип данных geometry)](../../t-sql/spatial-geometry/stdisjoint-geometry-data-type.md)  
  
 **Определение пересечения двух объектов**  
 [STIntersects (тип данных geometry)](../../t-sql/spatial-geometry/stintersects-geometry-data-type.md)  
  
 **Определение точки или точек пересечения двух объектов**  
 [STIntersection (тип данных geography)](../../t-sql/spatial-geography/stintersection-geography-data-type.md)  
  
 **Определение кратчайшего расстояния между точками двух географических объектов**  
 [STDistance (тип данных geometry)](../../t-sql/spatial-geometry/stdistance-geometry-data-type.md)  
  
 **Определение разницы в точках между двумя географическими объектами**  
 [STDifference (тип данных geography)](../../t-sql/spatial-geography/stdifference-geography-data-type.md)  
  
 **Получение симметрической разницы (или уникальных точек) между двумя экземплярами географических объектов**  
 [STSymDifference (тип данных geography)](../../t-sql/spatial-geography/stsymdifference-geography-data-type.md)  
  
##  <a name="geography-instances-must-use-supported-srid"></a><a name="supportedsrid"></a> Обязательное использование поддерживаемых SRID в экземплярах географических объектах  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживаются идентификаторы SRID, основанные на стандартах EPSG. При выполнении вычислений или применении методов работы с географическими пространственными данными должны использоваться поддерживаемые [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]идентификаторы SRID для экземпляров **geography** . Заданный SRID должен соответствовать одному из идентификаторов SRID, отображенных в представлении каталога **sys.spatial_reference_systems** . Как было упомянуто ранее, результаты вычислений на пространственных данных с использованием типа данных **geography** зависят от эллипсоида, использованного при создании рабочих данных, поскольку каждому эллипсоиду назначается отдельный идентификатор пространственной ссылки (SRID).  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в **geometry** . Если используются данные системы пространственных ссылок, отличной от WGS 84 (или если значение SRID отличается от 4326), для собственных географических пространственных данных необходимо определить отдельный идентификатор SRID.  
  
##  <a name="examples"></a><a name="examples"></a> Примеры  
В следующих примерах иллюстрируется добавление и запрос географических данных.  
  
### <a name="example-a"></a>Пример А. 
В этом примере создается таблица со столбцом идентификаторов и столбцом `GeogCol1` типа `geography`. Третий столбец обрабатывает столбец `geography` для представления в формате известного текста (WKT) OGC, используя метод `STAsText()` . Затем вставляются две строки: одна строка содержит объект `LineString` типа `geography`, а другая — объект `Polygon` .  
  
```sql  
IF OBJECT_ID ( 'dbo.SpatialTable', 'U' ) IS NOT NULL   
DROP TABLE dbo.SpatialTable;  
GO  
  
CREATE TABLE SpatialTable   
  ( id int IDENTITY (1,1),  
    GeogCol1 geography,   
    GeogCol2 AS GeogCol1.STAsText()
   );  
GO  
  
INSERT INTO SpatialTable (GeogCol1)  
VALUES (geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326));  
  
INSERT INTO SpatialTable (GeogCol1)  
VALUES (geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326));  
GO  
```  
  
### <a name="example-b"></a>Пример Б.
В этом примере метод `STIntersection()` используется для получения точек, в которых пересекаются два вставленных ранее объекта `geography`.  
  
```sql  
DECLARE @geog1 geography;  
DECLARE @geog2 geography;  
DECLARE @result geography;  
  
SELECT @geog1 = GeogCol1 FROM SpatialTable WHERE id = 1;  
SELECT @geog2 = GeogCol1 FROM SpatialTable WHERE id = 2;  
SELECT @result = @geog1.STIntersection(@geog2);  
SELECT @result.STAsText();  
```  
  
## <a name="see-also"></a>См. также  
 [Пространственные данные (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  

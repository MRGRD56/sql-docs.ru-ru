---
description: STCurveToLine (тип данных geometry)
title: STCurveToLine (тип данных geometry) | Документы Майкрософт
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- STCurveToLine method (geometry)
ms.assetid: abc80b32-4152-4e10-b816-798b901e0ac5
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 81b1a222937a19a8fe18e3cb2261581011206d1b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189500"
---
# <a name="stcurvetoline-geometry-data-type"></a>STCurveToLine (тип данных geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Возвращает приближение из многоугольников для экземпляра **geometry**, содержащего сегменты дуги.
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.STCurveToLine ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Тип возвращаемых данных CLR: **SqlGeometry**  
  
## <a name="remarks"></a>Remarks  
 Возвращает пустой экземпляр **GeometryCollection** для пустых переменных экземпляра **geometry** и возвращает значение **NULL** для неинициализированных переменных **geometry**.  
  
 Приближение из многоугольников, возвращаемое методом, зависит от экземпляра **geometry**, с помощью которого был вызван метод:  
  
-   Возвращает экземпляр **LineString** для экземпляра **CircularString** или **CompoundCurve**.  
  
-   Возвращает экземпляр **Polygon** для экземпляра **CurvePolygon**.  
  
-   Возвращает копию экземпляра **geometry**, если экземпляр не является экземпляром **CircularString**, **CompoundCurve** или **CurvePolygon**. Например, метод `STCurveToLine` возвращает экземпляр **Point** для экземпляра **geometry**, который является экземпляром **Point**.  
  
 В отличие от спецификации SQL/MM, метод `STCurveToLine` не использует значения координат z для расчета аппроксимации из многоугольников. Любое значение координаты z, представленное в вызываемом экземпляре **geometry**, игнорируется.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-an-uninitialized-geometry-variable-and-empty-instance"></a>A. Использование неинициализированной переменной геометрии и пустого экземпляра  
 В приведенном ниже примере первая инструкция **SELECT** вызывает метод `STCurveToLine` с помощью неинициализированного экземпляра **geometry**, а вторая инструкция **SELECT** использует пустой экземпляр **geometry**. Таким образом, метод возвращает значение **NULL** для первой инструкции и коллекцию **GeometryCollection** для второй инструкции.  
  
```
 DECLARE @g geometry; 
 SET @g = @g.STCurveToLine(); 
 SELECT @g.STGeometryType(); 
 SET @g = geometry::Parse('LINESTRING EMPTY'); 
 SELECT @g.STGeometryType();
 ```  
  
### <a name="b-using-a-linestring-instance"></a>Б. Использование экземпляра объекта LineString  
 Инструкция **SELECT** в приведенном ниже примере использует экземпляр **LineString** для вызова метода STCurveToLine. Таким образом, этот метод возвращает экземпляр **LineString**.  
  
```
 DECLARE @g geometry; 
 SET @g = geometry::Parse('LINESTRING(1 3, 5 5, 4 3, 1 3)'); 
 SET @g = @g.STCurveToLine(); 
 SELECT @g.STGeometryType();
 ```  
  
### <a name="c-using-a-circularstring-instance"></a>В. Использование экземпляра объекта CircularString  
 Инструкция **SELECT** в приведенном ниже примере использует экземпляр **CircularString** для вызова метода STCurveToLine. Таким образом, этот метод возвращает экземпляр **LineString**. Эта инструкция **SELECT** также сравнивает длину двух экземпляров, которые приблизительно одинаковы.  Наконец, вторая инструкция **SELECT** возвращает число точек для каждого экземпляра.  Она возвращает только 5 точек для экземпляра **CircularString**, но 65 точек для экземпляра **LineString**.  
  
```
 DECLARE @g1 geometry, @g2 geometry; 
 SET @g1 = geometry::Parse('CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0)'); 
 SET @g2 = @g1.STCurveToLine(); 
 SELECT @g1.STGeometryType() AS [G1 Type], @g2.STGeometryType() AS [G2 Type], @g1.STLength() AS [G1 Perimeter], @g2.STLength() AS [G2 Perimeter], @g2.ToString() AS [G2 Def]; 
 SELECT @g1.STNumPoints(), @g2.STNumPoints();
 ```  
  
### <a name="d-using-a-curvepolygon-instance"></a>Г. Использование экземпляра объекта CurvePolygon  
 Инструкция **SELECT** в приведенном ниже примере использует экземпляр **CurvePolygon** для вызова метода STCurveToLine. Таким образом, этот метод возвращает экземпляр **Polygon**.  
  
```
 DECLARE @g1 geometry, @g2 geometry; 
 SET @g1 = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0))'); 
 SET @g2 = @g1.STCurveToLine(); 
 SELECT @g1.STGeometryType() AS [G1 Type], @g2.STGeometryType() AS [G2 Type];
 ```  
  
## <a name="see-also"></a>См. также:  
 [Основные сведения о типах пространственных данных](../../relational-databases/spatial/spatial-data-types-overview.md)   
 [STLength (тип данных geometry)](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)   
 [STNumPoints (тип данных geometry)](../../t-sql/spatial-geometry/stnumpoints-geometry-data-type.md)   
 [STGeometryType (тип данных geometry)](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)  
  
  


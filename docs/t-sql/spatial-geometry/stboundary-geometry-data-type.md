---
description: STBoundary (тип данных geometry)
title: STBoundary (тип данных geometry) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STBoundary (geometry Data Type)
- STBoundary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STBoundary (geometry Data Type)
ms.assetid: f0551674-e6e8-4926-9038-df03f2c807d7
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d1585e69f471ef206ac5d0105bd2427ec3ed707b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204315"
---
# <a name="stboundary-geometry-data-type"></a>STBoundary (тип данных geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Задает границу экземпляра **geometry**.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.STBoundary ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Тип возвращаемых данных CLR: **SqlGeometry**  
  
## <a name="remarks"></a>Remarks  
 `STBoundary()` возвращает пустую коллекцию **GeometryCollection**, когда конечные точки для экземпляра **LineString**, **CircularString** или **CompoundCurve** совпадают.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-stboundary-on-a-linestring-instance-with-different-endpoints"></a>A. Использование STBoundary() в экземпляре LineString с разными конечными точками  
 В следующем примере создается экземпляр `LineString``geometry`. `STBoundary()` возвращает границы `LineString`.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 0 2, 2 0)', 0);  
SELECT @g.STBoundary().ToString();  
```  
  
### <a name="b-using-stboundary-on-a-linestring-instance-with-the-same-endpoints"></a>Б. Использование STBoundary() в экземпляре LineString с одинаковыми конечными точками  
 В следующем примере создается действительный экземпляр `LineString` с одинаковыми конечными точками. `STBoundary()` возвращает пустую коллекцию `GeometryCollection`.  
  
```
 DECLARE @g geometry;  
 SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 0 2, -2 2, 0 0)', 0);  
 SELECT @g.STBoundary().ToString();
 ```  
  
### <a name="c-using-stboundary-on-a-curvepolygon-instance"></a>В. Использование STBoundary() в экземпляре CurvePolygon  
 В следующем примере `STBoundary()` используется в пустом экземпляре `CurvePolygon`. `STBoundary()` возвращает экземпляр `CircularString`.  
  
```
 DECLARE @g geometry;  
 SET @g = geometry::STGeomFromText('CURVEPOLYGON(CIRCULARSTRING(0 0, 2 2, 0 2, -2 2, 0 0))', 0);  
 SELECT @g.STBoundary().ToString();
 ```  
  
## <a name="see-also"></a>См. также:  
 [Методы OGC в экземплярах Geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

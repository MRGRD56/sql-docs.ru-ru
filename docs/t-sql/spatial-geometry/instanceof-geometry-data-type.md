---
description: InstanceOf (тип данных geometry)
title: InstanceOf (тип данных geometry) | Документы Майкрософт
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- InstanceOf
- InstanceOf_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- InstanceOf (geometry Data Type)
ms.assetid: fdea1248-29a4-4bab-a60d-a1b359b5e109
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b1f57375c45bd6fcd78ff5f7e535b36cd2c89834
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159436"
---
# <a name="instanceof-geometry-data-type"></a>InstanceOf (тип данных geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Метод, который проверяет принадлежность экземпляра **geometry** к указанному типу. Возвращает значение 1, если тип экземпляра **geometry** соответствует указанному типу. Этот метод также возвращает значение 1, если указанный тип является предком типа экземпляра. В противном случае этот метод всегда возвращает значение 0.
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.InstanceOf (geometry_type )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*geometry_type*  
Строка **nvarchar(4000)**, задающая один из 15 типов, доступных в иерархии типов **geometry**.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **bit**  
  
 Тип возвращаемых данных CLR: **SqlBoolean**  
  
## <a name="remarks"></a>Комментарии  
 Входные данные метода должны быть одного из следующих типов: **Geometry**, **Point**, **Curve**, **LineString**, **CircularString**, **CompoundCurve**, **Surface**, **Polygon**, **CurvePolygon**, **GeometryCollection**, **MultiSurface**, **MultiPolygon**, **MultiCurve**, **MultiLineString** или **MultiPoint**. Если в качестве входного аргумента указана любая другая строка, этот метод вызовет исключение **ArgumentException**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере создается экземпляр `MultiPoint` и производится вызов метода `InstanceOf()`, позволяющего определить, является ли экземпляр `GeometryCollection`.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('MULTIPOINT(0 0, 13.5 2, 7 19)', 0);  
SELECT @g.InstanceOf('GEOMETRYCOLLECTION');  
```  
  
## <a name="see-also"></a>См. также:  
 [Расширенные методы экземпляров Geometry](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  
  


---
description: STInteriorRingN (тип данных geometry)
title: STInteriorRingN (тип данных geometry) | Документы Майкрософт
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STInteriorRingN_TSQL
- STInteriorRingN (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STInteriorRingN (geometry Data Type)
ms.assetid: 47310f9f-2cdb-41e0-a6da-7c3cfbf139ac
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b1df2763f3deadb459d352c1949a567d060b65fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198255"
---
# <a name="stinteriorringn-geometry-data-type"></a>STInteriorRingN (тип данных geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает указанное внутреннее кольцо экземпляра **Polygongeometry**.
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.STInteriorRingN ( expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *expression*  
 Выражение типа **int** от 1 до количества внутренних колец в экземпляре **geometry**.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Тип возвращаемых данных CLR: **SqlGeometry**  
  
 Тип открытого геопространственного консорциума (OGC): **LineString**  
  
## <a name="remarks"></a>Примечания  
 Этот метод возвращает значение **NULL**, если экземпляр **geometry** не является многоугольником. Этот метод также вызывает исключение **ArgumentOutOfRangeException**, если значение выражения больше количества колец. Количество колец можно получить с помощью метода `STNumInteriorRing``()`.  
  
## <a name="examples"></a>Примеры  
 В приведенном ниже примере создается экземпляр `Polygon` и используется метод `STInteriorRingN()` для возврата внутреннего кольца многоугольника в виде **LineString**.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);  
SELECT @g.STInteriorRingN(1).ToString();  
```  
  
## <a name="see-also"></a>См. также  
 [Методы OGC в экземплярах Geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  


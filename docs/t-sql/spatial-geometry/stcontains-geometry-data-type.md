---
description: STContains (тип данных geometry)
title: STContains (тип данных geometry) | Документы Майкрософт
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STContains (geometry Data Type)
- STContains_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STContains (geometry Data Type)
ms.assetid: 865ceca1-9200-45ed-a7d8-e286e2679fdc
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 69290b85b4788f0e35c4fb48cc05222211cd0413
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "88305791"
---
# <a name="stcontains-geometry-data-type"></a>STContains (тип данных geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Возвращает значение 1, если экземпляр **geometry** полностью содержит другой экземпляр **geometry**. В противном случае возвращается значение 0.
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.STContains ( other_geometry )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *other_geometry*  
 Другой экземпляр **geometry** для сравнения с экземпляром, для которого вызван метод `STContains()`.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **bit**  
  
 Тип возвращаемых данных CLR: **SqlBoolean**  
  
## <a name="remarks"></a>Комментарии  
 Метод `STContains()` всегда возвращает значение NULL, если идентификаторы пространственных ссылок (SRID) экземпляров **geometry** не совпадают.  
  
## <a name="examples"></a>Примеры  
 В следующем примере используется метод `STContains()`, чтобы проверить два экземпляра `geometry` и выяснить, заключается ли второй экземпляр внутри первого.  
  
```  
DECLARE @g geometry;  
DECLARE @h geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 2 0, 2 2, 0 2, 0 0))', 0);  
SET @h = geometry::STGeomFromText('POINT(1 1)', 0);  
SELECT @g.STContains(@h);  
```  
  
## <a name="see-also"></a>См. также  
 [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [Методы OGC в экземплярах Geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  


---
description: STPolyFromWKB (тип данных geometry)
title: STPolyFromWKB (тип данных geometry) | Документы Майкрософт
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STPolyFromWKB_TSQL
- STPolyFromWKB (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STPolyFromWKB (geometry Data Type)
ms.assetid: 8e8f0c41-0c62-4919-9d4c-d37c93fdd31c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: c81eaf173324e9f47ce77775a1ea64828b24c613
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184608"
---
# <a name="stpolyfromwkb-geometry-data-type"></a>STPolyFromWKB (тип данных geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает экземпляр **geometryPolygon** из WKB-представления открытого геопространственного консорциума (OGC).
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
STPolyFromWKB ( 'WKB_polygon' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *WKB_polygon*  
 WKB-представление возвращаемого экземпляра **geometryPolygon**. *WKB_polygon* — это выражение типа **varbinary(max)**.  
  
 *SRID*  
 Выражение типа **int**, представляющее идентификатор пространственной ссылки (SRID) возвращаемого экземпляра **geometryPolygon**.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Тип возвращаемых данных CLR: **SqlGeometry**  
  
 Тип OGC: **Polygon**  
  
## <a name="remarks"></a>Remarks  
 Этот метод вызывает исключение **FormatException**, если входные данные представлены в неверном формате.  
  
## <a name="examples"></a>Примеры  
 В следующем примере метод `STPolyFromWKB()` применяется для создания экземпляра `geometry`.  
  
```  
DECLARE @g geometry;   
SET @g = geometry::STPolyFromWKB(0x0103000000010000000400000000000000000014400000000000001440000000000000244000000000000014400000000000002440000000000000244000000000000014400000000000001440, 0);  
SELECT @g.STAsText();  
```  
  
## <a name="see-also"></a>См. также:  
 [Статические геометрические методы OGC](../../t-sql/spatial-geometry/ogc-static-geometry-methods.md)  
  
  


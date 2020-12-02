---
description: STNumPoints (тип данных geometry)
title: STNumPoints (тип данных geometry) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STNumPoints_TSQL
- STNumPoints (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STNumPoints (geometry Data Type)
ms.assetid: a19520fc-7f91-4a2c-856f-4d8b99a7e496
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 60c48dd009ed98e4faf5229f9f005f3bd5fb6ad9
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "88467396"
---
# <a name="stnumpoints-geometry-data-type"></a>STNumPoints (тип данных geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает суммарное количество точек в каждой из фигур в экземпляре **geometry**.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.STNumPoints ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **int**  
  
 Тип возвращаемых данных CLR: **SqlInt32**  
  
## <a name="remarks"></a>Примечания  
 Этот метод подсчитывает число точек в описании экземпляра **geometry**. Повторяющиеся точки учитываются. Если этот экземпляр принадлежит к типу **collection**, то метод возвращает сумму точек в каждом из его элементов.  
  
## <a name="examples"></a>Примеры  
 В следующем примере создается экземпляр `LineString` и вызывается метод `STNumPoints()`, определяющий число точек, использованных в его описании.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);  
SELECT @g.STNumPoints();  
```  
  
## <a name="see-also"></a>См. также:  
 [Методы OGC в экземплярах Geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

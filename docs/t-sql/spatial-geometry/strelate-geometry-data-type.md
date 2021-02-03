---
description: STRelate (тип данных geometry)
title: STRelate (тип данных geometry) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STRelate (geometry Data Type)
- STRelate_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STRelate (geometry Data Type)
ms.assetid: 9dcb5f58-35ab-4bb3-86ee-2d29eefba6d3
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 1e5e2a0ed17d34c178798770d386bf3a4dc9fe3d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184596"
---
# <a name="strelate-geometry-data-type"></a>STRelate (тип данных geometry)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает значение 1, если экземпляр **geometry** связан с другим экземпляром **geometry**, где связь определяется значением шаблона матрицы DE-9IM; в противном случае возвращается 0.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.STRelate ( other_geometry, intersection_pattern_matrix )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *other_geometry*  
 Другой экземпляр **geometry** для сравнения с экземпляром, для которого вызван метод `STRelate()`.  
  
 *intersection_pattern_matrix*  
 Строка типа **nchar(9)** кодирует приемлемые значения для устройства шаблона матрицы DE-9IM между двумя экземплярами **geometry**.  
  
## <a name="remarks"></a>Комментарии  
 Этот метод всегда возвращает значение NULL, если у экземпляров **geometry** не совпадают идентификаторы пространственных ссылок (SRID). Этот метод вызывает исключение **ArgumentException**, если формат матрицы неправильный.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **bit**  
  
 Тип возвращаемых данных CLR: **SqlBoolean**  
  
## <a name="examples"></a>Примеры  
 В приведенном ниже примере используется `STRelate()`, чтобы проверить отсутствие пространственного перекрытия двух экземпляров **geometry** с использованием явного шаблона DE-9IM.  
  
```  
DECLARE @g geometry;  
DECLARE @h geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 2, 2 0, 4 2)', 0);  
SET @h = geometry::STGeomFromText('POINT(5 5)', 0);  
SELECT @g.STRelate(@h, 'FF*FF****');  
```  
  
## <a name="see-also"></a>См. также  
 [Методы OGC в экземплярах Geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

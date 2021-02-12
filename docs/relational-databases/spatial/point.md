---
description: Точка
title: Точка | Документация Майкрософт
ms.date: 02/02/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- Point geometry subtype [SQL Server]
- geometry data type [SQL Server], spatial data
ms.assetid: 2a596ec4-8b2f-4962-bcb4-e5c8f77edad5
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bcbdda427a587e06bf1ad678939b8164178112dd
ms.sourcegitcommit: 05fc736e6b6b3a08f503ab124c3151f615e6faab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2021
ms.locfileid: "99478579"
---
# <a name="point"></a>Точка
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  В пространственных данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] экземпляр **Point** является объектом без измерения, представляющим отдельное месторасположение, и может содержать значения Z (уровень) и M (мера).  
  
## <a name="geography-data-type"></a>Тип данных Geography  
 Тип Point для типа данных geography представляет единичное расположение, где *Lat* представляет широту, а *Long* — долготу. Значения широты и долготы измеряются в градусах. Значения широты всегда находятся в интервале [-90, 90]. Все значения, находящиеся вне этого диапазона, вызывают исключение. Значения долготы всегда находятся в интервале [-180, 180]. Все значения, находящиеся вне этого диапазона, преобразуются в соответствующие значения в его пределах. Например, если введено значение долготы 190, то оно будет преобразовано в значение -170. *SRID* представляет идентификатор пространственной ссылки экземпляра **geography** , который необходимо вернуть.  
  
## <a name="geometry-data-type"></a>Тип данных Geometry  
 Тип элемента для геометрических типов данных представляет собой единое место, где *X* представляет координату x создаваемого экземпляра Point, а *Y* — координату Y создаваемого экземпляра Point. *SRID* представляет идентификатор пространственной ссылки экземпляра **geometry** , который необходимо вернуть.  
  
## <a name="examples"></a>Примеры  
### <a name="example-a"></a>Пример А.
В следующем примере показано создание экземпляра геометрической точки, представляющего точку (3, 4) со значением SRID, равным 0.  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POINT (3 4)', 0);  
```  
  
### <a name="example-b"></a>Пример Б.
В приведенном ниже примере показано создание экземпляра геометрической точки, представляющего точку (3, 4) со значениями Z (уровень) и M (мера), равными соответственно 7 и 2,5, и значением SRID по умолчанию, равным 0.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::Parse('POINT(3 4 7 2.5)');  
```  
  
### <a name="example-c"></a>Пример В.
В приведенном ниже примере возвращаются значения X, Y, Z и M для экземпляра геометрической точки.  
  
```  
SELECT @g.STX;  
SELECT @g.STY;  
SELECT @g.Z;  
SELECT @g.M;  
```  
  
### <a name="example-d"></a>Пример Г.
Для Z и M может быть явно указано значение NULL, как показано в следующем примере.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::Parse('POINT(3 4 NULL NULL)');  
```  
  
## <a name="see-also"></a>См. также  
 [MultiPoint](../../relational-databases/spatial/multipoint.md)   
 [STX (тип данных geometry)](../../t-sql/spatial-geometry/stx-geometry-data-type.md)   
 [STY (тип данных geometry)](../../t-sql/spatial-geometry/sty-geometry-data-type.md)   
 [Пространственные данные (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  

---
description: BufferWithTolerance (тип данных geometry)
title: BufferWithTolerance (тип данных geometry) | Документы Майкрософт
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BufferWithTolerance_TSQL
- BufferWithTolerance
dev_langs:
- TSQL
helpviewer_keywords:
- BufferWithTolerance (geometry Data Type)
ms.assetid: 7049d37a-3e72-4e93-87a1-c96a6f0e2b99
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: b84197d386d92da8593449142ae3e13b54b8a605
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205793"
---
# <a name="bufferwithtolerance-geometry-data-type"></a>BufferWithTolerance (тип данных geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает геометрический объект, представляющий объединение всех точек, расстояние от которых до указанного экземпляра **geometry** не превышает заданного значения с определенной погрешностью.
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.BufferWithTolerance ( distance, tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *distance*  
 Это выражение типа **float** задает расстояние от экземпляра **geometry**, для которого вычисляется буфер.  
  
 *tolerance*  
 Выражение типа **float**, задающее погрешность буферного расстояния.  
  
 Под *погрешностью* понимается максимальное отклонение от идеальной буферной дистанции для возвращаемого линейного приближения.  
  
 Например, идеальной границей буфера для точки является окружность, однако ее необходимо приблизительно изобразить многоугольником. Чем меньше заданная погрешность, тем из большего числа точек должен состоять многоугольник. Это увеличивает сложность результата, но уменьшает его погрешность.  
  
 *relative*  
 Значение типа **bit**, указывающее значение *tolerance*: относительное или абсолютное. Если задано значение TRUE или 1, то используется относительная погрешность, которая вычисляется как произведение параметра *tolerance* и диаметра ограничивающего прямоугольника экземпляра. Если этот аргумент имеет значение FALSE или 0, то погрешность является абсолютной, а значение *tolerance* задает максимальное абсолютное отклонение от идеальной буферной дистанции для возвращаемого линейного приближения.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geometry**  
  
 Тип возвращаемых данных CLR: **SqlGeometry**  
  
## <a name="exceptions"></a>Исключения  
 Параметр *tolerance* должен быть больше нуля. Если *tolerance* <= 0, вызывается исключение `System.ArgumentOutOfRangeException`.  
  
> [!NOTE]  
>  Так как параметр *tolerance* имеет тип **float**, то может возникнуть исключение `System.Runtime.InteropServices.COMException`, если значение, заданное в качестве погрешности, слишком мало, из-за особенностей округления типов с плавающей запятой.  
  
## <a name="remarks"></a>Комментарии  
 Когда *distance* > 0, возвращается экземпляр **Polygon** или **MultiPolygon**.  
  
> [!NOTE]  
>  Так как аргумент distance относится к типу **float**, то в расчетах очень маленькое значение может быть приравнено к нулю. Когда это происходит, возвращается экземпляр **geometry**. См. статью [Типы данных float и real (Transact-SQL)](../../t-sql/data-types/float-and-real-transact-sql.md).  
  
 Когда *distance* = 0, возвращается копия вызывающего экземпляра **geometry**.  
  
 Когда *distance* < 0, то  
  
-   возвращается пустой экземпляр **GeometryCollection**, если измерения экземпляра — 0 или 1.  
  
-   Возвращается отрицательный буфер, если измерений экземпляра 2 или более.  
  
    > [!NOTE]  
    >  Отрицательный буфер может также создать пустой экземпляр **GeometryCollection**.  
  
 Отрицательный буфер удаляет все точки на указанном расстоянии от границы экземпляра **geometry**.  
  
 Ошибкой между теоретическим и вычисляемым буфером является max(tolerance, extents \* 1.E-7), где погрешность является значением параметра *tolerance*. Дополнительные сведения об экстентах см. в статье [Справочник по методам типа данных geometry](./spatial-types-geometry-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
 В следующем примере создается экземпляр `Point`, а метод `BufferWithTolerance()` получает приблизительный буфер вокруг этого экземпляра.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POINT(3 3)', 0);  
SELECT @g.BufferWithTolerance(1, .5, 0).ToString();  
```  
  
## <a name="see-also"></a>См. также  
 [STBuffer (тип данных geometry)](../../t-sql/spatial-geometry/stbuffer-geometry-data-type.md)   
 [Расширенные методы экземпляров Geometry](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  

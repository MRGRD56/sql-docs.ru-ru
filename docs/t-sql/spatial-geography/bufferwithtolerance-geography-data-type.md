---
description: BufferWithTolerance (тип данных geography)
title: BufferWithTolerance (тип данных geography) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
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
- BefferWithTolerance method
ms.assetid: f1783e6b-0f17-464f-b1c7-1c3f7d8aa042
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 51be64effcbbb75abe7830949d8bdf83b5637307
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205045"
---
# <a name="bufferwithtolerance-geography-data-type"></a>BufferWithTolerance (тип данных geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает геометрический объект, представляющий объединение всех точек, расстояние от которых до заданного экземпляра **geography** не превышает заданного значения с указанной погрешностью.  
  
Этот метод типа данных geography поддерживает экземпляры **FullGlobe** или пространственные экземпляры, размер которых больше полушария.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.BufferWithTolerance ( distance, tolerance, relative )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
_distance_  
Это выражение типа **float** задает расстояние от экземпляра **geography**, для которого вычисляется буфер.  
  
Максимальное расстояние при вычислении буфера не может превышать 0,999 \* _π_ * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 окружности Земли) или полную окружность Земли.  
  
_tolerance_  
Выражение типа **float**, задающее погрешность буферного расстояния.  
  
Максимальное отклонение в идеальном буферном расстоянии для возвращенной линейной аппроксимации — это значение _tolerance_.  
  
Например, идеальной границей буфера для точки является окружность, однако ее необходимо приблизительно изобразить многоугольником. Чем меньше погрешность, тем из большего количества точек будет состоять многоугольник. Это увеличивает сложность результата, но уменьшает его погрешность.  
  
Минимальный предел составляет 0,1 % от расстояния, и любая меньшая погрешность будет округлена до этого значения.  
  
_relative_  
Значение типа **bit**, указывающее значение _tolerance_: относительное или абсолютное. Если значение равно "TRUE" или 1, погрешность является относительной. Это значение рассчитывается как произведение параметра _tolerance_ и углового фактора \* экваториальный радиус эллипсоида. Погрешность является абсолютной, если значение равно "FALSE" или 0. Это значение _tolerance_ является абсолютным максимальным отклонением в идеальном буферном расстоянии для возвращенной линейной аппроксимации.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
Тип возвращаемых данных CLR: **SqlGeography**  
  
## <a name="remarks"></a>Remarks  
Этот метод выдает исключение **ArgumentException**, если _distance_ не является числом (NAN) или если _distance_ равно плюс/минус бесконечности.  Этот метод также выдает исключение **ArgumentException**, если _tolerance_ равно нулю (0), не является числом (NAN), отрицательное число или равно плюс/минус бесконечности.  
  
`STBuffer()` возвращает экземпляр **FullGlobe** в определенных случаях; например, `STBuffer()` возвращает экземпляр **FullGlobe** для двух полюсов, если буферное расстояние больше, чем расстояние от экватора до полюса.  
  
Этот метод вызовет исключение **ArgumentException** в тех экземплярах **FullGlobe**, где расстояние буфера превышает следующее ограничение:  
  
0,999 \* _π_ * minorAxis \* minorAxis / majorAxis (~0,999 \* 1/2 окружности Земли)  
  
Ошибкой между теоретическим и вычисляемым буфером является max(tolerance, extents \* 1.E-7), где погрешность является значением параметра _tolerance_. Дополнительные сведения об экстентах см. в разделе [Справочник по методам типа данных geography](./stequals-geography-data-type.md).  
  
Этот метод не является точным.  
  
## <a name="examples"></a>Примеры  
В следующем примере создается экземпляр `Point`, а метод `BufferWithTolerance()` получает приблизительный буфер вокруг этого экземпляра.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POINT(-122.34900 47.65100)', 4326);  
SELECT @g.BufferWithTolerance(1, .5, 0).ToString();  
```  
  
## <a name="see-also"></a>См. также  
[STBuffer (тип данных geography)](../../t-sql/spatial-geography/stbuffer-geography-data-type.md)   
[Расширенные методы в экземплярах Geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  

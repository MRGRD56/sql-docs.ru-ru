---
description: STArea (тип данных geography)
title: STArea (тип данных geography) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STArea (geography Data Type)
- STArea_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STArea method
ms.assetid: cfc0b0e0-7fde-431a-863f-d13f3b1b1bef
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 8e83f44c9ed41e32ba0e47509139aee8c0aeb631
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210112"
---
# <a name="starea-geography-data-type"></a>STArea (тип данных geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает общую площадь поверхности экземпляра **geography**. Результаты для метода STArea() возвращаются в виде квадрата единицы измерения, используемой идентификатором пространственной ссылки экземпляра **geography**. Например, если SRID экземпляра равен 4326, то метод STArea() возвращает результаты в квадратных метрах.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.STArea ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **float**  
  
Тип возвращаемых данных CLR: **SqlDouble**  
  
## <a name="remarks"></a>Комментарии  
Метод STArea() возвращает значение 0, если экземпляр **geography** содержит только объекты размерности 0 и 1 либо является пустым.  
  
> [!NOTE]  
>  Методы, вызываемые для типа данных **geography** и возвращающие метрическое значение, могут возвращать различные результаты в зависимости от идентификатора SRID экземпляра. Дополнительные сведения об идентификаторах SRID см. в разделе [Идентификаторы пространственных ссылок (SRID)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md).  
  
## <a name="examples"></a>Примеры  
В следующем примере используется метод `STArea()` для создания экземпляра `Polygon geography` и вычисляется площадь этого многоугольника.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
SELECT @g.STArea();  
```  
  
## <a name="see-also"></a>См. также  
[Методы OGC в экземплярах Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  

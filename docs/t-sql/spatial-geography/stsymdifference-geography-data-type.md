---
description: STSymDifference (тип данных geography)
title: STSymDifference (тип данных geography) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STSymDifference (geography Data Type)
- STSymDifference_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STSymDifference (geography Data Type)
ms.assetid: 82bbfa2c-a61b-4f41-9bf8-6f720f363bae
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 26536b5ac3042c1341d75336a7d1b7d8cc439892
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "88305894"
---
# <a name="stsymdifference-geography-data-type"></a>STSymDifference (тип данных geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает объект, представляющий все точки, принадлежащие одному из экземпляров geography, но не лежащие одновременно в обоих экземплярах.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.STSymDifference ( other_geography )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *other_geography*  
 Другой экземпляр **geography** в дополнение к экземпляру, для которого вызван метод STSymDistance().  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
 Тип возвращаемых данных CLR: **SqlGeography**  
  
## <a name="remarks"></a>Remarks  
 Этот метод всегда возвращает значение NULL, если идентификаторы пространственных ссылок (SRID) экземпляров **geography** не совпадают.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает пространственные экземпляры, размер которых превышает полусферу. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] набор возможных результатов на сервере был пополнен экземплярами **FullGlobe**.  
  
 Результат может содержать сегменты дуги, только если во входном экземпляре содержатся сегменты дуги.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-computing-the-symmetric-difference-of-two-polygons"></a>A. Вычисление симметрической разницы двух многоугольников  
 В следующем примере метод `STSymDifference()` применяется для вычисления симметрической разности двух экземпляров `Polygon`.  
  
```  
DECLARE @g geography;  
DECLARE @h geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
SET @h = geography::STGeomFromText('POLYGON((-122.351 47.656, -122.341 47.656, -122.341 47.661, -122.351 47.661, -122.351 47.656))', 4326);  
SELECT @g.STSymDifference(@h).ToString();  
```  
  
### <a name="b-computing-the-symmetric-difference-with-fullglobe"></a>Б. Вычисление симметрической разницы с FullGlobe  
 В следующем примере сравнивается симметричная разница между `Polygon` и `FullGlobe`.  
  
```
 DECLARE @g geography = 'POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))';  
 SELECT @g.STSymDifference('FULLGLOBE').ToString();
 ```  
  
## <a name="see-also"></a>См. также:  
 [Методы OGC в экземплярах Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  

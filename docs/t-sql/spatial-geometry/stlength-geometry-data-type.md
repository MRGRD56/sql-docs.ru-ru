---
description: STLength (тип данных geometry)
title: STLength (тип данных geometry) | Документы Майкрософт
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STLength_TSQL
- STLength (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STLength (geometry Data Type)
ms.assetid: e34dc620-2a65-4248-b099-fff91830ab98
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: fa53274ed371828b0e124ef362dadfce2754de7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158319"
---
# <a name="stlength-geometry-data-type"></a>STLength (тип данных geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает общую длину элементов в экземпляре **geometry**.
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.STLength ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **float**  
  
 Тип возвращаемых данных CLR: **SqlDouble**  
  
## <a name="remarks"></a>Комментарии  
 Если экземпляр **geometry** замкнут, его длина вычисляется как общая длина пути обхода экземпляра; длина любого многоугольника есть его периметр, а длина точки — 0. Размер любого типа **geometrycollection** находится путем вычисления суммы размеров всех содержащихся в нем экземпляров **geometry**.  
  
 STLength() работает и с допустимыми и с недопустимыми объектами LineString. Обычно LineString является недопустимым вследствие перекрывающихся сегментов, которые могут быть вызваны аномалиями, например неточными трассировками GPS. STLength() не удаляет перекрывающиеся или недопустимые сегменты. Он включает перекрывающиеся и недопустимые сегменты в возвращаемое значение длины. Метод MakeValid() может удалять перекрывающиеся сегменты из LineString.  
  
## <a name="examples"></a>Примеры  
 В следующем примере создается экземпляр `LineString` и используется метод `STLength()` для получения его длины.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);  
SELECT @g.STLength();  
```  
  
## <a name="see-also"></a>См. также  
 [Методы OGC в экземплярах Geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  


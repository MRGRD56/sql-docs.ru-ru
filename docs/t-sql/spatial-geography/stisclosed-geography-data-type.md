---
description: STIsClosed (тип данных geography)
title: STIsClosed (тип данных geography) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STIsClosed (geography Data Type)
- STIsClosed_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STIsClosed method
ms.assetid: eba1643f-07c4-4500-8643-b7e90f908147
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 8c6643b37d421a5283cd8d44e3fadaf73ae69eab
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "88445224"
---
# <a name="stisclosed-geography-data-type"></a>STIsClosed (тип данных geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает значение 1, если начальная и конечная точки заданного экземпляра **geography** совпадают. Возвращает 1 для типов коллекции **geography**, если каждый содержащийся экземпляр **geography** замкнут. Возвращает значение 0, если экземпляр является незамкнутым.  
  
 Этот метод типа данных **geography** поддерживает экземпляры **FullGlobe** или пространственные экземпляры, размер которых больше полушария.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.STIsClosed ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **bit**  
  
 Тип возвращаемых данных CLR: **SqlBoolean**  
  
## <a name="remarks"></a>Комментарии  
 Этот метод возвращает значение 0, если среди фигур экземпляра **geography** есть точки или если экземпляр является пустым.  
  
 Метод возвращает значение true, если экземпляр **FullGlobe** имеет тип **Polygon** или является другим типом экземпляра.  
  
 Все экземпляры **Polygon** считаются замкнутыми.  
  
## <a name="examples"></a>Примеры  
 В следующем примере создается экземпляр `Polygon` и вызывается метод `STIsClosed()`, который определяет, является ли экземпляр `Polygon` замкнутым.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326);  
SELECT @g.STIsClosed();  
```  
  
## <a name="see-also"></a>См. также  
 [Методы OGC в экземплярах Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  

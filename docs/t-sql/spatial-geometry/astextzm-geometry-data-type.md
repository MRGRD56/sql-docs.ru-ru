---
description: AsTextZM (тип данных geometry)
title: AsTextZM (тип данных geometry) | Документы Майкрософт
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AsTextZM_TSQL
- AsTextZM
- AsTextZM (geometry Data Type)
- AsTextZM_(geometry_Data_Type)_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- AsTextZM (geometry Data Type)
ms.assetid: 08ac8aa0-aff7-4b22-87e0-1a1d55dcbc04
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: f00ce126a62beaf045d346bcaf9b349e69eb01fb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208707"
---
# <a name="astextzm-geometry-data-type"></a>AsTextZM (тип данных geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает представление WKT консорциума OGC экземпляра geometry, дополненного всеми значениями **Z** (высота) и **M** (мера) этого экземпляра.
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.AsTextZM ()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **nvarchar(max)**  
  
 Тип возвращаемых данных CLR: **SqlChars**  
  
## <a name="remarks"></a>Remarks  
  
## <a name="examples"></a>Примеры  
 В приведенном ниже примере создается экземпляр `Point`, содержащий значения **Z** (высота) и **M** (мера). Метод `STAsText()` выбирает значения WKT (1 2); метод `AsTextZM()` выбирает те же самые значения WKT, а также возвращает значения для **Z** и **M** (1 2 3 4).  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POINT(1 2 3 4)', 0);  
SELECT @g.STAsText();  
SELECT @g.AsTextZM();  
```  
  
## <a name="see-also"></a>См. также  
 [Расширенные методы экземпляров Geometry](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)   
 [M (тип данных geometry)](../../t-sql/spatial-geometry/m-geometry-data-type.md)   
 [Z (тип данных geometry)](../../t-sql/spatial-geometry/z-geometry-data-type.md)  
  
  


---
description: STAsText (тип данных geography)
title: STAsText (тип данных geography) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STAsText (geography Data Type)
- STAsText_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STAsText method
ms.assetid: d3d2635d-ca6c-4205-9d6c-eb939ee314fd
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: f89f2c2889d880388f74b563abc9aefe1eb28c82
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "88422318"
---
# <a name="stastext-geography-data-type"></a>STAsText (тип данных geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает WKT-представление OGC для экземпляра **geography**. Этот текст не будет содержать значений Z (высота) и M (мера), сопровождающих экземпляр.  
  
 Этот метод типа данных **geography** поддерживает экземпляры **FullGlobe** или пространственные экземпляры, размер которых больше полушария.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.STAsText ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **nvarchar(max)**  
  
 Тип возвращаемых данных CLR: **SqlChars**  
  
## <a name="remarks"></a>Remarks  
 Тип OGC экземпляра **geography** можно определить с помощью метода [STGeometryType()](../../t-sql/spatial-geography/stgeometrytype-geography-data-type.md).  
  
 В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] набор возможных результатов, возвращаемый на сервер, был пополнен экземплярами **FullGlobe**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере метод `STAsText()` формирует экземпляр `LineString``geography` на основе текстовых данных от (–122,360; 47,656) до (–122,343; 47,656). Затем возвращается результат в текстовом формате.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.STAsText();  
```  
  
## <a name="see-also"></a>См. также  
 [Методы OGC в экземплярах Geography](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  

---
description: ToString (тип данных geography)
title: ToString (тип данных geography) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ToString (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- ToString method
ms.assetid: 045c12fa-8fc6-441a-9500-7021cb4ff13e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d7d2d4b6d4f786c7962155e11b7fe89bbb491a7d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187740"
---
# <a name="tostring-geography-data-type"></a>ToString (тип данных geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает представление WKT открытого геопространственного консорциума (OGC) для экземпляра **geography**, дополненное всеми значениями Z (высота) и M (мера) этого экземпляра.  
  
 Этот метод типа данных geography поддерживает экземпляры **FullGlobe** или пространственные экземпляры, размер которых больше полушария.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.ToString ()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **nvarchar(max)**  
  
 Тип возвращаемых данных CLR: **SqlString**  
  
## <a name="remarks"></a>Комментарии  
 Метод возвращает строку NULL при вызове на экземплярах NULL. В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] набор возможных результатов на сервере был пополнен экземплярами **FullGlobe**. Этот метод возвратит такое же значение, что и функция `AsTextZM()`.  
  
 Этот метод не является точным.  
  
## <a name="examples"></a>Примеры  
 В приведенном ниже примере создается экземпляр `LineString` и используется метод `ToString()` для возврата текстового описания экземпляра.  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>См. также  
 [Расширенные методы в экземплярах Geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [AsTextZM (тип данных geography)](../../t-sql/spatial-geography/astextzm-geography-data-type.md)  
  
  

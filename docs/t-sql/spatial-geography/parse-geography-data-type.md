---
description: Parse (тип данных geography)
title: Parse (тип данных geography) | Документы Майкрософт
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Parse method
- Parse (geography Data Type)
ms.assetid: 21c402fa-fd0f-4d09-a097-49cee0316d4e
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 8cb4ab7b3362ef06b34bc7021f494a95ab502aed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210162"
---
# <a name="parse-geography-data-type"></a>Parse (тип данных geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает экземпляр **geography** из WKT-представления консорциума OGC. Функция Parse() эквивалентна функции [STGeomFromText](../../t-sql/spatial-geography/stgeomfromtext-geography-data-type.md), за исключением того, что ожидает в качестве параметра идентификатор пространственной ссылки (SRID) значение 4326. Входные данные могут дополнительно содержать значения Z (высота) и M (мера).
  
Этот метод типа данных **geography** поддерживает экземпляры **FullGlobe** или пространственные экземпляры, размер которых больше полушария.
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
Parse ( 'geography_tagged_text' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *geography_tagged_text*  
 WKT-представление возвращаемого экземпляра **geography**. *geography_tagged_text* является выражением типа **nvarchar**.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
 Тип возвращаемых данных CLR: **SqlGeography**  
  
## <a name="remarks"></a>Remarks  
 Тип OGC экземпляра **geography**, возвращаемый методом `Parse()`, получает значение в зависимости от соответствующих входных данных WKT.  
  
 Строка "Null" будет интерпретирована как экземпляр **geography** со значением NULL.  
  
 Этот метод вызывает исключение **ArgumentException**, если входные данные содержат противоположную границу.  
  
## <a name="examples"></a>Примеры  
 В следующем примере метод `Parse()` применяется для создания экземпляра `geography`.  
  
```  
DECLARE @g geography;   
SET @g = geography::Parse('LINESTRING(-122.360 47.656, -122.343 47.656)');  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>См. также:  
 [Расширенные статические географические методы](../../t-sql/spatial-geography/extended-static-geography-methods.md)   
 [STGeomFromText (тип данных geography)](../../t-sql/spatial-geography/stgeomfromtext-geography-data-type.md)  
  
  

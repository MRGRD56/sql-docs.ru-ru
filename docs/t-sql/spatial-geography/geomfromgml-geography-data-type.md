---
description: GeomFromGML (тип данных geography)
title: GeomFromGML (тип данных geography) | Документы Майкрософт
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- GeomFromGML_TSQL
- GeomFromGML
dev_langs:
- TSQL
helpviewer_keywords:
- GeomFromGML (geography Data Type)
- GeomFromGML method
ms.assetid: 470d0997-3cb0-4d34-9a45-b332fe432b14
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 44009b18bbe96c4d6aa8e402e584551e3c5ea521
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "88306468"
---
# <a name="geomfromgml-geography-data-type"></a>GeomFromGML (тип данных geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Создает экземпляр **geography** по заданному представлению в используемом в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подмножестве языка GML.
  
Дополнительные сведения о языке GML см. в следующих документах открытого геопространственного консорциума (OGC): [Спецификации OGC, язык GML](https://go.microsoft.com/fwlink/?LinkId=93629)
  
Этот метод типа данных **geography** поддерживает экземпляры **FullGlobe** или пространственные экземпляры, размер которых больше полушария.
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
GeomFromGml ( GML_input, SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *GML_input*  
 Входные XML-данные, из которых GML-код получит возвращаемое значение.  
  
 *SRID*  
 Выражение типа **int**, представляющее идентификатор пространственной ссылки (SRID) возвращаемого экземпляра **geography**.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
 Тип возвращаемых данных CLR: **SqlGeography**  
  
## <a name="remarks"></a>Remarks  
 Если входные данные имеют неверный формат, метод вызовет исключение **FormatException**.  
  
 Этот метод вызывает исключение **ArgumentException**, если входные данные содержат противоположную границу.  
  
## <a name="examples"></a>Примеры  
 В следующем примере метод `GeomFromGml()` применяется для создания экземпляра `geography`.  
  
```  
DECLARE @g geography;  
DECLARE @x xml;  
SET @x = '<LineString xmlns="https://www.opengis.net/gml"><posList>47.656 -122.36 47.656 -122.343</posList></LineString>';  
SET @g = geography::GeomFromGml(@x, 4326);  
SELECT @g.ToString();  
```  
  
 В следующем примере метод `GeomFromGml()` применяется для создания экземпляра `FullGlobe``geography`.  
  
```  
DECLARE @g geography;  
DECLARE @x xml;  
SET @x = '<FullGlobe xmlns="https://schemas.microsoft.com/sqlserver/2011/geography" />';  
SET @g = geography::GeomFromGml(@x, 4326);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>См. также:  
 [Расширенные статические географические методы](../../t-sql/spatial-geography/extended-static-geography-methods.md)  
  
  

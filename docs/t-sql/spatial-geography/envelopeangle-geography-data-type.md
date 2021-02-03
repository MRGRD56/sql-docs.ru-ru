---
description: EnvelopeAngle (тип данных geography)
title: EnvelopeAngle (тип данных geography) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EnvelopeAngle
- EnvelopeAngle_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- EnvelopeAngle method
ms.assetid: 14a7ba15-168c-4b08-ba3d-951d73092ac7
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 643a732e14f07802339df95f88ad59642c492694
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188606"
---
# <a name="envelopeangle-geography-data-type"></a>EnvelopeAngle (тип данных geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает максимальный угол (в градусах) между точкой, возвращаемой методом `EnvelopeCenter()`, и точкой в экземпляре **geography**.  
  
 Этот метод типа данных **geography** поддерживает экземпляры **FullGlobe** или пространственные экземпляры, размер которых больше полушария.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
EnvelopeAngle( )  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных  
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **float**  
  
 Тип возвращаемых данных CLR: **SqlDouble**  
  
## <a name="remarks"></a>Комментарии  
 Этот метод возвращает точку в экземпляре **geography** в градусах. При использовании с функцией EnvelopeCenter() `EnvelopeAngle()` возвращает ограничивающую окружность экземпляра **geography**.  
  
 В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] этот метод расширен для включения экземпляров **FullGlobe**.  
  
 Ограничение полушария, применяемое к `EnvelopeAngle()` в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], было снято. Однако будут возвращены экземпляры с углами более 90 градусов, 180 градусов. Функция `EnvelopeAngle()` не является точной для экземпляров **geography**, охватывающих более одного полушария.  
  
## <a name="examples"></a>Примеры  
  
```  
DECLARE @g geography = 'LINESTRING(-120 45, -120 0, -90 0)';   
SELECT @g.EnvelopeAngle();  
```  
  
## <a name="see-also"></a>См. также:  
 [Расширенные методы в экземплярах Geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [EnvelopeCenter (тип данных geography)](../../t-sql/spatial-geography/envelopecenter-geography-data-type.md)  
  
  

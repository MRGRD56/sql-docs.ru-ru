---
description: EnvelopeCenter (тип данных geography)
title: EnvelopeCenter (тип данных geography) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- EnvelopeCenter
- EnvelopeCenter_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- EnvelopeCenter method
ms.assetid: dee9d807-faad-45b8-b3f3-7e8aa7d07147
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 59428c1d87ab1a4c855e4890a113f96318c8c164
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211424"
---
# <a name="envelopecenter-geography-data-type"></a>EnvelopeCenter (тип данных geography)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Возвращает точку, которую можно использовать как центр ограничивающей окружности для экземпляра **geography**.  
  
Каждая точка в экземпляре описывается как вектор. Для построения ограничивающей окружности вектор направляется из центра Земли к точке на поверхности Земли. Центральная точка ограничивающей окружности рассчитывается как среднее значение всех векторов. Для закрытых циклов либо в экземпляре **polygon**, либо в экземпляре **LineString** первая и последняя точка используются только один раз.  
  
Этот метод типа данных **geography** поддерживает экземпляры **FullGlobe** или пространственные экземпляры, размер которых больше полушария.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
EnvelopeCenter( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **geography**  
  
Тип возвращаемых данных CLR: **SqlGeography**  
  
## <a name="remarks"></a>Remarks  
Этот метод возвращает **point**. При использовании с функцией `EnvelopeAngle()``EnvelopeCenter()` возвращает ограничивающую окружность экземпляра **geography**.  
  
> [!NOTE]  
>  `EnvelopeCenter()` возвращает ограничивающую окружность для экземпляра **geography**, но при этом не гарантируется создание на основе результатов минимальной ограничивающей окружности. Напротив, метод `STEnvelope()` типа данных **geometry** гарантирует возврат минимального ограничивающего прямоугольника при применении в экземпляре **geometry**.  
  
В [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] или более поздних версиях возвращает центр окружности, представляющей огибающую этого экземпляра в виде **point**. Для всех больших объектов, определенных параметром `EnvelopeAngle()` = 180, `EnvelopeCenter()` возвращает значение (90,0).  
  
Этот метод не является точным.  
  
## <a name="examples"></a>Примеры  
  
```  
DECLARE @g geography = 'LINESTRING(-120 45, -120 0, -90 0)';  
SELECT @g.EnvelopeCenter().ToString();  
```  
  
## <a name="see-also"></a>См. также:  
[Расширенные методы в экземплярах Geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
[EnvelopeAngle ( тип данных geography)](../../t-sql/spatial-geography/envelopeangle-geography-data-type.md)  
  
  

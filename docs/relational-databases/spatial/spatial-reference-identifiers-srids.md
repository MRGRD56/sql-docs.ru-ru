---
description: идентификаторы пространственных ссылок (SRID)
title: Идентификаторы пространственных ссылок | Документация Майкрософт
ms.date: 03/29/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- Spatial Reference Identifiers (SRIDs)
- geodetic spatial data [SQL Server], identifiers
- SRID
ms.assetid: 0612658a-7d1b-4178-bdc2-42b914ea31a7
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2873242ab181ce3d550c5b4f2274fef368d5c0f8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473135"
---
# <a name="spatial-reference-identifiers-srids"></a>идентификаторы пространственных ссылок (SRID)
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  У каждого пространственного экземпляра имеется идентификатор пространственной ссылки (SRID). Идентификатор SRID соответствует системе пространственных ссылок, основанной на конкретном эллипсоиде, используемом для плоского или сферического сопоставления.  
  
 Пространственный столбец может содержать объекты с различными идентификаторами SRID. Однако при выполнении операций над собственными данными при помощи методов работы с пространственными данными [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может использовать только пространственные экземпляры с одним и тем же индексом пространственной ссылки SRID. Результат любого пространственного метода, извлеченный на основе двух экземпляров с пространственными данными, допустим только в случае, если эти экземпляры имеют один и тот же идентификатор SRID, основанный на одних и тех же единице измерения, исходной точке и проекции, использованных для определения координат экземпляров. Наиболее распространенными единицами измерения идентификатора SRID являются метры или квадратные метры.  
  
 Если идентификаторы SRID двух пространственных экземпляров различаются, в результате применения к этим экземплярам методов работы с типами данных **geometry** или **geography** будет возвращено значение NULL. Например, чтобы при следующем условии предиката получить результат, отличный от значения NULL, два экземпляра **geometry** типа `geometry1` и `geometry2`должны иметь один и тот же идентификатор SRID:  
  
 `geometry1.STIntersects(geometry2) = 1`  
  
> [!NOTE]  
>  Система идентификации пространственных ссылок определена стандартом [Европейской группы Petroleum Survey (EPSG)](https://go.microsoft.com/fwlink/?LinkId=99349) , представляющим собой набор стандартов, разработанных для картографии, геодезии и хранилищ геодезических данных. Владельцем данного стандарта является комитет Oil and Gas Producers (OGP) Surveying and Positioning Committee.  
  
## <a name="see-also"></a>См. также  
 [Основные сведения о типах пространственных данных](../../relational-databases/spatial/spatial-data-types-overview.md)  
  
  

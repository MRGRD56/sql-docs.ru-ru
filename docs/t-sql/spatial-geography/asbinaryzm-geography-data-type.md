---
description: AsBinaryZM (тип данных geography)
title: AsBinaryZM (тип данных geography) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AsBinaryZM
- AsBinaryZM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- AsBinaryZM, geography
- AsBinaryZM
ms.assetid: 37246adb-814d-4113-9983-4d336de8182c
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 3bff9ed0acf12b1bd8d20c42b17ae59cd07cf5df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189231"
---
# <a name="asbinaryzm-geography-data-type"></a>AsBinaryZM (тип данных geography)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает представление в формате WKB открытого геопространственного консорциума (OGC) для экземпляра **geometry**, дополненное всеми значениями **Z** (высота) и **M** (мера), находящимися в экземпляре.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
.AsBinaryZM()  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных  
 Тип возвращаемых данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **varbinary(max)**  
  
 Тип возвращаемых данных CLR: **SqlBytes**  
  
## <a name="remarks"></a>Remarks  
  
## <a name="examples"></a>Примеры  
  
```sql  
DECLARE @g1 GEOGRAPHY = 'Point(1 1 2 3)';  
  
SELECT @g1.STAsBinary();  
-- Returns: 0x0101000000000000000000F03F000000000000F03F  
  
SELECT @g1.AsBinaryZM();  
--Returns: 0x01B90B0000000000000000F03F000000000000F03F00000000000000400000000000000840  
```  
  
## <a name="see-also"></a>См. также:  
 [Расширенные методы в экземплярах Geography](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)   
 [M (тип данных geography)](../../t-sql/spatial-geography/m-geography-data-type.md)   
 [Z (тип данных geography)](../../t-sql/spatial-geography/z-geography-data-type.md)  
  
  

---
description: sp_help_spatial_geometry_index (Transact-SQL)
title: sp_help_spatial_geometry_index (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_spatial_geometry_index
- sp_help_spatial_geometry_index_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_spatial_geometry_index procedure
ms.assetid: f1bcefb1-09c8-4b49-8c51-5d471065849f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: af6ed2f873a79be59af9adcd40311ef02fc715dd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207339"
---
# <a name="sp_help_spatial_geometry_index-transact-sql"></a>sp_help_spatial_geometry_index (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает имена и значения для указанного набора свойств пространственного индекса **Geometry** . Результат запроса возвращается в виде таблицы. Можно задать возврат основного набора свойств или всех свойств индекса.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_help_spatial_geometry_index [ @tabname =] 'tabname'   
     [ , [ @indexname = ] 'indexname' ]   
     [ , [ @verboseoutput = ] 'verboseoutput'   
     [ , [ @query_sample = ] 'query_sample']   
```  
  
## <a name="arguments"></a>Аргументы  
 См. раздел [аргументы и свойства хранимых процедур пространственного индекса](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="property-valuereturn-value"></a>Значение свойства/возвращаемое значение  
 См. раздел [аргументы и свойства хранимых процедур пространственного индекса](../../relational-databases/system-stored-procedures/spatial-index-stored-procedures-arguments-and-properties.md).  
  
## <a name="permissions"></a>Разрешения  
 Пользователю должна быть назначена роль PUBLIC для получения доступа к процедуре. Необходимо разрешение READ ACCESS на сервере и объекте.  
  
## <a name="remarks"></a>Замечания  
 Свойства, которые содержат значения NULL, не включаются в набор возвращаемых значений.  
  
## <a name="example"></a>Пример  
 В следующем примере используется `sp_help_spatial_geometry_index` для исследования пространственного индекса **SIndx_SpatialTable_geometry_col2** , определенного в таблице **geometry_col** для данного примера запроса в **\@ QS**. Этот пример возвращает только основные свойства указанного индекса.  
  
```  
declare @qs geometry  
        ='POLYGON((-90.0 -180.0, -90.0 180.0, 90.0 180.0, 90.0 -180.0, -90.0 -180.0))';  
exec sp_help_spatial_geometry_index 'geometry_col', 'SIndx_SpatialTable_geometry_col2', 0, @qs;  
```  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры пространственного индекса](./spatial-index-stored-procedures-arguments-and-properties.md)   
 [sp_help_spatial_geometry_index_xml](../../relational-databases/system-stored-procedures/sp-help-spatial-geometry-index-xml-transact-sql.md)   
 [Общие сведения о пространственных индексах](../../relational-databases/spatial/spatial-indexes-overview.md)   
 [Пространственные данные (SQL Server)](../../relational-databases/spatial/spatial-data-sql-server.md)  
  

---
description: sysdatatypemappings (Transact-SQL)
title: sysdatatypemappings (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysdatatypemappings
- sysdatatypemappings_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysdatatypemappings view
ms.assetid: 5dfafb70-3e3d-4465-b293-1acff1f855b6
author: stevestein
ms.author: sstein
ms.openlocfilehash: 753a18b314ea51fa341fe8e6f8b0fdfd235bf95b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160327"
---
# <a name="sysdatatypemappings-transact-sql"></a>sysdatatypemappings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Представление **sysdatatypemappings** используется для отображения сопоставления типов данных SQL Server и типов данных системы управления базами данных (СУБД), отличной от SQL Server. Это представление хранится в базе данных **msdb** .  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**mapping_id**|**int**|Идентификатор сопоставления типа данных.|  
|**source_dbms**|**sysname**|Указывается имя СУБД, из которой сопоставляются типы данных. Это имя может иметь одно из следующих значений.<br /><br /> **MSSQLServer** = источником является SQL Server база данных.<br /><br /> **Oracle** = источником является база данных Oracle.|  
|**source_version**|**sysname**|Указывается версия продукта СУБД источника.|  
|**source_type**|**sysname**|Указывается тип данных, представленный в СУБД источника.|  
|**source_length_min**|**bigint**|Минимальная длина типа данных в СУБД-источнике; причем значение NULL указывает, что длина не используется.|  
|**source_length_max**|**bigint**|Максимальная длина типа данных в СУБД-источнике; причем значение NULL указывает, что длина не используется.|  
|**source_precision_min**|**bigint**|Минимальная точность типа данных в СУБД-источнике; причем значение NULL указывает, что точность не используется.|  
|**source_precision_max**|**bigint**|Максимальная точность типа данных в СУБД-источнике; причем значение NULL указывает, что точность не используется.|  
|**source_scale_min**|**int**|Минимальный масштаб типа данных в СУБД-источнике; причем значение NULL указывает, что масштаб не используется.|  
|**source_scale_max**|**int**|Максимальный масштаб типа данных в СУБД-источнике; причем значение NULL указывает, что масштаб не используется.|  
|**source_nullable**|**bit**|Указывается, что тип данных назначения поддерживает значения NULL.|  
|**source_createparams**|**int**|Только для внутреннего использования.|  
|**destination_dbms**|**sysname**|Указывается имя целевой СУБД. Это имя может принимать одно из следующих значений.<br /><br /> **MSSQLServer** = назначение является SQL Server базой данных.<br /><br /> **Oracle** = назначение является базой данных Oracle.<br /><br /> **DB2** = назначением является база данных IBM DB2.<br /><br /> **SYBASE** = целевой является база данных SYBASE.|  
|**destination_version**|**sysname**|Версия продукта целевой СУБД.|  
|**destination_type**|**sysname**|Тип данных в целевой СУБД.|  
|**destination_length**|**bigint**|Длина типа данных в целевой СУБД.|  
|**destination_precision**|**bigint**|Точность типа данных в целевой СУБД.|  
|**destination_scale**|**int**|Масштаб типа данных в целевой СУБД.|  
|**destination_nullable**|**bit**|Указывает, поддерживает ли тип данных в целевой СУБД значения NULL.|  
|**destination_createparams**|**int**|Только для внутреннего использования.|  
|**потери данных**|**bit**|Указывает, возникают ли потери данных при сопоставлении типов данных СУБД источника и адресата.|  
|**is_default**|**bit**|Указывает, используется ли сопоставление типов данных по умолчанию.|  
  
## <a name="see-also"></a>См. также  
 [Разнородная репликация базы данных](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Таблицы репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Представления репликации &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helpdatatypemap &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdatatypemap-transact-sql.md)  
  
  

---
title: Новые возможности поддержки API для функций даты и времени (драйвер OLE DB)
description: Узнайте об API-интерфейсах OLE DB, поддерживающих расширенные функции для даты и времени, в том числе имена и описания функций.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8209c1b1ca6f3c00ce4cd10455c35c8b9d4dfad4
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2020
ms.locfileid: "88862349"
---
# <a name="ole-db-api-support-for-date-and-time-enhancements"></a>Новые возможности поддержки API OLE DB для функций даты и времени
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Следующие API-интерфейсы OLE DB поддерживают улучшенные возможности по работе с данными в формате даты-времени.  
  
|Компонент|Description|  
|--------------|-----------------|  
|IAccessor::CreateAccessor|Чтобы позволить приложениям различать значения **datetime**, **datetime2** и **smalldatetime**, в структуру DBBINDING добавлен новый флаг. Дополнительные сведения см. в статье [Метаданные — параметры и наборы строк](../../oledb/ole-db-date-time/metadata-parameter-and-rowset.md).|  
|IBCPSession::BCPColFmt|Подробнее см. статью [Изменения массового копирования для расширенных типов даты и времени &#40;OLE DB&#41;](../../oledb/ole-db-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db.md).|  
|ICommandWithParameters::GetParameterInfo|Дополнительные сведения см. в статье [Метаданные — параметры и наборы строк](../../oledb/ole-db-date-time/metadata-parameter-and-rowset.md).|  
|ICommandWithParameters::SetParameterinfo|Дополнительные сведения см. в статье [Метаданные — параметры и наборы строк](../../oledb/ole-db-date-time/metadata-parameter-and-rowset.md).|  
|IColumnsRowset::GetColumnsRowset|Дополнительные сведения см. в статье [Метаданные — параметры и наборы строк](../../oledb/ole-db-date-time/metadata-parameter-and-rowset.md).|  
|IColumnsInfo::GetColumnInfo|Дополнительные сведения см. в статье [Метаданные — параметры и наборы строк](../../oledb/ole-db-date-time/metadata-parameter-and-rowset.md).|  
|IDBSchemaRowset::GetRowset|Подробности о затронутых наборах строк схемы см. в статье [Метаданные — наборы строк даты и времени и схемы](../../oledb/ole-db-date-time/metadata-date-and-time-and-schema-rowsets.md).|  
|IRowsetFastLoad|Данный интерфейс поддерживает новые типы даты-времени, но он остается неизменным.|  
|ITableDefinition::CreateTable|Подробнее см. статью [Улучшения поддержки типов данных даты и времени OLE DB](../../oledb/ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md).|  
  
## <a name="see-also"></a>См. также:  
 [Улучшения функций даты и времени &#40;OLE DB&#41;](../../oledb/ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  

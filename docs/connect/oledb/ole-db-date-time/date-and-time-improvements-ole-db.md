---
title: Улучшения функций даты и времени в OLE DB
description: В этих статьях описывается, как OLE DB Driver for SQL Server поддерживает новые типы данных даты и времени.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- date/time [OLE DB]
- OLE DB, date/time improvements
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 89a52e5eafcf8d4ae9729a5410778f117dcd0e01
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633313"
---
# <a name="date-and-time-improvements-in-ole-db"></a>Улучшения функций даты и времени в OLE DB

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

В [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] добавлены новые типы данных даты-времени. В этой статье описано, как эти новые типы предоставляются в виде расширений в OLE DB Driver for SQL Server. Общие сведения о поддержке новых типов данных даты и времени в OLE DB Driver for SQL Server см. в статье [Улучшения функций даты и времени](../features/date-and-time-improvements.md). Пример их использования см. в статье [Использование улучшенных функций даты и времени (OLE DB)](../ole-db-how-to/use-enhanced-date-and-time-features-ole-db.md).

Общие сведения о типах данных даты и времени вы найдете в разделе документации [datetime (Transact-SQL)](../../../t-sql/data-types/datetime-transact-sql.md).

## <a name="in-this-section"></a>Содержание раздела

[Улучшения поддержки типов данных даты и времени OLE DB](data-type-support-for-ole-db-date-and-time-improvements.md)  
Предоставляет сведения о типах OLE DB (OLE DB Driver for SQL Server), которые поддерживают типы данных даты и времени [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

[Метаданные (OLE DB)](metadata-parameter-and-rowset.md)  
Содержит сведения о структуре DBBINDING, **ICommandWithParameters::GetParameterInfo**, **ICommandWithParameters::SetParameterInfo**, **IColumnsRowset::GetColumnsRowset** и I **ColumnsInfo::GetColumnInfo**. Также предоставляет сведения об обновлениях наборов строк схем OLE DB.

[Привязки и преобразования &#40;OLE DB&#41;](conversions-ole-db.md)  
Описывает правила преобразования существующих и новых типов данных между сервером и клиентом.

[Изменения массового копирования для расширенных типов даты и времени &#40;OLE DB&#41;](bulk-copy-changes-for-enhanced-date-and-time-types-ole-db.md)  
Описываются новые возможности даты-времени для поддержки операций массового копирования.

[Новые возможности поддержки API OLE DB для функций даты и времени](ole-db-api-support-for-date-and-time-enhancements.md)  
Описывает API-интерфейсы OLE DB, поддерживающие расширенные возможности типов даты-времени.

[Сравнимость для IRowsetFind](comparability-for-irowsetfind.md)  
Описывает типы даты и времени, а также интерфейс **IRowsetFind**.

## <a name="see-also"></a>См. также раздел

[Программирование драйвера OLE DB для SQL Server](../ole-db/oledb-driver-for-sql-server-programming.md)

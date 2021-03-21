---
title: Улучшения функций даты и времени | Документация Майкрософт
description: Узнайте, как OLE DB Driver for SQL Server поддерживает новые типы данных даты и времени, которые были введены в SQL Server 2008.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ca548ab01e4379b6cea92b5764820383abaf926b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742864"
---
# <a name="date-and-time-improvements"></a>Улучшения функций даты и времени
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  В этом разделе описано, как OLE DB Driver for SQL Server поддерживает новые типы данных даты и времени, которые были введены в [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)].  
  
 Дополнительные сведения об улучшениях функций даты и времени см. в [этой статье](../../oledb/ole-db-date-time/date-and-time-improvements-ole-db.md).  
  
 Сведения о примерах приложений, которые демонстрируют эту функцию, см. в разделе [Образцы программирования для SQL Server](https://msftdpprodsamples.codeplex.com/).  
  
## <a name="usage"></a>Использование  
 В следующих разделах приводится описание различных способов использования новых типов даты и времени.  
  
### <a name="use-date-as-a-distinct-data-type"></a>Использование Date как отдельного типа данных  
 Начиная с [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)], расширенная поддержка типов даты-времени делает использование типа DBTYPE_DBDATE OLE DB более эффективным.  
  
### <a name="use-time-as-a-distinct-data-type"></a>Использование Time как отдельного типа данных  
 OLE DB уже имеет тип данных, представляющий время с точностью до 1 секунды — DBTYPE_DBTIME.
  
 Новый тип данных времени [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] имеет точность до 100 наносекунд. Для этого в OLE DB Driver for SQL Server применяется новый тип: DBTYPE_DBTIME2. Существующие приложения, не работающие с долями секунд, могут пользоваться столбцами time(0). Существующий тип OLE DB DBTYPE_TIME и соответствующие ему структуры должны работать правильно, если приложение не использует тип, возвращаемый в метаданных.  
  
### <a name="use-time-as-a-distinct-data-type-with-extended-fractional-seconds-precision"></a>Использование Time как отдельного типа данных с расширенной точностью до долей секунд  
 Некоторым приложениям, например приложениям для управления производством и процессами, необходима возможность обработки времени с точностью до 100 наносекунд. Для этой цели в OLE DB добавлен новый тип DBTYPE_DBTIME2.  
  
### <a name="use-datetime-with-extended-fractional-seconds-precision"></a>Использование Datetime как отдельного типа данных с расширенной точностью до долей секунд  
 В OLE DB уже имеется определение типа с точностью до 1 наносекунды. Этот тип применяется в существующих приложениях [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , однако ожидаемая точность таких приложений составляет лишь 1/300 секунды. Новый тип **datetime2(3)** несовместим напрямую с существующим типом datetime. Если есть риск, что он повлияет на работу приложения, необходимо при определении фактического типа на сервере пользоваться новым флагом DBCOLUMN.    
  
### <a name="use-datetime-with-extended-fractional-seconds-precision-and-timezone"></a>Использование Datetime с расширенной точностью до долей секунд и часовым поясом  
 Некоторым приложениям необходимы значения даты-времени вместе со сведениями о часовом поясе. Эта возможность поддерживается новым типом DBTYPE_DBTIMESTAMPOFFSET (OLE DB).
  
### <a name="use-datetimedatetimedatetimeoffset-data-with-client-side-conversions-consistent-with-existing-conversions"></a>Использование данных Date/Time/Datetime/Datetimeoffset с преобразованиями на стороне клиента, согласующимися с существующими преобразованиями  
 Преобразования расширены для согласования с преобразованиями всех типов данных даты и времени, представленных в [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)].  
  
## <a name="see-also"></a>См. также:  
 [Возможности драйвера OLE DB для SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
  

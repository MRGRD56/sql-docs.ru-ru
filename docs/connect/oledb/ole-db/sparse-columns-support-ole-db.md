---
title: Поддержка разреженных столбцов (OLE DB) | Документация Майкрософт
description: Сведения о поддержке в OLE DB Driver for SQL Server разреженных столбцов, оптимизированных для хранения значений NULL.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 12503fa6339dbf3f16a6564cab899b8802f8e3fb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755314"
---
# <a name="sparse-columns-support-ole-db"></a>Поддержка разреженных столбцов (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  В этом разделе приводятся сведения о поддержке разреженных столбцов OLE DB Driver for SQL Server. Дополнительные сведения о разреженных столбцах см. в статье [Sparse Columns Support in OLE DB Driver for SQL Server](../../oledb/features/sparse-columns-support-in-oledb-driver-for-sql-server.md) (Поддержка разреженных столбцов в OLE DB Driver for SQL Server). Например, [отображение метаданных столбца и каталога для разреженных столбцов (OLE DB)](../../oledb/ole-db-how-to/display-column-and-catalog-metadata-for-sparse-columns-ole-db.md)  
  
## <a name="ole-db-statement-metadata"></a>Метаданные инструкции OLE DB  
 В версии [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] появилось новое значение DBCOLUMNFLAGS_SS_ISCOLUMNSET флага DBCOLUMNFLAGS. Это значение должно задаваться для столбцов со значениями **column_set**. Флаг DBCOLUMNFLAGS можно получить с помощью параметра *dwFlags* IColumnsInfo::GetColumnsInfo и столбца DBCOLUMN_FLAGS набора строк, возвращаемого IColumnsRowset::GetColumnsRowset.  
  
## <a name="ole-db-catalog-metadata"></a>Метаданные каталога OLE DB  
 В DBSCHEMA_COLUMNS были добавлены два дополнительных столбца, специфичных для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
|Имя столбца|Тип данных|Значения/комментарии|  
|-----------------|---------------|---------------------|  
|SS_IS_SPARSE|DBTYPE_BOOL|Если столбец является разреженным, то значение VARIANT_TRUE. В противном случае — значение VARIANT_FALSE.|  
|SS_IS_COLUMN_SET|DBTYPE_BOOL|Если столбец является разреженным столбцом **column_set**, то значение VARIANT_TRUE. В противном случае значение VARIANT_FALSE.|  
  
 Кроме этого, были добавлены два дополнительных набора строк схемы. Эти наборы строк имеют ту же структуру, что и DBSCHEMA_COLUMNS, но возвращают другое содержимое. DBSCHEMA_COLUMNS_EXTENDED возвращает все столбцы независимо от членства в **column_set**. DBSCHEMA_SPARSE_COLUMN_SET возвращает только те столбцы, которые являются членами разреженного **column_set**.  
  
## <a name="ole-db-datatypecompatibility-behavior"></a>Поведение OLE DB DataTypeCompatibility  
 Поведение с **DataTypeCompatibility=80[!INCLUDE[ssVersion2000](../../../includes/ssversion2000-md.md)] (в строке подключения) согласовано с**  следующим образом.  
  
-   Новые наборы строк схемы невидимы; для них нет строк в наборе строк схемы.  
  
-   Новые столбцы в наборе строк COLUMNS невидимы.  
  
-   DBCOLUMNFLAGS_SS_ISCOLUMNSET не устанавливается для столбцов **column_set**.  
  
-   DBCOMPUTEMODE_NOTCOMPUTED устанавливается для столбцов **column_set**.  
  
## <a name="ole-db-support-for-sparse-columns"></a>Поддержка разреженных столбцов в OLE DB  
 Для поддержки разреженных столбцов были изменены следующие интерфейсы OLE DB Driver for SQL Server.  
  
|Тип или функция-элемент|Описание|  
|-----------------------------|-----------------|  
|IColumnsInfo::GetColumnsInfo|Новое значение DBCOLUMNFLAGS_SS_ISCOLUMNSET флага DBCOLUMNFLAGS задается для столбцов **column_set** в *dwFlags*.<br /><br /> Значение DBCOLUMNFLAGS_WRITE устанавливается для столбцов **column_set**.|  
|IColumsRowset::GetColumnsRowset|Новое значение DBCOLUMNFLAGS_SS_ISCOLUMNSET флага DBCOLUMNFLAGS задается для столбцов **column_set** в DBCOLUMN_FLAGS.<br /><br /> DBCOLUMN_COMPUTEMODE устанавливается в значение DBCOMPUTEMODE_DYNAMIC для столбцов **column_set**.|  
|IDBSchemaRowset::GetSchemaRowset|DBSCHEMA_COLUMNS возвращает два новых столбца: SS_IS_COLUMN_SET и SS_IS_SPARSE.<br /><br /> DBSCHEMA_COLUMNS возвращает только те столбцы, которые не являются членами **column_set**.<br /><br /> Добавлены два новых набора строк схемы. DBSCHEMA_COLUMNS_EXTENDED возвращает все столбцы, независимо от разреженности членства в **column_set**. DBSCHEMA_SPARSE_COLUMN_SET возвращает только те столбцы, которые являются членами столбца **column_set**. Новые наборы строк содержат те же столбцы и ограничения, что и DBSCHEMA_COLUMNS.|  
|IDBSchemaRowset::GetSchemas|Метод IDBSchemaRowset::GetSchemas включает идентификаторы GUID для новых наборов строк DBSCHEMA_COLUMNS_EXTENDED и DBSCHEMA_SPARSE_COLUMN_SET в списке доступных наборов строк схемы.|  
|ICommand::Execute|Если используется запрос **select \* from** *table*, то он возвращает все столбцы, не являющиеся членами разреженного **column_set**, а также столбец XML, содержащий значения всех столбцов со значениями, отличными от NULL, которые являются членами разреженного **column_set**, если они есть.|  
|IOpenRowset::OpenRowset|IOpenRowset::OpenRowset возвращает набор строк с теми же столбцами, что и ICommand::Execute, с запросом **select \*** в той же таблице.|  
|ITableDefinition|Этот интерфейс не изменился для разреженных столбцов или столбцов **column_set**. Приложения, которым необходимо изменить схему, должны выполнить соответствующий код [!INCLUDE[tsql](../../../includes/tsql-md.md)] напрямую.|  
  
## <a name="see-also"></a>См. также:  
 [Программирование драйвера OLE DB для SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  

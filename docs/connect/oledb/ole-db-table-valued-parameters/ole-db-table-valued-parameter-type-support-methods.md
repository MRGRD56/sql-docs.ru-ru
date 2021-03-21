---
title: Поддержка типов параметров OLE DB, возвращающих табличные значения (методы) | Документы Майкрософт
description: Сведения о стандартных методах OLE DB, которые поддерживают возвращающие табличное значение параметры в OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (OLE DB), API support (methods)
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c8c6cf1cf3822e5822185211ccce02017ccf90d1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750824"
---
# <a name="ole-db-table-valued-parameter-type-support-methods"></a>Поддержка типов параметров OLE DB, возвращающих табличное значение (методы)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Следующие стандартные методы OLE DB поддерживают возвращающие табличные значения параметры.  
  
|Метод|Поддержка возвращающих табличные значения параметров|  
|------------|-------------------------------------|  
|ITableDefinitionWithConstraints::CreateTableWithConstraints|Используется при наличии сведений о типе возвращающего табличное значение параметра и необходимости создания экземпляра объекта набора строк возвращающего табличное значение параметра на основе сведений о типе.<br /><br /> Дополнительные сведения вы найдете в разделе "Статический сценарий" статьи [о создании набора строк для возвращающего табличные значения параметра](../../oledb/ole-db-table-valued-parameters/table-valued-parameter-rowset-creation.md).|  
|IOpenRowset::OpenRowset|Используется при отсутствии сведений о типе возвращающего табличное значение параметра и необходимости создания экземпляра объекта набора строк возвращающего табличное значение параметра на основе метаданных, полученных от сервера.<br /><br /> Дополнительные сведения вы найдете в разделе "Динамический сценарий" статьи [о создании набора строк для возвращающего табличные значения параметра](../../oledb/ole-db-table-valued-parameters/table-valued-parameter-rowset-creation.md).|  
|ISSCommandWithParameters::SetParameterInfo|Для указания возвращающего табличное значение параметра команды потребитель указывает тип параметра "table" или "DBTYPE_TABLE" в элементе *pwszName* структуры DBPARAMBINDINFO. *ulParamSize* имеет значение ~0. Дополнительные сведения см. в разделе "Спецификация возвращающих табличные значения параметров" статьи [о выполнении команд с возвращающим табличные значения параметром](../../oledb/ole-db-table-valued-parameters/executing-commands-containing-table-valued-parameters.md).|  
|ISSCommandWithParameters::SetParameterProperties|Задает свойства, определенные для возвращающих табличные значения параметров, например имя схемы, имя типа, порядок столбца и столбцы по умолчанию.<br /><br /> Потребитель указывает порядковый номер параметра в элементе *iOrdinal* структуры SSPARAMPROPS. Запрошенный набор свойств — DBPROPSET_SQLSERVERPARAMETER.|  
|ISSCommandWithParameters::GetParameterInfo|Возвращает типы всех параметров в указанную команду.<br /><br /> Для возвращающих табличные значения параметров поле *wType* в структуре DBPARAMINFO будет иметь тип DBTYPE_TABLE. Для определения неизвестной длины поле *ulParamSize* будет установлено в значение ~0.|  
|ISSCommandWithParameters::GetParameterProperties|Возвращает дополнительные сведения о типе для параметров типа DBTYPE_TABLE.<br /><br /> Потребитель указывает порядковый номер параметра в элементе *iOrdinal* структуры SSPARAMPROPS. Потребитель может запросить любое свойство из набора свойств DBPROPSET_SQLSERVERPARAMETER, которые перечислены в ISSCommandWithParameters::SetParameterProperties.<br /><br /> Поскольку потребителю неизвестен тип возвращающего табличное значение параметра, поставщик должен установить правильные значения свойств SSPROP_PARAM_TYPE_TYPENAME, SSPROP_PARAM_TYPE_SCHEMANAME и SSPROP_PARAM_TYPE_CATALOGNAME. Оставшиеся свойства, SSPROP_PARAM_TABLE_DEFAULT_COLUMNS и SSPROP_PARAM_TABLE_COLUMN_SORT_ORDER, сохранят значения по умолчанию. После обнаружения потребителем имени типа возвращающего табличное значение параметра он использует метод IOpenRowset::OpenRowset для создания экземпляра этого параметра, указав имя его типа. Дополнительные сведения см. в статье [Обнаружение типа возвращающего табличное значение параметра](../../oledb/ole-db-table-valued-parameters/table-valued-parameter-type-discovery.md).|  
|IRowsetInfo::GetProperties|Возвращает свойства набора строк возвращающего табличное значение параметра. Потребитель может использовать эти параметры для оптимальной установки привязок.|  
|IColumnsRowset::GetColumnsRowset|Получает метаданные о таблице [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Для возвращающих табличные значения параметров этот же интерфейс предоставляет подробные метаданные о каждом столбце, например:<br /><br /> DBCOLUMN_FLAGS обозначает допустимость значений типа NULL в бите DBCOLUMNFLAGS_ISNULLABLE;<br /><br /> DBCOLUMN_ISUNIQUE обозначает, является ли столбец столбцом идентификаторов;<br /><br /> DBCOLUMN_COMPUTEMODE обозначает, является ли столбец вычисляемым.|  
|IAccessor::CreateAccessor|Для привязки объекта набора строк возвращающего табличное значение параметра создается метод доступа с элементом *wType*, установленным в значение DBTYPE_TABLE. Структура DBOBJECT будет содержать IID_IRowset или любой другой допустимый интерфейс объекта набора строк в элементе *iid*. Оставшиеся поля обрабатываются тем же способом, как и DBTYPE_IUNKNOWN.|  
  
## <a name="see-also"></a>См. также:  
 [Поддержка типов параметров OLE DB, возвращающих табличные значения](../../oledb/ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support.md)   
 [Создание набора строк возвращающего табличное значение параметра](../../oledb/ole-db-table-valued-parameters/table-valued-parameter-rowset-creation.md)   
 [Использование возвращающих табличные значения параметров &#40;OLE DB&#41;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  

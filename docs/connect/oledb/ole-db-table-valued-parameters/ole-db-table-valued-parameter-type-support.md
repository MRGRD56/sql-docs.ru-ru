---
title: Поддержка типов параметров, возвращающих табличное значение (драйвер OLE DB)
description: Узнайте, как создать специализированный объект набора строк для возвращающих табличное значение параметров в OLE DB Driver for SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (OLE DB), API support (OLE DB)
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5cbbf3b0319208c6f467f4236824197e22f2a71a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750814"
---
# <a name="ole-db-table-valued-parameter-type-support"></a>Поддержка типа возвращающего табличное значение параметра OLE DB
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  В этой статье описывается поддержка типов OLE DB для параметров, возвращающих табличные значения.  
  
## <a name="table-valued-parameter-rowset-object"></a>Объект набора строк параметра, возвращающего табличное значение  
 Можно создать специальный объект набора строк для параметров, возвращающих табличное значение. Чтобы создать объект набора строк для параметра, возвращающего табличное значение, можно применить ITableDefinitionWithConstraints::CreateTableWithConstraints или IOpenRowset::OpenRowset. Для этого установите элемент *eKind* параметра *pTableID* в значение DBKIND_GUID_NAME и укажите CLSID_ROWSET_INMEMORY как элемент *guid*. Имя типа сервера для параметра, возвращающего табличное значение, необходимо указать в элементе *pwszName* параметра *pTableID* при вызове IOpenRowset::OpenRowset. Объект набора строк параметра, возвращающего табличное значение, действует так же, как объект OLE DB Driver for SQL Server.  
  
```  
const GUID CLSID_ROWSET_TVP =   
{0xc7ef28d5, 0x7bee, 0x443f, {0x86, 0xda, 0xe3, 0x98, 0x4f, 0xcd, 0x4d, 0xf9}};  
  
CoType RowsetTVP  
{  
[mandatory] interface IAccessor;  
[mandatory] interface IColumnsInfo;  
[mandatory] interface IConvertType;  
[mandatory] interface IRowset;  
[mandatory] interface IRowsetInfo;  
[optional]  interface IColumnsRowset;  
[optional]  interface IRowsetChange;  
[optional]  interface ISupportErrorInfo;  
};  
```  
  
## <a name="dbtype_table"></a>DBTYPE_TABLE  
 Новый тип, DBTYPE_TABLE, представляет собой табличный тип. Этот тип описывает возвращающие табличное значение параметры в различных интерфейсах OLE DB, где требуется тип DBTYPE.  
  
```  
#define DBTYPE_TABLE (143)  
```  
  
 DBTYPE_TABLE имеет такой же формат, что и DBTYPE_IUNKNOWN. Представляет собой указатель на объект в буфере данных. Чтобы определить полную спецификацию в привязках, потребитель заполняет буфер DBOBJECT, задавая для параметра *iid* один из интерфейсов объекта набора строк (IID_IRowset). Если объект DBOBJECT в привязках не указан, то предполагается использование объекта IID_IRowset.  
  
 Прямые и обратные преобразования в тип DBTYPE_TABLE для каких-либо других типов не поддерживаются. Метод IConvertType::CanConvert возвращает значение S_FALSE для неподдерживаемого преобразования применительно к любому запросу, отличному от преобразования DBTYPE_TABLE в DBTYPE_TABLE. При этом предполагается использование параметра DBCONVERTFLAGS_PARAMETER объекта Command.  
  
## <a name="methods"></a>Методы  
 Сведения о методах OLE DB, поддерживающих параметры, возвращающие табличное значение, см. в статье [Поддержка типа возвращающего табличное значение параметра OLE DB (методы)](../../oledb/ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support-methods.md).  
  
## <a name="properties"></a>Свойства  
 Сведения о свойствах OLE DB, поддерживающих параметры, возвращающие табличное значение, см. в статье [Поддержка типа возвращающего табличное значение параметра OLE DB (свойства)](../../oledb/ole-db-table-valued-parameters/ole-db-table-valued-parameter-type-support-properties.md).  
  
## <a name="see-also"></a>См. также:  
 [Возвращающие табличные значения параметры &#40;OLE DB&#41;](../../oledb/ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [Использование возвращающих табличные значения параметров &#40;OLE DB&#41;](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  

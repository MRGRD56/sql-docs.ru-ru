---
description: Сопоставление типов данных в наборах строк и параметрах (поставщик собственного клиента OLE DB)
title: Сопоставление типов данных в наборах строк и параметрах (поставщик собственного клиента OLE DB) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- mapping data types [OLE DB]
- DBTYPE_SQLVARIANT data type
- SQL Server Native Client OLE DB provider, data types
- rowsets [OLE DB], data type mapping
- data types [OLE DB]
- GetColumnInfo function
- parameters [OLE DB]
- SSPROP_ALLOWNATIVEVARIANT property
- GetParameterInfo function
- OLE DB, data types
ms.assetid: 3d831ff8-3b79-4698-b2c1-2b5dd2f8235c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: aa356a77c5dffca7565fe563eeea3a4bdc1e5055
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748484"
---
# <a name="sql-server-native-client-data-type-mapping-in-rowsets-and-parameters"></a>SQL Server Native Client сопоставление типов данных в наборах строк и параметрах
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  В наборах строк и в качестве значений параметров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поставщик собственного клиента OLE DB представляет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] данные с помощью следующих OLE DB определенных типов данных, о которых сообщается в функциях **IColumnsInfo:: GetColumnInfo** и **ICommandWithParameters:: GetParameterInfo**.  
  
|Тип данных SQL Server|Тип данных OLE DB|  
|--------------------------|----------------------|  
|**bigint**|DBTYPE_I8|  
|**binary**|DBTYPE_BYTES|  
|**bit**|DBTYPE_BOOL|  
|**char**|DBTYPE_STR|  
|**datetime**|DBTYPE_DBTIMESTAMP|  
|**datetime2**|DBTYPE_DBTIME2|  
|**decimal**|DBTYPE_NUMERIC|  
|**float**|DBTYPE_R8|  
|**image**|DBTYPE_BYTES|  
|**int**|DBTYPE_I4|  
|**money**|DBTYPE_CY|  
|**nchar**|DBTYPE_WSTR|  
|**ntext**|DBTYPE_WSTR|  
|**numeric**|DBTYPE_NUMERIC|  
|**nvarchar**|DBTYPE_WSTR|  
|**real**|DBTYPE_R4|  
|**smalldatetime**|DBTYPE_DBTIMESTAMP|  
|**smallint**|DBTYPE_I2|  
|**smallmoney**|DBTYPE_CY|  
|**sql_variant**|DBTYPE_VARIANT, DBTYPE_SQLVARIANT|  
|**sysname**|DBTYPE_WSTR|  
|**text**|DBTYPE_STR|  
|**timestamp**|DBTYPE_BYTES|  
|**tinyint**|DBTYPE_UI1|  
|**UDT**|DBTYPE_UDT|  
|**uniqueidentifier**|DBTYPE_GUID|  
|**varbinary**|DBTYPE_BYTES|  
|**varchar**|DBTYPE_STR|  
|**XML**|DBTYPE_XML|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Поставщик OLE DB собственного клиента поддерживает преобразования данных, запрошенные потребителем, как показано на рисунке.  
  
 Объекты **sql_variant** могут хранить данные [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] любого типа, кроме text, ntext, image, varchar(max), nvarchar(max), varbinary(max), xml, timestamp и пользовательских типов данных CLR платформы Microsoft .NET Framework. Экземпляр данных sql_variant не может также иметь sql_variant в качестве базового типа данных. Например, столбец может содержать значения **smallint** в некоторых строках, значения **float** в других строках и значения **char**/**nchar** в остальных.  
  
> [!NOTE]  
>  Тип данных **sql_variant** аналогичен типу данных Variant в Microsoft Visual Basic® и типу данных DBTYPE_VARIANT, DBTYPE_SQLVARIANT в OLEDB.  
  
 Если данные **sql_variant** получены как DBTYPE_VARIANT, они размещаются в структуре VARIANT в буфере. Однако подтипы в структуре VARIANT могут не соответствовать подтипам, определенным в типе данных **sql_variant**. Затем данные **sql_variant** должны быть выбраны как DBTYPE_SQLVARIANT для сопоставления всех подтипов.  
  
## <a name="dbtype_sqlvariant-data-type"></a>Тип данных DBTYPE_SQLVARIANT  
 Для поддержки **sql_variant** типа данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поставщик OLE DB собственного клиента предоставляет тип данных, зависящий от поставщика, который называется DBTYPE_SQLVARIANT. Если данные **sql_variant** получены как DBTYPE_SQLVARIANT, они размещаются в специфической для поставщика структуре SSVARIANT. Структура SSVARIANT содержит все подтипы, сопоставленные с подтипами типа данных **sql_variant**.  
  
 Свойство сеанса SSPROP_ALLOWNATIVEVARIANT должно быть равно true.  
  
## <a name="provider-specific-property-ssprop_allownativevariant"></a>Специфическое для поставщика свойство SSPROP_ALLOWNATIVEVARIANT  
 При выборке данных можно явно указать разновидность типа данных, которые должны быть возвращены для столбца или для параметра. Интерфейс **IColumnsInfo** может также использоваться с целью получения сведений о столбце и их применения для привязки. Если интерфейс **IColumnsInfo** используется для получения сведений о столбце с целью привязки, а свойство сеанса SSPROP_ALLOWNATIVEVARIANT имеет значение FALSE (по умолчанию), для столбцов **sql_variant** возвращается DBTYPE_VARIANT. Если свойство SSPROP_ALLOWNATIVEVARIANT имеет значение FALSE, DBTYPE_SQLVARIANT не поддерживается. Если свойство SSPROP_ALLOWNATIVEVARIANT имеет значение TRUE, возвращается тип столбца DBTYPE_SQLVARIANT. В этом случае в буфере сохраняется структура SSVARIANT. При выборке данных **sql_variant** как данных типа DBTYPE_SQLVARIANT свойство сеанса SSPROP_ALLOWNATIVEVARIANT должно быть равно TRUE.  
  
 Свойство SSPROP_ALLOWNATIVEVARIANT является частью специфического для поставщика набора свойств DBPROPSET_SQLSERVERSESSION и свойством сеанса.  
  
 DBTYPE_VARIANT применяется ко всем другим поставщикам OLE DB.  
  
## <a name="ssprop_allownativevariant"></a>Свойство SSPROP_ALLOWNATIVEVARIANT  
 Свойство SSPROP_ALLOWNATIVEVARIANT является свойством сеанса и частью набора свойств DBPROPSET_SQLSERVERSESSION.  
  
|Идентификатор свойства|Описание|  
|-|-|  
|Свойство SSPROP_ALLOWNATIVEVARIANT|Тип: VT_BOOL.<br /><br /> Ч/З Чтение/запись<br /><br /> Значение по умолчанию: VARIANT_FALSE<br /><br /> Описание. Определяет тип для извлеченных данных: DBTYPE_VARIANT или DBTYPE_SQLVARIANT.<br /><br /> VARIANT_TRUE: Возвращается тип столбца DBTYPE_SQLVARIANT, и в буфер помещается структура SSVARIANT.<br /><br /> VARIANT_FALSE: Возвращается тип столбца DBTYPE_VARIANT, и в буфер помещается структура VARIANT.|  
  
## <a name="see-also"></a>См. также:  
 [Типы данных (OLE DB)](../../relational-databases/native-client-ole-db-data-types/data-types-ole-db.md)  
  
  

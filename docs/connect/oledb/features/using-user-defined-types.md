---
title: Использование пользовательских типов | Документация Майкрософт
description: OLE DB Driver for SQL Server поддерживает пользовательские типы как двоичные типы с метаданными, что позволяет управлять пользовательскими типами как объектами.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DBPROPSET_DATASOURCEINFO property set
- UDTs [OLE DB Driver for SQL Server]
- user-defined types [SQL Server], OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server, user-defined types
- DBPROPSET_SQLSERVERPARAMETER property set
- IColumnsRowset interface
- MSOLEDBSQL, user-defined types
- OLE DB Driver for SQL Server, user-defined types
- data access [OLE DB Driver for SQL Server], user-defined types
- ISSCommandWithParameters interface
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 575d81916ee080a649ff27aa49dc32e802c739bd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751104"
---
# <a name="using-user-defined-types"></a>Использование определяемых пользователем типов данных
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  В [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] представлены определяемые пользователем типы данных (UDT). Пользовательские типы расширяют систему типов SQL путем разрешения хранения объектов и пользовательских структур данных в базе данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Определяемые пользователем типы могут содержать несколько типов данных, и их поведение может отличаться от традиционных псевдонимов типов данных, которые состоят из одного системного типа данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Определяемые пользователем типы определяются с помощью любого языка, поддерживаемого средой .NET CLR, который создает поддающийся проверке код. Сюда входят языки Microsoft Visual C#<sup>®</sup> и Visual Basic<sup>®</sup> .NET. Данные представляются в виде полей и свойств класса или структуры .NET, а поведения определяются методами класса или структуры.  
  
 Пользовательский тип можно использовать в качестве определения столбца таблицы, переменной в пакете [!INCLUDE[tsql](../../../includes/tsql-md.md)], аргумента функции или хранимой процедуры [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
## <a name="ole-db-driver-for-sql-server"></a>Драйвер OLE DB для SQL Server  
 Драйвер OLE DB для SQL Server поддерживает пользовательские типы как двоичные типы с метаданными, что позволяет управлять пользовательскими типами как объектами. Столбцы пользовательских типов представляются как DBTYPE_UDT, и их метаданные доступны через основной интерфейс OLE DB **IColumnRowset** и новый интерфейс [ISSCommandWithParameters](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md).  
  
> [!NOTE]  
>  Метод **IRowsetFind::FindNextRow** не работает с пользовательским типом данных. Если определяемый пользователем тип используется в качестве типа столбца поиска, возвращается значение DB_E_BADCOMPAREOP.  
  
### <a name="data-bindings-and-coercions"></a>Привязки данных и приведение типов  
 В следующей таблице описаны привязка и приведение типа данных, которые возникают при использовании перечисленных определенных пользователем типов данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Столбцы пользовательских типов представляются через OLE DB Driver for SQL Server как тип DBTYPE_UDT. Метаданные можно получать через соответствующие наборы строк схемы, так что можно управлять собственными определенными типами как объектами.  
  
|Тип данных|На сервер<br /><br /> **UDT**|На сервер<br /><br /> **Не пользовательский тип**|С сервера<br /><br /> **UDT**|С сервера<br /><br /> **Не пользовательский тип**|  
|---------------|---------------------------|--------------------------------|-----------------------------|----------------------------------|  
|DBTYPE_UDT|Поддерживается<sup>6</sup>|Error<sup>1</sup>|Поддерживается<sup>6</sup>|Ошибка<sup>5</sup>|  
|DBTYPE_BYTES|Поддерживается<sup>6</sup>|Н/Д<sup>2</sup>|Поддерживается<sup>6</sup>|Н/Д<sup>2</sup>|  
|DBTYPE_WSTR|Поддерживается<sup>3,6</sup>|Н/Д<sup>2</sup>|Поддерживается<sup>4,6</sup>|Н/Д<sup>2</sup>|  
|DBTYPE_BSTR|Поддерживается<sup>3,6</sup>|Н/Д<sup>2</sup>|Поддерживается<sup>4</sup>|Н/Д<sup>2</sup>|  
|DBTYPE_STR|Поддерживается<sup>3,6</sup>|Н/Д<sup>2</sup>|Поддерживается<sup>4,6</sup>|Н/Д<sup>2</sup>|  
|DBTYPE_IUNKNOWN|Не поддерживается|Н/Д<sup>2</sup>|Не поддерживается|Н/Д<sup>2</sup>|  
|DBTYPE_VARIANT (VT_UI1 &#124; VT_ARRAY)|Поддерживается<sup>6</sup>|Н/Д<sup>2</sup>|Поддерживается<sup>4</sup>|Н/Д<sup>2</sup>|  
|DBTYPE_VARIANT (VT_BSTR)|Поддерживается<sup>3,6</sup>|Н/Д<sup>2</sup>|Недоступно|Н/Д<sup>2</sup>|  
  
 <sup>1</sup>Если тип сервера, отличный от DBTYPE_UDT, указывается с помощью метода **ICommandWithParameters::SetParameterInfo** и типом метода доступа является DBTYPE_UDT, то при выполнении инструкции возникает ошибка (DB_E_ERRORSOCCURRED; состояние параметра — DBSTATUS_E_BADACCESSOR). В остальных случаях данные отсылаются на сервер, но сервер возвращает ошибку, указывающую на то, что нет неявного преобразования определяемого пользовательского типа в тип данных параметра.  
  
 <sup>2</sup>Выходит за рамки этой статьи.  
  
 <sup>3</sup> Происходит преобразование шестнадцатеричной строки в двоичные данные.  
  
 <sup>4</sup> Происходит преобразование двоичных данных в шестнадцатеричную строку.  
  
 <sup>5</sup>Во время создания метода доступа или во время выборки может произойти проверка данных. Ошибка — DB_E_ERRORSOCCURRED, состояние привязки устанавливается в значение DBBINDSTATUS_UNSUPPORTEDCONVERSION.  
  
 <sup>6</sup>Может использоваться BY_REF.  
  
 Типы DBTYPE_NULL и DBTYPE_EMPTY могут быть привязаны только для входных параметров. Они не могут быть привязаны для выходных параметров или результатов. При привязке входных параметров состояние должно быть установлено в значение DBSTATUS_S_ISNULL или DBSTATUS_S_DEFAULT.  
  
 Тип DBTYPE_UDT также может быть преобразован в типы DBTYPE_EMPTY и DBTYPE_NULL, но тип DBTYPE_NULL и DBTYPE_EMPTY нельзя преобразовать в тип DBTYPE_UDT. Это правило обеспечивает согласование с DBTYPE_BYTES.  
  
> [!NOTE]  
>  Новый интерфейс **ISSCommandWithParameters**, наследуемый от интерфейса **ICommandWithParameters**, используется для работы с пользовательскими типами как с параметрами. Приложения должны использовать этот интерфейс для установки, по крайней мере, атрибута SSPROP_PARAM_UDT_NAME набора свойств DBPROPSET_SQLSERVERPARAMETER для параметров определяемых пользователем типов. Если это не сделано, метод **ICommand::Execute** возвратит ошибку DB_E_ERRORSOCCURRED. Этот интерфейс и набор свойств описан далее в этой статье.  
  
 Если определяемый пользователем тип вставлен в столбец, который недостаточно велик для хранения всех его данных, метод **ICommand::Execute** вернет S_OK с состоянием DB_E_ERRORSOCCURRED.  
  
 Преобразования данных, выполняемые основными службами OLE DB (**IDataConvert**), неприменимы к типу DBTYPE_UDT. Другие привязки не поддерживаются.  
  
### <a name="ole-db-rowset-additions-and-changes"></a>Добавления и изменения для наборов строк OLE DB  
 OLE DB Driver for SQL Server добавляет новые значения или изменяет многие из основных наборов строк схемы OLE DB.  
  
#### <a name="the-procedure_parameters-schema-rowset"></a>Набор строк схемы PROCEDURE_PARAMETERS  
 В набор строк схемы PROCEDURE_PARAMETERS были сделаны следующие добавления.  
  
|Имя столбца|Тип|Description|  
|-----------------|----------|-----------------|  
|SS_UDT_CATALOGNAME|DBTYPE_WSTR|Идентификатор трехкомпонентного имени.|  
|SS_UDT_SCHEMANAME|DBTYPE_WSTR|Идентификатор трехкомпонентного имени.|  
|SS_UDT_NAME|DBTYPE_WSTR|Идентификатор трехкомпонентного имени.|  
|SS_UDT_ASSEMBLY_TYPENAME|DBTYPE_WSTR|Полное имя сборки, которое включает имя типа и все идентификационные данные сборки, необходимые для ссылки на сборку из среды CLR.|  
  
#### <a name="the-sql_assemblies-schema-rowset"></a>Набор строк схемы SQL_ASSEMBLIES  
 Драйвер OLE DB для SQL Server представляет новый набор строк схемы, определенной поставщиком, который описывает зарегистрированные пользовательские типы. Сервер ASSEMBLY может быть указан как тип DBTYPE_WSTR, но он отсутствует в наборе строк. Если значение не задано, то по умолчанию для набора строк будет использоваться текущий сервер. Набор строк схемы SQL_ASSEMBLIES определен в приведенной ниже таблице.  
  
|Имя столбца|Тип|Description|  
|-----------------|----------|-----------------|  
|ASSEMBLY_CATALOG|DBTYPE_WSTR|Имя каталога сборки, содержащей тип.|  
|ASSEMBLY_SCHEMA|DBTYPE_WSTR|Имя схемы или имя владельца сборки, содержащей тип. Хотя сборки ограничены базой данных, а не схемой, они все еще имеют владельца, отраженного здесь.|  
|ASSEMBLY_NAME|DBTYPE_WSTR|Имя сборки, содержащей тип.|  
|ASSEMBLY_ID|DBTYPE_UI4|Идентификатор объекта сборки, содержащей тип.|  
|PERMISSION_SET|DBTYPE_WSTR|Значение, определяющее область доступа сборки. Значения включают «SAFE», «EXTERNAL_ACCESS» и «UNSAFE».|  
|ASSEMBLY_BINARY|DBTYPE_BYTES|Двоичное представление сборки.|  
  
#### <a name="the-sql_assemblies_-dependencies-schema-rowset"></a>Набор строк схемы SQL_ASSEMBLIES_ DEPENDENCIES  
 Драйвер OLE DB для SQL Server представляет новый набор строк схемы, определенной поставщиком, который описывает зависимости сборки указанного сервера. ASSEMBLY_SERVER может быть указан участником как тип DBTYPE_WSTR, но отсутствовать в наборе строк. Если значение не задано, то по умолчанию для набора строк будет использоваться текущий сервер. Набор строк схемы SQL_ASSEMBLY_DEPENDENCIES определен в приведенной ниже таблице.  
  
|Имя столбца|Тип|Description|  
|-----------------|----------|-----------------|  
|ASSEMBLY_CATALOG|DBTYPE_WSTR|Имя каталога сборки, содержащей тип.|  
|ASSEMBLY_SCHEMA|DBTYPE_WSTR|Имя схемы или имя владельца сборки, содержащей тип. Хотя сборки ограничены базой данных, а не схемой, они все еще имеют владельца, отраженного здесь.|  
|ASSEMBLY_ID|DBTYPE_UI4|Идентификатор объекта сборки.|  
|REFERENCED_ASSEMBLY_ID|DBTYPE_UI4|Идентификатор объекта сборки, на которую присутствует ссылка.|  
  
#### <a name="the-sql_user_types-schema-rowset"></a>Набор строк схемы SQL_USER_TYPES  
 Драйвер OLE DB для SQL Server предоставляет новый набор строк схемы — SQL_USER_TYPES, который описывает момент добавления зарегистрированных пользовательских типов для указанного сервера. UDT_SERVER должен быть указан участником как тип DBTYPE_WSTR, но отсутствовать в наборе строк. Набор строк схемы SQL_USER_TYPES определен в следующей таблице.  
  
|Имя столбца|Тип|Description|  
|-----------------|----------|-----------------|  
|UDT_CATALOGNAME|DBTYPE_WSTR|Для столбцов пользовательского типа это свойство содержит строку, представляющую имя каталога, в котором определен этот тип.|  
|UDT_SCHEMANAME|DBTYPE_WSTR|Для столбцов пользовательского типа это свойство содержит строку, представляющую имя схемы, в которой определен этот тип.|  
|UDT_NAME|DBTYPE_WSTR|Имя сборки, содержащей класс определяемого пользователем типа.|  
|UDT_ASSEMBLY_TYPENAME|DBTYPE_WSTR|Полное имя типа (AQN) включает имя типа и префикс пространства имен (если оно задано).|  
  
#### <a name="the-columns-schema-rowset"></a>Набор строк схемы COLUMNS  
 Добавления в наборе строк схемы COLUMNS включают перечисленные ниже столбцы.  
  
|Имя столбца|Тип|Description|  
|-----------------|----------|-----------------|  
|SS_UDT_CATALOGNAME|DBTYPE_WSTR|Для столбцов пользовательского типа это свойство содержит строку, представляющую имя каталога, в котором определен этот тип.|  
|SS_UDT_SCHEMANAME|DBTYPE_WSTR|Для столбцов пользовательского типа это свойство содержит строку, представляющую имя схемы, в которой определен этот тип.|  
|SS_UDT_NAME|DBTYPE_WSTR|Имя определяемого пользователем типа.|  
|SS_UDT_ASSEMBLY_TYPENAME|DBTYPE_WSTR|Полное имя типа (AQN) включает имя типа и префикс пространства имен (если оно задано).|  
  
### <a name="ole-db-property-set-additions-and-changes"></a>Добавления и изменения для наборов свойств OLE DB  
 OLE DB Driver for SQL Server добавляет новые значения или изменяет многие из основных наборов свойств OLE DB.  
  
#### <a name="the-dbpropset_sqlserverparameter-property-set"></a>Набор свойств DBPROPSET_SQLSERVERPARAMETER  
 Для поддержки пользовательских типов через OLE DB в драйвере OLE DB для SQL Server реализован новый набор свойств DBPROPSET_SQLSERVERPARAMETER, который содержит указанные ниже значения.  
  
|Имя|Тип|Description|  
|----------|----------|-----------------|  
|SSPROP_PARAM_UDT_CATALOGNAME|DBTYPE_WSTR|Идентификатор трехкомпонентного имени.<br /><br /> Для параметров определяемого пользователем типа это свойство содержит строку, представляющую имя каталога, в котором определен этот тип.|  
|SSPROP_PARAM_UDT_SCHEMANAME|DBTYPE_WSTR|Идентификатор трехкомпонентного имени.<br /><br /> Для параметров определяемого пользователем типа данное свойство содержит строку, представляющую имя схемы, в которой определен этот тип.|  
|SSPROP_PARAM_UDT_NAME|DBTYPE_WSTR|Идентификатор трехкомпонентного имени.<br /><br /> Для столбцов определяемого пользователем типа это свойство содержит строку, представляющую однокомпонентное имя определяемого пользователем типа.|  
  
 Свойство SSPROP_PARAM_UDT_NAME является обязательным. Свойства SSPROP_PARAM_UDT_CATALOGNAME и SSPROP_PARAM_UDT_SCHEMANAME не обязательны. Если какое-либо свойство указывается неправильно, возвращается ошибка DB_E_ERRORSINCOMMAND. Если свойства SSPROP_PARAM_UDT_CATALOGNAME и SSPROP_PARAM_UDT_SCHEMANAME не указаны, то определяемый пользователем тип должен быть определен в той же базе данных и схеме, что и таблица. Если определение определяемого пользователем типа и таблица находятся в разных схемах (но в одной базе данных), то должно быть указано свойство SSPROP_PARAM_UDT_SCHEMANAME. Если определение определяемого пользователем типа находится в другой базе данных, то должны быть указаны свойства SSPROP_PARAM_UDT_CATALOGNAME и SSPROP_PARAM_UDT_SCHEMANAME.  
  
#### <a name="the-dbpropset_sqlservercolumn-property-set"></a>Набор свойств DBPROPSET_SQLSERVERCOLUMN  
 Для поддержки создания таблиц с помощью интерфейса **ITableDefinition** драйвер OLE DB для SQL Server добавляет к набору свойств DBPROPSET_SQLSERVERCOLUMN три новых столбца.  
  
|Имя|Описание|Тип|Description|  
|----------|-----------------|----------|-----------------|  
|SSPROP_COL_UDT_CATALOGNAME|UDT_CATALOGNAME|VT_BSTR|Для столбцов типа DBTYPE_UDT это свойство содержит строку, указывающую имя каталога, в котором определен определяемый пользователем тип.|  
|SSPROP_COL_UDT_SCHEMANAME|UDT_SCHEMANAME|VT_BSTR|Для столбцов типа DBTYPE_UDT это свойство содержит строку, указывающую имя схемы, в которой определен определяемый пользователем тип.|  
|SSPROP_COL_UDT_NAME|UDT_NAME|VT_BSTR|Для столбцов типа DBTYPE_UDT это свойство содержит строку, указывающую однокомпонентное имя определяемого пользователем типа. Для других типов столбцов это свойство возвращает пустую строку.|  
  
> [!NOTE]  
>  Определяемые пользователем типы не отображаются в наборе строк схемы PROVIDER_TYPES. Все столбцы доступны для чтения и записи.  
  
 ADO будет ссылаться на эти свойства с помощью соответствующей записи в столбце «Описание».  
  
 Свойство SSPROP_COL_UDTNAME является обязательным. Свойства SSPROP_COL_UDT_CATALOGNAME и SSPROP_COL_UDT_SCHEMANAME не обязательны. Если какое-либо свойство указывается неправильно, возвращается ошибка **DB_E_ERRORSINCOMMAND**.  
  
 Если ни свойство SSPROP_PARAM_UDT_CATALOGNAME, ни свойство SSPROP_PARAM_UDT_SCHEMANAME не указаны, то определяемый пользователем тип должен быть определен в той же базе данных и схеме, что и таблица.  
  
 Если определение определяемого пользователем типа и таблица находятся в разных схемах (но в одной базе данных), то должно быть указано свойство SSPROP_COL_UDT_SCHEMANAME.  
  
 Если определение определяемого пользователем типа находится в другой базе данных, то должны быть указаны свойства SSPROP_COL_UDT_CATALOGNAME и SSPROP_COL_UDT_CATALOGNAME.  
  
### <a name="ole-db-interface-additions-and-changes"></a>Добавления и изменения для интерфейсов OLE DB  
 OLE DB Driver for SQL Server добавляет новые значения или изменяет многие из основных интерфейсов OLE DB.  
  
#### <a name="the-isscommandwithparameters-interface"></a>Интерфейс ISSCommandWithParameters  
 Для поддержки пользовательских типов посредством OLE DB в драйвере OLE DB для SQL Server реализовано несколько изменений, включая добавление интерфейса **ISSCommandWithParameters**. Этот новый интерфейс наследует основной интерфейс OLE DB — **ICommandWithParameters**. Помимо трех методов, наследуемых от интерфейса **ICommandWithParameters** — **GetParameterInfo**, **MapParameterNames** и **SetParameterInfo**, — интерфейс **ISSCommandWithParameters** содержит методы **GetParameterProperties** и **SetParameterProperties**, которые используются для обработки серверных типов данных.  
  
> [!NOTE]  
>  Интерфейс **ISSCommandWithParameters** также задействует возможности новой структуры SSPARAMPROPS.  
  
#### <a name="the-icolumnsrowset-interface"></a>Интерфейс IColumnsRowset  
 Помимо интерфейса **ISSCommandWithParameters**, драйвер OLE DB для SQL Server добавляет новые значения в набор строк, возвращаемый вызовом метода **IColumnsRowset::GetColumnRowset**, включая указанные ниже.  
  
|Имя столбца|Тип|Description|  
|-----------------|----------|-----------------|  
|DBCOLUMN_SS_UDT_CATALOGNAME|DBTYPE_WSTR|Идентификатор имени каталога определяемого пользователем типа.|  
|DBCOLUMN_SS_UDT_SCHEMANAME|DBTYPE_WSTR|Идентификатор имени схемы определяемого пользователем типа.|  
|DBCOLUMN_SS_UDT_NAME|DBTYPE_WSTR|Идентификатор имени определяемого пользователем типа.|  
|DBCOLUMN_SS_ASSEMBLY_TYPENAME|DBTYPE_WSTR|Полное имя сборки, которое включает имя типа и все идентификационные данные сборки, необходимые для ссылки на сборку из среды CLR.|  
  
 Если столбец DBCOLUMN_TYPE имеет значение DBTYPE_UDT, то серверный столбец пользовательского типа можно отличить от других двоичных типов, изучив добавленные метаданные пользовательского типа, указанные в предыдущей таблице. Если данные частично завершены, то тип сервера является определяемым пользователем типом. Для типов сервера, отличных от определяемых пользователем типов, эти столбцы всегда возвращают значение NULL.  
 
  
## <a name="see-also"></a>См. также:  
 [Возможности драйвера OLE DB для SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  

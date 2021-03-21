---
title: Использование типов данных XML | Документация Майкрософт
description: Сведения о типах данных XML в OLE DB Driver for SQL Server, позволяющих хранить XML-документы и фрагменты в базе данных SQL Server.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- IRowsetChange interface
- IRowsetUpdate interface
- data access [OLE DB Driver for SQL Server], xml data type
- OLE DB Driver for SQL Server schema rowsets
- PROVIDER_TYPES rowset
- IColumnsInfo interface
- IRowsetFind interface
- IColumnsRowset interface
- PROCEDURE_PARAMETERS rowset
- MSOLEDBSQL, XML
- xml data type [SQL Server], OLE DB Driver for SQL Server
- OLE DB Driver for SQL Server, XML
- IRowset interface
- ISequentialStream interface
- ISSCommandWithParameters interface
- SS_XMLSCHEMA rowset
- OLE DB Driver for SQL Server OLE DB interfaces
- XML [SQL Server], OLE DB Driver for SQL Server
- COLUMNS rowset
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 087429db4c42793799eb1abb7a08ab4267b1afa0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742434"
---
# <a name="using-xml-data-types"></a>Использование типов данных XML
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  В версии [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] был введен тип данных **xml**, позволяющий хранить XML-документы и их фрагменты в базе данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Тип данных **xml** — это встроенный в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] тип данных, несколько напоминающий другие встроенные типы данных, такие как **int** и **varchar**. Как и другие встроенные типы данных, тип данных **xml** можно использовать как тип столбца при создании таблицы, как тип переменной, параметра, тип возвращаемого функцией значения, а также в инструкциях CAST и CONVERT.  
  
## <a name="programming-considerations"></a>Замечания по программированию  
 Язык XML может описывать сам себя в том смысле, что он может по желанию включать заголовок XML, описывающий кодировку документа, например:  
  
 `<?xml version="1.0" encoding="windows-1252"?><doc/>`  
  
 Стандарт языка XML описывает, как обработчик XML определяет кодировку, использованную в документе, по первым нескольким байтам документа. Существует возможность, что кодировка, заданная приложением, не совпадет с кодировкой, заданной в документе. Для документов, переданных как связанные параметры, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] обрабатывает XML как двоичные данные, поэтому никаких преобразований не производится, и синтаксический анализатор XML может без проблем использовать кодировку, указанную в документе. Однако для XML-данных, привязанных как WSTR, приложение должно проверить, что документ имеет кодировку Юникод. В этом сценарии, возможно, придется загрузить документ в модель DOM, изменить кодировку на Юникод и сериализовать документ. Если этого не сделать, может произойти преобразование данных, в результате чего образуется недопустимый или испорченный XML-документ.  
  
 Возможен также конфликт, если XML задается в виде литерала. Например, приведенные ниже инструкции являются недопустимыми.  
  
 `INSERT INTO xmltable(xmlcol) VALUES('<?xml version="1.0" encoding="UTF-16"?><doc/>')`  
  
 `INSERT INTO xmltable(xmlcol) VALUES(N'<?xml version="1.0" encoding="UTF-8"?><doc/>')`  
  
## <a name="ole-db-driver-for-sql-server"></a>Драйвер OLE DB для SQL Server 
 DBTYPE_XML — новый, специфичный для XML тип данных в OLE DB Driver for SQL Server. Кроме того, к XML-данным можно получить доступ через существующие типы OLE DB: DBTYPE_BYTES, DBTYPE_WSTR, DBTYPE_BSTR, DBTYPE_XML, DBTYPE_STR, DBTYPE_VARIANT и DBTYPE_IUNKNOWN. Данные, хранимые в столбцах типа XML, можно получить из столбца в наборе строк драйвера OLE DB для SQL Server в следующих форматах:  
  
-   Текстовая строка.  
  
-   Интерфейс **ISequentialStream**  
  
> [!NOTE]  
>  Драйвер OLE DB для SQL Server не включает в себя функции чтения SAX, но интерфейс **ISequentialStream** легко передать объектам SAX и DOM в MSXML.  
  
 Для получения больших XML-документов следует использовать интерфейс **ISequentialStream**. При работе с типами больших значений в XML используются те же технологии, что и для работы с другими типами больших значений. Дополнительные сведения см. ниже в разделе [Using Large Value Types](../../oledb/features/using-large-value-types.md) (Использование типов большого объема).  
  
 Приложение может также получать, вставлять и изменять данные, хранящиеся в столбцах типа XML в наборе строк, через обычные интерфейсы, такие как **IRow::GetColumns**, **IRowChange::SetColumns** и **ICommand::Execute**. Как и в случае с получением, приложение может передавать текстовую строку или интерфейс **ISequentialStream** драйверу OLE DB для SQL Server.  
  
> [!NOTE]  
>  Для отправки данных XML в строковом формате через интерфейс **ISequentialStream** нужно получить этот интерфейс **ISequentialStream**, задав DBTYPE_IUNKNOWN и установив в привязке аргумент *pObject*, равный NULL.  
  
 Если полученные XML-данные усечены из-за недостаточного размера буфера потребителя, возвращаемое значение длины может быть равно 0xffffffff — это означает, что длина данных неизвестна. Это поведение согласуется с реализацией в виде типа данных, передаваемого клиенту в потоке без пересылки информации о длине до пересылки самих данных. В некоторых случаях может возвращаться реальная длина: если значение целиком помещено в буфер поставщика (например, **IRowset::GetData**) или если производится преобразование данных.  
  
 XML-данные, переданные в SQL Server, сервер обрабатывает как двоичные данные. Таким образом, предотвращается преобразование данных, и это позволяет средству синтаксического анализа XML автоматически обнаружить кодировку XML. Это позволяет принимать в качестве входных данных для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] более широкий диапазон XML-документов (например, документы в кодировке UTF-8).  
  
 Если входные данные XML привязаны как тип DBTYPE_WSTR, приложение должно убедиться, что данные уже находятся в кодировке Юникод, чтобы предотвратить всякую возможность повреждения данных ненужными преобразованиями.  
  
### <a name="data-bindings-and-coercions"></a>Привязки данных и приведение типов  
 В приведенной ниже таблице перечислены привязки и приведения, происходящие, когда указанный тип данных используется вместе с типом данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **xml**.  
  
|Тип данных|На сервер<br /><br /> **XML**|На сервер<br /><br /> **Не XML**|С сервера<br /><br /> **XML**|С сервера<br /><br /> **Не XML**|  
|---------------|---------------------------|--------------------------------|-----------------------------|----------------------------------|  
|DBTYPE_XML|Передать<sup>6,7</sup>|Error<sup>1</sup>|ОК<sup>11, 6</sup>|Error<sup>8</sup>|  
|DBTYPE_BYTES|Передать<sup>6,7</sup>|Н/Д<sup>2</sup>|ОК <sup>11, 6</sup>|Н/Д <sup>2</sup>|  
|DBTYPE_WSTR|Передать<sup>6,10</sup>|Н/Д <sup>2</sup>|ОК<sup>4, 6, 12</sup>|Н/Д <sup>2</sup>|  
|DBTYPE_BSTR|Передать<sup>6,10</sup>|Н/Д <sup>2</sup>|ОК <sup>3</sup>|Н/Д <sup>2</sup>|  
|DBTYPE_STR|ОК<sup>6, 9, 10</sup>|Н/Д <sup>2</sup>|ОК<sup>5, 6, 12</sup>|Н/Д <sup>2</sup>|  
|DBTYPE_IUNKNOWN|Байтовый поток через интерфейс **ISequentialStream**<sup>7</sup>|Н/Д <sup>2</sup>|Байтовый поток через интерфейс **ISequentialStream**<sup>11</sup>|Н/Д <sup>2</sup>|  
|DBTYPE_VARIANT (VT_UI1 &#124; VT_ARRAY)|Передать<sup>6,7</sup>|Н/Д <sup>2</sup>|Недоступно|Н/Д <sup>2</sup>|  
|DBTYPE_VARIANT (VT_BSTR)|Передать<sup>6,10</sup>|Н/Д <sup>2</sup>|ОК<sup>3</sup>|Н/Д <sup>2</sup>|  
  
 <sup>1</sup>Если вместе с интерфейсом **ICommandWithParameters::SetParameterInfo** указан тип сервера, отличный от DBTYPE_XML, а в качестве типа метода доступа задан DBTYPE_XML, при выполнении инструкции произойдет ошибка (DB_E_ERRORSOCCURRED, состояние параметра будет равно DBSTATUS_E_BADACCESSOR). В противном случае данные будут отправлены на сервер, но сервер вернет ошибку, указывающую, что неявного преобразования из XML в тип данных параметра не существует.  
  
 <sup>2</sup>Выходит за рамки этой статьи.  
  
 <sup>3</sup>Формат UTF-16, метка порядка байтов (BOM) отсутствует, кодировка не указана, данные не завершаются нулем.  
  
 <sup>4</sup>Формат UTF-16, метка порядка байтов (BOM) отсутствует, кодировка не указана, данные завершаются нулем.  
  
 <sup>5</sup>Формат многобайтовый, кодировка совпадает с кодовой страницей клиента, данные завершаются нулем. Преобразование из переданной с сервера кодировки Юникода может вызвать повреждение данных, поэтому такую привязку не рекомендуется использовать.  
  
 <sup>6</sup>Может использоваться BY_REF.  
  
 <sup>7</sup>Данные в формате UTF-16 должны начинаться с метки порядка байтов (BOM). Если метка отсутствует, то кодировка может быть неправильно распознана сервером.  
  
 <sup>8</sup>Проверка может происходить во время создания метода доступа или во время получения данных. Значение ошибки — DB_E_ERRORSOCCURRED, для привязки устанавливается состояние DBBINDSTATUS_UNSUPPORTEDCONVERSION.  
  
 <sup>9</sup>Данные преобразуются в кодировку Юникода с помощью клиентской кодовой страницы до отправки данных на сервер. Если кодировка документа не соответствует клиентской кодовой странице, может произойти повреждение данных, поэтому такую привязку крайне не рекомендуется использовать.  
  
 <sup>10</sup>Метка порядка байтов (BOM) всегда добавляется к данным, отправляемым на сервер. Если в начале данных уже стоит метка порядка байтов, то в начале буфера будет две метки порядка байтов. Сервер использует первую метку для распознавания кодировки как UTF-16, а затем отбрасывает ее. Вторая метка порядка следования байтов рассматривается как символ неразрывного пробела нулевой ширины.  
  
 <sup>11</sup>Данные в формате UTF-16, кодировка не указана, метка порядка байтов (BOM) добавляется к данным, полученным с сервера. Если сервер вернул пустую строку, метка порядка байтов все равно будет возвращена приложению. Если длина буфера составляет нечетное число байтов, данные усекаются правильно. Если все значение возвращается по фрагментам данных, их можно объединить для восстановления правильного значения.  
  
 <sup>12</sup>Если длина буфера меньше двух символов — иными словами, не хватает места для завершения данных нулем, — возникает ошибка переполнения.  
  
> [!NOTE]  
>  Для XML-данных со значением NULL не возвращаются никакие данные.  
  
 Согласно стандарту XML, XML-документы в кодировке UTF-16 должны начинаться с метки порядка следования байтов (BOM); в UTF-16 это код символа 0xFEFF. При работе с привязками типов WSTR и BSTR драйвер OLE DB для SQL Server не требует и не добавляет метки порядка байтов, так как кодировка задана привязкой. При работе с привязками типов BYTES, XML и IUNKNOWN целью является предоставить возможности для наиболее простого взаимодействия с другими обработчиками XML и хранилищами данных. В этом случае в XML с кодировкой UTF-16 нужно включить метку порядка байтов, и приложение может не заботиться о том, какова на самом деле кодировка данных, потому что большинство обработчиков XML (в том числе SQL Server) вычислят кодировку по нескольким первым байтам данных. XML-данные, полученные от драйвера OLE DB для SQL Server с использованием привязок типов BYTES, XML или IUNKNOWN, всегда закодированы в кодировке UTF-16 с меткой порядка байтов и не содержат внедренной декларации кодировки.  
  
 Преобразование данных, выполняемое основными службами OLE DB (**IDataConvert**), неприменимо к типу DBTYPE_XML.  
  
 Проверка производится при отсылке данных на сервер. Изменения в отношении проверки и кодирования на стороне клиента должны обрабатываться приложением. Рекомендуется не обрабатывать XML-данные напрямую, а использовать вместо этого средство чтения DOM или SAX.  
  
 Типы DBTYPE_NULL и DBTYPE_EMPTY могут быть привязаны только для входных параметров. Они не могут быть привязаны для выходных параметров или результатов. При привязке входных параметров следует установить состояние DBSTATUS_S_ISNULL или DBSTATUS_S_DEFAULT.  
  
 DBTYPE_XML можно преобразовать в DBTYPE_EMPTY и DBTYPE_NULL, DBTYPE_EMPTY можно преобразовать в DBTYPE_XML, но DBTYPE_NULL нельзя преобразовать в DBTYPE_XML. Это согласуется с DBTYPE_WSTR.  
  
 Привязка DBTYPE_IUNKNOWN поддерживается, как показано в приведенной выше таблице, но преобразований между типами DBTYPE_XML и DBTYPE_IUNKNOWN не существует. DBTYPE_IUNKNOWN нельзя использовать с DBTYPE_BYREF.  
  
### <a name="ole-db-rowset-additions-and-changes"></a>Добавления и изменения для наборов строк OLE DB  
 OLE DB Driver for SQL Server добавляет новые значения или изменяет многие из основных наборов строк схемы OLE DB.  
  
#### <a name="the-columns-and-procedure_parameters-schema-rowsets"></a>Наборы строк схем COLUMNS и PROCEDURE_PARAMETERS  
 К наборам строк схем COLUMNS и PROCEDURE_PARAMETERS добавлены следующие столбцы:  
  
|Имя столбца|Тип|Description|  
|-----------------|----------|-----------------|  
|SS_XML_SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|Имя каталога, в котором определена коллекция схем XML. Значение NULL для столбцов, отличных от XML или нетипизированных XML-столбцов.|  
|SS_XML_SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|Имя схемы, в которой определена коллекция схем XML. Значение NULL для столбцов, отличных от XML или нетипизированных XML-столбцов.|  
|SS_XML_SCHEMACOLLECTIONNAME|DBTYPE_WSTR|Имя коллекции схем XML. Значение NULL для столбцов, отличных от XML или нетипизированных XML-столбцов.|  
  
#### <a name="the-provider_types-schema-rowset"></a>Набор строк схемы PROVIDER_TYPES  
 В наборе строк схемы PROVIDER_TYPES значение параметра COLUMN_SIZE для типа данных **xml** равно 0, а DATA_TYPE равен DBTYPE_XML.  
  
#### <a name="the-ss_xmlschema-schema-rowset"></a>Набор строк схемы SS_XMLSCHEMA  
 Для клиентов добавлен новый набор строк схемы SS_XMLSCHEMA для получения информации о схеме XML. Набор строк SS_XMLSCHEMA содержит следующие столбцы:  
  
|Имя столбца|Тип|Description|  
|-----------------|----------|-----------------|  
|SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|Каталог, которому принадлежит коллекция XML.|  
|SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|Схема, которой принадлежит коллекция XML.|  
|SCHEMACOLLECTIONNAME|DBTYPE_WSTR|Имя коллекции схем XML для типизированных XML-столбцов, NULL для всех остальных столбцов.|  
|TARGETNAMESPACEURI|DBTYPE_WSTR|Имя целевого пространства имен схемы XML.|  
|SCHEMACONTENT|DBTYPE_WSTR|Содержимое схемы XML.|  
  
 Для каждой схемы XML область действия ограничивается именем каталога, именем схемы, именем коллекции схем и URI-идентификатором целевого пространства имен. Кроме того, задан новый идентификатор GUID с именем DBSCHEMA_XML_COLLECTIONS. Количество ограничений и столбцы с ограничениями для набора строк схемы SS_XMLSCHEMA определены следующим образом.  
  
|GUID|Количество ограничений|Столбцы с ограничениями|  
|----------|----------------------------|------------------------|  
|DBSCHEMA_XML_COLLECTIONS|4|SCHEMACOLLECTION_CATALOGNAME<br /><br /> SCHEMACOLLECTION_SCHEMANAME<br /><br /> SCHEMACOLLECTIONNAME<br /><br /> TARGETNAMESPACEURI|  
  
### <a name="ole-db-property-set-additions-and-changes"></a>Добавления и изменения для наборов свойств OLE DB  
 OLE DB Driver for SQL Server добавляет новые значения или изменяет многие из основных наборов свойств OLE DB.  
  
#### <a name="the-dbpropset_sqlserverparameter-property-set"></a>Набор свойств DBPROPSET_SQLSERVERPARAMETER  
 Для поддержки типа данных **xml** при работе через OLE DB драйвер OLE DB для SQL Server реализует новый набор свойств DBPROPSET_SQLSERVERPARAMETER, содержащий следующие значения:  
  
|Имя|Тип|Description|  
|----------|----------|-----------------|  
|SSPROP_PARAM_XML_SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|Имя каталога (базы данных), где определена коллекция схем XML. Часть идентификатора трехкомпонентного имени SQL.|  
|SSPROP_PARAM_XML_SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|Имя схемы XML в коллекции схемы XML. Часть идентификатора трехкомпонентного имени SQL.|  
|SSPROP_PARAM_XML_SCHEMACOLLECTIONNAME|DBTYPE_WSTR|Имя коллекции схем XML в каталоге, представляющее собой часть трехчастного идентификатора имени SQL.|  
  
#### <a name="the-dbpropset_sqlservercolumn-property-set"></a>Набор свойств DBPROPSET_SQLSERVERCOLUMN  
 Для поддержки создания таблиц с помощью интерфейса **ITableDefinition** драйвер OLE DB для SQL Server добавляет к набору свойств DBPROPSET_SQLSERVERCOLUMN три новых столбца.  
  
|Имя|Тип|Description|  
|----------|----------|-----------------|  
|SSPROP_COL_XML_SCHEMACOLLECTION_CATALOGNAME|VT_BSTR|Для типизированных столбцов XML данное свойство содержит строку, представляющую имя каталога, где хранится схема XML. Для других типов столбцов это свойство возвращает пустую строку.|  
|SSPROP_COL_XML_SCHEMACOLLECTION_SCHEMANAME|VT_BSTR|Для типизированных столбцов XML данное свойство содержит строку, представляющую имя схемы XML, задающей этот столбец.|  
|SSPROP_COL_XML_SCHEMACOLLECTIONNAME|VT_BSTR|Для типизированных столбцов XML данное свойство содержит строку, представляющую имя коллекции схем XML, определяющей значение.|  
  
 Подобно значениям SSPROP_PARAM, все эти свойства являются необязательными и по умолчанию пусты. SSPROP_COL_XML_SCHEMACOLLECTION_CATALOGNAME и SSPROP_COL_XML_SCHEMACOLLECTION_SCHEMANAME можно задавать только при заданном свойстве SSPROP_COL_XML_SCHEMACOLLECTIONNAME. При передаче данных в формате XML на сервер, если эти значения включены, они будут проверены на существование (допустимость) в текущей базе данных, а экземпляр данных проверяется по схеме. Во всех случаях, чтобы данные были допустимыми, эти столбцы должны быть все одновременно пусты или все одновременно заполнены.  
  
### <a name="ole-db-interface-additions-and-changes"></a>Добавления и изменения для интерфейсов OLE DB  
 OLE DB Driver for SQL Server добавляет новые значения или изменяет многие из основных интерфейсов OLE DB.  
  
#### <a name="the-isscommandwithparameters-interface"></a>Интерфейс ISSCommandWithParameters  
 Для поддержки типа данных **xml** при работе через OLE DB драйвер OLE DB для SQL Server содержит ряд изменений, в том числе в него добавлен интерфейс [ISSCommandWithParameters](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md). Этот новый интерфейс наследует основной интерфейс OLE DB — **ICommandWithParameters**. Помимо трех методов, наследуемых от интерфейса **ICommandWithParameters** — **GetParameterInfo**, **MapParameterNames** и **SetParameterInfo**, — интерфейс **ISSCommandWithParameters** содержит методы [GetParameterProperties](../../oledb/ole-db-interfaces/isscommandwithparameters-getparameterproperties-ole-db.md) и [SetParameterProperties](../../oledb/ole-db-interfaces/isscommandwithparameters-setparameterproperties-ole-db.md), которые используются для обработки серверных типов данных.  
  
> [!NOTE]  
>  Интерфейс **ISSCommandWithParameters** также задействует возможности новой структуры SSPARAMPROPS.  
  
#### <a name="the-icolumnsrowset-interface"></a>Интерфейс IColumnsRowset  
 Драйвер OLE DB для SQL Server добавляет следующие, специфичные для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], столбцы к набору строк, возвращаемому методом **IColumnRowset::GetColumnsRowset**. Эти столбцы содержат трехчастное имя коллекции схем XML. Для столбцов не в формате XML и нетипизированных столбцов XML все три данных столбца по умолчанию имеют значение NULL.  
  
|Имя столбца|Тип|Description|  
|-----------------|----------|-----------------|  
|DBCOLUMN_SS_XML_SCHEMACOLLECTION_CATALOGNAME|DBTYPE_WSTR|Каталог, которому принадлежит коллекция схем XML.<br /><br /> В противном случае — значение NULL.|  
|DBCOLUMN_SS_XML_SCHEMACOLLECTION_SCHEMANAME|DBTYPE_WSTR|Схема, которой принадлежит коллекция схем XML. В противном случае — значение NULL.|  
|DBCOLUMN_SS_XML_SCHEMACOLLECTIONNAME|DBTYPE_WSTR|Имя коллекции схем XML для типизированных XML-столбцов, NULL — для всех остальных столбцов.|  
  
#### <a name="the-irowset-interface"></a>Интерфейс IRowset  
 Метод **IRowset::GetData** служит для получения экземпляра XML в столбце XML. В зависимости от привязки, указанной клиентом, экземпляр XML может быть получен в виде типа DBTYPE_BSTR, DBTYPE_WSTR, DBTYPE_VARIANT, DBTYPE_XML, DBTYPE_STR, DBTYPE_BYTES или как интерфейс через DBTYPE_IUNKNOWN. Если потребитель задает тип DBTYPE_BSTR, DBTYPE_WSTR или DBTYPE_VARIANT, поставщик преобразует экземпляр XML в запрошенный пользователем тип и помещает в местонахождение, заданное соответствующей привязкой.  
  
 Если потребитель задает DBTYPE_IUNKNOWN и устанавливает аргумент *pObject* равным NULL или задает для аргумента *pObject* значение IID_ISequentialStream, поставщик возвращает потребителю интерфейс **ISequentialStream**, чтобы потребитель мог получить из столбца данные XML в виде потока. Затем интерфейс **ISequentialStream** возвращает данные XML в виде потока символов в формате Юникода.  
  
 При возврате значения XML, привязанного к DBTYPE_IUNKNOWN, поставщик передает значение размера `sizeof (IUnknown *)`. Это поведение согласуется с подходом, который используется при передаче привязанного столбца как DBTYPE_IUnknown или DBTYPE_IDISPATCH, а также при передаче в формате DBTYPE_IUNKNOWN/ISequentialStream, когда точный размер столбца установить не удается.  
  
#### <a name="the-irowsetchange-interface"></a>Интерфейс IRowsetChange  
 Потребитель может изменить экземпляр XML в столбце двумя способами. Первый способ использует объект хранилища с интерфейсом **ISequentialStream**, созданный поставщиком. Потребитель может вызвать метод **ISequentialStream::Write** для непосредственного изменения экземпляра XML, возвращенного поставщиком.  
  
 Второй подход использует методы **IRowsetChange::SetData** или **IRowsetChange::InsertRow**. При таком подходе экземпляр XML в буфере потребителя можно задать привязкой типа DBTYPE_BSTR, DBTYPE_WSTR, DBTYPE_VARIANT, DBTYPE_XML или DBTYPE_IUNKNOWN.  
  
 Если указан тип DBTYPE_BSTR, DBTYPE_WSTR или DBTYPE_VARIANT, поставщик сохраняет экземпляр XML из буфера потребителя в соответствующий столбец.  
  
 Если указан тип DBTYPE_IUNKNOWN/ISequentialStream и потребитель не задал объект хранилища, потребитель должен создать объект **ISequentialStream** заранее, привязать к этому объекту XML-документ, а затем передать объект поставщику методом **IRowsetChange::SetData**. Потребитель может также создать объект хранилища, установить аргумент pObject равным IID_ISequentialStream, создать объект **ISequentialStream**, а затем передать этот объект **ISequentialStream** в метод **IRowsetChange::SetData**. В обоих случаях поставщик может получить объект XML через объект **ISequentialStream** и вставить в нужный столбец.  
  
#### <a name="the-irowsetupdate-interface"></a>Интерфейс IRowsetUpdate  
 Интерфейс **IRowsetUpdate** предоставляет функциональность отсроченных изменений. Данные, доступные наборам строк, недоступны другим транзакциям, пока потребитель не вызовет метод **IRowsetUpdate::Update**.  
  
#### <a name="the-irowsetfind-interface"></a>Интерфейс IRowsetFind  
 Метод **IRowsetFind::FindNextRow** не работает с типом **xml**. При вызове **IRowsetFind::FindNextRow**, если в аргументе *hAccessor* передается столбец типа DBTYPE_XML, будет возвращен результат DB_E_BADBINDINFO. Это происходит независимо от типа столбца, в котором производится поиск данных. Для любого другого типа привязки вызов **FindNextRow** возвращает результат DB_E_BADCOMPAREOP, если столбец, в котором производится поиск данных, имеет тип **xml**.  
 
  
## <a name="see-also"></a>См. также:  
 [Возможности драйвера OLE DB для SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md)    
 [ISSCommandWithParameters &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/isscommandwithparameters-ole-db.md)  
  
  

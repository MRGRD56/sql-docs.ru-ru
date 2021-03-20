---
title: Структура SSVARIANT (драйвер OLE DB)
description: Структура SSVARIANT в OLE DB Driver for SQL Server
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
f1_keywords:
- SSVARIANT
helpviewer_keywords:
- SSVARIANT struct
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7095942d695c57667e47d32bb6c60a97c51fbd59
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752024"
---
# <a name="ssvariant-structure"></a>Структура SSVARIANT
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Структура **SSVARIANT**, определенная в файле msoledbsql.h, соответствует значению DBTYPE_SQLVARIANT в OLE DB Driver for SQL Server.  
  
 **SSVARIANT** представляет собой избирательное соединение. В зависимости от значения элемента vt объект-получатель может определить, какой элемент следует считывать. Значения vt соответствуют типам данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Таким образом, структура **SSVARIANT** может содержать любой тип SQL Server. Дополнительные сведения о структуре данных для стандартных типов OLE DB см. в статье об [индикаторах типа](/previous-versions/windows/desktop/ms711251(v=vs.85)).  
  
## <a name="remarks"></a>Remarks  
 Если DataTypeCompat==80, несколько подтипов **SSVARIANT** становятся строками. Например, следующие значения vt будут представлены в **SSVARIANT** в виде VT_SS_WVARSTRING:  
  
-   VT_SS_DATETIMEOFFSET  
  
-   VT_SS_DATETIME2  
  
-   VT_SS_TIME2  
  
-   VT_SS_DATE  
  
 Если DateTypeCompat == 0, то эти типы будут представлены в собственном формате.  
  
 Дополнительные сведения о SSPROP_INIT_DATATYPECOMPATIBILITY см. в статье об [использовании ключевых слов строки подключения с OLE DB Driver for SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md).  
  
 Файл msoledbsql.h содержит макросы для доступа к значениям variant, которые упрощают разыменование типов, входящих в структуру **SSVARIANT**. В качестве примера можно рассмотреть макрос V_SS_DATETIMEOFFSET, который можно использовать следующим образом:  
  
```  
memcpy(&V_SS_DATETIMEOFFSET(pssVar).tsoDateTimeOffsetVal, pDTO, cbNative);  
V_SS_DATETIMEOFFSET(pssVar).bScale = bScale;  
```  
  
 Полный набор макросов доступа для каждого элемента структуры **SSVARIANT** см. в файле msoledbsql.h.  
  
 В приведенной ниже таблице описываются элементы структуры **SSVARIANT**.  
  
|Участник|Индикатор типа OLE DB|Тип данных OLE DB|Значение vt|Комментарии|  
|------------|---------------------------|------------------------|--------------|--------------|  
|vt|SSVARTYPE|||Указывает тип значения, которое содержится в структуре **SSVARIANT**.|  
|bTinyIntVa|DBTYPE_UI1|**BYTE**|**VT_SS_UI1**|Поддерживает тип данных **tinyint**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|sShortIntVal|DBTYPE_I2|**SHORT**|**VT_SS_I2**|Поддерживает тип данных **smallint**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|lIntVal|DBTYPE_I4|**LONG**|**VT_SS_I4**|Поддерживает тип данных **int**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|llBigIntVal|DBTYPE_I8|**LARGE_INTEGER**|**VT_SS_I8**|Поддерживает тип данных **bigint**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|fltRealVal|DBTYPE_R4|**float**|**VT_SS_R4**|Поддерживает тип данных **real**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|dblFloatVal|DBTYPE_R8|**double**|**VT_SS_R8**|Поддерживает тип данных **float**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|cyMoneyVal|DBTYPE_CY|**LARGE_INTEGER**|**VT_SS_MONEY VT_SS_SMALLMONEY**|Поддерживает типы данных **money** и **smallmoney**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|fBitVal|DBTYPE_BOOL|**VARIANT_BOOL**|**VT_SS_BIT**|Поддерживает тип данных **bit**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|rgbGuidVal|DBTYPE_GUID|**GUID**|**VT_SS_GUID**|Поддерживает тип данных **uniqueidentifier**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|numNumericVal|DBTYPE_NUMERIC|**DB_NUMERIC**|**VT_SS_NUMERIC**|Поддерживает тип данных **numeric**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|dDateVal|DBTYPE_DATE|**DBDATE**|**VT_SS_DATE**|Поддерживает тип данных **date**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|tsDateTimeVal|DBTYPE_DBTIMESTAMP|**DBTIMESTAMP**|**VT_SS_SMALLDATETIME VT_SS_DATETIME VT_SS_DATETIME2**|Поддерживает типы данных **smalldatetime**, **datetime** и **datetime2**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|Time2Val|DBTYPE_DBTIME2|**DBTIME2**|**VT_SS_TIME2**|Поддерживает тип данных **time**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Содержит следующие элементы:<br /><br /> *tTime2Val* (**DBTIME2**)<br /><br /> *bScale* (**BYTE**) задает масштаб для значения *tTime2Val*.|  
|DateTimeVal|DBTYPE_DBTIMESTAMP|**DBTIMESTAMP**|**VT_SS_DATETIME2**|Поддерживает тип данных **datetime2**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Содержит следующие элементы:<br /><br /> *tsDataTimeVal* (DBTIMESTAMP)<br /><br /> *bScale* (**BYTE**) задает масштаб для значения *tsDataTimeVal*.|  
|DateTimeOffsetVal|DBTYPE_DBTIMESTAMPOFSET|**DBTIMESTAMPOFFSET**|**VT_SS_DATETIMEOFFSET**|Поддерживает тип данных **atetimeoffset**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Содержит следующие элементы:<br /><br /> *tsoDateTimeOffsetVal* (**DBTIMESTAMPOFFSET**)<br /><br /> *bScale* (**BYTE**) задает масштаб для значения *tsoDateTimeOffsetVal*.|  
|NCharVal|Отсутствует соответствующий индикатор типа OLE DB.|**struct _NCharVal**|**VT_SS_WVARSTRING,**<br /><br /> **VT_SS_WSTRING**|Поддерживает типы данных **nchar** и **nvarchar**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Содержит следующие элементы:<br /><br /> *sActualLength* (**SHORT**) задает фактическую длину строки, на которую указывает *pwchNCharVal*. Не содержит завершающего нуля.<br /><br /> *sMaxLength* (**SHORT**) задает максимальную длину строки, на которую указывает *pwchNCharVal*.<br /><br /> *pwchNCharVal* (**WCHAR** \*) — указатель на строку.<br /><br /> *rgbReserved* (**BYTE[5]** ) задает сведения о параметрах сортировки.<br /><br /> Неиспользуемые элементы: *dwReserved* и *pwchReserved*.|  
|CharVal|Отсутствует соответствующий индикатор типа OLE DB.|**struct _CharVal**|**VT_SS_STRING,**<br /><br /> **VT_SS_VARSTRING**|Поддерживает типы данных **char** и **varchar**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Содержит следующие элементы:<br /><br /> *sActualLength* (**SHORT**) задает фактическую длину строки, на которую указывает *pwchNCharVal*. Не содержит завершающего нуля.<br /><br /> *sMaxLength* (**SHORT**) задает максимальную длину строки, на которую указывает *pwchNCharVal*.<br /><br /> *pchCharVal* (**CHAR** \*) — указатель на строку.<br /><br /> *rgbReserved* (**BYTE[5]** ) задает сведения о параметрах сортировки.<br /><br /> Неиспользуемые элементы:<br /><br /> *dwReserved* и *pwchReserved*.|  
|BinaryVal|Отсутствует соответствующий индикатор типа OLE DB.|**struct _BinaryVal**|**VT_SS_VARBINARY,**<br /><br /> **VT_SS_BINARY**|Поддерживает типы данных **binary** и **varbinary**[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].<br /><br /> Содержит следующие элементы:<br /><br /> *sActualLength* (**SHORT**) задает фактическую длину для данных, на которую указывает *pwchNCharVal*.<br /><br /> *sMaxLength* (**SHORT**) задает максимальную длину для данных, на которые указывает *pwchNCharVal*.<br /><br /> *prgbBinaryVal* (**BYTE** \*) — указатель на двоичные данные.<br /><br /> Неиспользуемый элемент: *dwReserved*.|  
|UnknownType|НЕ ИСПОЛЬЗУЕТСЯ|НЕ ИСПОЛЬЗУЕТСЯ|НЕ ИСПОЛЬЗУЕТСЯ|НЕ ИСПОЛЬЗУЕТСЯ|  
|BLOBType|НЕ ИСПОЛЬЗУЕТСЯ|НЕ ИСПОЛЬЗУЕТСЯ|НЕ ИСПОЛЬЗУЕТСЯ|НЕ ИСПОЛЬЗУЕТСЯ|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; | &nbsp; |
  
## <a name="known-issues"></a>Известные проблемы
### <a name="possible-narrow-string-data-corruption"></a>Возможное повреждение данных для узких строк
До версии 18.4 драйвера OLE DB вставка в столбец `sql_variant` могла привести к повреждению данных на сервере при соблюдении всех следующих условий:
- Кодовая страница клиентского компьютера не соответствует кодовой странице параметров сортировки базы данных.
- Буфер клиента для вставки отличных от ASCII символов узких строк, закодированных с использованием кодовой страницы клиента.
- Выполняется одно из следующих условий:
  - Для поля `pwszDataSourceType` в структуре `DBPARAMBINDINFO`, описывающее параметр, соответствующий столбцу `sql_variant`, было установлено значение `L"DBTYPE_SQLVARIANT"`, `L"DBTYPE_VARIANT"` или `L"sql_variant"`. Подробная информация доступна в следующих статьях: [ICommandWithParameters::SetParameterInfo](/previous-versions/windows/desktop/ms725393(v=vs.85)).

    *или диспетчер конфигурации служб*
  - Подготовлен параметризованный SQL-запрос, используемый для вставки.

В частности, драйвер OLE DB не преобразовывал данные в кодовую страницу параметров сортировки базы данных перед их вставкой. Однако драйвер ошибочно сообщал серверу, что данные были закодированы с использованием кодовой страницы параметров сортировки базы данных. Такое поведение привело к несовпадению данных и соответствующей кодовой страницы, хранящейся в столбце `sql_variant`.

Аналогичным образом, при извлечении одного и того же значения драйвер OLE DB не преобразовывал строки в кодовую страницу клиента. Однако так как вставленные данные уже находятся в кодовой странице клиента (см. абзац выше), клиентское приложение может интерпретировать данные правильно. Несмотря на это, приложения, использующие другие драйверы, извлекают такие значения в поврежденном формате. Повреждение возникает из-за того, что другие драйверы интерпретируют строку в кодовой странице параметров сортировки базы данных и пытаются преобразовать ее в кодовую страницу клиента.

Начиная с версии 18.4 драйвер OLE DB преобразует узкие строки в кодовую страницу параметров сортировки базы данных перед вставкой. Аналогичным образом, при извлечении драйвер преобразует данные обратно в кодовую страницу клиента. В результате при получении данных, вставленных с помощью более ранней версии драйвера OLE DB, клиентские приложения, которые используют описанную выше ошибку, могут столкнуться с проблемами. Описанная ниже [процедура восстановления](#recovery-procedure) предназначена для предоставления рекомендаций по устранению этих проблем.

### <a name="recovery-procedure"></a>Процедура восстановления
> [!IMPORTANT]  
> Прежде чем выполнять указанные ниже шаги по восстановлению, выполните резервное копирование существующих данных.

Если в приложении возникают проблемы с извлечением данных из столбца `sql_variant` после перехода на версию 18.4 драйвера OLE DB, поврежденные данные необходимо изменить, чтобы они имели те же параметры сортировки, что и база данных, в которой они хранятся. Следующий скрипт можно использовать для восстановления одного значения из столбца `sql_variant`. Этот скрипт является шаблоном, и его необходимо изменить в соответствии с вашим сценарием.

> [!IMPORTANT]  
> Так как исходная кодовая страница данных не сохраняется, необходимо сообщить серверу, как данные были закодированы изначально. Для этого выполните скрипт в контексте базы данных, которая имеет ту же кодовую страницу, что и кодовая страница клиента, который первоначально вставлял данные. Например, если поврежденные данные были вставлены из клиента, настроенного с использованием кодовой страницы `932`, следующий скрипт необходимо выполнить в контексте базы данных с параметрами сортировки на японском языке (например, `Japanese_XJIS_100_CS_AI`).

```sql
/*
    Description:
        Template that can be used to recover the corrupted value inserted into the sql_variant column.

    Scenario:
        The database is named [YourDatabase] and it contains a table named [YourTable], which contains the corrupted value.
        Schema is named [dbo].
        The corrupted value is stored in a column of type sql_variant named [YourColumn].
        The corrupted value is sql_variant of BaseType char. For details on sql_variant properties, see:
            https://docs.microsoft.com/sql/t-sql/functions/sql-variant-property-transact-sql
*/

-- Base type in sql_variant can hold a maximum of 8000 bytes
-- For details see: 
--  https://docs.microsoft.com/sql/t-sql/data-types/sql-variant-transact-sql#remarks
DECLARE @bin VARBINARY(8000)

-- In the following lines we convert the sql_variant base type to binary.
-- <FilterExpression>
--      Is a placeholder and must be replaced with an expression that filters a single corrupted value to be recovered.
--      Therefore, the expression must result in a single value being returned only.
SET @bin = (SELECT CAST([YourColumn] AS VARBINARY(8000)) FROM [YourDatabase].[dbo].[YourTable] WHERE <FilterExpression>)

-- In the following lines we store the binary value in char(59) (a fixed-size character data type).
-- IMPORTANT NOTE: 
--      This example assumes the corrupted sql_variant's base type is char(59).
--      You MUST adjust the type (that is, char/varchar) and size to match your scenario exactly.
DECLARE @char CHAR(59)
SET @char = CAST((@bin) AS CHAR(59))
DECLARE @sqlvariant sql_variant

-- The following lines recover the corrupted value by translating the value to the collation of the database.
-- <DBCollation>
--      Must be replaced with the collation (for example, Latin1_General_100_CI_AS_SC_UTF8) of the database holding the data.
SET @sqlvariant = @char collate <DBCollation>

-- Finally, we update the corrupted value with the recovered value.
-- "<FilterExpression>"
--      Is a placeholder and must be replaced with an expression that filters a single corrupted value to be recovered.
--      Therefore, the expression must result in a single value being returned only.
UPDATE [YourDatabase].[dbo].[YourTable] SET [YourColumn] = @sqlvariant WHERE <FilterExpression>
```

## <a name="see-also"></a>См. также:  
 [Типы данных (OLE DB)](../../oledb/ole-db-data-types/data-types-ole-db.md)  
  

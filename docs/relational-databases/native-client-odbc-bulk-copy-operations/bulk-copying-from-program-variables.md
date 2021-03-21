---
description: Массовое копирование из переменных приложения
title: Групповое копирование из переменных программы | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- bulk copy [ODBC], program variables
- bcp_bind function
- mapping data types [ODBC]
- SQL Server Native Client ODBC driver, bulk copy
- data types [ODBC], bulk copying
- ODBC, bulk copy operations
- program variables [ODBC]
ms.assetid: e4284a1b-7534-4b34-8488-b8d05ed67b8c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 92ba50ba85056dc9a55bc68f91a370015ec91f2a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754774"
---
# <a name="bulk-copying-from-program-variables"></a>Массовое копирование из переменных приложения
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Массовое копирование можно производить напрямую из переменных программы. После выделения переменных для хранения данных для строки и вызова [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) для запуска операции копирования, вызовите [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) для каждого столбца, чтобы указать расположение и формат программной переменной, связываемой со столбцом. Заполните каждую переменную данными, а затем вызовите [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) , чтобы отправить на сервер одну строку данных. Повторите процесс заполнения переменных и вызова **bcp_sendrow** , пока все строки не будут отправлены на сервер, а затем вызовите метод [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md) , чтобы указать, что операция завершена.  
  
 Параметр **bcp_bind**_pData_ содержит адрес переменной, привязанной к столбцу. Данные каждого столбца можно сохранить двумя способами:  
  
-   выделить одну переменную для хранения данных;  
  
-   выделить переменную признака, сопровождаемую переменной данных.  
  
 Переменная признака определяет длину данных столбцов переменной длины, а также значения типа NULL, если они разрешены столбцом. Если используется только переменная данных, адрес этой переменной сохраняется в параметре **bcp_bind**_pData_ . Если используется переменная индикатора, адрес переменной индикатора сохраняется в параметре **bcp_bind**_pData_ . Функции операций с массовым копированием вычисляют расположение переменной данных, добавляя параметры **bcp_bind**_кбиндикатор_ и *pData* .  
  
 **bcp_bind** поддерживает три метода для работы с данными переменной длины:  
  
-   Используйте *cbData* только с переменной данных. Поместите длину данных в *cbData*. При каждом изменении длины данных, которые необходимо скопировать, вызовите [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md), чтобы сбросить *cbData*. Если используется один из двух других методов, укажите SQL_VARLEN_DATA для *cbData*. Если все значения данных, передаваемые столбцу, имеют значение NULL, укажите SQL_NULL_DATA для *cbData*.  
  
-   Используйте переменные признака. При каждом перемещении нового значения в переменную данных следует сохранять длину значения в переменной признака. Если используется один из двух других методов, укажите 0 для *кбиндикатор*.  
  
-   Используйте указатели признака конца. Загрузите параметр **bcp_bind**_птерм_ с адресом битового шаблона, который завершает данные. Если используется один из двух других методов, укажите значение NULL для *птерм*.  
  
 Все три из этих методов можно использовать в одном вызове **bcp_bind** , в этом случае используется спецификация, которая приводит к наименьшему объему копируемых данных.  
  
 Параметр _типа_ bcp_bind использует идентификаторы типов данных DB-Library, а не типы данных ODBC. DB-Library идентификаторы типов данных определяются в sqlncli. h для использования с функцией ODBC **bcp_bind** .  
  
 Функции массового копирования данных поддерживают не все типы данных ODBC C. Например, функции с массовым копированием не поддерживают структуру ODBC SQL_C_TYPE_TIMESTAMP, поэтому используйте [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) или [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) для преобразования данных ODBC SQL_TYPE_TIMESTAMP в переменную SQL_C_CHAR. Если затем использовать **bcp_bind** с параметром *типа* SQLCHARACTER для привязки переменной к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] столбцу **DateTime** , функции с массовым копированием преобразуют предложение escape-метки в символьной переменной в правильный формат DateTime.  
  
 В следующей таблице перечислены типы данных, рекомендуемые для использования в сопоставлении типа данных ODBC SQL с типом данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Тип данных ODBC SQL|Тип данных ODBC C|параметр *типа* bcp_bind|Тип данных SQL Server|  
|-----------------------|----------------------|--------------------------------|--------------------------|  
|SQL_CHAR|SQL_C_CHAR|SQLCHARACTER|**character**<br /><br /> **char**|  
|SQL_VARCHAR|SQL_C_CHAR|SQLCHARACTER|**varchar**<br /><br /> **character varying**<br /><br /> **char varying**<br /><br /> **sysname**|  
|SQL_LONGVARCHAR|SQL_C_CHAR|SQLCHARACTER|**text**|  
|SQL_WCHAR|SQL_C_WCHAR|SQLNCHAR|**nchar**|  
|SQL_WVARCHAR|SQL_C_WCHAR|SQLNVARCHAR|**nvarchar**|  
|SQL_WLONGVARCHAR|SQL_C_WCHAR|SQLNTEXT|**ntext**|  
|SQL_DECIMAL|SQL_C_CHAR|SQLCHARACTER|**decimal**<br /><br /> **уменьшение**<br /><br /> **money**<br /><br /> **smallmoney**|  
|SQL_NUMERIC|SQL_C_NUMERIC|SQLNUMERICN|**numeric**|  
|SQL_BIT|SQL_C_BIT|SQLBIT|**bit**|  
|SQL_TINYINT (подписанный)|SQL_C_SSHORT|SQLINT2|**smallint**|  
|SQL_TINYINT (неподписанный)|SQL_C_UTINYINT|SQLINT1|**tinyint**|  
|SQL_SMALL_INT (подписанный)|SQL_C_SSHORT|SQLINT2|**smallint**|  
|SQL_SMALL_INT (неподписанный)|SQL_C_SLONG|SQLINT4|**int**<br /><br /> **integer**|  
|SQL_INTEGER (подписанный)|SQL_C_SLONG|SQLINT4|**int**<br /><br /> **integer**|  
|SQL_INTEGER (неподписанный)|SQL_C_CHAR|SQLCHARACTER|**decimal**<br /><br /> **уменьшение**|  
|SQL_BIGINT (подписанный и неподписанный)|SQL_C_CHAR|SQLCHARACTER|**bigint**|  
|SQL_REAL|SQL_C_FLOAT|SQLFLT4|**real**|  
|SQL_FLOAT|SQL_C_DOUBLE|SQLFLT8|**float**|  
|SQL_DOUBLE|SQL_C_DOUBLE|SQLFLT8|**float**|  
|SQL_BINARY|SQL_C_BINARY|SQLBINARY|**binary**<br /><br /> **timestamp**|  
|SQL_VARBINARY|SQL_C_BINARY|SQLBINARY|**varbinary**<br /><br /> **Двоичные переменные**|  
|SQL_LONGVARBINARY|SQL_C_BINARY|SQLBINARY|**image**|  
|SQL_TYPE_DATE|SQL_C_CHAR|SQLCHARACTER|**datetime**<br /><br /> **smalldatetime**|  
|SQL_TYPE_TIME|SQL_C_CHAR|SQLCHARACTER|**datetime**<br /><br /> **smalldatetime**|  
|SQL_TYPE_TIMESTAMP|SQL_C_CHAR|SQLCHARACTER|**datetime**<br /><br /> **smalldatetime**|  
|SQL_GUID|SQL_C_GUID|SQLUNIQUEID|**uniqueidentifier**|  
|SQL_INTERVAL_|SQL_C_CHAR|SQLCHARACTER|**char**|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не имеет подписанного типа данных **tinyint**, без знака **smallint** **или целых чисел** без знака. Для предотвращения потери значений данных при миграции этих типов данных создайте таблицу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] со следующим наибольшим типом данных integer. Для предотвращения последующего добавления пользователями значений, находящихся за пределами диапазона, разрешенного исходными типом данных, примените к столбцу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] правило ограничения разрешенных значений до диапазона, поддерживаемого типом данных в исходном источнике:  
  
```  
CREATE TABLE Sample_Ints(STinyIntCol   SMALLINT,  
USmallIntCol INT)  
GO  
CREATE RULE STinyInt_Rule  
AS   
@range >= -128 AND @range <= 127  
GO  
CREATE RULE USmallInt_Rule  
AS   
@range >= 0 AND @range <= 65535  
GO  
sp_bindrule STinyInt_Rule, 'Sample_Ints.STinyIntCol'  
GO  
sp_bindrule USmallInt_Rule, 'Sample_Ints.USmallIntCol'  
GO  
```  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не поддерживает напрямую типы данных интервала. Однако приложение может сохранять управляющие последовательности интервала в виде символьных строк в символьном столбце [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Приложение может считывать их для дальнейшего использования, но они не могут использоваться в инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 Функции массового копирования могут использоваться для быстрой загрузки данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , который был считан из источника данных ODBC. Используйте [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md) для привязки столбцов результирующего набора к переменным программы, а затем используйте **bcp_bind** , чтобы привязать одни и те же программные переменные к операции копирования. При вызове [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) или **SQLFetch** извлекается строка данных из источника данных ODBC в переменные программы, а при вызове [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) выполняется операция копирования данных из переменных программы в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Приложение может использовать функцию [bcp_colptr](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colptr.md) в любое время, когда ей нужно изменить адрес переменной данных, первоначально заданной в параметре **bcp_bind** _pData_ . Приложение может использовать функцию [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md) в любое время, когда ей нужно изменить длину данных, изначально указанную в параметре **bcp_bind**_cbData_ .  
  
 С помощью операции массового копирования нельзя считать данные из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в переменные программы. Она не похожа на функцию «bcp_readrow». Отправлять данные можно только от приложения на сервер.  
  
## <a name="see-also"></a>См. также:  
 [Выполнение операций с массовым копированием &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)  
  
  

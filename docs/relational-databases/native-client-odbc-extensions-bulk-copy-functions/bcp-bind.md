---
description: bcp_bind
title: bcp_bind | Документация Майкрософт
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_bind
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_bind function
ms.assetid: 6e335a5c-64b2-4bcf-a88f-35dc9393f329
author: markingmyname
ms.author: maghan
ms.custom: ''
ms.reviewer: ''
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3d73f055a77bb120d8c56425426273d6a06412b0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749644"
---
# <a name="bcp_bind"></a>bcp_bind

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Привязывает данные программной переменной к столбцу таблицы для массового копирования в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="syntax"></a>Синтаксис

```  
  
RETCODE bcp_bind (  
        HDBC hdbc,
        LPCBYTE pData,  
        INT cbIndicator,  
        DBINT cbData,  
        LPCBYTE pTerm,  
        INT cbTerm,  
        INT eDataType,  
        INT idxServerCol);  
```  
  
## <a name="arguments"></a>Аргументы

 *hdbc*  
 Дескриптор соединения ODBC с поддержкой массового копирования.  
  
 *pData*  
 Указатель на копируемые данные. Если *eDataType* имеет значение SQLTEXT, SQLNTEXT, SQLXML, SQLUDT, SQLCHARACTER, SQLVARCHAR, SQLVARBINARY, SqlBinary, SQLNCHAR или SQLIMAGE, *pData* может быть равен null. ЗНАЧЕНИЕ типа *pData* , указывающее, что значения длинных данных будут отправляться [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в блоках с помощью [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md). Пользователь должен задать для *pData* значение null, если столбец, соответствующий привязанному к пользователю полю, является столбцом большого двоичного объекта, иначе **bcp_bind** завершится ошибкой.  
  
 Если в данных присутствуют признаки, они размещаются в памяти непосредственно перед данными. Параметр *pData* указывает на переменную индикатора в этом случае, а ширина индикатора, параметр *кбиндикатор* , используется массовым копированием для правильного адресации данных пользователя.  
  
 *cbIndicator*  
 Ширина индикатора в байтах или значение NULL для данных столбца. Допускаются следующие значения длины признака: 0 (если признак не используется), 1, 2, 4 или 8. Признаки размещаются в памяти непосредственно перед данными. Например, следующее определение типа структуры можно использовать для вставки целочисленных значений в таблицу [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью массового копирования:  
  
```
typedef struct tagBCPBOUNDINT  
    {  
    int iIndicator;  
    int Value;  
    } BCPBOUNDINT;  
```  
  
 В этом примере параметру *pData* присваивается адрес объявленного экземпляра структуры, адрес члена структуры *iIndicator* iIndicator. Для параметра *кбиндикатор* будет задан размер целого числа (sizeof (int)), а для параметра *cbData* снова будет задан размер целого числа (sizeof (int)). Чтобы выполнить операцию с массовым копированием строки на сервер, содержащий значение NULL для привязанного столбца, значение элемента *iIndicator* экземпляра должно быть установлено в SQL_NULL_DATA.  
  
 *cbData*  
 Количество байт данных в программной переменной, исключая любую длину, NULL-индикатор или признак конца.  
  
 Установка параметра *cbData* в значение SQL_NULL_DATA означает, что все строки, скопированные на сервер, содержат значение NULL для столбца.  
  
 Установка параметра *cbData* в значение SQL_VARLEN_DATA указывает, что система будет использовать признак конца строки или другой метод, чтобы определить длину копируемых данных.  
  
 Для типов данных фиксированной длины, например для целых чисел, система определяет длину данных на основе типа. Таким образом, для типов данных фиксированной длины *cbData* можно безопасно SQL_VARLEN_DATA или длину данных.  
  
 Для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] символьных и двоичных типов данных *cbData* может быть SQL_VARLEN_DATA, SQL_NULL_DATA, положительное значение или 0. Если *cbData* имеет SQL_VARLEN_DATA, система использует либо индикатор длины, либо значение null (если присутствует), либо последовательность признаков конца для определения длины данных. Если представлено и то, и другое, система использует то значение, которое приводит к наименьшему объему копируемых данных. Если *cbData* имеет SQL_VARLEN_DATA, то типом данных столбца является [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] символьный или двоичный тип, и не указан ни индикатор длины, ни последовательность признака конца, система возвращает сообщение об ошибке.  
  
 Если значение *cbData* равно 0 или положительному значению, система использует *cbData* в качестве длины данных. Однако если в дополнение к положительному значению *cbData* предоставляется индикатор длины или последовательность признака конца, система определяет длину данных с помощью метода, который приводит к наименьшему объему копируемых данных.  
  
 Значение параметра *cbData* представляет число байтов данных. Если символьные данные представлены расширенными символами Юникода, то положительное значение параметра *cbData* представляет собой число символов, умноженное на размер в байтах каждого символа.  
  
 *pTerm*  
 Указатель на байтовый шаблон, если таковой имеется, являющийся признаком конца программной переменной. Например, строки ANSI и MBCS C обычно имеют 1-байтовый признаки конца строки (\0).  
  
 Если для переменной нет терминатора, задайте для *птерм* значение null.  
  
 Для обозначения символа конца строки в качестве признака конца программной переменной в языке C можно использовать пустую строку (""). Поскольку пустая строка, заканчивающаяся нулем, образует один байт (сам байт признака конца), установите для *кбтерм* значение 1. Например, чтобы указать, что строка в параметре *szName* завершается нулем, и что для обозначения длины следует использовать признак конца:  
  
```
bcp_bind(hdbc, szName, 0,  
   SQL_VARLEN_DATA, "", 1,  
   SQLCHARACTER, 2)  
```

 Незавершенная форма этого примера может означать, что 15 символов будут скопированы из переменной *szName* во второй столбец связанной таблицы:  

```
bcp_bind(hdbc, szName, 0, 15,
   NULL, 0, SQLCHARACTER, 2)  
```

 При необходимости API-интерфейс массового копирования выполнит преобразование символов из Юникода в многобайтовую кодировку (MBCS). Убедитесь, что правильно задана строка байтов признака конца и количество байт в строке. Например, чтобы указать, что строка в параметре *szName* является строкой расширенных символов Юникода, заканчивающейся значением конца Юникода NULL:  
  
```  
bcp_bind(hdbc, szName, 0,
   SQL_VARLEN_DATA, L"",  
   sizeof(WCHAR), SQLNCHAR, 2)  
```  
  
 Если привязанный [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] столбец является расширенным символом, преобразование в [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md)не выполняется. Если столбец [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] имеет символьный тип в кодировке MBCS, при передаче данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняется преобразование расширенных символов в несколько символов.  
  
 *cbTerm*  
 Количество байт в признаке конца программной переменной, если такой имеется. Если для переменной нет терминатора, задайте для *кбтерм* значение 0.  

*eDataType* Тип данных C переменной программы. Данные в программной переменной преобразуются в тип столбца базы данных. Если этот параметр равен 0, преобразование не выполняется.  

Параметр *eDataType* перечисляется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] маркерами типа данных в sqlncli. h, а не в перечислителях типа данных ODBC C. Например, можно задать целое двухбайтовое значение ODBC типа SQL_C_SHORT с помощью типа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQLINT2.  

[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Добавлена поддержка токенов типа данных SQLXML и SQLUDT в параметре **_eDataType_** .  

В приведенной ниже таблице перечислены допустимые перечисляемые типы данных и соответствующие типы данных C в ODBC.

|eDataType|Тип C|
|-----------------------|------------|  
|SQLTEXT|char *|  
|SQLNTEXT|wchar_t *|  
|SQLCHARACTER|char *|  
|SQLBIGCHAR|char *|  
|SQLVARCHAR|char *|  
|SQLBIGVARCHAR|char *|  
|SQLNCHAR|wchar_t *|  
|SQLNVARCHAR|wchar_t *|  
|SQLBINARY|unsigned char *|  
|SQLBIGBINARY|unsigned char *|  
|SQLVARBINARY|unsigned char *|  
|SQLBIGVARBINARY|unsigned char *|  
|SQLBIT|char|  
|SQLBITN|char|  
|SQLINT1|char|  
|SQLINT2|short int|  
|SQLINT4|INT|  
|SQLINT8|_int64|  
|SQLINTN|*cbIndicator*<br /> 1: SQLINT1<br /> 2: SQLINT2<br /> 4: SQLINT4<br /> 8: SQLINT8|  
|SQLFLT4|FLOAT|  
|SQLFLT8|FLOAT|  
|SQLFLTN|*cbIndicator*<br /> 4: SQLFLT4<br /> 8: SQLFLT8|  
|SQLDECIMALN|SQL_NUMERIC_STRUCT|  
|SQLNUMERICN|SQL_NUMERIC_STRUCT|  
|SQLMONEY|DBMONEY|  
|SQLMONEY4|DBMONEY4|  
|SQLMONEYN|*cbIndicator*<br /> 4: SQLMONEY4<br /> 8: SQLMONEY|  
|SQLTIMEN|SQL_SS_TIME2_STRUCT|  
|SQLDATEN|SQL_DATE_STRUCT|  
|SQLDATETIM4|DBDATETIM4|  
|SQLDATETIME|DBDATETIME|  
|SQLDATETIMN|*cbIndicator*<br /> 4: SQLDATETIM4<br /> 8: SQLDATETIME|  
|SQLDATETIME2N|SQL_TIMESTAMP_STRUCT|  
|SQLDATETIMEOFFSETN|SQL_SS_TIMESTAMPOFFSET_STRUCT|  
|SQLIMAGE|unsigned char *|  
|SQLUDT|unsigned char *|  
|SQLUNIQUEID|SQLGUID|  
|SQLVARIANT|*Любой тип данных, кроме следующих:*<br />— text<br />— ntext<br />— image<br />— varchar(max)<br />— varbinary(max)<br />— nvarchar(max)<br />— xml<br />— timestamp|  
|SQLXML|*Поддерживаемые типы данных C:*<br />— char *<br />— wchar_t *<br />— unsigned char *|  

*idxServerCol* Порядковый номер столбца в таблице базы данных, в которую копируются данные. Первый столбец в таблице имеет порядковый номер 1. Порядковый номер столбца возвращается функцией [SQLColumns](../../relational-databases/native-client-odbc-api/sqlcolumns.md).  
  
## <a name="returns"></a>Возвращаемое значение

 SUCCEED или FAIL.

## <a name="remarks"></a>Remarks

Используйте **bcp_bind** для быстрого и эффективного способа копирования данных из программной переменной в таблицу в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

Вызовите [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) перед вызовом этой или любой другой функции небольшого копирования. Вызов **bcp_init** задает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] целевую таблицу для выполнения операции с массовым копированием. При вызове **bcp_init** для использования с **bcp_bind** и [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md)параметр **bcp_init** _сздатафиле_ , указывающий файл данных, устанавливается в значение null. параметр **bcp_init**_eDirection_ имеет значение DB_IN.  

Создайте отдельный **bcp_bind** вызов для каждого столбца в таблице, в которую необходимо выполнить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] копирование. После выполнения необходимых **bcp_bind** вызовов вызовите **bcp_sendrow** , чтобы отправить строку данных из переменных программы в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Повторная привязка столбца не поддерживается.

Когда нужно [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] зафиксировать уже полученные строки, вызовите [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md). Например, вызовите **bcp_batch** один раз для каждых 1000 строк или в любой другой интервал.  

Если строки для вставки не найдены, вызовите [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md). Несоблюдение этого правила приведет к ошибке.

Параметры управления, указанные в параметре [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md), не влияют на передачу **bcp_bind** строк.  

Если *pData* для столбца имеет значение null, так как оно будет предоставлено вызовами [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md), все последующие столбцы с *eDataType* , для которых заданы значения SQLTEXT, SQLNTEXT, SQLXML, SQLUDT, SQLCHARACTER, SQLVARCHAR, SQLVARBINARY, SqlBinary, SQLNCHAR или SQLIMAGE, также должны быть связаны с параметром *pData* , имеющим значение null, и их значения также должны быть предоставлены вызовами **bcp_moretext**.  

Для новых типов больших значений, таких как **varchar (max)**, **varbinary (max)** или **nvarchar (max)**, можно использовать SQLCHARACTER, SQLVARCHAR, SQLVARBINARY, SqlBinary и SQLNCHAR в качестве индикаторов типа в параметре *eDataType* .  

Если *кбтерм* не равен 0, то любое значение (1, 2, 4 или 8) допустимо для префикса (*кбиндикатор*). В этой ситуации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] собственный клиент выполняет поиск терминатора, вычисляет длину данных относительно терминатора (*i*) и устанавливает для *cbData* меньшее значение i и значение prefix.  

Если *кбтерм* имеет значение 0 и *кбиндикатор* (префикс) не равно 0, *кбиндикатор* должен быть равен 8. 8-байтовый префикс может принимать следующие значения:  

- 0xFFFFFFFFFFFFFFFF означает значение NULL для поля.  

- 0xFFFFFFFFFFFFFFFE рассматривается как специальное значение префикса, которое используется для эффективной отправки данных в блоки на сервер. Данные со специальным префиксом имеют следующий формат.  

- <SPECIAL_PREFIX> \<0 or more  DATA CHUNKS> <ZERO_CHUNK>, где:  

- СПЕЦИАЛЬНЫЙ_ПРЕФИКС имеет значение 0xFFFFFFFFFFFFFFFE.  

- DATA_CHUNK — это 4-байтовый префикс, содержащий длину фрагмента, за которым следуют фактические данные, длина которых указана в 4-байтовом префиксе.  

- ZERO_CHUNK является 4-байтовым значением, содержащим все нули (00000000), обозначающее конец данных.  

- Любая другая допустимая 8-байтовая длина обрабатывается как обычная длина данных.  

 Вызов [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md) при использовании **bcp_bind** приводит к ошибке.  
  
## <a name="bcp_bind-support-for-enhanced-date-and-time-features"></a>Поддержка функцией bcp_bind улучшенных возможностей даты и времени

Дополнительные сведения о типах, используемых с параметром *eDataType* для типов даты и времени, см. в разделе " [изменения в ходе операции копирования для расширенных типов даты и времени &#40;OLE DB и ODBC&#41;](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md).  

Дополнительные сведения см. в разделе [улучшения даты и времени &#40;&#41;ODBC ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md).  

## <a name="example"></a>Пример
  
```
#include sql.h  
#include sqlext.h  
#include odbcss.h  
// Variables like henv not specified.  
HDBC      hdbc;  
char         szCompanyName[MAXNAME];  
DBINT      idCompany;  
DBINT      nRowsProcessed;  
DBBOOL      bMoreData;  
char*      pTerm = "\t\t";  
  
// Application initiation, get an ODBC environment handle, allocate the  
// hdbc, and so on.  
...
  
// Enable bulk copy prior to connecting on allocated hdbc.  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP, (SQLPOINTER) SQL_BCP_ON,  
   SQL_IS_INTEGER);  
  
// Connect to the data source; return on error.  
if (!SQL_SUCCEEDED(SQLConnect(hdbc, _T("myDSN"), SQL_NTS,  
   _T("myUser"), SQL_NTS, _T("myPwd"), SQL_NTS)))  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Initialize bcp.
if (bcp_init(hdbc, "comdb..accounts_info", NULL, NULL  
   DB_IN) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Bind program variables to table columns.
if (bcp_bind(hdbc, (LPCBYTE) &idCompany, 0, sizeof(DBINT), NULL, 0,  
   SQLINT4, 1)    == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
if (bcp_bind(hdbc, (LPCBYTE) szCompanyName, 0, SQL_VARLEN_DATA,  
   (LPCBYTE) pTerm, strnlen(pTerm, sizeof(pTerm)), SQLCHARACTER, 2) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
while (TRUE)  
   {  
   // Retrieve and process program data.
   if ((bMoreData = getdata(&idCompany, szCompanyName)) == TRUE)  
      {  
      // Send the data.
      if (bcp_sendrow(hdbc) == FAIL)  
         {  
         // Raise error and return.  
         return;  
         }  
      }  
   else  
      {  
      // Break out of loop.  
      break;  
      }  
   }  
  
// Terminate the bulk copy operation.  
if ((nRowsProcessed = bcp_done(hdbc)) == -1)  
   {  
   printf_s("Bulk-copy unsuccessful.\n");  
   return;  
   }  
  
printf_s("%ld rows copied.\n", nRowsProcessed);  
```  
  
## <a name="see-also"></a>См. также:

 [Bulk Copy Functions](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)

---
title: Использование функции Always Encrypted с драйвером ODBC
description: Узнайте, как разрабатывать приложения ODBC с помощью Always Encrypted и Microsoft ODBC Driver for SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 02e306b8-9dde-4846-8d64-c528e2ffe479
ms.reviewer: v-daenge
ms.author: v-chojas
author: v-chojas
ms.openlocfilehash: 7da6c0d9864f514d757d0872fd32ce1e0219f01d
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464782"
---
# <a name="using-always-encrypted-with-the-odbc-driver-for-sql-server"></a>Использование функции Always Encrypted с драйвером ODBC для SQL Server

[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

### <a name="applicable-to"></a>Применимо к

- ODBC Driver for SQL Server версии 13.1
- ODBC Driver for SQL Server версии 17

### <a name="introduction"></a>Введение

В этой статье содержатся сведения о разработке приложений ODBC с помощью [Always Encrypted (ядро СУБД)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) или [Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/always-encrypted-enclaves.md) и [ODBC Driver for SQL Server](microsoft-odbc-driver-for-sql-server.md).

Функция Always Encrypted позволяет шифровать конфиденциальные данные в клиентских приложениях, не раскрывая данные или ключи шифрования для SQL Server или Базы данных SQL Azure. Драйвер с поддержкой Always Encrypted, такой как драйвер ODBC для SQL Server, реализует эту функцию безопасности за счет прозрачного шифрования и расшифровки конфиденциальных данных в клиентском приложении. Драйвер автоматически определяет, какие параметры запроса соответствуют важным столбцам базы данных (защищенным с помощью Always Encrypted), и шифрует значения этих параметров перед передачей данных в SQL Server или Базу данных SQL Azure. Аналогичным образом драйвер прозрачно расшифровывает данные, полученные из зашифрованных столбцов базы в результатах запроса. Always Encrypted *с безопасными анклавами* обеспечивает расширенные функции защиты конфиденциальных данных.

Дополнительные сведения см. в статьях об использовании [Always Encrypted (ядро СУБД)](../../relational-databases/security/encryption/always-encrypted-database-engine.md) и [Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

### <a name="prerequisites"></a>Предварительные требования

Настройте функцию постоянного шифрования в базе данных. В процесс настройки входят действия по подготовке ключей Always Encrypted и настройке шифрования для выбранных столбцов базы данных. Если в базе данных постоянное шифрование еще не настроено, следуйте инструкциям в разделе [Приступая к работе с постоянным шифрованием](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted). В частности база данных должна содержать определения метаданных для главного ключа столбца (CMK), ключа шифрования столбца (CEK) и таблицы с одним или несколькими столбцами, зашифрованными с помощью этого ключа CEK.

Если вы используете Always Encrypted с безопасными анклавами, см. статью [Разработка приложений с помощью Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/always-encrypted-enclaves-client-development.md) со сведениями о дополнительных предварительных требованиях.

### <a name="enabling-always-encrypted-in-an-odbc-application"></a>Включение Always Encrypted в приложении ODBC

Самым простым способом одновременно включить шифрование параметров и расшифровку столбца с зашифрованным набором результатов будет установка значения **Enabled** для ключевого слова строки подключения `ColumnEncryption`. Ниже приведен пример строки подключения, включающий постоянное шифрование.

```cpp
SQLWCHAR *connString = L"Driver={ODBC Driver 17 for SQL Server};Server={myServer};Trusted_Connection=yes;ColumnEncryption=Enabled;";
```

Always Encrypted также можно включить в конфигурации имени DSN, используя тот же ключ и значение (которое переопределяется параметром строки подключения, если он указан), или указать программно с помощью атрибута предварительного подключения `SQL_COPT_SS_COLUMN_ENCRYPTION`. Такая настройка переопределяет значение, указанное в строке подключения или имени DSN:

```cpp
 SQLSetConnectAttr(hdbc, SQL_COPT_SS_COLUMN_ENCRYPTION, (SQLPOINTER)SQL_COLUMN_ENCRYPTION_ENABLE, 0);
```

Включив Always Encrypted для подключения, вы можете изменять его поведение для отдельных запросов. Дополнительные сведения см. в разделе [Управление влиянием Always Encrypted на производительность](#controlling-the-performance-impact-of-always-encrypted) ниже.

Включенной функции Always Encrypted недостаточно для успешного шифрования или расшифровки. Вам нужно убедиться в следующем:

- Приложение имеет разрешения *VIEW ANY COLUMN MASTER KEY DEFINITION* и *VIEW ANY COLUMN ENCRYPTION KEY DEFINITION* для базы данных, необходимые для доступа к метаданным о ключах постоянного шифрования в базе данных. Дополнительные сведения см. в описании [разрешений базы данных](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).

- Приложение может обращаться к CMK для защиты ключей CEK для запрашиваемых зашифрованных столбцов. Это поведение зависит от поставщика хранилища ключей, который хранит CMK. См. подробнее о [работе с хранилищами главных ключей столбцов](#working-with-column-master-key-stores).

### <a name="enabling-always-encrypted-with-secure-enclaves"></a>Включение Always Encrypted с безопасными анклавами

> [!NOTE]
> В Linux и macOS для использования Always Encrypted с безопасными анклавами требуется OpenSSL версии 1.0.1 или более поздней.

Начиная с версии 17.4 драйвер поддерживает Always Encrypted с безопасными анклавами. Чтобы включить использование анклава при подключении к базе данных, задайте ключ DSN `ColumnEncryption`, ключевое слово строки подключения или атрибут подключения, указав значение `<attestation protocol>\<attestation URL>`, где:

- `<attestation protocol>` — определяет протокол, используемый для аттестации анклава.
  - Если вы используете [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)] и службу защиты узла (HGS), значение `<attestation protocol>` должно быть `VBS-HGS`.
  - Если вы используете [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] и Аттестацию Microsoft Azure, `<attestation protocol>`значение должно быть `SGX-AAS`.

- `<attestation URL>` — определяет URL-адрес аттестации (конечная точка службы аттестации). Необходимо получить URL-адрес аттестации для имеющейся среды у администратора службы аттестации.

  - Если вы используете [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)] и службу защитника узлов (HGS), см. сведения в разделе об [определении и совместном использовании URL-адреса аттестации HGS](../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Если вы используете [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] и Аттестацию Microsoft Azure, см. сведения об [определении URL-адреса аттестации для политики аттестации](../../relational-databases/security/encryption/always-encrypted-enclaves.md#secure-enclave-attestation).

Примеры строк подключения, позволяющие включить вычисления анклавов для подключений к базе данных:

- [!INCLUDE[ssnoversion-md](../../includes/ssnoversion-md.md)]:
  
   ```
   Driver=ODBC Driver 17 for SQL Server;Server=myServer.myDomain;Database=myDataBase;Trusted_Connection=Yes;ColumnEncryption=VBS-HGS,http://myHGSServer.myDomain/Attestation
   ```

- [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]:
  
   ```
   Driver=ODBC Driver 17 for SQL Server;Server=myServer.database.windows.net;Database=myDataBase;Uid=myUsername;Pwd=myPassword;Encrypt=yes;ColumnEncryption=SGX-AAS,https://myAttestationProvider.uks.attest.azure.net/attest/SgxEnclave
   ```

Если сервер и служба аттестации настроены правильно, как и CMK и CEK с поддержкой анклава для нужных столбцов, вы сможете выполнять запросы, использующие анклав, например шифрование на месте и расширенные вычисления, в дополнение к существующим функциональным возможностям, которые предоставляет Always Encrypted. Дополнительные сведения см. в статье [Настройка и использование Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/configure-always-encrypted-enclaves.md).

### <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Получение и изменение данных в зашифрованных столбцах

После включения Always Encrypted для подключения можно использовать стандартные API-интерфейсы ODBC. API-интерфейсы ODBC могут извлекать или изменять данные в зашифрованных столбцах базы данных. Вам могут быть полезны следующие ресурсы документации:

- [Пример кода ODBC](cpp-code-example-app-connect-access-sql-db.md).
- [Справочник по программированию ODBC](../../odbc/reference/odbc-programmer-s-reference.md)

Приложение должно иметь необходимые разрешения базы данных и доступ к главному ключу столбца. Затем драйвер шифрует все параметры запроса, предназначенные для зашифрованных столбцов. Драйвер также расшифровывает данные, полученные из зашифрованных столбцов. Драйвер выполняет все эти шифрования и расшифровки без помощи исходного кода. Для программы это так, как если бы столбцы не были зашифрованы.

Если постоянное шифрование не включено, выполнение запросов с параметрами, предназначенными для зашифрованных столбцов, завершится ошибкой. Данные по-прежнему могут извлекаться из зашифрованных столбцов, пока для них не будут указаны параметры, предназначенные для зашифрованных столбцов. Однако драйвер не будет применять расшифровку, и приложение будет получать двоичные зашифрованные данные (в виде массивов байтов).

В приведенной ниже таблице описывается поведение запросов в зависимости от того, включена функция Always Encrypted или нет.

|Характеристика запроса | Постоянное шифрование включено, и приложение может получать доступ к ключам и метаданным ключей|Постоянное шифрование включено, и приложение не может получать доступ к ключам и метаданным ключей | Постоянное шифрование отключено|
|:---|:---|:---|:---|
| Параметры, предназначенные для зашифрованных столбцов. | Значения параметров прозрачно шифруются. | Error | Error|
| Извлечение данных из зашифрованных столбцов без параметров, предназначенных для зашифрованных столбцов.| Результаты из зашифрованных столбцов прозрачно расшифровываются. Приложение получает значения столбца в виде обычного текста. | Error | Результаты из зашифрованных столбцов не расшифровываются. Приложение получает зашифрованные значения в виде массивов байтов.

В следующих примерах показано получение и изменение данных в зашифрованных столбцах. В этом примере предполагается, что таблица имеет следующую схему. Столбцы SSN и BirthDate зашифрованы.

```tsql
CREATE TABLE [dbo].[Patients](
 [PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY] )
 GO
```

#### <a name="data-insertion-example"></a>Пример вставки данных

В этом примере показана вставка строки в таблицу Patients. Обратите внимание на следующие сведения:

- В образце кода нет ничего, связанного с шифрованием. Драйвер автоматически обнаруживает и шифрует значения параметров SSN и даты, которые предназначены зашифрованных столбцов. Благодаря этому шифрование является прозрачным для приложения.

- Данные, вставленные в столбцы базы данных (в том числе в зашифрованные) передаются в качестве привязанных параметров (см. [Функция SQLBindParameter](../../odbc/reference/syntax/sqlbindparameter-function.md)). Несмотря на то, что при отправке значений в незашифрованные столбцы использовать параметры необязательно (но настоятельно рекомендуется, так как это помогает предотвратить внедрение кода SQL), они требуются для значений, предназначенных для зашифрованных столбцов. Если значения, вставленные в столбцы SSN или BirthDate, были переданы в качестве внедренных в инструкцию запроса литералов, выполнение запроса завершится ошибкой, так как драйвер не пытается шифровать или иным образом обрабатывать литералы в запросах. В результате сервер отклонит их как несовместимые с зашифрованными столбцами.

- Для параметров, вставляемых в столбец SSN, устанавливается тип SQL SQL_CHAR, который сопоставляется с типом данных SQL Server **char** (`rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);`). Если для параметра был задан тип SQL_WCHAR, который сопоставляется с типом данных **nchar**, выполнение запроса завершится ошибкой, так как Always Encrypted не поддерживает преобразования на стороне сервера из зашифрованных значений nchar в зашифрованные значения char. Сведения о сопоставлении типов данных см. в [этом приложении справочника по программированию ODBC о типах данных](../../odbc/reference/appendixes/appendix-d-data-types.md).

```cpp
    SQL_DATE_STRUCT date;
    SQLLEN cbdate;   // size of date structure  

    SQLCHAR SSN[12];
    strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

    SQLWCHAR* firstName = L"Catherine";
    SQLWCHAR* lastName = L"Abel";
    SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

    // Initialize the date structure  
    date.day = 10;
    date.month = 9;
    date.year = 1996;

    // Size of structures   
    cbdate = sizeof(SQL_DATE_STRUCT);

    SQLRETURN rc = 0;

    string queryText = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?) ";

    rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

    //SSN
    rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);
    //FirstName
    rc = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)firstName, 0, &cbFirstName);
    //LastName
    rc = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);
    //BirthDate
    rc = SQLBindParameter(hstmt, 4, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TYPE_DATE, 10, 0, (SQLPOINTER)&date, 0, &cbdate);

    rc = SQLExecute(hstmt);
```

#### <a name="plaintext-data-retrieval-example"></a>Пример получения данных в виде открытого текста

В следующем примере показана фильтрация данных на основе зашифрованных значений и получение данных в виде открытого текста из зашифрованных столбцов. Обратите внимание на следующие сведения:

- Значение, используемое в предложении WHERE для фильтрации по столбцу SSN, необходимо передавать, используя SQLBindParameter, чтобы перед отправкой на сервер драйвер мог его прозрачно зашифровать.

- Все значения, выводимые программой, будут представлены в виде обычного текста, так как драйвер прозрачно расшифрует данные, полученные из столбцов SSN и BirthDate.

> [!NOTE]
> Запросы могут выполнять сравнение на равенство для зашифрованных столбцов, только если используется детерминированное шифрование или включен безопасный анклав. Дополнительные сведения см. в разделе [Выбор детерминированного или случайного шифрования](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

```cpp
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[SSN] = ? ";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//SSN
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 11, 0, (SQLPOINTER)SSN, 0, &cbSSN);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="ciphertext-data-retrieval-example"></a>Пример получения данных в виде зашифрованного текста

Если постоянное шифрование не включено, запрос может получать данные из зашифрованных столбцов, пока для него не будут указаны параметры, предназначенные для зашифрованных столбцов.

В приведенном ниже примере показано извлечение двоичных зашифрованных данных из зашифрованных столбцов. Обратите внимание на следующие сведения:

- Так как постоянное шифрование не включено в строке подключения, запрос будет возвращать зашифрованные значения SSN и BirthD в виде байтовых массивов (программа преобразует значения в строки).
- Запрос, получающий данные из зашифрованных столбцов с отключенным постоянным шифрованием, может иметь параметры при условии, что ни один из параметров не предназначен для зашифрованного столбца. Приведенный выше запрос выполняет фильтрацию по LastName, который не зашифрован в базе данных. Запрос, отфильтрованный по SSN или BirthDate, завершится ошибкой.

```cpp
SQLCHAR SSN[12];
strcpy_s((char*)SSN, _countof(SSN), "795-73-9838");

SQLWCHAR* firstName = L"Catherine";
SQLWCHAR* lastName = L"Abel";
SQLINTEGER cbSSN = SQL_NTS, cbFirstName = SQL_NTS, cbLastName = SQL_NTS;

SQLRETURN rc = 0;
string empty = "";
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] " + empty +
    "FROM  [dbo].[Patients]" +
    "WHERE " +
    "[LastName] = ?";

rc = SQLPrepare(hstmt, (SQLCHAR *)queryText.c_str(), SQL_NTS);

//LastName
rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_WCHAR, SQL_WCHAR, 50, 0, (SQLPOINTER)lastName, 0, &cbLastName);

rc = SQLExecute(hstmt);
HandleDiagnosticRecord(hstmt, SQL_HANDLE_STMT, rc);

SQL_DATE_STRUCT dateVal;
SQLWCHAR firstNameVal[50];
SQLWCHAR lastNameVal[50];
SQLCHAR SSNVal[12];
SQLLEN cbdate;   // size of date structure  

int rowcount = 0;
while (SQL_SUCCEEDED(SQLFetch(hstmt)))
{
    rowcount++;
    SQLGetData(hstmt, 1, SQL_C_CHAR, &SSNVal, 11, &cbSSN);
    SQLGetData(hstmt, 2, SQL_C_WCHAR, &firstNameVal, 50, &cbFirstName);
    SQLGetData(hstmt, 3, SQL_C_WCHAR, &lastNameVal, 50, &cbLastName);
    SQLGetData(hstmt, 4, SQL_C_TYPE_DATE, &dateVal, 10, &cbdate);        
}
```

#### <a name="moneysmallmoney-encryption"></a>Шифрование Money и SmallMoney

Начиная с версии 17.7 драйвера функцию Always Encrypted можно использовать с типами MONEY и SMALLMONEY. Однако необходимо выполнить ряд дополнительных действий.
При вставке данных в зашифрованные столбцы MONEY или SMALLMONEY используйте один из следующих типов C:

```cpp
SQL_C_CHAR
SQL_C_WCHAR
SQL_C_SHORT
SQL_C_LONG
SQL_C_FLOAT
SQL_C_DOUBLE
SQL_C_BIT
SQL_C_TINYINT
SQL_C_SBIGINT
SQL_C_NUMERIC
```

и тип SQL `SQL_NUMERIC` или `SQL_DOUBLE` (при использовании этого типа точность может снижаться).

##### <a name="binding-the-variable"></a>Привязка переменной

При привязке переменной типа MONEY или SMALLMONEY в зашифрованном столбце необходимо задать следующие поля дескриптора:

```cpp
// n is the descriptor record of the MONEY/SMALLMONEY parameter
// the type is assumed to be SMALLMONEY if isSmallMoney is true and MONEY otherwise

SQLHANDLE ipd = 0;
SQLGetStmtAttr(hStmt, SQL_ATTR_IMP_PARAM_DESC, (SQLPOINTER)&ipd, SQL_IS_POINTER, NULL);
SQLSetDescField(ipd, n, SQL_CA_SS_SERVER_TYPE, isSmallMoney ? (SQLPOINTER)SQL_SS_TYPE_SMALLMONEY :
                                                              (SQLPOINTER)SQL_SS_TYPE_MONEY, SQL_IS_INTEGER);

// If the variable is bound as SQL_NUMERIC, additional descriptor fields have to be set
// var is SQL_NUMERIC_STRUCT containing the value to be inserted

SQLHDESC   hdesc = NULL;
SQLGetStmtAttr(hStmt, SQL_ATTR_APP_PARAM_DESC, &hdesc, 0, NULL);
SQLSetDescField(hdesc, n, SQL_DESC_PRECISION, (SQLPOINTER)(var.precision), 0);
SQLSetDescField(hdesc, n, SQL_DESC_SCALE, (SQLPOINTER)(var.scale), 0);
SQLSetDescField(hdesc, n, SQL_DESC_DATA_PTR, &var, 0);
```

#### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Как избежать распространенных проблем при запросе зашифрованных столбцов

В этом разделе описываются общие категории ошибок, возникающих при выполнении запросов к зашифрованным столбцам из приложений ODBC, и приводятся рекомендации о том, как их избежать.

##### <a name="unsupported-data-type-conversion-errors"></a>Ошибки преобразования неподдерживаемых типов данных

Постоянное шифрование поддерживает несколько преобразований для зашифрованных типов данных. Подробный список поддерживаемых преобразований типов см. в статье [Always Encrypted (ядро СУБД)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Чтобы избежать ошибок преобразования типов данных, старайтесь соблюдать следующие моменты при использовании SQLBindParameter с параметрами, предназначенными для зашифрованных столбцов.

- Тип SQL для параметра всегда точно совпадает с типом целевого столбца или может быть преобразован в тип этого столбца.

- Точность и масштаб параметров, предназначенных для столбцов типов данных SQL Server `decimal` и `numeric`, соответствуют точности и масштабу, настроенным для конечного столбца.

- В запросах, которые изменяют конечный столбец, точность параметров, предназначенных для столбцов типов данных SQL Server `datetime2`, `datetimeoffset` или `time`, не превышает точность для конечного столбца.  

##### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Ошибки, возникающие из-за передачи значений в виде открытого текста, а не в зашифрованном виде

Любое значение, предназначенное для зашифрованного столбца, должно быть зашифровано до отправки на сервер. Попытка вставки, изменения или фильтрации по значению в виде открытого текста в зашифрованном столбце приведет к возникновению ошибки. Чтобы избежать таких ошибок, убедитесь в том, что:

- Функция Always Encrypted включена (через имя DSN, в строке подключения, перед подключением через атрибут подключения `SQL_COPT_SS_COLUMN_ENCRYPTION` для конкретного подключения или через атрибут инструкции `SQL_SOPT_SS_COLUMN_ENCRYPTION` для конкретной инструкции).

- для отправки данных, предназначенных для зашифрованных столбцов, используется параметр SQLBindParameter. В примере ниже показан запрос, который вместо передачи литерала как аргумента объекта SQLBindParameter неправильно выполняет фильтрацию по литералу или константе в зашифрованном столбце (SSN).

```cpp
string queryText = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

### <a name="precautions-when-using-sqlsetpos-and-sqlmoreresults"></a>Меры предосторожности при использовании SQLSetPos и SQLMoreResults

#### <a name="sqlsetpos"></a>функция SQLSetPos;

API `SQLSetPos` позволяет приложению обновлять строки в результирующем наборе с использованием буферов, которые были привязаны с помощью SQLBindCol и в которые были получены данные строк. Из-за асимметричного поведения заполнения для зашифрованных типов фиксированной длины существует риск неожиданно изменить данные в этих столбцах при обновлении других столбцов в той же строке. При использовании Always Encrypted символьные значения фиксированной длины будут заполняться в тех случаях, когда значение меньше размера буфера.

Чтобы устранить это поведение, используйте флаг `SQL_COLUMN_IGNORE` для пропуска тех столбцов, которые не должны обновляться при `SQLBulkOperations` и при использовании `SQLSetPos` для обновлений на основе курсора.  Все столбцы, которые не изменяются приложением напрямую, следует игнорировать по соображениям производительности и для того, чтобы избежать усечения столбцов, привязанных к буферу *меньше* их фактического размера (в базе данных). Дополнительные сведения см. в [справочнике по функции SQLSetPos](../../odbc/reference/syntax/sqlsetpos-function.md).

#### <a name="sqlmoreresults--sqldescribecol"></a>SQLMoreResults и SQLDescribeCol

Приложения могут вызывать [SQLDescribeCol](../../odbc/reference/syntax/sqldescribecol-function.md), чтобы возвращать метаданные о столбцах в подготовленных инструкциях.  Если включена функция Always Encrypted, вызов `SQLMoreResults`*перед* вызовом `SQLDescribeCol` приводит к вызову [sp_describe_first_result_set](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md), в результате чего неверно возвращаются метаданные с указанием открытого текста для зашифрованных столбцов. Чтобы избежать этой проблемы, вызывайте `SQLDescribeCol` для подготовленных инструкций *перед* вызовом `SQLMoreResults`.

## <a name="controlling-the-performance-impact-of-always-encrypted"></a>Управление влиянием Always Encrypted на производительность

Так как Always Encrypted является технологией шифрования на стороне клиента, значительное влияние на производительность происходит на стороне клиента, а не в базе данных. Помимо затрат на операции шифрования и расшифровки существуют и другие источники снижения производительности на стороне клиента:

- Дополнительные обращения к базе данных для получения метаданных для параметров запроса.

- Вызовы хранилища главных ключей столбцов для доступа к главному ключу столбца.

В этом разделе описываются процессы оптимизации производительности, встроенные в ODBC Driver for SQL Server, и способы управления влиянием двух указанных выше факторов на производительность.

### <a name="controlling-round-trips-to-retrieve-metadata-for-query-parameters"></a>Управление обращениями для получения метаданных для параметров запроса

Если для соединения включена функция Always Encrypted, по умолчанию драйвер будет вызывать [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) для каждого параметризованного запроса, передавая инструкцию запроса (без значений параметров) в SQL Server. Эта хранимая процедура анализирует инструкцию запроса и выясняет, нужно ли шифровать параметры. Если да, то она возвращает по каждому параметру связанные с шифрованием сведения, позволяющие драйверу шифровать их. Описанное выше поведение гарантирует для клиентского приложения высокий уровень прозрачности. Так как значения, предназначенные для зашифрованных столбцов, передаются драйверу в параметрах, приложению (и разработчику приложения) не нужно знать, какие запросы обращаются к зашифрованным столбцам.

Начиная с версии 17.6 драйвер также кэширует метаданные шифрования для подготовленных инструкций, повышая производительность и позволяя дальнейшим вызовам к `SQLExecute` обходиться без дополнительного обращения для получения метаданных шифрования.

### <a name="per-statement-always-encrypted-behavior"></a>Поведение Always Encrypted для отдельных инструкций

Чтобы управлять тем, как получение метаданных шифрования для параметризованных запросов влияет на производительность, вы можете изменять поведение Always Encrypted для отдельных запросов, если эта функция уже включена для подключения. Это позволяет гарантировать, что `sys.sp_describe_parameter_encryption` вызывается только для тех запросов, в которых точно есть параметры для зашифрованных столбцов. Обратите внимание, что в этом случае снижается прозрачность шифрования: при шифровании дополнительных столбцов в базе данных может потребоваться изменить код приложения в соответствии с изменениями схемы.

Чтобы изменить поведение Always Encrypted для инструкции, вызовите SQLSetStmtAttr и задайте одно из следующих значений для атрибута инструкции `SQL_SOPT_SS_COLUMN_ENCRYPTION`:

|Значение|Описание|
|-|-|
|`SQL_CE_DISABLED` (0)|Функция Always Encrypted отключена для инструкции|
|`SQL_CE_RESULTSETONLY` (1)|Только расшифровка. Расшифровываются результирующие наборы и возвращаемые значения, а параметры не шифруются.|
|`SQL_CE_ENABLED` (3) | Функция Always Encrypted включена и используется как для параметров, так и для результатов|

Новые дескрипторы инструкций, которые создаются для подключения с включенной функцией Always Encrypted, по умолчанию получают значение SQL_CE_ENABLED. Дескрипторы, создаваемые для подключений, где эта функция отключена, по умолчанию получают значение SQL_CE_DISABLED (и вы не сможете включить для них Always Encrypted).

Если большинство запросов клиентского приложения обращаются к зашифрованным столбцам, мы рекомендуем выполнить следующее.

- Присвойте ключевому слову `ColumnEncryption` строки подключения значение `Enabled`.

- Задайте атрибуту `SQL_SOPT_SS_COLUMN_ENCRYPTION` значение `SQL_CE_DISABLED` для инструкций, которые не обращаются к зашифрованным столбцам. Этот параметр отключает возможность вызова `sys.sp_describe_parameter_encryption` и расшифровки значений в результирующем наборе.

- Задайте атрибуту `SQL_SOPT_SS_COLUMN_ENCRYPTION` значение `SQL_CE_RESULTSETONLY` для запросов, которые не имеют параметров, требующих шифрования, но получают данные из зашифрованных столбцов. Этот параметр отключает вызов `sys.sp_describe_parameter_encryption` и шифрование параметров. Результаты, содержащие зашифрованные столбцы, будут по-прежнему расшифровываться.

- Используйте подготовленные инструкции для запросов, которые будут выполняться многократно; подготовьте запрос с помощью `SQLPrepare` и сохраните дескриптор инструкции, повторно используя его с `SQLExecute` при каждом выполнении. Этот метод является рекомендуемым для поддержания производительности даже при отсутствии зашифрованных столбцов и позволяет драйверу использовать преимущества кэширования метаданных.

## <a name="always-encrypted-security-settings"></a>Параметры безопасности Always Encrypted

### <a name="force-column-encryption"></a>Принудительное шифрование столбцов

Чтобы применить шифрование параметра, задайте значение для поля `SQL_CA_SS_FORCE_ENCRYPT` (дескриптор параметра реализации), вызвав функцию SQLSetDescField. Если это значение отличается от нуля, драйвер будет возвращать ошибку при отсутствии в ответе метаданных шифрования для соответствующего параметра.

```cpp
SQLHDESC ipd;
SQLGetStmtAttr(hStmt, SQL_ATTR_IMP_PARAM_DESC, &ipd, 0, 0);
SQLSetDescField(ipd, paramNum, SQL_CA_SS_FORCE_ENCRYPT, (SQLPOINTER)TRUE, SQL_IS_SMALLINT);   
```

Если SQL Server сообщает драйверу, что параметр не должен быть зашифрован, запросы, использующие этот параметр, завершатся ошибкой. Это поведение обеспечивает дополнительную защиту от атак на систему безопасности, включающих предоставление клиенту скомпрометированным SQL Server неверных метаданных шифрования, что может привести к раскрытию данных.

### <a name="column-encryption-key-caching"></a>Кэширование ключа шифрования столбца

Чтобы уменьшить количество вызовов к хранилищу главных ключей столбцов для расшифровки ключей шифрования столбцов (CEK), драйвер кэширует ключи CEK в памяти в открытом тексте. Кэш CEK является глобальным для драйвера, а не связывается с отдельным подключением. Получив ECEK из метаданных базы данных, драйвер сначала пытается найти в кэше CEK в открытом тексте, который соответствует зашифрованному значению ключа. Драйвер обращается к хранилищу ключей, где хранится главный ключ столбца, только в том случае, если ему не удается найти в кэше значение CEK в открытом тексте.

> [!NOTE]
> Драйвер ODBC Driver for SQL Server вытесняет записи из этого кэша через два часа. Это означает, что для каждого ECEK драйвер обращается в хранилище ключей только один раз за цикл жизни приложения либо каждые два часа в зависимости от того, что меньше.

Начиная с драйвера ODBC Driver for SQL Server версии 17.1, время ожидания кэша для CEK можно изменить с помощью атрибута подключения `SQL_COPT_SS_CEKCACHETTL`, который указывает количество секунд хранения CEK в кэше. В силу глобальной природы кэша этот атрибут можно изменять из любого дескриптора подключения, который допустим для драйвера. При уменьшении значения срока жизни кэша из хранилища вытесняются все значения CEK, срок хранения которых не соответствует новому условию. Если здесь задано значение 0, ключи CEK не кэшируются.

### <a name="trusted-key-paths"></a>Доверенные пути ключей

Начиная с драйвера ODBC Driver for SQL Server версии 17.1, применяется атрибут подключения `SQL_COPT_SS_TRUSTEDCMKPATHS`, который позволяет приложению разрешить для операций Always Encrypted только конкретный список ключей CMK, который определяется по путям ключей. По умолчанию этот атрибут имеет значение NULL, и при этом драйвер принимает любой путь ключа. Чтобы использовать эту возможность, присвойте `SQL_COPT_SS_TRUSTEDCMKPATHS` нуль-терминированное строковое значение с расширенными символами, в котором перечислены разделенные нулевым значением допустимые пути ключей. Участки памяти, на которые ссылается этот атрибут, должны оставаться допустимыми на всем протяжении операций шифрования и расшифровки для того дескриптора подключения, в котором он задан. Драйвер будет постоянно проверять путь CMK, указанный в метаданных сервера, на наличие соответствующего значения в этом списке (без учета регистра). Если путь CMK отсутствует в этом списке, операция завершается ошибкой. Чтобы изменить список доверенных ключей CMK, приложение может изменять содержимое памяти, на которую указывает этот атрибут, не изменяя значение самого атрибута.

## <a name="working-with-column-master-key-stores"></a>Работа с хранилищами главных ключей столбцов

Для шифрования или расшифровки данных драйвер должен получить ключ CEK, который настроен для целевого столбца. Ключи CEK хранятся в зашифрованном виде (ECEK) в метаданных базы данных. Каждому ключу CEK соответствует определенный ключ CMK, который использовался для его шифрования. [Метаданные базы данных](../../t-sql/statements/create-column-master-key-transact-sql.md) не хранят значения CMK, а только имя хранилища ключей и информацию, позволяющую найти нужные CMK в этом хранилище ключей.

Чтобы получить значение ECEK в открытом тексте, драйвер сначала получает метаданные о ключах CEK и CMK, а затем на основе этих сведений обращается в хранилище ключей, где содержится CMK, и запрашивает расшифрованную версию ECEK. Драйвер взаимодействует с хранилищем ключей через поставщик хранилища ключей.

### <a name="built-in-keystore-providers"></a>Встроенные поставщики хранилища ключей

Драйвер ODBC Driver for SQL Server поставляется со следующими встроенными поставщиками хранилища ключей.

| Имя | Описание | Имя (метаданные) поставщика |Доступность|
|:---|:---|:---|:---|
|Azure Key Vault |Хранит CMK в Azure Key Vault | `AZURE_KEY_VAULT` |Windows, macOS, Linux|
|Хранилище сертификатов Windows|Хранит CMK локально в хранилище ключей Windows| `MSSQL_CERTIFICATE_STORE`|Windows|

- Вы (или ваш администратор баз данных) должны проверить правильность имени поставщика, настроенного в метаданных главного ключа столбца, и убедиться в том, что путь к ключу главного ключа столбца соответствует формату пути к ключу для данного поставщика. Для настройки ключей рекомендуется использовать средство, такое как среда SQL Server Management Studio, которое при выполнении инструкции [CREATE COLUMN MASTER KEY (Transact-SQL)](../../t-sql/statements/create-column-master-key-transact-sql.md) автоматически создает допустимые имена поставщиков и пути к ключам.

- Убедитесь, что приложение может получить доступ к ключу в хранилище ключей. Для этого может потребоваться предоставить приложению доступ к ключу или хранилищу ключей (в зависимости от хранилища ключей) либо выполнить другие действия по настройке конкретного хранилища ключей. Например, для доступа к Azure Key Vault необходимо предоставить правильные учетные данные для хранилища ключей.

### <a name="using-the-azure-key-vault-provider"></a>Использование поставщика Azure Key Vault

Хранилище ключей Azure удобно использовать, чтобы хранить главные ключи столбцов для постоянного шифрования, особенно если приложения размещены в Azure. Драйвер ODBC Driver for SQL Server в Linux, macOS и Windows содержит встроенный поставщик хранилища ключей Azure Key Vault для хранения главного ключа. Дополнительные сведения о настройке Azure Key Vault для Always Encrypted вы найдете в [этом пошаговом руководстве](/archive/blogs/kv/azure-key-vault-step-by-step), в статье [Об Azure Key Vault](/azure/key-vault/general/overview) и разделе [Создание главных ключей столбцов в хранилище ключей Azure](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md#creating-column-master-keys-in-azure-key-vault).

> [!NOTE]
> Драйвер ODBC поддерживает только проверку подлинности AKV непосредственно в Azure Active Directory. Если вы используете проверку подлинности Azure Active Directory для AKV и для конфигурации Active Directory требуется проверка подлинности в конечной точке служб федерации Active Directory, проверка может завершиться ошибкой.
> В Linux и macOS, если используется драйвер версии 17.2 или более поздней, для использования этого поставщика требуется библиотека `libcurl`, но она не считается явной зависимостью, так как не нужна для других операций с драйвером. Если возникает связанная с пакетом `libcurl` ошибка, убедитесь в том, что он установлен.

Драйвер поддерживает проверку подлинности в Azure Key Vault со следующими типами учетных данных.

- Имя пользователя и пароль — этот метод использует в качестве учетных данных имя пользователя Azure Active Directory и его пароль.

- Идентификатор и секрет клиента — этот метод использует в качестве учетных данных идентификатор клиента приложения и секрет приложения.

- Управляемое удостоверение (17.5.2 и выше) — системное или назначенное пользователем; дополнительные сведения см. в статье [Управляемые удостоверения для ресурсов Azure](/azure/active-directory/managed-identities-azure-resources/).

- Интерактивная с помощью Azure Key Vault (драйверы Windows версии 17.7 и более поздних) — при использовании этого метода учетные данные проверяются в Azure Active Directory по имени входа.

Чтобы разрешить драйверу использовать для шифрования столбцов ключи CMK, хранящиеся в Azure Key Vault, укажите в строке подключения следующие ключевые слова:

|Тип учетных данных|<code>KeyStoreAuthentication</code>|<code>KeyStorePrincipalId</code>|<code>KeyStoreSecret</code>|
|-|-|-|-|
|Имя пользователя и пароль| `KeyVaultPassword`|Имя участника-пользователя|Пароль|
|Идентификатор и секрет клиента| `KeyVaultClientSecret`|Идентификатор клиента|Секрет|
|Управляемое удостоверение|`KeyVaultManagedIdentity`|Идентификатор объекта (необязательно, только для назначенного пользователем)|(не указано)|
|Интерактивная AKV|`KeyVaultInteractive`|(не задано)|(не задано)|

#### <a name="example-connection-strings"></a>Примеры строк подключения

Следующие строки подключения демонстрируют проверку подлинности в Azure Key Vault с двумя разными типами учетных данных.

**Идентификатор и секрет клиента**:

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<clientId>;KeyStoreSecret=<secret>
```

**Имя пользователя и пароль**:

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultPassword;KeyStorePrincipalId=<username>;KeyStoreSecret=<password>
```

**Управляемое удостоверение (назначенное системой)**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultManagedIdentity
```

**Управляемое удостоверение (назначенное пользователем)**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultManagedIdentity;KeyStorePrincipalId=<objectID>
```

**Интерактивная AKV**

```
DRIVER=ODBC Driver 17 for SQL Server;SERVER=myServer;Trusted_Connection=Yes;DATABASE=myDB;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultInteractive;UID=<userID>;PWD=<password>
```

Чтобы использовать Azure Key Vault в качестве хранилища для CMK не нужно вносить других изменений в приложение ODBC.

> [!NOTE]
> Драйвер содержит список конечных точек AKV, которым он доверяет. Начиная с версии 17.5.2 драйвера, этот список можно настроить: задайте для свойства `AKVTrustedEndpoints` в разделе реестра ODBCINST.INI или ODBC.INI драйвера или имени DSN (Windows) или в разделе файла `odbcinst.ini` или `odbc.ini` (Linux/macOS) список, разделенный точкой с запятой. Задание списка в имени DSN имеет приоритет над заданием списка в драйвере. Если значение начинается с точки с запятой, оно расширяет список по умолчанию; в противном случае оно заменяет список по умолчанию. Список по умолчанию (начиная с версии 17.5) — `vault.azure.net;vault.azure.cn;vault.usgovcloudapi.net;vault.microsoftazure.de`. Начиная с версии 17.7 в список также включается `managedhsm.azure.net;managedhsm.azure.cn;managedhsm.usgovcloudapi.net;managedhsm.microsoftazure.de`.

### <a name="using-the-windows-certificate-store-provider"></a>Использование поставщика хранилища сертификатов Windows

Драйвер ODBC Driver for SQL Server в Windows содержит встроенный поставщик `MSSQL_CERTIFICATE_STORE` хранилища сертификатов Windows для хранения главного ключа. (Этот поставщик недоступен для macOS и Linux.) При использовании этого поставщика ключи CMK хранятся локально на клиентском компьютере, и для его использования с драйвером не нужно вносить никаких дополнительных изменений в конфигурацию приложения. Но такому приложению необходимы права доступа к сертификату и закрытому ключу, размещенным в хранилище. Дополнительные сведения см. в разделе [Create and Store Column Master Keys (Always Encrypted)](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)(Создание и хранение главных ключей столбцов (постоянное шифрование)).

### <a name="using-custom-keystore-providers"></a>Использование настраиваемых поставщиков хранилища ключей

Драйвер ODBC Driver for SQL Server также поддерживает пользовательские сторонние поставщики хранилища ключей через интерфейс CEKeystoreProvider. Это позволяет приложению загружать, запрашивать и настраивать поставщики хранилища ключей, чтобы драйвер мог использовать их для доступа к зашифрованным столбцам. Приложения могут напрямую взаимодействовать с поставщиком хранилища ключей, чтобы шифровать ключи CEK для хранения в SQL Server и выполнять другие задачи ODBC помимо доступа к зашифрованным столбцам. Дополнительные сведения см. в [статье о пользовательских поставщиках хранилища ключей](custom-keystore-providers.md).

Для взаимодействия с пользовательскими поставщиками хранилища ключей используются два атрибута подключения. К ним относятся:

- `SQL_COPT_SS_CEKEYSTOREPROVIDER`

- `SQL_COPT_SS_CEKEYSTOREDATA`

Первый из них позволяет загружать и перечислять загруженные поставщики хранилища ключей, а второй нужен для взаимодействия между поставщиком и приложением. Эти атрибуты подключения можно применять в любое время, как до так и после установки подключения, так как взаимодействие между поставщиком и приложением не требует обмена данными с SQL Server. Но перед подключением, когда драйвер еще не загружен, установка и получение этих атрибутов будет обрабатываться диспетчером драйверов и может приводить к неожиданным результатам.

#### <a name="loading-a-keystore-provider"></a>Загрузка поставщика хранилища ключей

Настройка атрибута подключения `SQL_COPT_SS_CEKEYSTOREPROVIDER` позволяет клиентскому приложению загрузить библиотеку поставщика, что делает доступными для использования содержащихся в этой библиотеке поставщиков хранилища ключей.

```cpp
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```

| Аргумент | Описание |
|:---|:---|
|`ConnectionHandle`|[Input] Дескриптор подключения Это должен быть допустимый дескриптор подключения, но загруженные через любой дескриптор подключения поставщики становятся доступны из любого дескриптора в том же процессе.|
|`Attribute`|[Input] Атрибут для задания константы `SQL_COPT_SS_CEKEYSTOREPROVIDER`.|
|`ValuePtr`|[Input] Указатель на нуль-терминированную строку, которая определяет имя файла библиотеки поставщика. Для SQLSetConnectAttrA этим значением является строка ANSI (multibyte). Для SQLSetConnectAttrW этим значением является строка Юникода (wchar_t).|
|`StringLength`|[Input] Длина строки ValuePtr или SQL_NTS.|

Драйвер пытается загрузить библиотеку, указанную параметром ValuePtr, через определяемый платформой механизм загрузки динамических библиотек (`dlopen()` в Linux и macOS, `LoadLibrary()` в Windows) и добавляет все определенные в ней поставщики в список известных поставщиков для драйвера. Могут возникать следующие ошибки.

| Error | Описание |
|:--|:--|
|`CE203`|Не удалось загрузить динамическую библиотеку.|
|`CE203`|В библиотеке не удалось найти экспортированный символ CEKeyStoreProvider.|
|`CE203`|Один или несколько поставщиков из этой библиотеки уже загружены.|

`SQLSetConnectAttr` возвращает обычные значения ошибки или успешного завершения, а дополнительные сведения по любым возникающим ошибкам можно получить через стандартный механизм диагностики ODBC.

> [!NOTE]
> Создателю приложения следует следить за тем, чтобы все пользовательские поставщики загружались до отправки использующих их запросов через любое подключение. Несоблюдение этого правила приведет к ошибке.

| Error | Описание |
|:--|:--|
|`CE200`|Поставщик хранилища ключей %1 не найден. Убедитесь, что загружена соответствующая библиотека поставщика хранилища ключей.|

> [!NOTE]
> Средства реализации поставщика хранилища ключей не должны использовать строку `MSSQL` в именах пользовательских поставщиков. Этот термин зарезервирован исключительно для корпорации Майкрософт, и его применение может привести к конфликту с будущими встроенными поставщиками. Наличие этого термина в имени пользовательского поставщика может создавать предупреждение ODBC.

#### <a name="getting-the-list-of-loaded-providers"></a>Получение списка загруженных поставщиков

Получение этого атрибута подключения позволяет клиентскому приложению определить, какие поставщики хранилища ключей в настоящее время загружены в драйвер (включая встроенные поставщики). Это можно сделать только после подключения.

```cpp
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```

| Аргумент | Описание |
|:---|:---|
|`ConnectionHandle`|[Input] Дескриптор подключения Это должен быть допустимый дескриптор подключения, но загруженные через любой дескриптор подключения поставщики становятся доступны из любого дескриптора в том же процессе.|
|`Attribute`|[Input] Атрибут для получения константы `SQL_COPT_SS_CEKEYSTOREPROVIDER`.|
|`ValuePtr`|[Output] Указатель на участок памяти, в котором будет возвращено имя следующего загруженного поставщика.|
|`BufferLength`|[Input] Длина буфера ValuePtr.|
|`StringLengthPtr`|[Output] Указатель на буфер, в котором будет возвращено общее число байтов (за исключением нулевого символа завершения строки), доступных для получения из \*ValuePtr. Если ValuePtr является пустым указателем, длина не возвращается. Если значением атрибута является строка символов, в которой количество доступных байтов превышает разность между значением BufferLength и длиной нуль-терминированной строки, драйвер усекает данные в \*ValuePtr до значения этой разности и завершает полученную строку нулевым символом.|

Каждая операция Get возвращает имя текущего поставщика и увеличивает значение внутреннего счетчика до следующего поставщика, чтобы обеспечить возможность получить весь список. Когда этот счетчик достигнет конца списка, возвращается пустая строка ("") и счетчик сбрасывается. Следующая операция Get снова начинает процесс с начала списка.

### <a name="communicating-with-keystore-providers"></a>Взаимодействие с поставщиками хранилища ключей

Атрибут подключения `SQL_COPT_SS_CEKEYSTOREDATA` позволяет клиентскому приложению взаимодействовать с загруженными поставщиками хранилища ключей, например для настройки дополнительных параметров или передачи материала ключей и т. д. Взаимодействие между клиентским приложением и поставщиком выполняется по простому протоколу формата "запрос и ответ" через запросы Get и Set с помощью этого атрибута соединения. Взаимодействие всегда инициируется клиентским приложением.

> [!NOTE]
> В силу природы вызовов ODBC и реакции CEKeyStoreProvider на них (SQLGet/SetConnectAttr) интерфейс ODBC поддерживает задание данных с точностью до контекста подключения.

Приложение взаимодействует с поставщиками хранилища ключей через драйвер с использованием структуры CEKeystoreData:

```cpp
typedef struct CEKeystoreData {
wchar_t *name;
unsigned int dataSize;
char data[];
} CEKEYSTOREDATA;
```

| Аргумент | Описание |
|:---|:---|
|`name`|[Input] В запросе Set содержит имя поставщика, которому отправляются данные. Не учитывается в запросе Get. Нуль-терминированная строка расширенных символов.|
|`dataSize`|[Input] Размер массива данных в соответствии со структурой.|
|`data`|[InOut] В запросе Set содержит данные для отправки поставщику. Это могут быть произвольные данные, драйвер не пытается их обрабатывать. В запросе Get указывает на буфер, в который будут помещаться полученные из поставщика данные.|

#### <a name="writing-data-to-a-provider"></a>Запись данных в поставщик

Вызов `SQLSetConnectAttr` с атрибутом `SQL_COPT_SS_CEKEYSTOREDATA` записывает "пакет" данных в указанный поставщик хранилища ключей.

```cpp
SQLRETURN SQLSetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER StringLength);
```

| Аргумент | Описание |
|:---|:---|
|`ConnectionHandle`| [Input] Дескриптор подключения Это должен быть допустимый дескриптор подключения, но загруженные через любой дескриптор подключения поставщики становятся доступны из любого дескриптора в том же процессе.|
|`Attribute`|[Input] Атрибут для задания константы `SQL_COPT_SS_CEKEYSTOREDATA`.|
|`ValuePtr`|[Input] Указатель на структуру CEKeystoreData. Поле имени структуры идентифицирует поставщик, для которого предназначены данные.|
|`StringLength`|[Input] Константа SQL_IS_POINTER|

Подробные сведения об ошибке можно получить с помощью [SQLGetDiacRec](../../odbc/reference/syntax/sqlgetdescrec-function.md).

> [!NOTE]
> Поставщик может использовать дескриптор подключения для связи записываемых данных с конкретным подключением, если потребуется. Эта возможность полезна для реализации раздельных конфигураций для подключений. Также он может игнорировать контекст подключения и всегда использовать данные одинаково, независимо от подключения, через которые они отправлены. Дополнительные сведения см. в разделе о [контексте связи](custom-keystore-providers.md#context-association).

#### <a name="reading-data-from-a-provider"></a>Считывание данных из поставщика

Вызов `SQLGetConnectAttr` с атрибутом `SQL_COPT_SS_CEKEYSTOREDATA` считывает "пакет" данных из поставщика, *в который осуществлялась последняя операция записи*. Если операций записи еще не было, возникает ошибка последовательности функций. При реализации поставщиков хранилища ключей мы рекомендуем реализовать поддержку "фиктивных записей" длиной 0 байт, которые позволят выбирать поставщик для операций чтения без побочных эффектов, если такой сценарий имеет смысл.

```cpp
SQLRETURN SQLGetConnectAttr( SQLHDBC ConnectionHandle, SQLINTEGER Attribute, SQLPOINTER ValuePtr, SQLINTEGER BufferLength, SQLINTEGER * StringLengthPtr);
```

| Аргумент | Описание |
|:---|:---|
|`ConnectionHandle`|[Input] Дескриптор подключения Это должен быть допустимый дескриптор подключения, но загруженные через любой дескриптор подключения поставщики становятся доступны из любого дескриптора в том же процессе.|
|`Attribute`|[Input] Атрибут для получения константы `SQL_COPT_SS_CEKEYSTOREDATA`.|
|`ValuePtr`|[Output] Указатель на структуру CEKeystoreData, в которой размещаются считанные из поставщика данные.|
|`BufferLength`|[Input] Константа SQL_IS_POINTER|
|`StringLengthPtr`|[Output] Указатель на буфер, в котором возвращается значение BufferLength. Если *ValuePtr является пустым указателем, длина не возвращается.|

Вызывающий объект должен выделить буфер достаточной длины для структуры CEKEYSTOREDATA, чтобы записать данные из поставщика. При завершении операции поле dataSize заполняется фактической длиной данных, считанных из поставщика. Подробные сведения об ошибке можно получить с помощью [SQLGetDiacRec](../../odbc/reference/syntax/sqlgetdescrec-function.md).

Этот интерфейс не предъявляет дополнительных требований к формату передачи данных между приложением и поставщиком хранилища ключей. Каждый поставщик может определить свой формат данных и протоколов в зависимости от конкретных потребностей.

Пример реализации поставщика хранилища ключей можно изучить в статье [Custom Keystore Providers](custom-keystore-providers.md) (Пользовательские поставщики хранилища ключей).

## <a name="limitations-of-the-odbc-driver-when-using-always-encrypted"></a>Ограничения драйвера ODBC при использовании Always Encrypted

### <a name="asynchronous-operations"></a>Асинхронные операции

Драйвер ODBC разрешает использование [асинхронных операций](../../relational-databases/native-client/odbc/creating-a-driver-application-asynchronous-mode-and-sqlcancel.md) совместно с Always Encrypted, но такой режим использования негативно влияет на производительность операций. Вызов `sys.sp_describe_parameter_encryption` для определения метаданных шифрования для инструкции является блокирующим, то есть драйвер будет ждать получения метаданных от сервера, прежде чем возвращать `SQL_STILL_EXECUTING`.

### <a name="retrieve-data-in-parts-with-sqlgetdata"></a>Получение данных частями с помощью SQLGetData

В драйвере ODBC Driver for SQL Server версий старше 17 не было возможности получать по частям зашифрованные столбцы с символьными или двоичными данными через SQLGetData. Допускался только один вызов SQLGetData, для которого требовался буфер достаточной длины для размещения полных данных из столбца.

### <a name="send-data-in-parts-with-sqlputdata"></a>Отправка данных частями с помощью SQLGetData

В драйвере ODBC Driver for SQL Server версий старше 17.3 не было возможности отправлять частями данные для вставки или сравнения через SQLPutData. Допускался только один вызов SQLPutData с буфером, который содержит полные данные. Для вставки в зашифрованные столбцы длинных данных используйте интерфейс API массового копирования, который описан в следующем разделе, передавая ему файл входных данных.

### <a name="encrypted-money-and-smallmoney"></a>Зашифрованные данные money и smallmoney

Шифрование столбцов типа **money** или **smallmoney** нельзя указать с помощью параметров, так как с этими типами не сопоставляется ни один тип данных ODBC. Подобная попытка приведет к ошибкам конфликта типов операндов.

## <a name="bulk-copy-of-encrypted-columns"></a>Массовое копирование зашифрованных столбцов

Начиная с драйвера Microsoft ODBC Driver for SQL Server версии 17 для данных Always Encrypted поддерживаются [функции массового копирования SQL](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md) и служебная программа **bcp**. API-интерфейсы массового копирования (bcp_&#42;) и служебная программа **bcp** поддерживают вставку и извлечение данных в форматах открытого текста (с шифрованием при вставке и расшифровкой при извлечении) и зашифрованного текста (без дополнительной обработки).

- Чтобы получить зашифрованный текст в формате varbinary(max) (например, для массовой загрузки в другую базу данных), создайте подключение без параметра `ColumnEncryption` (или присвойте ему значение `Disabled`) и выполните операцию BCP OUT.

- Чтобы вставка и извлечение выполнялись в открытом тексте, который драйвер будет прозрачно шифровать и расшифровывать, достаточно задать параметру `ColumnEncryption` значение `Enabled`. Все остальные функции API BCP сохраняются неизменными.

- Чтобы вставка и извлечение зашифрованного текста выполнялись в формате varbinary(max) (как в примере выше), присвойте параметру `BCPMODIFYENCRYPTED` значение TRUE и выполните операцию BCP IN. Чтобы результирующие данные можно было расшифровать, ключ CEK для целевого столбца должен совпадать с тем ключом CEK, который изначально применялся для получения зашифрованного текста.

Если вы используете служебную программу **bcp**, для указания параметра `ColumnEncryption` следует указать аргумент — D и имя источника данных, содержащего нужное значение. Чтобы вставить зашифрованные данные, включите для пользователя параметр `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS`.

Следующая таблица содержит краткое описание действий, доступных при работе с зашифрованным столбцом.

|<code>ColumnEncryption</code>|Направление массового копирования|Описание|
|----------------|-------------|-----------|
|`Disabled`|Исходящее (к клиенту)|Получение зашифрованного текста. Данные предоставляются в формате **varbinary(max)** .|
|`Enabled`|Исходящее (к клиенту)|Получение открытого текста. Драйвер расшифрует данные столбца.|
|`Disabled`|Входящее (к серверу)|Вставка зашифрованного текста Этот параметр предназначен для непрозрачного перемещения зашифрованных данных, которые не нужно расшифровывать. Операция завершится ошибкой, если для пользователя не установлен параметр `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` или для дескриптора соединения не указан атрибут BCPMODIFYENCRYPTED. Дополнительные сведения приведены ниже.|
|`Enabled`|Входящее (к серверу)|Вставка обычного текста. Драйвер зашифрует данные столбца.|

### <a name="the-bcpmodifyencrypted-option"></a>Параметр BCPMODIFYENCRYPTED

Чтобы предотвратить повреждение данных, сервер обычно не допускает прямую вставку зашифрованных данных в зашифрованный столбец, то есть такая попытка завершится ошибкой. Но для массовой загрузки зашифрованных данных с помощью API BCP вы можете установить для параметра `BCPMODIFYENCRYPTED` [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md) значение TRUE, которое разрешает прямую вставку зашифрованного текста и снижает риск повреждения зашифрованных данных по сравнению с применением параметра `ALLOW_ENCRYPTED_VALUE_MODIFICATIONS` для учетной записи пользователя. Но при этом ключи должны всегда соответствовать данным. Мы рекомендуем выполнить несколько проверок чтением после массовой вставки данных и перед их дальнейшим использованием.

Дополнительные сведения см. в разделе [Перенос конфиденциальных данных с помощью функции Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).

## <a name="always-encrypted-api-summary"></a>Сводные данные по API Always Encrypted

### <a name="connection-string-keywords"></a>Ключевые слова в строке подключения

|Имя|Описание|  
|----------|-----------------|  
|`ColumnEncryption`|Допустимые значения: `Enabled`/`Disabled`.<br>`Enabled` — включает функцию Always Encrypted для подключения.<br>`Disabled` — отключает для подключения функцию Always Encrypted.<br>*attestation protocol*, *attestation URL* (версия 17.4 и выше) — включает Always Encrypted с безопасным анклавом, используя определенные протокол и URL-адрес аттестации. <br><br>Значение по умолчанию — `Disabled`.|
|`KeyStoreAuthentication` | Допустимые значения: `KeyVaultPassword`, `KeyVaultClientSecret`, `KeyVaultInteractive` |
|`KeyStorePrincipalId` | Если `KeyStoreAuthentication` = `KeyVaultPassword`, укажите здесь допустимое имя участника-пользователя (UPN) Azure Active Directory. <br>Если `KeyStoreAuthetication` = `KeyVaultClientSecret`, укажите здесь допустимый идентификатор клиента приложения Azure Active Directory. |
|`KeyStoreSecret` | Если `KeyStoreAuthentication` = `KeyVaultPassword`, укажите здесь пароль для соответствующего имени пользователя. <br>Если `KeyStoreAuthentication` = `KeyVaultClientSecret`, укажите здесь секрет приложения, связанный с допустимым идентификатором клиента приложения Azure Active Directory. |

### <a name="connection-attributes"></a>Атрибуты соединения

|Имя|Тип|Описание|  
|----------|-------|----------|  
|`SQL_COPT_SS_COLUMN_ENCRYPTION`|Перед подключением|`SQL_COLUMN_ENCRYPTION_DISABLE` (0) — функция Always Encrypted отключена <br>`SQL_COLUMN_ENCRYPTION_ENABLE` (1) — функция Always Encrypted включена<br> Указатель на строку *attestation protocol*,*attestation URL* (версия 17.4 и выше); включение с безопасным анклавом.|
|`SQL_COPT_SS_CEKEYSTOREPROVIDER`|После подключения|[Set] Попытка загрузки CEKeystoreProvider<br>[Get] Возвращение имени CEKeystoreProvider|
|`SQL_COPT_SS_CEKEYSTOREDATA`|После подключения|[Set] Запись данных в CEKeystoreProvider<br>[Get] Чтение данных из CEKeystoreProvider|
|`SQL_COPT_SS_CEKCACHETTL`|После подключения|[Set] Указание срока жизни для ключей CEK в кэше<br>[Set] Получение текущего значения срока жизни для ключей CEK в кэше|
|`SQL_COPT_SS_TRUSTEDCMKPATHS`|После подключения|[Set] Сохранение указателя на доверенные пути CMK<br>[Get] Получение текущего указателя на доверенные пути CMK|

### <a name="statement-attributes"></a>Атрибуты инструкции

|Имя|Описание|  
|----------|-----------------|  
|`SQL_SOPT_SS_COLUMN_ENCRYPTION`|`SQL_CE_DISABLED` (0) — функция Always Encrypted отключена для инструкции. <br>`SQL_CE_RESULTSETONLY` (1) — только расшифровка. Расшифровываются результирующие наборы и возвращаемые значения, а параметры не шифруются. <br>`SQL_CE_ENABLED` (3) — функция Always Encrypted включена и используется как для параметров, так и для результатов.|

### <a name="descriptor-fields"></a>Поля дескриптора

|Поле дескриптора параметра реализации (IPD)|Размер и тип|Значение по умолчанию|Описание|
|-|-|-|-|  
|`SQL_CA_SS_FORCE_ENCRYPT` (1236)|Машинное слово (2 байта)|0|Значение 0 (по умолчанию): решение о шифровании этого параметра определяется наличием метаданных шифрования.<br><br>Отличное от нуля значение: параметр шифруется, если для него доступны метаданные шифрования. В противном случае запрос завершается ошибкой [CE300] [Microsoft][ODBC Driver 17 for SQL Server] "Для параметра указано обязательное шифрование, но сервер не предоставил метаданные шифрования".|

### <a name="bcp_control-options"></a>Параметры bcp_control

|Имя параметра|Значение по умолчанию|Описание|
|-|-|-|
|`BCPMODIFYENCRYPTED` (21)|FALSE|Если значение равно TRUE, в зашифрованный столбец можно вставлять значения в формате varbinary(max). Если значение равно FALSE, вставка не допускается без указания правильных метаданных типа и шифрования.|

## <a name="troubleshooting"></a>Устранение неполадок

При возникновении трудностей в использовании Always Encrypted сначала проверьте следующие моменты.

- Наличие и доступность на сервере ключа CEK, который шифрует нужный столбец.

- Наличие ключа CMK, который шифрует ключ CEK, имеет доступные метаданные на сервере, а также доступен из клиента.

- `ColumnEncryption` включается в DSN, строке подключения или атрибуте подключения, а при использовании безопасного анклава имеет правильный формат.

Кроме того, при использовании безопасного анклава ошибки аттестации указывают на шаг в процессе аттестации, на котором произошел сбой (см. следующую таблицу).

|Шаг|Описание|
|----|-----------|
|0–99| Недопустимый ответ аттестации или ошибка при проверке подписи. |
|100–199| Ошибка при получении сертификатов из URL-адреса аттестации. Убедитесь, что `<attestation URL>/v2.0/signingCertificates` является допустимым и доступным. |
|200–299| Непредвиденный или недопустимый формат идентификатора анклава. |
|300–399| Ошибка при установке безопасного канала с анклавом. |

## <a name="see-also"></a>См. также:

- [Always Encrypted (ядро СУБД)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/always-encrypted-enclaves.md)

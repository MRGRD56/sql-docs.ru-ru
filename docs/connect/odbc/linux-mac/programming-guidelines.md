---
title: Указания по программированию (драйвер ODBC)
description: Функции программирования Microsoft ODBC Driver for SQL Server на платформах macOS и Linux основаны на ODBC в SQL Server Native Client (ODBC).
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
author: v-makouz
ms.author: v-daenge
ms.openlocfilehash: 6d602db9c189b6e7fce8b767b60204253ccb7f67
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464400"
---
# <a name="programming-guidelines"></a>Указания по программированию

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Функции программирования [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на платформах macOS и Linux основаны на ODBC в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ([SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)). [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client основан на ODBC в компонентах доступа к данным Windows DAC ([Справочник по программированию ODBC](../../../odbc/reference/odbc-programmer-s-reference.md)).  

Приложение ODBC может использовать множественные активные результирующие наборы (MARS) и другие функции, характерные для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], путем включения `/usr/local/include/msodbcsql.h` после включения заголовков unixODBC (`sql.h`, `sqlext.h`, `sqltypes.h` и `sqlucode.h`). Затем используйте те же символьные имена для элементов, связанных с [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], которые вы бы стали использовать в приложениях ODBC Windows.

## <a name="available-features"></a>Доступные функции  
Следующие разделы документации по [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client для ODBC ([SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)) применяются при использовании драйвера ODBC в macOS и Linux:  

-   [Взаимодействие с SQL Server (ODBC)](../../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
-   [Поддержка времени ожидания соединения и запроса](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
-   [Курсоры](../../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
-   [Улучшенная обработка даты и времени (ODBC)](../../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
-   [Выполнение запросов (ODBC)](../../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
-   [Обработка ошибок и сообщений](../../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
-   [Проверка подлинности Kerberos](../../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)  
-   [Определяемые пользователем типы данных больших значений CLR (ODBC)](../../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)  
-   [Выполнение транзакций (ODBC) (за исключением распределенных транзакций)](../../../relational-databases/native-client/odbc/performing-transactions-in-odbc.md)  
-   [Обработка результатов (ODBC)](../../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
-   [Выполнение хранимых процедур](../../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)
-   [Поддержка разреженных столбцов (ODBC)](../../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md)
-   [Использование шифрования без проверки](../../../relational-databases/native-client/features/using-encryption-without-validation.md)
-   [Возвращающие табличные значения параметры](../../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)
-   [UTF-8 и UTF-16 для команд и API данных](../../../relational-databases/native-client/features/utf-16-support-in-sql-server-native-client-11-0.md)
-   [Использование функций каталога](../../../relational-databases/native-client/odbc/using-catalog-functions.md)  

## <a name="unsupported-features"></a>Неподдерживаемые функции

Правильность работы следующих функций в этой версии драйвера ODBC не была проверена на платформах macOS и Linux:

-   Подключение к отказоустойчивому кластеру
-   [Разрешение IP-адресов прозрачной сети](../using-transparent-network-ip-resolution.md) (до версии 17 драйвера ODBC)
-   [Расширенная трассировка драйвера](/archive/blogs/mattn/enabling-advanced-driver-tracing-for-the-sql-native-client-odbc-drivers)

Следующие функции недоступны в этой версии драйвера ODBC на платформах macOS и Linux: 

-   Распределенные транзакции (атрибут SQL_ATTR_ENLIST_IN_DTC не поддерживается)  
-   Зеркальное отображение базы данных  
-   FILESTREAM  
-   Профилирование производительности драйвера ODBC, рассматриваемое в статье [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md), и следующие атрибуты соединения, связанные с производительностью:  
    -   SQL_COPT_SS_PERF_DATA  
    -   SQL_COPT_SS_PERF_DATA_LOG  
    -   SQL_COPT_SS_PERF_DATA_LOG_NOW  
    -   SQL_COPT_SS_PERF_QUERY  
    -   SQL_COPT_SS_PERF_QUERY_INTERVAL  
    -   SQL_COPT_SS_PERF_QUERY_LOG  
-   SQLBrowseConnect (до версии 17.2)
-   Типы интервалов C, такие как SQL_C_INTERVAL_YEAR_TO_MONTH (задокументированы в статье [Идентификаторы и дескрипторы типов данных](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md))
-   Значение SQL_CUR_USE_ODBC атрибута SQL_ATTR_ODBC_CURSORS функции SQLSetConnectAttr.

## <a name="character-set-support"></a>Поддержка кодировок

Для версий 13 и 13.1 драйвера ODBC данные SQLCHAR должны иметь кодировку UTF-8. Другие кодировки не поддерживаются.

Для версии 17 драйвера ODBC поддерживаются данные SQLCHAR в одной из следующих кодировок:

> [!NOTE]  
> Из-за `iconv` различий в `musl` и `glibc` многие из этих языковых стандартов не поддерживаются в Alpine Linux.
>
> Дополнительные сведения см. в статье [Functional differences from glibc](https://wiki.musl-libc.org/functional-differences-from-glibc.html) (Функциональные различия с glibc).

|Имя|Описание|
|-|-|
|UTF-8|Юникод|
|CP437|Латиница MS-DOS (США)|
|CP850|Латиница 1 MS-DOS|
|CP874|Латиница/тайский|
|CP932|Японский, Shift-JIS|
|CP936|Китайский (упрощенный), GBK|
|CP949|Корейский, EUC-KR|
|CP950|Китайский (традиционный), Big5|
|CP1251|Кириллица|
|CP1253|Греческий|
|CP1256|Арабский|
|CP1257|Балтийская|
|CP1258|Вьетнамский|
|ISO-8859-1 / CP1252|Латиница-1|
|ISO-8859-2 / CP1250|Латиница-2|
|ISO-8859-3|Латиница-3|
|ISO-8859-4|Латиница-4|
|ISO-8859-5|Латиница/кириллица|
|ISO-8859-6|Латиница/арабский|
|ISO-8859-7|Латиница/греческий|
|ISO-8859-8 / CP1255|Иврит|
|ISO-8859-9 / CP1254|Турецкий|
|ISO-8859-13|Латиница-7|
|ISO-8859-15|Латиница-9|

При подключении драйвер определяет текущий языковой стандарт процесса, в котором он загружен. Если используется одна из перечисленных выше кодировок, драйвер применяет ее для данных SQLCHAR (узкий набор символов). В противном случае по умолчанию применяется кодировка UTF-8. Так как все процессы по умолчанию запускаются с языковым стандартом "C" (из-за чего драйвер по умолчанию использует кодировку UTF-8), если приложению необходимо использовать одну из перечисленных выше кодировок, перед подключением оно должно задать соответствующий языковой стандарт с помощью функции **setlocale**. При этом нужный языковой стандарт необходимо указать либо явным образом, либо с помощью пустой строки, например `setlocale(LC_ALL, "")`, чтобы использовался языковой стандарт среды.

В обычной среде Linux или macOS применяется кодировка UTF-8, поэтому после обновления драйвера ODBC с версии 13 или 13.1 до версии 17 пользователи не заметят разницы. Однако если приложение использует отличную от UTF-8 кодировку из приведенного выше списка с помощью функции `setlocale()`, оно должно применять ее для данных, передаваемых в драйвер и из него, вместо UTF-8.

Данные SQLWCHAR должны иметь кодировку UTF-16LE (прямой порядок байтов).

Если при привязке входных параметров с помощью SQLBindParameter указан узкосимвольный тип SQL, такой как SQL_VARCHAR, драйвер преобразует предоставленные данные из кодировки клиента в кодировку [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] по умолчанию (обычно это кодовая страница 1252). Выходные параметры драйвер преобразует из кодировки, указанной в сведениях о параметрах сортировки, связанных с данными, в кодировку клиента. Однако возможна потеря данных — символы исходной кодировки, не имеющие соответствия в конечной кодировке, преобразуются в вопросительные знаки (?).

Чтобы избежать этого при привязке входных параметров, укажите символьный тип Юникода SQL, например SQL_NVARCHAR. В этом случае драйвер выполняет преобразование из кодировки клиента в кодировку UTF-16, в которой могут быть представлены все символы Юникода. Кроме того, конечный столбец или параметр на сервере также должен иметь тип Юникода (**nchar**, **nvarchar**, **ntext**) или такой тип, в параметрах сортировки или кодировке которого могут быть представлены все символы исходных данных. Чтобы избежать потери данных в выходных параметрах, укажите тип Юникода SQL, а также либо тип Юникода C (SQL_C_WCHAR), вследствие чего драйвер вернет данные в кодировке UTF-16, либо узкосимвольный тип C, и убедитесь в том, что в кодировке клиента могут быть представлены все символы исходных данных (это всегда возможно в случае с UTF-8).

Дополнительные сведения о параметрах сортировки и кодирования см. в разделе [Поддержка параметров сортировки и Юникода](../../../relational-databases/collations/collation-and-unicode-support.md).

Существуют различия в преобразовании кодировки между Windows и рядом версий библиотеки iconv в Linux и macOS. Текстовые данные в кодовой странице 1255 (иврит) имеют одну кодовую точку (0xCA), которая ведет себя по-разному при преобразовании в Юникод. В Windows этот символ преобразуется в кодовую точку UTF-16 0x05BA. В macOS и Linux с библиотекой libiconv до версии 1.15 он преобразуется в кодовую точку 0x00CA. В Linux с библиотеками iconv, которые не поддерживают редакцию 2003 кодировки Big5/CP950 (`BIG5-2003`), символы, добавленные с использованием этой редакции, преобразуются неправильно. В кодовой странице 932 (японский, Shift-JIS) результат декодирования символов, которые изначально не определены в стандарте кодировки, также различается. Например, байт 0x80 преобразуется в U+0080 в Windows, но может стать U+30FB в Linux и macOS в зависимости от версии iconv.

Когда в ODBC Driver 13 и 13.1 многобайтовые символы UTF-8 или суррогаты UTF-16 распределяются между буферами SQLPutData, это приводит к повреждению данных. Для потоковой передачи SQLPutData используйте буферы, которые не заканчиваются на частичные кодировки символов. В версии 17 драйвера ODBC это ограничение снято.

## <a name="openssl"></a><a name="bkmk-openssl"></a>OpenSSL
Начиная с версии 17.4, драйвер загружает OpenSSL динамически, что позволяет запускать его на системах, имеющих версию 1.0, либо 1.1, без надобности в отдельных файлах драйверов. При наличии нескольких версий OpenSSL драйвер пытается загрузить последнюю версию. Сейчас драйвер поддерживает OpenSSL 1.0.x и 1.1.x

> [!NOTE]  
> При этом возможен конфликт, если приложение, использующее драйвер (или один из его компонентов), связано с другой версией OpenSSL или динамически загружает ее. Если в системе присутствует несколько версий OpenSSL и приложение используют их, настоятельно рекомендуется убедится, что версии, загруженные приложением и драйвером, совпадают, так как ошибки могут привести к повреждению памяти и, таким образом, они не обязательно будут проявляться очевидным или последовательным образом.

## <a name="alpine-linux"></a><a name="bkmk-alpine"></a>Alpine Linux
На момент написания этой статьи размер стека по умолчанию в MUSL составляет 128 КБ, что достаточно для основных функций драйвера ODBC, но в зависимости от того, что делает приложение, это ограничение можно легко увеличить, особенно при вызове драйвера из нескольких потоков. Для увеличения размера стека рекомендуется компилировать приложение ODBC в Alpine Linux с `-Wl,-z,stack-size=<VALUE IN BYTES>`. Для сведения, размер стека по умолчанию в большинстве систем GLIBC — 2 МБ.

## <a name="additional-notes"></a>Дополнительные замечания  

1.  Вы можете создать выделенное административное соединение с использованием проверки подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и комбинации **узел,порт**. Члену роли Sysadmin сначала нужно сначала обнаружить порт выделенного административного соединения. Инструкции см. в статье [Диагностическое соединение для администраторов баз данных](../../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md#dac-port). Например, если бы порт выделенного административного соединения имел значение 33000, вы могли бы подключиться к нему с помощью `sqlcmd` следующим образом:  

    ```
    sqlcmd -U <user> -P <pwd> -S <host>,33000
    ```

    > [!NOTE]  
    > Выделенные административные соединения должны использовать проверку подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
    
2.  Диспетчер драйверов UnixODBC возвращает "Недопустимый идентификатор атрибута или параметра" для всех атрибутов инструкции, когда они передаются через SQLSetConnectAttr. Когда в Windows SQLSetConnectAttr получает значение атрибута инструкции, он заставляет драйвер установить это значение для всех активных инструкций, являющихся дочерними элементами дескриптора соединения.  

3.  При использовании драйвера с очень многопоточными приложениями проверка обработки unixODBC может стать узким местом производительности. Вы можете получить большую производительность в таких сценариях, выполнив компиляцию unixODBC с параметром `--enable-fastvalidate`. Однако следует помнить, что это может привести к аварийному завершению работы приложений, которые передают в ODBC API некорректные дескрипторы, вместо того, чтобы возвращать ошибки `SQL_INVALID_HANDLE`.

## <a name="see-also"></a>См. также:  
[Часто задаваемые вопросы](frequently-asked-questions-faq-for-odbc-linux.yml)

[Известные проблемы в данной версии драйвера](known-issues-in-this-version-of-the-driver.md)

[Заметки о выпуске](release-notes-odbc-sql-server-linux-mac.md)

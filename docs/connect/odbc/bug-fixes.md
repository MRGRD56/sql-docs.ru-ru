---
title: Список исправленных ошибок
description: На этой странице содержится список ошибок, исправленных в каждом выпуске, начиная с Microsoft ODBC Driver 17 for SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: v-chojas
ms.author: v-jizho2
manager: kenvh
ms.openlocfilehash: 1816f1a8255041d6211931ffc41963acc460e15d
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770481"
---
# <a name="list-of-bugs-fixed"></a>Список исправленных ошибок

На этой странице содержится список ошибок, исправленных в каждом выпуске, начиная с [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

### <a name="bug-fixes-in-the-msconame-odbc-driver-1772-for-ssnoversion"></a>Исправления ошибок в [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.7.2 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Устранение проблемы с ошибками "404 Не найдено" при использовании проверки подлинности Управляемого удостоверения службы
- Исправление для периодических ошибок "Шифрование не поддерживается" при высоких многопотоковых нагрузках
- Исправление для периодического сбоя при высоких многопотоковых нагрузках

### <a name="bug-fixes-in-the-msconame-odbc-driver-177-for-ssnoversion"></a>Исправления ошибок в [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.7 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Исправление кодировки символов для столбцов VARIANT в режиме BCP NATIVE
- Исправление работы параметра SQL_ATTR_PARAMS_PROCESSED_PTR в некоторых условиях
- Исправление SQLDescribeParam в режиме FMTONLY для инструкций с комментариями
- Устранение проблемы с федеративной проверкой подлинности при использовании Okta
- Исправление чрезмерного использования памяти в многопроцессорных системах
- Исправление проверки подлинности Azure AD для некоторых вариантов Базы данных SQL Azure

### <a name="bug-fixes-in-the-msconame-odbc-driver-176-for-ssnoversion"></a>Исправления ошибок в [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.6 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- Исправлена ошибка ADAL, которая происходит при проверке подлинности с помощью федеративной учетной записи (Windows).
- Устранена проблема, из-за которой драйвер перестает отвечать на запросы при превышении времени ожидания во время асинхронной операции уведомления.
- Исправлена работа счетчика ссылок драйвера при обновлении в Alpine Linux.
- Исправлена версия в зависимости libc6 для Ubuntu.
- Добавлено отсутствующее определение в msodbcsql.h для Linux и Mac.

### <a name="bug-fixes-in-the-msconame-odbc-driver-17522-for-ssnoversion-alpine-linux-only"></a>Исправления ошибок в [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.5.2.2 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].(только для Alpine Linux)

- Устранена причина сбоя при использовании Always Encrypted с безопасными анклавами в Alpine Linux.

### <a name="bug-fixes-in-the-msconame-odbc-driver-1752-for-ssnoversion"></a>Исправления ошибок в [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.5.2 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- В пакет Alpine Linux добавлен msodbcsql.h.

### <a name="bug-fixes-in-the-msconame-odbc-driver-175-for-ssnoversion"></a>Исправления ошибок в [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.5 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Устранена ошибка вычисления хэша метаданных AKV CMK в Linux и macOS
- Исправлена ошибка при загрузке OpenSSL 1.0.0.
- Устранены проблемы с преобразованием при использовании кодовых страниц ISO-8859-1 и ISO-8859-2.
- Включен номер версии в имя внутренней библиотеки в macOS
- Исправлена ошибка установки индикатора null при использовании отдельных привязок длины и индикатора.

### <a name="bug-fixes-in-the-msconame-odbc-driver-1742-for-ssnoversion"></a>Исправления ошибок в [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.4.2 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

 - Устранена проблема, когда идентификатор процесса и имя приложения неправильно отправлялись в SQL Server (для анализа sys.dm_exec_sessions) (Linux).
 - Устранена избыточная зависимость от libuuid (Linux).
 - Исправлена ошибка с отправкой данных UTF8 в SQL Server 2019.
 - Исправлена ошибка, при которой языковые стандарты, заканчивающиеся на "@euro", неправильно обнаруживались (Linux).
 - Исправлены XML-данные, возвращаемые неправильно при извлечении в качестве выходного параметра и использовании Always Encrypted.

### <a name="bug-fixes-in-the-msconame-odbc-driver-174-for-ssnoversion"></a>Исправления ошибок в [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.4 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Исправлена периодически возникающая проблема при включенном режиме множественных активных результирующих наборов (MARS), когда драйвер перестает отвечать на запросы.
- Устранена проблема устойчивости подключения при включении асинхронного уведомления, когда драйвер перестает отвечать на запросы.
- Устранены сбои при получении диагностических записей в случае попыток установки многопотоковых подключений.
- Исправлена ошибка Encryption not supported (Шифрование не поддерживается) при повторном подключении после вызова SQLGetInfo () с использованием SQL_USER_NAME и SQL_DATA_SOURCE_READ_ONLY.
- Исправлена ошибка инициализации COM во время интерактивной проверки подлинности в Azure Active Directory.
- Исправлена ошибка SQLGetData() для многобайтных данных в формате UTF8.
- Исправлена ошибка получения длины столбцов sql_variant с использованием SQLGetData().
- Исправлен импорт столбцов sql_variant, содержащих более 7992 байт с помощью программы bcp.
- Исправлена ошибка отправки корректной кодировки на сервер для узкосимвольных данных.

### <a name="bug-fixes-in-the-msconame-odbc-driver-173-for-ssnoversion"></a>Исправления ошибок в [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.3 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Исправлена утечка памяти для дескриптора события отправки TCP.
- Исправлена проблема переопределения перечисления _SQL_FILESTREAM_DESIRED_ACCESS в файле заголовка msodbcsql.h.
- Исправлено отсутствие определения связанного с ACCESS_TOKEN и AUTHENTICATION в файле заголовка msodbcsql.h для Linux.

### <a name="bug-fixes-in-the-msconame-odbc-driver-172-for-ssnoversion"></a>Исправления ошибок в [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.2 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Исправлено сообщение об ошибке проверки подлинности Azure Active Directory.
- Исправлена ошибка обнаружения кодировки, когда переменные среды языкового стандарта заданы по-разному.
- Исправлено аварийное завершение при отключении с восстановлением подключения.
- Исправлено обнаружение активности подключения.
- Исправлено неправильное обнаружение закрытых сокетов.
- Устранено бесконечное ожидание при попытке освободить дескриптор инструкции во время восстановления после сбоя.
- Исправлено неправильное поведение при удалении, если в Windows установлены обе версии: 13 и 17.
- Исправлено поведение расшифровки на более старой платформе Windows (Windows 7, 8 и Server 2012).
- Исправлена проблема с кэшем при использовании проверки подлинности ADAL в Windows.
- Исправлена проблема блокировки и перезаписи журналов трассировки в Windows.

### <a name="bug-fixes-in-the-msconame-odbc-driver-171-for-ssnoversion"></a>Исправления ошибок в [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17.1 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Исправлена 1-секундная задержка при вызове SQLFreeHandle с включенным режимом MARS и атрибутом подключения Encrypt=yes.
- Исправлен сбой с ошибкой 22003 в SQLGetData, происходящий, если размер переданного буфера меньше, чем получаемые данные (Windows).
- Исправлены усеченные сообщения об ошибках ADAL.
- Исправлена редкая ошибка в 32-разрядной системе Windows при преобразовании числа с плавающей запятой в целое число.
- Исправлена проблема, при которой после вставки двоичного значения в десятичное поле с включенной функцией Always Encrypted возвращается ошибка усечения данных.
- Исправлено предупреждение в установщике macOS
- Исправлена ошибка отправки неверного состояния в SQL Server во время попытки восстановления сеанса, когда включены отказоустойчивость подключения и пул подключений, что приводит к удалению сеанса сервером.

### <a name="bug-fixes-in-the-msconame-odbc-driver-17-for-ssnoversion"></a>Исправления ошибок в [!INCLUDE[msCoName](../../includes/msconame_md.md)] ODBC Driver 17 for [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

- Исправлена ошибка, когда при использовании проверки подлинности Kerberos может произойти сбой в процессе выполнения массовой вставки с ошибкой "Доступ запрещен".
- Удален обходной путь для ошибки unixODBC, появившейся в версии 2.3.1 (драйвер вдвое превышал размеры определенных буферов, переданных в unixODBC).
- Исправлена проблема, когда функция устойчивости подключения перестает отвечать на запросы (повторное подключение), при использовании параметра ColumnEncryption=enabled.
- Исправлена ошибка создания имени DSN, когда при использовании параметра интерактивной проверки подлинности Active Directory окно проверки подлинности Azure могло перестать отвечать (Windows).
- Устранен редкий сбой, который происходил при завершении работы ODBC при включенном асинхронном выполнении (происходил при очистке дескриптора подключения).
- Исправлена ошибка, из-за которой драйвер SQL приводил к высокому потреблению ресурсов ЦП при выполнении длинных хранимых процедур.
- Исправлена невозможность получения данных в зашифрованном столбце varbinary(max) без преобразования.
- Исправлена проблема, из-за которой зашифрованный столбец типа varchar(max) со значением NULL извлекался с помощью SQLGetData() при статическом курсоре, а следующий столбец также имел значение null, даже если содержал данные.
- Исправлена проблема с получением поля varbinary(max) при включенном параметре Always Encrypted.
- Исправлена ошибка, из-за которой функция setlocale() не работала с параметром Always Encrypted.
- Исправлена проблема возврата ошибки SQLDescribeParam() при вызове параметра хранимой процедуры XML-типа с включенной функцией Always Encrypted.
- Исправлена проблема, при которой экранированные знаки подчеркивания не работали в SQLTables.
- Исправлена ошибка, при которой данные на иврите (varchar) усекались при возврате в виде расширенных символов в Linux.
- Исправлена проблема с запросом Shift-JIS в кодировке char/varchar из приложения UTF-8.
- Исправлена ошибка, при которой вызов SQLGetInfo с параметром SQL_DRIVER_NAME возвращал имя файла в формате Linux в macOS
- Исправлена проблема, при которой загрузка символьных данных Windows-1252 с использованием входных файлов размером более 32 КБ в столбцы типа VARCHAR с помощью служебной программы BCP приводила к сбоям.

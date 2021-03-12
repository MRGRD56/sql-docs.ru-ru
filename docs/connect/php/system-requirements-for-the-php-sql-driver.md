---
title: Системные требования драйверов Майкрософт для PHP
description: Драйверы Майкрософт для PHP для SQL Server поддерживают широкий спектр версий PHP, операционных систем и версий SQL Server.
ms.custom: ''
ms.date: 03/09/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- requirements
ms.assetid: 5db4b75f-c605-4785-9560-399a533c0fc9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b52bb4597b76ca831b94899e040a814b11b2a903
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622670"
---
# <a name="system-requirements-for-the-microsoft-drivers-for-php-for-sql-server"></a>Системные требования драйверов Майкрософт для PHP для SQL Server

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

В этой статье перечислены компоненты, которые должны быть установлены в системе для доступа к данным в SQL Server или Базе данных SQL Azure с помощью [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].

Официально поддерживаются версия 4.0 и более поздние версии драйверов PHP Майкрософт для SQL Server. Подробные сведения о жизненных циклах поддержки и требованиях к драйверам PHP см. в [таблице поддержки](microsoft-php-drivers-for-sql-server-support-matrix.md).

## <a name="php"></a>PHP

Сведения о том, как скачать и установить актуальные и стабильные двоичные файлы, см. на [веб-сайте PHP](https://php.net).  Для использования драйверов Майкрософт для PHP для SQL Server требуются правильные версии PHP, как указано в разделе [Поддержка версий PHP](microsoft-php-drivers-for-sql-server-support-matrix.md#php-version-support).

-   Необходимо включить правильную версию файла драйвера с соответствующей версией PHP. Сведения о разных файлах драйвера см. в разделе [Версии драйвера](#driver-versions). Сведения о скачивании драйверов см. в статье [Драйверы Майкрософт для PHP для SQL Server](download-drivers-php-sql-server.md). Сведения о настройке драйвера для PHP см. в статье [Загрузка драйверов Майкрософт для PHP для SQL Server](loading-the-php-sql-driver.md).

-   Требуется веб-сервер. Веб-сервер должен быть настроен на выполнение PHP. Сведения о размещении приложений PHP с IIS см. в [учебнике на веб-сайте PHP](http://docs.php.net/manual/da/install.windows.iis7.php).

    [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] был протестирован в службе IIS 10 с помощью FastCGI.

> [!NOTE]
> Корпорация Майкрософт обеспечивает поддержку только для служб IIS.

## <a name="odbc-driver"></a>Драйвер ODBC

На компьютере, на котором работает PHP, требуется правильная версия Microsoft ODBC Driver for SQL Server. Все версии драйвера для поддерживаемых платформ можно скачать на [этой странице](../odbc/download-odbc-driver-for-sql-server.md).

Если вы скачиваете версию драйвера для 64-разрядной версии Windows, установщик 64-разрядной ODBC установит как 32-разрядную, так и 64-разрядную версии драйвера ODBC. При использовании 32-разрядной версии Windows используйте установщик ODBC x86. На платформах, отличных от Windows, доступны только 64-разрядные версии драйвера.

|Версия драйвера PHP &#8594;<br />&#8595; Версия драйвера ODBC|5.9|5.8|5.6|5,3|5,2|4.3|4,0|3.2|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Драйвер ODBC 17+ |Да|Да|Да|Да|Да|   |   |   |
|Драйвер ODBC 13.1|Да|Да|Да|Да|Да|Да|Да|   |
|Драйвер ODBC 13  |   |   |   |   |   |   |Да|   |
|Драйвер ODBC 11  |   |Да|Да|Да|Да|Да|Да|Да|

Если вы используете драйвер SQLSRV, [sqlsrv_client_info](sqlsrv-client-info.md) возвращает сведения о том, какая версия [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Microsoft ODBC Driver for SQL Server применяется [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]. Если вы используете драйвер PDO_SQLSRV, можно применить [PDO::getAttribute](pdo-getattribute.md) для определения версии.

## <a name="sql-server"></a>SQL Server

Дополнительные сведения о поддерживаемых версиях SQL Server см. в [списке поддерживаемых версий баз данных](microsoft-php-drivers-for-sql-server-support-matrix.md#sql-server-version-certified-compatibility).

## <a name="operating-systems"></a>Операционные системы

Дополнительные сведения о поддерживаемых операционных системах см. в [этом разделе](microsoft-php-drivers-for-sql-server-support-matrix.md#supported-operating-systems).

## <a name="driver-versions"></a>Версии драйвера

Этот раздел содержит драйверы, которые входят в состав версий [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]. Каждый пакет установки содержит файлы драйверов SQLSRV и PDO_SQLSRV в потоковых и отдельных вариантах. В Windows они также доступны в 32-разрядных и 64-разрядных версиях. Чтобы настроить драйвер для использования в среде выполнения PHP, следуйте инструкциям по установке [драйверов Microsoft для PHP для SQL Server](loading-the-php-sql-driver.md).

В поддерживаемых версиях Linux и macOS соответствующие драйверы можно установить с помощью системы пакетов PECL для PHP, следуя [инструкциям по установке для Linux и macOS](installation-tutorial-linux-mac.md). Кроме того, вы можете скачать готовые двоичные файлы для платформы на странице [драйверов Майкрософт для PHP для SQL Server](https://github.com/Microsoft/msphpsql/releases) на сайте GitHub. В приведенных ниже таблицах перечислены файлы из предварительно созданных двоичных пакетов.

**Драйверы Майкрософт версии 5.9 для PHP для SQL Server:**

В Windows предоставляются следующие файлы драйверов:

|Файл драйвера|Версия PHP|Является потокобезопасным?|Использование с DLL PHP|
|---------------|:---------------:|:----------------:|---------------------|
|32-bit php_sqlsrv_73_nts.dll<br />32-bit php_pdo_sqlsrv_73_nts.dll|7.3|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_73_ts.dll <br />32-bit php_pdo_sqlsrv_73_ts.dll |7.3|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_73_nts.dll<br />64-bit php_pdo_sqlsrv_73_nts.dll|7.3|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_73_ts.dll <br />64-bit php_pdo_sqlsrv_73_ts.dll |7.3|да|64-разрядная php7ts.dll|
|32-разрядная php_sqlsrv_74_nts.dll<br />32-разрядная php_pdo_sqlsrv_74_nts.dll|7.4|нет |32-разрядная php7.dll|
|32-разрядная php_sqlsrv_74_ts.dll <br />32-разрядная php_pdo_sqlsrv_74_ts.dll |7.4|да|32-разрядная php7ts.dll|
|64-разрядная php_sqlsrv_74_nts.dll<br />64-разрядная php_pdo_sqlsrv_74_nts.dll|7.4|нет |64-разрядная php7.dll|
|64-разрядная php_sqlsrv_74_ts.dll <br />64-разрядная php_pdo_sqlsrv_74_ts.dll |7.4|да|64-разрядная php7ts.dll|
|32-bit php_sqlsrv_80_nts.dll<br />32-bit php_pdo_sqlsrv_80_nts.dll|8.0|нет |32-bit php8.dll|
|32-bit php_sqlsrv_80_ts.dll <br />32-bit php_pdo_sqlsrv_80_ts.dll |8.0|да|32-bit php8ts.dll|
|64-bit php_sqlsrv_80_nts.dll<br />64-bit php_pdo_sqlsrv_80_nts.dll|8.0|нет |64-bit php8.dll|
|64-bit php_sqlsrv_80_ts.dll <br />64-bit php_pdo_sqlsrv_80_ts.dll |8.0|да|64-bit php8ts.dll|

В Linux предоставляются следующие файлы драйверов:

|Файл драйвера|Версия PHP|Является потокобезопасным?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_73_nts.so<br />php_pdo_sqlsrv_73_nts.so|7.3|нет |
|php_sqlsrv_73_ts.so <br />php_pdo_sqlsrv_73_ts.so |7.3|да|
|php_sqlsrv_74_nts.so<br />php_pdo_sqlsrv_74_nts.so|7.4|нет |
|php_sqlsrv_74_ts.so <br />php_pdo_sqlsrv_74_ts.so |7.4|да|
|php_sqlsrv_80_nts.so<br />php_pdo_sqlsrv_80_nts.so|8.0|нет |
|php_sqlsrv_80_ts.so <br />php_pdo_sqlsrv_80_ts.so |8.0|да|

**Драйверы Майкрософт версии 5.8 для PHP для SQL Server**

В Windows включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|Использование с DLL PHP|
|---------------|:---------------:|:----------------:|---------------------|
|32-bit php_sqlsrv_72_nts.dll<br />32-bit php_pdo_sqlsrv_72_nts.dll|7.2|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_72_ts.dll <br />32-bit php_pdo_sqlsrv_72_ts.dll |7.2|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_72_nts.dll<br />64-bit php_pdo_sqlsrv_72_nts.dll|7.2|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_72_ts.dll <br />64-bit php_pdo_sqlsrv_72_ts.dll |7.2|да|64-разрядная php7ts.dll|
|32-bit php_sqlsrv_73_nts.dll<br />32-bit php_pdo_sqlsrv_73_nts.dll|7.3|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_73_ts.dll <br />32-bit php_pdo_sqlsrv_73_ts.dll |7.3|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_73_nts.dll<br />64-bit php_pdo_sqlsrv_73_nts.dll|7.3|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_73_ts.dll <br />64-bit php_pdo_sqlsrv_73_ts.dll |7.3|да|64-разрядная php7ts.dll|
|32-разрядная php_sqlsrv_74_nts.dll<br />32-разрядная php_pdo_sqlsrv_74_nts.dll|7.4|нет |32-разрядная php7.dll|
|32-разрядная php_sqlsrv_74_ts.dll <br />32-разрядная php_pdo_sqlsrv_74_ts.dll |7.4|да|32-разрядная php7ts.dll|
|64-разрядная php_sqlsrv_74_nts.dll<br />64-разрядная php_pdo_sqlsrv_74_nts.dll|7.4|нет |64-разрядная php7.dll|
|64-разрядная php_sqlsrv_74_ts.dll <br />64-разрядная php_pdo_sqlsrv_74_ts.dll |7.4|да|64-разрядная php7ts.dll|

В Linux включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|нет |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|да|
|php_sqlsrv_73_nts.so<br />php_pdo_sqlsrv_73_nts.so|7.3|нет |
|php_sqlsrv_73_ts.so <br />php_pdo_sqlsrv_73_ts.so |7.3|да|
|php_sqlsrv_74_nts.so<br />php_pdo_sqlsrv_74_nts.so|7.4|нет |
|php_sqlsrv_74_ts.so <br />php_pdo_sqlsrv_74_ts.so |7.4|да|

**Драйверы Майкрософт версии 5.6 для PHP для SQL Server:**

В Windows включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|Использование с DLL PHP|
|---------------|:---------------:|:----------------:|---------------------|
|32-bit php_sqlsrv_71_nts.dll<br />32-bit php_pdo_sqlsrv_71_nts.dll|7.1|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_71_ts.dll <br />32-bit php_pdo_sqlsrv_71_ts.dll |7.1|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_71_nts.dll<br />64-bit php_pdo_sqlsrv_71_nts.dll|7.1|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_71_ts.dll <br />64-bit php_pdo_sqlsrv_71_ts.dll |7.1|да|64-разрядная php7ts.dll|
|32-bit php_sqlsrv_72_nts.dll<br />32-bit php_pdo_sqlsrv_72_nts.dll|7.2|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_72_ts.dll <br />32-bit php_pdo_sqlsrv_72_ts.dll |7.2|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_72_nts.dll<br />64-bit php_pdo_sqlsrv_72_nts.dll|7.2|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_72_ts.dll <br />64-bit php_pdo_sqlsrv_72_ts.dll |7.2|да|64-разрядная php7ts.dll|
|32-bit php_sqlsrv_73_nts.dll<br />32-bit php_pdo_sqlsrv_73_nts.dll|7.3|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_73_ts.dll <br />32-bit php_pdo_sqlsrv_73_ts.dll |7.3|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_73_nts.dll<br />64-bit php_pdo_sqlsrv_73_nts.dll|7.3|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_73_ts.dll <br />64-bit php_pdo_sqlsrv_73_ts.dll |7.3|да|64-разрядная php7ts.dll|

В Linux включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|нет |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|да|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|нет |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|да|
|php_sqlsrv_73_nts.so<br />php_pdo_sqlsrv_73_nts.so|7.3|нет |
|php_sqlsrv_73_ts.so <br />php_pdo_sqlsrv_73_ts.so |7.3|да|

**Драйверы Майкрософт версии 5.3 для PHP для SQL Server:**

В Windows включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|Использование с DLL PHP|
|---------------|:---------------:|:----------------:|---------------------|
|32-bit php_sqlsrv_7_nts.dll <br />32-bit php_pdo_sqlsrv_7_nts.dll |7.0|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_7_ts.dll  <br />32-bit php_pdo_sqlsrv_7_ts.dll  |7.0|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_7_nts.dll <br />64-bit php_pdo_sqlsrv_7_nts.dll |7.0|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_7_ts.dll  <br />64-bit php_pdo_sqlsrv_7_ts.dll  |7.0|да|64-разрядная php7ts.dll|
|32-bit php_sqlsrv_71_nts.dll<br />32-bit php_pdo_sqlsrv_71_nts.dll|7.1|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_71_ts.dll <br />32-bit php_pdo_sqlsrv_71_ts.dll |7.1|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_71_nts.dll<br />64-bit php_pdo_sqlsrv_71_nts.dll|7.1|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_71_ts.dll <br />64-bit php_pdo_sqlsrv_71_ts.dll |7.1|да|64-разрядная php7ts.dll|
|32-bit php_sqlsrv_72_nts.dll<br />32-bit php_pdo_sqlsrv_72_nts.dll|7.2|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_72_ts.dll <br />32-bit php_pdo_sqlsrv_72_ts.dll |7.2|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_72_nts.dll<br />64-bit php_pdo_sqlsrv_72_nts.dll|7.2|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_72_ts.dll <br />64-bit php_pdo_sqlsrv_72_ts.dll |7.2|да|64-разрядная php7ts.dll|

В Linux включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|нет |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|да|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|нет |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|да|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|нет |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|да|

**Драйверы Майкрософт версии 5.2 для PHP для SQL Server:**

В Windows включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|Использование с DLL PHP|
|---------------|:---------------:|:----------------:|---------------------|
|32-bit php_sqlsrv_7_nts.dll <br />32-bit php_pdo_sqlsrv_7_nts.dll |7.0|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_7_ts.dll  <br />32-bit php_pdo_sqlsrv_7_ts.dll  |7.0|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_7_nts.dll <br />64-bit php_pdo_sqlsrv_7_nts.dll |7.0|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_7_ts.dll  <br />64-bit php_pdo_sqlsrv_7_ts.dll  |7.0|да|64-разрядная php7ts.dll|
|32-bit php_sqlsrv_71_nts.dll<br />32-bit php_pdo_sqlsrv_71_nts.dll|7.1|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_71_ts.dll <br />32-bit php_pdo_sqlsrv_71_ts.dll |7.1|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_71_nts.dll<br />64-bit php_pdo_sqlsrv_71_nts.dll|7.1|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_71_ts.dll <br />64-bit php_pdo_sqlsrv_71_ts.dll |7.1|да|64-разрядная php7ts.dll|
|32-bit php_sqlsrv_72_nts.dll<br />32-bit php_pdo_sqlsrv_72_nts.dll|7.2|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_72_ts.dll <br />32-bit php_pdo_sqlsrv_72_ts.dll |7.2|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_72_nts.dll<br />64-bit php_pdo_sqlsrv_72_nts.dll|7.2|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_72_ts.dll <br />64-bit php_pdo_sqlsrv_72_ts.dll |7.2|да|64-разрядная php7ts.dll|

В Linux включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|нет |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|да|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|нет |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|да|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|нет |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|да|

**Драйверы Майкрософт версии 4.3 для PHP для SQL Server:**

В Windows включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|Использование с DLL PHP|
|---------------|:---------------:|:----------------:|---------------------|
|32-bit php_sqlsrv_7_nts.dll <br />32-bit php_pdo_sqlsrv_7_nts.dll |7.0|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_7_ts.dll  <br />32-bit php_pdo_sqlsrv_7_ts.dll  |7.0|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_7_nts.dll <br />64-bit php_pdo_sqlsrv_7_nts.dll |7.0|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_7_ts.dll  <br />64-bit php_pdo_sqlsrv_7_ts.dll  |7.0|да|64-разрядная php7ts.dll|
|32-bit php_sqlsrv_71_nts.dll<br />32-bit php_pdo_sqlsrv_71_nts.dll|7.1|нет |32-разрядная php7.dll|
|32-bit php_sqlsrv_71_ts.dll <br />32-bit php_pdo_sqlsrv_71_ts.dll |7.1|да|32-разрядная php7ts.dll|
|64-bit php_sqlsrv_71_nts.dll<br />64-bit php_pdo_sqlsrv_71_nts.dll|7.1|нет |64-разрядная php7.dll|
|64-bit php_sqlsrv_71_ts.dll <br />64-bit php_pdo_sqlsrv_71_ts.dll |7.1|да|64-разрядная php7ts.dll|

В Linux включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|нет |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|да|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|нет |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|да|

**Драйверы Майкрософт версии 4.0 для PHP для SQL Server:**

В Windows включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|Использование с DLL PHP|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_7_nts_x86.dll<br />php_pdo_sqlsrv_7_nts_x86.dll|7.0|нет|32-разрядная php7.dll|
|php_sqlsrv_7_ts_x86.dll<br />php_pdo_sqlsrv_7_ts_x86.dll|7.0|да|32-разрядная php7ts.dll|
|php_sqlsrv_7_nts_x64.dll<br />php_pdo_sqlsrv_7_nts_x64.dll|7.0|нет|64-разрядная php7.dll|
|php_sqlsrv_7_ts_x64.dll<br />php_pdo_sqlsrv_7_ts_x64.dll|7.0|да|64-разрядная php7ts.dll|

В Linux включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|нет |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|да|

**Драйверы Майкрософт версии 3.2 для PHP для SQL Server:**

В Windows включены следующие версии драйвера:

|Файл драйвера|Версия PHP|Является потокобезопасным?|Использование с DLL PHP|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_54_nts.dll<br />php_pdo_sqlsrv_54_nts.dll|5.4|нет|php5.dll|
|php_sqlsrv_54_ts.dll<br />php_pdo_sqlsrv_54_ts.dll|5.4|да|php5ts.dll|
|php_sqlsrv_55_nts.dll<br />php_pdo_sqlsrv_55_nts.dll|5.5|нет|php5.dll|
|php_sqlsrv_55_ts.dll<br />php_pdo_sqlsrv_55_ts.dll|5.5|да|php5ts.dll|
|php_sqlsrv_56_nts.dll<br />php_pdo_sqlsrv_56_nts.dll|5.6|нет|php5.dll|
|php_sqlsrv_56_ts.dll<br />php_pdo_sqlsrv_56_ts.dll|5.6|да|php5ts.dll|

## <a name="see-also"></a>См. также:

- [Getting Started with the Microsoft Drivers for PHP for SQL Server](getting-started-with-the-php-sql-driver.md) (Начало работы с драйверами Майкрософт для PHP для SQL Server)
- [Руководство по программированию драйверов Microsoft для PHP для SQL Server](programming-guide-for-php-sql-driver.md)
- [Справочник по API для драйвера SQLSRV](sqlsrv-driver-api-reference.md)
- [Справочник по API драйвера PDO_SQLSRV](pdo-sqlsrv-driver-reference.md)

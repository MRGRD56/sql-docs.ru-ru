---
title: Скачивание драйвера ODBC Driver for SQL Server
description: Скачайте драйвер Microsoft ODBC Driver for SQL Server, чтобы разрабатывать приложения в собственном коде с подключением к SQL Server и базе данных SQL Azure.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 53b09784-bb9d-4fd4-99d3-0492b3308ac4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b0f215b387cb772220fb56e504e1a5a8c19daa7d
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770552"
---
# <a name="download-odbc-driver-for-sql-server"></a>Скачивание драйвера ODBC Driver for SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Microsoft ODBC Driver for SQL Server — это отдельная библиотека динамической компоновки (DLL), которая содержит поддержку времени выполнения для приложений, использующих API машинного кода для подключения к SQL Server. Используйте Microsoft ODBC Driver 17 for SQL Server для создания новых и расширения существующих приложений, которым необходимо использовать новые возможности SQL Server.

## <a name="download-for-windows"></a>Скачать для Windows

Распространяемый установщик Microsoft ODBC Driver for SQL Server версии 17 устанавливает клиентские компоненты, необходимые во время выполнения, чтобы воспользоваться преимуществами функциями нового SQL Server. При необходимости он устанавливает файлы заголовков, которые требуются для разработки приложения, использующего API ODBC. Начиная с версии 17.4.2, установщик также включает и устанавливает библиотеку проверки подлинности Microsoft Active Directory (ADAL.dll).

Версия 17.7.2 является последней общедоступной (GA) версией. Если у вас установлена предыдущая версия Microsoft ODBC Driver for SQL Server 17, то при установке версии 17.7.2 она обновляется до версии 17.7.2.

**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 17 (x64)](https://go.microsoft.com/fwlink/?linkid=2156851)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 17 (x86)](https://go.microsoft.com/fwlink/?linkid=2156749)**  

### <a name="version-information"></a>Сведения о версии

- Номер выпуска: 17.7.2.1
- Дата выпуска: 10 марта 2021 г.

> [!Note]
> Если вы открываете локализованную версию этой страницы и хотите просмотреть актуальные материалы, посетите эту страницу на [версии сайта на языке US-English](). С версии сайта US-English вы можете скачать SSMS на других [языках из числа доступных](#available-languages).

## <a name="available-languages"></a>Доступные языки

Этот выпуск драйвера Microsoft ODBC Driver for SQL Server доступен для установки на следующих языках:

Microsoft ODBC Driver for SQL Server версии 17.7.2 (версия x64):  
[Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2156851&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2156851&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2156851&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2156851&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2156851&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2156851&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2156851&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2156851&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2156851&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2156851&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2156851&clcid=0x40a)

Microsoft ODBC Driver for SQL Server версии 17.7.2 (версия x86):  
[Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2156749&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2156749&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2156749&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2156749&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2156749&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2156749&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2156749&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2156749&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2156749&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2156749&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2156749&clcid=0x40a)

### <a name="release-notes-for-windows"></a>Заметки о выпуске для Windows

Дополнительные сведения об этом выпуске для Windows см. в [заметках о выпуске для Windows](windows\release-notes-odbc-sql-server-windows.md).

### <a name="previous-releases-for-windows"></a>Предыдущие выпуски для Windows

Чтобы скачать предыдущие версии для Windows, см. страницу с [предыдущими выпусками Microsoft ODBC Driver for SQL Server](windows\release-notes-odbc-sql-server-windows.md#previous-releases).

## <a name="download-for-linux-and-macos"></a>Загрузка для Linux и macOS

Microsoft ODBC Driver for SQL Server можно скачать и установить с помощью диспетчеров пакетов для Linux и macOS, используя соответствующие инструкции по установке.  
[Установка ODBC для SQL Server (Linux)](linux-mac\installing-the-microsoft-odbc-driver-for-sql-server.md)  
[Установка ODBC для SQL Server (macOS)](linux-mac\install-microsoft-odbc-driver-sql-server-macos.md)  

Если необходимо скачать пакеты для автономной установки, все версии доступны по следующим ссылкам.

> [!Note]
> Пакеты с именем `msodbcsql17-*` являются последней версией. Пакеты с именем `msodbcsql-*` являются версией 13 драйвера.

### <a name="alpine"></a>Alpine

- [Пакет Alpine 17.7.2.1](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.2.1-1_amd64.apk) ([подпись PGP](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.2.1-1_amd64.sig))
- [Пакет Alpine 17.7.1.1](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.1.1-1_amd64.apk) ([подпись PGP](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.1.1-1_amd64.sig))
- [Пакет Alpine 17.6.1.1](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.6.1.1-1_amd64.apk) ([подпись PGP](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.6.1.1-1_amd64.sig))
- [Пакет Alpine 17.5.2.2](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.2-1_amd64.apk) ([подпись PGP](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.2-1_amd64.sig))
- [Пакет Alpine 17.5.2.1](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.1-1_amd64.apk) ([подпись PGP](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.1-1_amd64.sig))
- [Пакет Alpine 17.5.1.1](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.1.1-1_amd64.apk) ([подпись PGP](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.1.1-1_amd64.sig))

### <a name="debian"></a>Debian

- [DEB-пакеты для Debian 10](https://packages.microsoft.com/debian/10/prod/pool/main/m/msodbcsql17/)
- [DEB-пакеты для Debian 9](https://packages.microsoft.com/debian/9/prod/pool/main/m/msodbcsql17/)
- [DEB-пакеты для Debian 8](https://packages.microsoft.com/debian/8/prod/pool/main/m/msodbcsql17/)
- [DEB-пакеты для Debian 8 (msodbcsql 13.x)](https://packages.microsoft.com/debian/8/prod/pool/main/m/msodbcsql/)

### <a name="redhat"></a>RedHat

- [RPM-пакеты для RedHat 8](https://packages.microsoft.com/rhel/8/prod/)
- [RPM-пакеты для RedHat 7](https://packages.microsoft.com/rhel/7/prod/)
- [RPM-пакеты для RedHat 6](https://packages.microsoft.com/rhel/6/prod/)

### <a name="suse"></a>Suse

- [RPM-пакеты для SuSE 15](https://packages.microsoft.com/sles/15/prod/)
- [RPM-пакеты для SuSE 12](https://packages.microsoft.com/sles/12/prod/)
- [RPM-пакеты для SuSE 11](https://packages.microsoft.com/sles/11/prod/)

### <a name="ubuntu"></a>Ubuntu

- [DEB-пакеты для Ubuntu 20.10](https://packages.microsoft.com/ubuntu/20.10/prod/pool/main/m/msodbcsql17/)
- [DEB-пакеты для Ubuntu 20.04](https://packages.microsoft.com/ubuntu/20.04/prod/pool/main/m/msodbcsql17/)
- [DEB-пакеты для Ubuntu 18.04](https://packages.microsoft.com/ubuntu/18.04/prod/pool/main/m/msodbcsql17/)
- [DEB-пакеты для Ubuntu 16.04](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql17/)
- [DEB-пакеты для Ubuntu 14.04](https://packages.microsoft.com/ubuntu/14.04/prod/pool/main/m/msodbcsql17/)
- [DEB-пакеты для Ubuntu 16.04 (msodbcsql 13.x)](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/)
- [DEB-пакеты для Ubuntu 14.04 (msodbcsql 13.x)](https://packages.microsoft.com/ubuntu/14.04/prod/pool/main/m/msodbcsql/)

См. также статью [Установка драйвера Linux](linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="macos"></a>macOS

- Дополнительные сведения см. в статье [формулы Homebrew](https://github.com/Microsoft/homebrew-mssql-release).

См. также статью [Установка драйвера macOS](linux-mac/install-microsoft-odbc-driver-sql-server-macos.md).

### <a name="older-linux-releases"></a>Старые выпуски Linux

- **Red Hat Enterprise Linux 5 и 6 (64-разрядные версии)**  - [Скачать Microsoft ODBC Driver 11 for SQL Server — Red Hat Linux](https://go.microsoft.com/fwlink/?LinkId=267321)  
- **SUSE Linux Enterprise 11 с пакетом обновления 2 (64-разрядная версия)**  - [Скачать Microsoft ODBC Driver 11 Preview for SQL Server — SUSE Linux](https://go.microsoft.com/fwlink/?LinkId=264916)

### <a name="release-notes-for-linux-and-macos"></a>Заметки о выпуске для Linux и macOS

Дополнительные сведения о выпусках для Linux и macOS см. в разделе [заметок о выпуске для Linux и macOS](linux-mac\release-notes-odbc-sql-server-linux-mac.md).

---
title: Microsoft ODBC Driver for SQL Server
description: Драйвер Microsoft ODBC Driver for SQL Server обеспечивает подключение к SQL Server и Базе данных SQL Azure через стандартные API ODBC.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9f2ae91b-06af-4c9a-9d24-062df7bc4662
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b107155d9672b2a1d7b905a387e48373905dab55
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464195"
---
# <a name="microsoft-odbc-driver-for-sql-server"></a>Microsoft ODBC Driver for SQL Server

[!INCLUDE[ODBC_Current_Version](../../includes/odbc-latest-release.md)]

[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

ODBC — это основной собственный интерфейс API для доступа к данным, предназначенный для приложений, написанных на языках C и C++ для SQL Server. Драйвер ODBC существует для большинства источников данных. К другим языкам, которые могут использовать ODBC, относятся COBOL, Perl, PHP и Python. ODBC активно применяется для интеграции данных.

Драйвер ODBC поставляется с такими средствами как [**sqlcmd**](../../tools/sqlcmd-utility.md) и [**bcp**](../../tools/bcp-utility.md). Служебная программа **sqlcmd** позволяет выполнять инструкции Transact-SQL, системные процедуры и сценарии SQL. Служебная программа **bcp** используется для массового копирования данных в нужном вам формате между экземпляром Microsoft SQL Server и файлом данных. Также можно использовать **bcp** для импорта большого количества новых строк в таблицы SQL Server или экспорта данных из таблиц в файлы данных.  

## <a name="code-example-in-c"></a>Пример кода на C++

В следующем примере на C++ показано, как использовать интерфейсы API ODBC для подключения и доступа к базе данных:

- [Пример кода C++ с использованием ODBC](../../odbc/reference/sample-odbc-program.md)

## <a name="download"></a>Скачивание

- ![Стрелка скачивания в круге](../../ssms/media/download-icon.png)[Скачать драйвер ODBC](download-odbc-driver-for-sql-server.md)

## <a name="documentation"></a>Документация

### <a name="features"></a>Компоненты

- [Пользовательские поставщики хранилища ключей](../../connect/odbc/custom-keystore-providers.md)
- [Классификация данных](../../connect/odbc/data-classification.md)
- [Ключевые слова и атрибуты строки подключения и имени DSN](dsn-connection-string-attribute.md)
- [SQL Server Native Client](../../relational-databases/native-client/features/sql-server-native-client-features.md) (доступные функции, кроме OLEDB, также применимы к ODBC Driver for SQL Server)
- [Использование Always Encrypted](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
- [Использование Azure Active Directory](../../connect/odbc/using-azure-active-directory.md)
- [Использование разрешения IP-адресов прозрачной сети](../../connect/odbc/using-transparent-network-ip-resolution.md)
- [Using XA Transactions](../../connect/odbc/use-xa-with-dtc.md) (Использование транзакций XA)

### <a name="linux-and-macos"></a>Linux и macOS

- [Установка драйвера в Linux](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)
- [Установка драйвера в macOS](../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md)
- [Подключение к SQL Server](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md)
- [Подключение с помощью **bcp**](../../connect/odbc/linux-mac/connecting-with-bcp.md)
- [Соединение с помощью **sqlcmd**](../../connect/odbc/linux-mac/connecting-with-sqlcmd.md)
- [Трассировка доступа к данным](../../connect/odbc/linux-mac/data-access-tracing-with-the-odbc-driver-on-linux.md)
- [Часто задаваемые вопросы](../../connect/odbc/linux-mac/frequently-asked-questions-faq-for-odbc-linux.yml)
- [Установка диспетчера драйверов](../../connect/odbc/linux-mac/installing-the-driver-manager.md)
- [Известные проблемы](../../connect/odbc/linux-mac/known-issues-in-this-version-of-the-driver.md)
- [Указания по программированию](../../connect/odbc/linux-mac/programming-guidelines.md)
- [Заметки о выпуске](../../connect/odbc/linux-mac/release-notes-odbc-sql-server-linux-mac.md)
- [Заметки о выпуске (mssql-tools)](../../connect/odbc/linux-mac/release-notes-tools.md)
- [Поддержка высокого уровня доступности и аварийного восстановления](../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md)
- [Использование встроенной проверки подлинности (Kerberos)](../../connect/odbc/linux-mac/using-integrated-authentication.md)

### <a name="windows"></a>Windows

- [Образец асинхронного выполнения (метод уведомления)](../../connect/odbc/windows/asynchronous-execution-notification-method-sample.md)
- [Устойчивость подключения в драйвере ODBC в Windows](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)
- [Организация пулов соединений с учетом драйвера](../../connect/odbc/windows/driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md)
- [Изменения в возможностях и работе](../../connect/odbc/windows/features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md)
- [Заметки о выпуске ODBC для SQL Server в Windows](windows/release-notes-odbc-sql-server-windows.md)
- [Системные требования, установка и файлы драйвера](../../connect/odbc/windows/system-requirements-installation-and-driver-files.md)

## <a name="community"></a>Сообщество

- [Блог о драйверах SQL Server](https://techcommunity.microsoft.com/t5/SQL-Server/bg-p/SQLServer/label-name/SQLServerDrivers)  
- [Форум по доступу к данным SQL Server](https://social.technet.microsoft.com/Forums/en/sqldataaccess/threads)  

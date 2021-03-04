---
title: Заметки о выпуске Microsoft ODBC Driver for SQL Server в Linux и macOS
description: Узнайте о новых возможностях и изменениях в выпущенных версиях Microsoft ODBC Driver for SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
author: v-chojas
ms.author: v-chojas
manager: kenvh
ms.openlocfilehash: 2c178b7afd7bf46003b689ef56af67f12609e0a9
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837344"
---
# <a name="release-notes-for-the-microsoft-odbc-driver-for-sql-server-on-linux-and-macos"></a>Заметки о выпуске Microsoft ODBC Driver for SQL Server в Linux и macOS

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

В этой статье перечисляются и описываются новые возможности в версиях [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на платформах Linux и macOS.

<!--
Going forward, please use the new 2-column markdown table for each new H2 version section.
And we are keeping the H2 titles much shorter, partly by removing redundant or obvious info.
We want to track the Month YYYY each new version H2 section is added, at the end of the H2 title.
This is the new standard format for Release Notes article content.

OLD     FILE NAME:    linux-mac/release-notes.md
NOW NEW FILE NAME:    linux-mac/release-notes-odbc-sql-server-linux-mac.md

Thank you.
GeneMi.  2019/04/03.
-->

## <a name="177-january-2021"></a>17.7 — январь 2021 г.

| Изменения | Сведения |
| :------- | :------ |
| Поддерживаются новые дистрибутивы. | Ubuntu 20,10, macOS Big Sur (11.0), Oracle Linux 7 |
| Проверка подлинности на основе субъекта-службы | Подробнее см. статью [DSN and Connection String Keywords and Attributes](../dsn-connection-string-attribute.md) (Ключевые слова и атрибуты строки подключения и имени DSN). |
| Возможность вставки в зашифрованные столбцы money и smallmoney | Подробные сведения см. в статье [Использование Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Исправления ошибок. | [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="176-july-2020"></a>17.6, июль 2020 г.

| Изменения | Сведения |
| :------- | :------ |
| Поддерживаются новые дистрибутивы. | Ubuntu 20.04 |
| Поддержка федеративной проверки подлинности | Подробные сведения см. в статье [Использование Azure Active Directory](../using-azure-active-directory.md). |
| Кэширование метаданных для подготовленных инструкций | Подробные сведения см. в статье [Использование Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md). |
| Атрибут подключения SQL_COPT_SS_AUTOBEGINTXN, определяющий, выполняется ли автоматически инструкция BEGIN TRANSACTION после ROLLBACK или COMMIT. | Подробнее см. статью [DSN and Connection String Keywords and Attributes](../dsn-connection-string-attribute.md) (Ключевые слова и атрибуты строки подключения и имени DSN). |
| Исправления ошибок. | [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="17522-april-2020-alpine-linux-only"></a>Версия 17.5.2.2, апрель 2020 г. (только для Alpine Linux)

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Исправлены ошибки. | См. статью [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="1752-march-2020"></a>17.5.2, март 2020 г.

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Поддержка проверки подлинности с помощью управляемого удостоверения для Azure Key Vault | См. сведения об [использовании функции Always Encrypted с драйвером ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Поддержка дополнительных конечных точек Azure Key Vault | См. сведения об [использовании функции Always Encrypted с драйвером ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Исправления ошибок. | См. статью [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="175-january-2020"></a>17.5, январь 2020 г.

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Атрибут подключения SQL_COPT_SS_SPID для получения SPID без обращения к серверу | Подробнее см. статью [DSN and Connection String Keywords and Attributes](../dsn-connection-string-attribute.md) (Ключевые слова и атрибуты строки подключения и имени DSN). |
| Поддержка указания о принятии условий лицензии через `debconf` в Debian и Ubuntu. | См. [Installing the Microsoft ODBC Driver for SQL Server on Linux and macOS](./installing-the-microsoft-odbc-driver-for-sql-server.md) (Установка Microsoft ODBC Driver for SQL Server на Linux и macOS). |
| Поддерживаются новые дистрибутивы. | &bull; &nbsp; &nbsp; Alpine Linux (3.10, 3.11)<br/>&bull; &nbsp; &nbsp; Oracle Linux 8<br/>&bull; &nbsp; &nbsp; Ubuntu 19.10<br/>&bull; &nbsp; &nbsp; macOS 10.15 |
| Исправления ошибок. | См. статью [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="1742-october-2019"></a>17.4.2, октябрь 2019 г.

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Поддержка дополнительных конечных точек Azure Key Vault | См. сведения об [использовании функции Always Encrypted с драйвером ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Поддержка настройки версии классификации данных | См. статью [Классификация данных](../data-classification.md#bkmk-version). |
| Исправления ошибок. | См. статью [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

**Известная проблема.**

При использовании Always Encrypted с безопасными анклавами и Azure Key Vault, нечетная длина пути ключа может привести к ошибкам проверки подписи главного ключа шифрования. Если вы столкнулись с этой проблемой, попробуйте изменить длину пути на один символ, переименовав ключ Azure Key Vault.

## <a name="174-august-2019"></a>17.4, август 2019 г.

| Добавленная возможность | Сведения |
| :------------ | :------ |
| Always Encrypted с безопасными анклавами | См. сведения об [использовании функции Always Encrypted с драйвером ODBC](../using-always-encrypted-with-the-odbc-driver.md). |
| Динамическая загрузка OpenSSL | См. [указания по программированию](programming-guidelines.md#bkmk-openssl). |
| Настраиваемые параметры проверки активности TCP. | См. сведения о [подключении к SQL Server](connection-string-keywords-and-data-source-names-dsns.md). |
| Исправления ошибок. | См. статью [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="173-february-2019"></a>Версия 17.3, февраль 2019 г.

| Изменения | Сведения |
| :------- | :------ |
| Поддерживаются новые дистрибутивы. | &bull; &nbsp; &nbsp; SUSE 15<br/>&bull; &nbsp; &nbsp; Ubuntu 18.10<br/>&bull; &nbsp; &nbsp; macOS 10.14 |
| Режим проверки подлинности Управляемого удостоверения Azure Active Directory (назначаемого системой и пользователем). | См. статью [Использование Azure Active Directory с драйвером ODBC](../using-azure-active-directory.md). |
| Возможность передавать входные параметры в потоковом режиме для столбцов Always Encrypted. | Дополнительные сведения см. в разделе [Ограничения драйвера ODBC при использовании Always Encrypted](../using-always-encrypted-with-the-odbc-driver.md#limitations-of-the-odbc-driver-when-using-always-encrypted). |
| Распределенные транзакции XA. | См. статью [Использование транзакций XA](../use-xa-with-dtc.md).<br/><br/>XA — это сокращение от _eXtended Architecture_ (расширенная архитектура). Так называется стандарт выполнения глобальных транзакций, которые обращаются к нескольким системам хранения данных на стороне сервера. |
| &nbsp; | &nbsp; |

## <a name="172-july-2018"></a>Версия 17.2, июль 2018 г.

| Изменения | Сведения |
| :------- | :------ |
| Поддерживаются новые дистрибутивы. | &bull; &nbsp; &nbsp; Ubuntu 18.04 |
| Классификация данных для Базы данных SQL Azure и SQL Server. | См. статью [Классификация данных](../data-classification.md). |
| Поддержка кодировки UTF-8 на сервере. | &nbsp; |
| `SQLBrowseConnect` | &nbsp; |
| Динамическая зависимость от `libcurl`. | Начиная с этой версии пакет `libcurl` не является явной зависимостью.<br/>Пакет `libcurl` для OpenSSL или NSS требуется при использовании Azure Key Vault или проверки подлинности Azure Active Directory.<br/>Если возникает связанная с пакетом `libcurl` ошибка, убедитесь в том, что он установлен. |
| Обеспечение устойчивости соединения в режиме ожидания с помощью ключевых слов ConnectRetryCount и ConnectRetryInterval в строке подключения. | &bull; &nbsp; &nbsp; Чтобы извлечь количество повторных попыток подключения, используйте атрибут `SQL_COPT_SS_CONNECT_RETRY_COUNT`(только для чтения).<br/><br/>&bull; &nbsp; &nbsp; Чтобы извлечь продолжительность интервала повтора подключения, используйте атрибут `SQL_COPT_SS_CONNECT_RETRY_INTERVAL`(только для чтения).<br/><br/>См. статью [Устойчивость подключения в драйвере ODBC в Windows](../windows/connection-resiliency-in-the-windows-odbc-driver.md). |
| Исправления ошибок. | [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="171-march-2018"></a>Версия 17.1, март 2018 г.

| Изменения | Сведения |
| :------- | :------ |
| Поддержка атрибутов подключения `SQL_COPT_SS_CEKCACHETTL` и `SQL_COPT_SS_TRUSTEDCMKPATHS`. | &bull; &nbsp; &nbsp; `SQL_COPT_SS_CEKCACHETTL` позволяет управлять временем существования локального кэша для ключей шифрования столбцов, а также освобождать его.<br/><br/>&bull; &nbsp; &nbsp; `SQL_COPT_SS_TRUSTEDCMKPATHS` позволяет приложению ограничивать главные ключи столбцов, используемые для операций Always Encrypted, определенным списком.<br/><br/>См. статью [Использование функции Always Encrypted с драйвером ODBC для SQL Server](../using-always-encrypted-with-the-odbc-driver.md). |
| Поддержка загрузки файла `.rll` из расположения по умолчанию. | См. раздел [Загрузка файла ресурсов](installing-the-microsoft-odbc-driver-for-sql-server.md#resource-file-loading) в документе об установке. |
| Исправления ошибок. | [Исправления ошибок](../bug-fixes.md). |
| &nbsp; | &nbsp; |

## <a name="17"></a>17

**Поддерживаются новые дистрибутивы**: macOS High Sierra и Ubuntu 17.10 

**Повышение производительности**. Производительность при выполнении драйвером преобразования из UTF-8 в UTF-16 и обратно увеличена более чем в 10 раз.

**Добавлены возможности**:

Поддержка Always Encrypted для API BCP

Новый атрибут строки подключения UseFMTOnly предписывает драйверу использовать старые метаданные в особых случаях, в которых требуются временные таблицы.

Поддержка Управляемого экземпляра SQL Azure. 
> [!NOTE]
> При использовании Управляемого экземпляра есть ряд особенностей.
> -   FILESTREAM не поддерживается. 
> -   Доступ к локальной файловой системе не поддерживается, однако требуется, например, для файлов трассировки. 
> -   Создание пользовательских типов по локальным путям не поддерживается. 
> -   Встроенная проверка подлинности Windows не поддерживается. 
> -   DTC не поддерживается 
> -   Учетная запись sa отсутствует (учетная запись по умолчанию называется cloudSA).
> -   В ошибке токена TDS (0xAA) возвращается неправильное имя сервера.
> -   Специальные символы в имени базы данных не поддерживаются. 
> -   Инструкция ALTER DATABASE [имя_БД1] MODIFY NAME = [имя_БД2] не поддерживается.
> -   Сообщения об ошибках всегда выводятся на английском языке независимо от выбранного языка (так же как в Azure). 

## <a name="131-for-ssnoversion-on-linux-and-macos-may-2017"></a>Версия 13.1, для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в Linux и macOS, май 2017 г.

В драйвере ODBC 13.1 для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] добавлена поддержка Always Encrypted и Azure Active Directory при использовании в сочетании с Microsoft SQL Server 2016.

**Поддерживаются новые дистрибутивы**: OS X 10.11 и macOS 10.12 поддерживаются в первой версии драйвера ODBC для macOS. Кроме того, теперь поддерживается Ubuntu 16.10 наравне с Red Hat 6 и 7 и SUSE 12. Для каждой платформы есть соответствующий пакет (RPM или DEB), упрощающий установку и настройку. Дополнительные сведения см. в инструкциях по установке драйвера ODBC для [Linux](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md) и [macOS](../../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md).

**Изменения поддержки диспетчера драйверов 2.3.1 unixODBC** Драйвер ODBC больше не зависит от пользовательских пакетов для диспетчера драйверов unixODBC (исключением является RedHat 6). Вместо этого используется диспетчер пакетов дистрибутива для разрешения зависимости UnixODBC из репозиториев дистрибутива.

**Поддержка API-интерфейса BCP**. Драйвер ODBC в Linux и macOS теперь поддерживает использование [функций API BCP (**bcp_init** и других)](../../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md).

## <a name="130-for-ssnoversion-on-linux"></a>Версия 13.0, для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в Linux

Драйвер Microsoft ODBC Driver 13.0 for SQL Server теперь также поддерживает SQL Server 2014 и SQL Server 2016.  

**Поддерживаются новые дистрибутивы**:

Теперь Ubuntu поддерживается наравне с Red Hat и SUSE. Для каждой платформы есть соответствующий пакет (RPM или DEB), упрощающий установку и настройку.  Инструкции по установке см. в статье [Установка Microsoft ODBC Driver for SQL Server на Linux и macOS](../../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

**Поддержка диспетчера драйверов 2.3.1 unixODBC**. Помимо обновления диспетчера драйверов, также появился пакет, который упрощает установку и настройку этой зависимости.  

**Разрешение IP-адресов прозрачной сети**. Это вариант существующей функции отработки отказа в сети с подсетями, который влияет на последовательность подключения драйвера в случае, когда с именем узла связано несколько IP-адресов, но первый разрешенный IP-адрес не отвечает на запросы.

**Поддержка TLS 1.2**. Драйвер Microsoft ODBC Driver 13.0 for SQL Server в Linux теперь поддерживает протокол TLS 1.2 при использовании защищенного обмена данными с SQL Server.

## <a name="11-for-ssnoversion-on-linux"></a>Версия 11, для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в Linux

Драйвер ODBC в SUSE Linux (предварительная версия) поддерживает 64-разрядную версию SUSE Linux Enterprise 11 с пакетом обновления 2. Дополнительные сведения см. в статье [Требования к системе](../../../connect/odbc/linux-mac/system-requirements.md).  

Драйвер ODBC для Linux поддерживает [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)]. Дополнительные сведения см. в статье [Поддержка высокой доступности и аварийного восстановления в драйвере ODBC для Linux](../../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md).  

Драйвер ODBC для Linux поддерживает подключения к Базе данных SQL Azure.

В `bcp` добавлен параметр `-l` (время ожидания входа). Дополнительные сведения см. в статье [Подключение с помощью **bcp**](../../../connect/odbc/linux-mac/connecting-with-bcp.md).

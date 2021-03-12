---
title: Заметки о выпуске для драйвера JDBC
description: В этой статье перечислены выпуски Microsoft JDBC Driver for SQL Server. Для каждой версии выпуска названы и описаны изменения.
ms.custom: ''
ms.date: 03/02/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 074f211e-984a-4b76-bb15-ee36f5946f12
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c394eaeb6ec8361dd429b944b128d19a0cf33319
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837531"
---
# <a name="release-notes-for-the-microsoft-jdbc-driver-for-sql-server"></a>Заметки о выпуске Microsoft JDBC Driver for SQL Server

В этой статье перечислены выпуски _Microsoft JDBC Driver для SQL Server_ . Для каждой версии выпуска названы и описаны изменения.

## <a name="92"></a><a id="92"></a> 9.2

**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft JDBC Driver 9.2.1 для SQL Server (ZIP)](https://go.microsoft.com/fwlink/?linkid=2155948)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft JDBC Driver 9.2.1 для SQL Server (TAR.GZ)](https://go.microsoft.com/fwlink/?linkid=2155949)**  

Номер версии: 9.2.1  
Дата выпуска: 02 марта 2021 г.

Если необходимо загрузить драйвер на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера в ZIP-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x40a)  
Для драйвера в TAR.GZ-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x40a)  

### <a name="compliance"></a>Соответствие нормативным требованиям

| Изменение соответствий | Сведения |
| :---------------- | :------ |
| Скачайте последние обновления для драйвера JDBC версии 9.2. | &bull; &nbsp; [GitHub, 9.2.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v9.2.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Полностью соответствует спецификации API JDBC версии 4.2. | JAR-файлам в пакете 9.2 имена присваиваются с учетом совместимости версий Java.<br/><br/>Например, файл mssql-jdbc-9.2.1.jre15.jar из пакета 9.2 должен использоваться с Java 15. |
| Совместимость с пакетом Java Development Kit (JDK) версий 15.0, 11.0 и 1.8. | Microsoft JDBC Driver for SQL Server версии 9.2 теперь совместим с пакетом Java Development Kit (JDK) версии 15.0 наряду с JDK версий 11.0 и 1.8. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Выпуски

Номер версии: 9.2.1  
Дата выпуска: 02 марта 2021 г.

Устраненные проблемы в версии 9.2.1:

Исправлена проблема, из-за которой выдавалась ошибка о пустом секрете клиента во время проверки подлинности ActiveDirectoryServicePrincipal в среде Azure.

Номер версии: 9.2.0  
Выпущено: 29 января 2021 г.

**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft JDBC Driver 9.2.0 для SQL Server (ZIP)](https://go.microsoft.com/fwlink/?linkid=2153622)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft JDBC Driver 9.2.0 для SQL Server (TAR.GZ)](https://go.microsoft.com/fwlink/?linkid=2153521)**  

ZIP-файл: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x40a)  
Файл TAR.GZ: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x40a)  

### <a name="support-for-jdk-15"></a>Поддержка JDK 15

Microsoft JDBC Driver for SQL Server версии 9.2 теперь совместим с пакетом Java Development Kit (JDK) версии 15.0 наряду с JDK версий 11.0 и 1.8.

### <a name="added-support-for-azure-active-directory-interactive-authentication"></a>Добавлена поддержка интерактивной проверки подлинности Azure Active Directory
| Добавлен тип проверки подлинности | Сведения |
| :---------- | :------ |
| Microsoft JDBC Driver for SQL Server версии 9.2 теперь поддерживает интерактивную проверку подлинности в Azure Key Vault. | См. дополнительные сведения об [установке подключения с использованием проверки подлинности Azure Active Directory](../../connect/jdbc/connecting-using-azure-active-directory-authentication.md). |
| &nbsp; | &nbsp; |

### <a name="added-support-for-azure-active-directory-service-principal-authentication"></a>Добавлена поддержка проверки подлинности субъекта-службы Azure Active Directory
| Добавлен тип проверки подлинности | Сведения |
| :---------- | :------ |
| Microsoft JDBC Driver for SQL Server версии 9.2 теперь поддерживает проверку подлинности в Azure Key Vault с помощью идентификатора клиента и секрета удостоверения субъекта-службы. | См. дополнительные сведения об [установке подключения с использованием проверки подлинности Azure Active Directory](../../connect/jdbc/connecting-using-azure-active-directory-authentication.md). |
| &nbsp; | &nbsp; |

### <a name="updated-azure-key-vault-library-to-use-modern-azure-key-vault-library"></a>Библиотека Azure Key Vault обновлена до современной версии
| Изменения библиотеки для Azure Key Vault | Сведения |
| :------------------- | :------ |
| Microsoft JDBC Driver версии 9.2 переведен с библиотеки Azure Key Vault и библиотек ADAL предыдущего поколения на более современные библиотеки Azure Key Vault и идентификации Azure. | **ИЗМЕНЕНИЕ ЗАВИСИМОСТЕЙ**. Если вы используете проверку подлинности Azure Active Directory или Azure Key Vault, обновите зависимости приложения. Ознакомьтесь с требованиями к зависимостям при [работе с поставщиком Azure Key Vault](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md#working-with-the-azure-key-vault-provider) или [проверкой подлинности Azure Active Directory](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md#working-with-azure-active-directory-authentication). |
| &nbsp; | &nbsp; |

### <a name="enabled-usebulkcopyforbatchinsert-for-non-azure-synapse-analytics-servers"></a>Параметр useBulkCopyForBatchInsert включен для серверов, отличных от Azure Synapse Analytics
| Изменения в использовании useBulkCopyForBatchInsert для серверов, отличных от Azure Synapse Analytics | Сведения |
| :------------------- | :------ |
| Microsoft JDBC Driver версии 9.2 теперь поддерживает интерфейс API массового копирования для выполнения операций пакетной вставки на серверах, отличных от Azure Synapse Analytics. | См. статью [Использование API массового копирования для операции пакетной вставки](../../connect/jdbc/use-bulk-copy-api-batch-insert-operation.md). |
| &nbsp; | &nbsp; |


## <a name="previous-releases"></a>Предыдущие выпуски

## <a name="84"></a><a id="84"></a> 8.4

**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 8.4 (ZIP)](https://go.microsoft.com/fwlink/?linkid=2137600)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 8.4 (TAR.GZ)](https://go.microsoft.com/fwlink/?linkid=2137502)**  

Номер версии: 8.4.1  
Выпущено: 27 августа 2020 г.

Если необходимо загрузить драйвер на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера в ZIP-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2137600&clcid=0x40a)  
Для драйвера в TAR.GZ-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2137502&clcid=0x40a)  

### <a name="compliance"></a>Соответствие нормативным требованиям

| Изменение соответствий | Сведения |
| :---------------- | :------ |
| Загрузите последние обновления для драйвера JDBC версии 8.4. | &bull; &nbsp; [GitHub, 8.4.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v8.4.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Полностью соответствует спецификации API JDBC версии 4.2. | JAR-файлам в пакете 8.4 имена присваиваются с учетом совместимости версий Java.<br/><br/>Например, файл mssql-jdbc-8.4.1.jre14.jar из пакета 8.4 должен использоваться с Java 14. |
| Совместимость с пакетом Java Development Kit (JDK) версий 14.0, 11.0 и 1.8. | Microsoft JDBC Driver версии 8.4 для SQL Server теперь совместим с пакетом Java Development Kit (JDK) версии 14.0 наряду с JDK версий 11.0 и 1.8. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Выпуски

Номер версии: 8.4.1  
Выпущено: 27 августа 2020 г.  
Исправленные проблемы:  

- Исправлена проблема несовместимости `SQLServerConnectionPoolProxy` с `delayLoadingLobs`
- Исправлена потенциальная проблема `NullPointerException` с `delayLoadingLobs`
- Исправлена проблема с расшифровкой ключей шифрования столбцов при использовании хранилища сертификатов Windows.

Номер версии: 8.4.0  
Выпущено: 31 июля 2020 г.  

### <a name="support-for-jdk-14"></a>Поддержка JDK 14

Microsoft JDBC Driver версии 8.4 для SQL Server теперь совместим с пакетом Java Development Kit (JDK) версии 14.0 наряду с JDK версий 11.0 и 1.8.

### <a name="added-support-for-authentication-to-azure-key-vault-using-managed-identity"></a>Добавлена поддержка проверки подлинности для Azure Key Vault с помощью управляемого удостоверения.

| Добавлен тип проверки подлинности | Сведения |
| :---------- | :------ |
| Microsoft JDBC Driver 8.4 для SQL Server теперь поддерживает проверку подлинности для Azure Key Vault с помощью управляемого удостоверения. | См. [Использование функции Always Encrypted с драйвером JDBC](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md) |
| &nbsp; | &nbsp; |

### <a name="extended-support-for-bulk-copy-for-azure-data-warehouse"></a>Расширенная поддержка массового копирования для хранилища данных Azure

| Изменения в массовом копировании для хранилища данных Azure | Сведения |
| :------------------- | :------ |
| В Microsoft JDBC 8.4 добавлено новое свойство соединения `sendTemporalDataTypesAsStringForBulkCopy`. Это логическое свойство равно TRUE по умолчанию. | См. [Использование массового копирования с драйвером JDBC](../../connect/jdbc/using-bulk-copy-with-the-jdbc-driver.md). |
| &nbsp; | &nbsp; |

### <a name="added-support-for-azure-sql-dns-caching"></a>Добавлена поддержка кэширования DNS в Azure SQL.

| Кэширование DNS | Сведения |
| :------------------- | :------ |
| Microsoft JDBC Driver 8.4 для SQL Server теперь поддерживает кэширование DNS для серверов SQL Azure. | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="added-backwards-compatibility-for-streaming-lob-objects"></a>Добавлена обратная совместимость для потоковой передачи объектов LOB

| Потоковая передача LOB | Сведения |
| :------------------- | :------ |
| В Microsoft JDBC Driver версии 8.4 для SQL Server добавлено новое свойство соединения `delayLoadingLobs`. | Установка для `delayLoadingLobs` значения FALSE приведет к тому, что все объекты LOB, полученные из ResultSet, не будут передаваться в поток. Это означает, что драйвер будет загружать весь объект LOB в память за один раз, аналогично тому, как драйвер работал до выпуска версии 6.4. |
| &nbsp; | &nbsp; |

### <a name="added-support-for-client-certificate-authentication-for-loopback-scenarios"></a>Добавлена поддержка проверки подлинности сертификата клиента для сценариев замыкания на себя.

| Аутентификация на основе сертификата клиента | Сведения |
| :------------------- | :------ |
| В Microsoft JDBC Driver 8.4 для SQL Server добавлен новый метод проверки подлинности, который называется "проверка подлинности с помощью сертификата клиента", для сценариев замыкания на себя. | См. [Проверка подлинности сертификата клиента для сценариев замыкания на себя](../../connect/jdbc/client-certification-authentication-for-loopback-scenarios.md). |

## <a name="82"></a><a id="82"></a> 8.2

**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 8.2 (ZIP)](https://go.microsoft.com/fwlink/?linkid=2122433)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 8.2 (TAR.GZ)](https://go.microsoft.com/fwlink/?linkid=2122536)**  

Номер версии: 8.2.2 Выпущено: 24 марта 2020 г.

Если необходимо загрузить драйвер на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера в ZIP-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122433&clcid=0x40a)  
Для драйвера в TAR.GZ-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122536&clcid=0x40a)  

### <a name="compliance"></a>Соответствие нормативным требованиям

| Изменение соответствий | Сведения |
| :---------------- | :------ |
| Скачайте последние обновления для драйвера JDBC версии 8.2. | &bull; &nbsp; [GitHub, 8.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v8.2.2)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Полностью соответствует спецификации API JDBC версии 4.2. | JAR-файлам в пакете 8.2 имена присваиваются с учетом совместимости версий Java.<br/><br/>Например, файл mssql-jdbc-8.2.2.jre11.jar из пакета 8.2 должен использоваться с Java 11. |
| Совместимость с пакетом Java Development Kit (JDK) версий 13.0, 11.0 и 1.8. | Microsoft JDBC Driver версии 8.2 для SQL Server теперь совместим с пакетом Java Development Kit (JDK) версии 13.0 наряду с JDK версий 11.0 и 1.8. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Выпуски

Номер версии: 8.2.2  
Выпущено: 24 марта 2020 г.  
Исправленные проблемы:  

- Добавлен параметр для настройки списка доверенных конечных точек Azure Key Vault

Номер версии: 8.2.1  
Выпущено: 26 февраля 2020 г.  
Исправленные проблемы:  

- Исправлена потенциальная проблема `NullPointerException` при получении данных в качестве типа `java.time.LocalTime` или `java.time.LocalDate` с помощью `SQLServerResultSet.getObject()`

Номер версии: 8.2.0  
Выпущено: 31 января 2020 г.  

### <a name="support-for-jdk-13"></a>Поддержка JDK 13

Microsoft JDBC Driver версии 8.2 для SQL Server теперь совместим с пакетом Java Development Kit (JDK) версии 13.0 наряду с JDK версий 11.0 и 1.8.

### <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted с безопасными анклавами.

| Изменение функции Always Encrypted | Сведения |
| :--------- | :------ |
| Microsoft JDBC Driver версии 8.2 для SQL Server теперь поддерживает Always Encrypted с безопасными анклавами. Подробные сведения можно найти здесь: Always Encrypted с безопасными анклавами. |
| Дополнительные сведения и пример кода. | См. подробнее об [использовании Always Encrypted с безопасными анклавами](../../connect/jdbc/using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver.md). |
| &nbsp; | &nbsp; |

### <a name="performance-improvement-when-retrieving-temporal-datatypes-from-sql-server-sup1sup"></a>Повышение производительности при извлечении темпоральных типов данных из SQL Server <sup>1</sup>

| Изменение, касающееся темпоральных типов данных | Сведения |
| :---------- | :------ |
| Повышена производительность Microsoft JDBC Driver версии 8.2 для SQL Server при извлечении темпоральных типов данных из SQL Server. | Это изменение устраняет ненужные преобразования темпоральных типов данных, исключая использование java.util.Calendar, где это возможно. |
| Ниже приведен список темпоральных типов данных, которые были затронуты этим улучшением производительности, в виде типа данных SQL Server, за которым следует соответствующее сопоставление Java. | date (java.sql.Date), datetime (java.sql.Timestamp), datetime2 (java.sql.Timestamp), smalldatetime (java.sql.Timestamp) и time (java.sql.Time). |
| &nbsp; | &nbsp; |

<sup>1</sup> Из-за различий в обработке часовых поясов интерфейсами API java.util.Calendar и java.time.LocalDateTime это улучшение не влияет на темпоральные типы данных, с которыми связаны предоставленные пользователем объекты java.util.Calendar, и типы данных microsoft.sql.DateTimeOffset.

### <a name="deployment-of-mssql-jdbc_auth-version-archdll-previously-sqljdbc_authdll-to-maven-repository"></a>Развертывание mssql-jdbc_auth-\<version>-\<arch>.dll (ранее sqljdbc_auth.dll) в репозитории Maven

| Изменение, касающееся sqljdbc_auth.dll | Сведения |
| :------------------- | :------ |
| Начиная с версии 8.2 драйвер Microsoft JDBC Driver for SQL Server использует библиотеку mssql-jdbc_auth-\<version>-\<arch>.dll вместо sqljdbc_auth.dll при проверке подлинности посредством Azure Active Directory. | &nbsp; |
| Эта библиотека DLL также была добавлена в репозиторий Maven для упрощения доступа. | См. [эту страницу](https://search.maven.org/artifact/com.microsoft.sqlserver/mssql-jdbc_auth). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Известные проблемы

| Известные проблемы | Сведения |
| :----------- | :------ |
| При использовании Always Encrypted с безопасными анклавами с Java 8. | Пользователи должны включить поставщик BouncyCastle в качестве зависимости или сопоставить либо загрузить поставщик безопасности, который поддерживает алгоритм подписи RSASSA-PSS. |
| &nbsp; | &nbsp; |

## <a name="a-id74-741"></a><a id="74"> 7.4.1

**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft JDBC Driver for SQL Server версии 7.4.1 (самораспаковывающийся EXE-файл)](https://go.microsoft.com/fwlink/?linkid=2122712)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 7.4.1 (TAR.GZ)](https://go.microsoft.com/fwlink/?linkid=2122613)**  

Номер версии: 7.4.1  
Выпущено: 2 августа 2019 г.

Если необходимо загрузить драйвер на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера в самораспаковывающемся EXE-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122712&clcid=0x40a)  
Для драйвера в TAR.GZ-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122613&clcid=0x40a)  

### <a name="compliance"></a>Соответствие нормативным требованиям

| Изменение соответствий | Сведения |
| :---------------- | :------ |
| Скачайте последние обновления для драйвера JDBC версии 7.4. | &bull; &nbsp; [GitHub, 7.4.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v7.4.1)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Полностью соответствует спецификации API JDBC версии 4.2. | JAR-файлам в пакете 7.4 имена присваиваются с учетом совместимости версий Java.<br/><br/>Например, файл mssql-jdbc-7.4.1.jre11.jar из пакета 7.4 должен использоваться с Java 11. |
| Совместимость с пакетом Java Development Kit (JDK) версий 12.0, 11 и 1.8. | Microsoft JDBC Driver версии 7.4 для SQL Server совместим с пакетом Java Development Kit (JDK) версии 12.0 наряду с JDK версии 11.0 и 1.8. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Выпуски

Номер версии: 7.4.1  
Выпущено: 2 августа 2019 г.  
Исправленные проблемы:  

- Отменены новые реализации API `hashCode()` и `equals()` из `SQLServerDataTable` и `SQLServerDataColumn`, так как изменение API нарушило обратную совместимость

Номер версии: 7.4.0  
Выпущено: 31 июля 2019 г.  

### <a name="support-for-jdk-12"></a>Поддержка JDK 12

Microsoft JDBC Driver версии 7.4 для SQL Server совместим с пакетом Java Development Kit (JDK) версии 12.0 наряду с JDK версии 11.0 и 1.8.

### <a name="introduces-ntlm-authentication"></a>Добавлена проверка подлинности NTLM

| Изменение NTLM | Сведения |
| :--------- | :------ |
| Поддержка режима проверки подлинности NTLM. | Этот режим проверки подлинности позволяет клиентам под управлением Windows и других ОС проходить проверку подлинности в SQL Server с учетными данными пользователей домена Windows. |
| Дополнительные сведения и пример приложения для использования этого режима проверки подлинности. | См. статью [о подключении с использованием проверки подлинности NTLM](using-ntlm-authentication-to-connect-to-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="introduces-querying-parametermetadata-via-_usefmtonly_"></a>Добавлена возможность запросов ParameterMetaData через _useFmtOnly_

| Изменение useFmtOnly | Сведения |
| :---------- | :------ |
| Добавлено свойство подключения **useFmtOnly**. | Эта функция позволяет пользователям при необходимости запрашивать ParameterMetaData через устаревший API `SET FMTONLY ON`. Это полезно в сценариях, где `sp_describe_undeclared_parameters` не работает должным образом. |
| Подробные сведения и ограничения. | См. об [использовании useFmtOnly](using-usefmtonly.md). |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-key-vault-sdk-for-java_-version-121"></a>_Пакет SDK для Microsoft Azure Key Vault для Java_ обновлен до версии 1.2.1.

| Изменение пакета SDK для Key Vault | Сведения |
| :------------------- | :------ |
| Зависимость Maven для _пакета SDK Microsoft Azure Key Vault для Java_ обновлена до версии 1.2.1. | &nbsp; |
| _Пакет SDK для Microsoft Azure Key Vault WebKey_ удален как зависимость Maven. | &nbsp; |
| Дополнительные сведения. | См. дополнительные сведения о [зависимостях компонентов Microsoft JDBC Driver для SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Известные проблемы

| Известные проблемы | Сведения |
| :----------- | :------ |
| При использовании проверки подлинности NTLM. | Одновременное включение расширенной защиты и шифрования соединений в настоящий момент не поддерживается. |
| При использовании useFmtOnly. | С этой функцией имеются некоторые проблемы, причиной которых являются недостатки в логике синтаксического анализа SQL. Дополнительные сведения и возможные обходные решения описаны в статье [об использовании useFmtOnly](using-usefmtonly.md). |
| &nbsp; | &nbsp; |

## <a name="a-id72-722"></a><a id="72"> 7.2.2

**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft JDBC Driver for SQL Server версии 7.2.2 (самораспаковывающийся EXE-файл)](https://go.microsoft.com/fwlink/?linkid=2122435)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 7.2.2 (TAR.GZ)](https://go.microsoft.com/fwlink/?linkid=2122434)**  

Номер версии: 7.2.2  
Выпущено: 16 апреля 2019 г.

Если необходимо загрузить драйвер на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера в самораспаковывающемся EXE-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122435&clcid=0x40a)  
Для драйвера в TAR.GZ-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122434&clcid=0x40a)  

### <a name="compliance"></a>Соответствие нормативным требованиям

| Изменение соответствий | Сведения |
| :---------------- | :------ |
| Скачайте последние обновления для драйвера JDBC версии 7.2. | &bull; &nbsp; [GitHub, 7.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v7.2.2)<br/>&bull; &nbsp; [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver) |
| Полностью соответствует спецификации API JDBC версии 4.2. | JAR-файлам в пакете 7.2 имена присваиваются в соответствии с совместимостью версий Java.<br/><br/>Например, файл mssql-jdbc-7.2.2.jre11.jar из пакета 7.2 должен использоваться с Java 11. |
| Совместимость с пакетом Java Development Kit (JDK) версии 11.0 в дополнение к JDK версии 1.8. | Microsoft JDBC Driver версии 7.2 для SQL Server совместим с пакетом Java Development Kit (JDK) версии 11.0 в дополнение к JDK версии 1.8. |
| &nbsp; | &nbsp; |

### <a name="releases"></a>Выпуски

Номер версии: 7.2.2  
Выпущено: 16 апреля 2019 г.  
Исправленные проблемы:  

- Исправлены проблемы, при которых идентификаторы действий (ActivityID) не очищались должным образом.

Номер версии: 7.2.1  
Выпущено: 11 февраля 2019 г.  
Исправленные проблемы:  

- Исправлены проблемы синтаксического анализа с определенными параметризованными запросами.

Номер версии: 7.2.0  
Выпущено: 31 января 2019 г.  

### <a name="active-directory-_managed-identity_-msi-authentication"></a>Проверка подлинности Azure Active Directory с использованием _управляемого удостоверения_.

| Изменение MSI | Сведения |
| :--------- | :------ |
| Поддерживается режим проверки подлинности с помощью Управляемого удостоверения (MSI) Active Directory. | Этот режим проверки подлинности применяется в ресурсах Azure с поддержкой активной функции Identity.<br/><br/>Оба типа Управляемых удостоверений (MSI) поддерживаются драйвером для получения **accessToken** для установления безопасного соединения. |
| Дополнительные сведения и пример приложения для использования этого режима проверки подлинности. | См. дополнительные сведения об [установке подключения с использованием аутентификации Azure Active Directory](connecting-using-azure-active-directory-authentication.md). |
| &nbsp; | &nbsp; |

### <a name="introduces-_open-service-gateway-initiative_-osgi-support"></a>Представление поддержки _Open Service Gateway Initiative_ (OSGi)

| Изменение OSGi | Сведения |
| :---------- | :------ |
| Добавлена реализация **DataSourceFactory**. | &bull; &nbsp; `org.osgi.service.jdbc.DataSourceFactory`<br/>&bull; &nbsp; `com.microsoft.sqlserver.jdbc.osgi.SQLServerDataSourceFactory` |
| Добавлена реализация **Activator**. | &bull; &nbsp; `org.osgi.framework.BundleActivator`<br/>&bull; &nbsp; `com.microsoft.sqlserver.jdbc.osgi.Activator` |
| &nbsp; | &nbsp; |

### <a name="introduces-_sqlservererror_-apis"></a>Представление API-интерфейсов _SQLServerError_

| Изменение API ошибок | Сведения |
| :--------------- | :------ |
| Представлен API SQLServerError. | Метод получения API для извлечения дополнительных сведений об ошибке, созданной на сервере.<br/><br/>&bull; &nbsp; `SQLServerException.getSQLServerError()`<br/>&bull; &nbsp; `SQLServerError` |
| Дополнительные сведения. | См. дополнительные сведения об [обработке ошибок](handling-errors.md). |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-active-directory-authentication-library-adal4j-for-java_-version-163"></a>Обновлена _Библиотека проверки подлинности Microsoft Azure Active Directory (ADAL4J) для Java_ версии 1.6.3.

| Изменение ADAL4J | Сведения |
| :------------ | :------ |
| Зависимость Maven в ADAL4J обновлена до версии 1.6.3. | &nbsp; |
| _Клиентская среда выполнения Java для AutoRest_ представлена в качестве зависимости Maven версии 1.6.5. | &nbsp; |
| Дополнительные сведения. | См. дополнительные сведения о [зависимостях компонентов Microsoft JDBC Driver для SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="updated-_microsoft-azure-key-vault-sdk-for-java_-version-120"></a>_Пакет SDK для Microsoft Azure Key Vault для Java_ обновлен до версии 1.2.0

| Изменение пакета SDK для Key Vault | Сведения |
| :------------------- | :------ |
| Зависимость Maven _для пакета SDK для Microsoft Azure Key Vault для Java_ обновлена до версии 1.2.0. | &nbsp; |
| Общие сведения о зависимости Maven версии 1.2.0 для пакета _Microsoft Azure SDK для Key Vault WebKey_. | &nbsp; |
| Дополнительные сведения. | См. дополнительные сведения о [зависимостях компонентов Microsoft JDBC Driver для SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md). |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>Известные проблемы

| Известные проблемы | Сведения |
| :----------- | :------ |
| Параметризованные запросы в некоторых случаях. | Чтобы устранить эту проблему, в феврале 2019 года было выпущено обновление версии 7.2.0 (7.2.1). |
| Удаление из ActivityIds. | Чтобы устранить эту проблему, в апреле 2019 года было выпущено обновление версии 7.2.1 (7.2.2). |
| &nbsp; | &nbsp; |

## <a name="70"></a>7.0

**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft JDBC Driver for SQL Server версии 7.0 (самораспаковывающийся EXE-файл)](https://go.microsoft.com/fwlink/?linkid=2122713)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 7.0 (TAR.GZ)](https://go.microsoft.com/fwlink/?linkid=2122614)**  

Номер версии: 7.0.0  
Выпущено: 31 июля 2018 г.

Если необходимо загрузить драйвер на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера в самораспаковывающемся EXE-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122713&clcid=0x40a)  
Для драйвера в TAR.GZ-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122614&clcid=0x40a)  

Microsoft JDBC Driver версии 7.0 для SQL Server полностью соответствует спецификации API JDBC версии 4.2. JAR-файлам в пакете 7.0 имена присваиваются в соответствии с совместимостью версий Java. Например, файл mssql-jdbc-7.0.0.jre10.jar из пакета 7.0 должен использоваться с Java 10.

### <a name="support-for-jdk-10"></a>Поддержка JDK 10

Microsoft JDBC Driver версии 7.0 для SQL Server совместим с пакетом Java Development Kit (JDK) версии 10.0 в дополнение к JDK версии 1.8. Это обновление также предоставляет `Automatic-Module-Name` драйвера в качестве `com.microsoft.sqlserver.jdbc` с помощью файла манифеста.

### <a name="support-for-spatial-datatypes"></a>Поддержка пространственных типов данных

Microsoft JDBC Driver версии 7.0 для SQL Server теперь поддерживает пространственные типы данных, такие как география и геометрия. Дополнительные сведения о пространственных типах данных API-интерфейсов и их использовании см. [здесь](use-spatial-datatypes.md).

### <a name="implementation-for-jdbc-43-introduced-javasqlconnection-apis-beginrequest-and-endrequest"></a>Реализация для JDBC 4.3, в которой представлены API java.sql.Connection beginRequest() и endRequest()

Microsoft JDBC Driver версии 7.0 для SQL Server теперь реализует API `beginRequest()` и `endRequest()` из класса `java.sql.Connection`. Эти API-интерфейсы были представлены в спецификации JDBC 4.3 и JDK 9. Дополнительные сведения о возможности драйвера реализовать эти API-интерфейсы см. в статье[Соответствие требованиям JDBC 4.3 для JDBC Driver](jdbc-4-3-compliance-for-the-jdbc-driver.md).

### <a name="support-for-sql-data-discovery-and-classification"></a>Включение обнаружения и классификации данных SQL

Microsoft JDBC Driver версии 7.0 для SQL Server позволяет включить обнаружение и классификацию данных SQL в любой целевой базе данных, поддерживающей эту функцию. Драйвер теперь предоставляет API-интерфейсы `SQLServerResultSet.getSensitivityClassification()` для извлечения этой информации из полученного `ResultSet`.

Дополнительные сведения см. в статье об [обнаружении и классификации данных SQL с помощью JDBC Driver](data-discovery-classification-sample.md).

### <a name="added-connection-property-usebulkcopyforbatchinsert"></a>Добавлено свойство подключения: useBulkCopyForBatchInsert

Microsoft JDBC Driver версии 7.0 для SQL Server предоставляет новое свойство соединения — `useBulkCopyForBatchInsert`. Это свойство поддерживается только Azure Synapse Analytics.

По умолчанию это свойство отключено. Вы можете включить его, чтобы повысить производительность пользовательских приложений при отправке больших объемов данных в Azure Synapse Analytics. Включение этого свойства изменяет поведение операций пакетной вставки на переход на операции массового копирования с предоставленными пользователем данными. Дополнительные сведения об этом свойстве и его ограничениях см. в статье [Использование API массового копирования для операции пакетной вставки](use-bulk-copy-api-batch-insert-operation.md).

### <a name="added-connection-property-cancelquerytimeout"></a>Добавлено свойство подключения: cancelQueryTimeout

Microsoft JDBC Driver версии 7.0 для SQL Server предоставляет новое свойство подключения, `cancelQueryTimeout`, для отключения `queryTimeout` на объектах `java.sql.Connection` и `java.sql.Statement`.

### <a name="added-azure-key-vault-provider-constructors"></a>Добавлены конструкторы поставщика Azure Key Vault

Microsoft JDBC Driver версии 7.0 для SQL Server возвращает ранее удаленный конструктор для `SQLServerColumnEncryptionAzureKeyVaultProvider`. Это дало возможность при выполнении проверки подлинности с помощью пользовательского метода, реализованного через `SQLServerKeyVaultAuthenticationCallback`, получить токен доступа.

Новые конструкторы имеют следующее определение.

```java
/* This constructor is added to provide backward compatibility with 6.0
* version of the driver. It is marked deprecated for removal in the next
* stable release.
*/
@Deprecated
public SQLServerColumnEncryptionAzureKeyVaultProvider(
        SQLServerKeyVaultAuthenticationCallback authenticationCallback,
        ExecutorService executorService) throws SQLServerException;

/*New constructor to replace the above constructor*/
public SQLServerColumnEncryptionAzureKeyVaultProvider(
            SQLServerKeyVaultAuthenticationCallback authenticationCallback) throws SQLServerException;
```

### <a name="updated-microsoft-azure-active-directory-authentication-library-adal4j-for-java-version-160"></a>Обновлена "Библиотека проверки подлинности Microsoft Azure Active Directory (ADAL4J) для Java" версии 1.6.0

Microsoft JDBC Driver версии 7.0 для SQL Server обновил зависимость Maven "Библиотека проверки подлинности Microsoft Azure Active Directory (ADAL4J) для Java" до версии 1.6.0. Дополнительные сведения см. в статье [Зависимости компонентов Microsoft JDBC Driver для SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md).

## <a name="64"></a>6.4

**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft JDBC Driver for SQL Server версии 6.4 (самораспаковывающийся EXE-файл)](https://go.microsoft.com/fwlink/?linkid=2122436)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 6.4 (TAR.GZ)](https://go.microsoft.com/fwlink/?linkid=2122537)**  

Номер версии: 6.4.0  
Выпущено: 27 февраля 2018 г

Если необходимо загрузить драйвер на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера в самораспаковывающемся EXE-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122436&clcid=0x40a)  
Для драйвера в TAR.GZ-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122537&clcid=0x40a)  

Microsoft JDBC Driver версии 6.4 для SQL Server полностью соответствует спецификации JDBC версий 4.1 и 4.2. JAR-файлам в пакете 6.4 имена присваиваются в соответствии с совместимостью версий Java. Например, файл mssql-jdbc-6.4.0.jre8.jar из пакета 6.4 должен использоваться с Java 8.

### <a name="support-for-jdk-9"></a>Поддержка JDK 9

Драйвер поддерживает JDK версии 9.0 в дополнение к версиям 8.0 и 7.0.

### <a name="jdbc-43-compliance"></a>Соответствие требованиям JDBC 4.3

Драйвер поддерживает спецификацию Java Database Connectivity API 4.3 в дополнение к версиям 4.1 и 4.2. Методы API JDBC 4.3 добавлены, но еще не реализованы. Дополнительные сведения см. в статье [Соответствие требованиям JDBC 4.3 для JDBC Driver](jdbc-4-3-compliance-for-the-jdbc-driver.md).

### <a name="added-connection-property-sslprotocol"></a>Добавлено свойство подключения: sslProtocol

Новое свойство подключения дает пользователям возможность указывать ключевое слово протокола TLS. Возможны следующие значения: "TLS", "TLSv1", "TLSv1.1" и "TLSv1.2". Дополнительные сведения см. на странице [SSLProtocol](https://github.com/Microsoft/mssql-jdbc/wiki/SSLProtocol).

### <a name="deprecated-connection-property-fipsprovider"></a>Устаревшее свойство подключения: fipsProvider

Свойство подключения `fipsProvider` удалено из списка принятых свойств подключения. Дополнительные сведения см. на странице [Запрос на вытягивание GitHub](https://github.com/Microsoft/mssql-jdbc/pull/460).

### <a name="added-connection-properties-for-specifying-a-custom-trustmanager"></a>Добавлены свойства подключения для указания пользовательского TrustManager

Теперь драйвер поддерживает указание пользовательского TrustManager с добавленными свойствами подключения `trustManagerClass` и `trustManagerConstructorArg`. Вы можете динамически указывать набор сертификатов, которые проверяются при каждом подключении, без изменения глобальных параметров среды виртуальной машины Java (JVM).

### <a name="added-support-for-datetimesmalldatetime-in-table-valued-parameters"></a>В параметры, возвращающие табличные значения, добавлена поддержка datetime или smallDatetime

Теперь при использовании параметров, возвращающих табличные значения, драйвер поддерживает типы данных `datetime` и `smallDatetime`.

### <a name="added-support-for-the-sql_variant-datatype"></a>Добавлена поддержка типа данных sql_variant

Теперь для использования с SQL Server драйвер JDBC поддерживает типы данных `sql_variant`. Тип данных `sql_variant` также поддерживается такими функциями, как параметры табличных значений и массовое копирование, со следующими ограничениями.

* **Для значений даты**:

  Когда вы используете параметр табличного значения для заполнения таблицы, которая содержит значения `datetime`, `smalldatetime` или `date`, хранящиеся в столбце `sql_variant`, вызывается метод `getDateTime()`, `getSmallDateTime()` или `getDate()`, когда результирующий набор не работает и выдает следующее исключение.

  `java java.lang.String cannot be cast to java.sql.Timestamp`

  Чтобы этого избежать, используйте метод `getString()` или `getObject()`.

* **Использование возвращающего табличное значение параметра с sql_variant для значений null**.
  
  При использовании параметра табличного значения для заполнения таблицы и отправки значения NULL в столбец типа `sql_variant` будет возникать исключение. Вставка значения NULL в столбец типа `sql_variant` в параметре табличного значения в настоящее время не поддерживается.

### <a name="implemented-prepared-statement-metadata-caching"></a>Реализовано кэширование метаданных подготовленной инструкции

В драйвере JDBC реализовано кэширование метаданных подготовленной инструкции для повышения производительности. Теперь драйвер поддерживает кэширование метаданных подготовленной инструкции в драйвере с использованием свойств подключения `disableStatementPooling` и `statementPoolingCacheSize`. Эта функция по умолчанию отключена. Дополнительные сведения см. в разделе [Кэширование метаданных подготовленной инструкции для JDBC Driver](prepared-statement-metadata-caching-for-the-jdbc-driver.md).

### <a name="added-support-for-azure-ad-integrated-authentication-on-linuxmacos"></a>Добавлена ​​поддержка интегрированной проверки подлинности Azure AD в Linux и macOS

Драйвер JDBC поддерживает встроенную проверку подлинности Azure Active Directory во всех поддерживаемых операционных системах (Windows, Linux, macOS) с Kerberos. Кроме того, в операционных системах Windows пользователи могут проходить проверку подлинности с помощью mssql-jdbc_auth-\<version>-\<arch>.dll.

### <a name="updated-microsoft-azure-active-directory-authentication-library-adal4j-for-java-version-140"></a>Обновлена "Библиотека проверки подлинности Microsoft Azure Active Directory (ADAL4J) для Java" версии 1.4.0

В драйвере JDBC зависимость Maven "Библиотека проверки подлинности Microsoft Azure Active Directory (ADAL4J) для Java" обновлена до версии 1.4.0. Дополнительные сведения см. в статье [Зависимости компонентов Microsoft JDBC Driver для SQL Server](feature-dependencies-of-microsoft-jdbc-driver-for-sql-server.md).

## <a name="62"></a>6.2

**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft JDBC Driver for SQL Server версии 6.2 (самораспаковывающийся EXE-файл)](https://go.microsoft.com/fwlink/?linkid=2122616)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 6.2 (TAR.GZ)](https://go.microsoft.com/fwlink/?linkid=2122615)**  

Номер версии: 6.2.2  
Выпущено: 29 сентября 2017 г.

Если необходимо загрузить драйвер на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера в самораспаковывающемся EXE-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122616&clcid=0x40a)  
Для драйвера в TAR.GZ-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122615&clcid=0x40a)  

Microsoft JDBC Driver версии 6.2 для SQL Server полностью соответствует спецификации JDBC версий 4.1 и 4.2. JAR-файлам в пакете 6.2 имена присваиваются в соответствии с совместимостью версий Java. Например, файл mssql-jdbc-6.2.2.jre8.jar из пакета 6.2 должен использоваться с Java 8.

### <a name="releases"></a>Выпуски

Номер версии: 6.2.2  
Выпущено: 3 октября 2017 г.  
Исправленные проблемы:  

- Изменена зависимость ADAL4J до версии 1.2.0 и зависимости Azure Key Vault до версии 1.0.0

Номер версии: 6.2.1  
Выпущено: 14 июля 2017 г.  
Исправленные проблемы:  

- Исправлена проблема при выполнении запросов без параметров с помощью `preparedStatement`

Номер версии: 6.2.0  
Выпущено: 30 июня 2017 г.  

> [!NOTE]  
> Проблема с улучшением кэширования метаданных была обнаружена в RTW JDBC 6.2, выпущенном 29 июня 2017 года. Выполнен откат улучшений, а новые JAR-файлы (версии 6.2.1) выпущены 17 июля 2017 года.
>
> С помощью еще одного улучшения версия зависимой библиотеки Azure Key Vault обновлена до версии 1.0.0, а новые файлы JAR-файлы (версии 6.2.2) были выпущены 19 октября 2017 года.
>
> Скачайте последние обновления для JDBC Driver 6.2 с помощью приведенных выше ссылок, [GitHub](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.2) или [Maven Central](https://search.maven.org/search?q=g:com.microsoft.sqlserver). Для использования JAR-файлов выпуска 6.2.2 обновите проекты. Дополнительные сведения см. в заметках о выпуске [6.2.1](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.1) и [6.2.2](https://github.com/Microsoft/mssql-jdbc/releases/tag/v6.2.2).

### <a name="azure-ad-support-for-linux"></a>Поддержка Azure AD для Linux

Подключите свои приложения Linux к базе данных SQL Azure, используя проверку подлинности Azure AD через имя пользователя и пароль, а также методы доступа к токену.

### <a name="fips-enabled-jvms"></a>Виртуальные машины Java с поддержкой стандарта FIPS

Теперь драйвер JDBC можно использовать на виртуальных машинах Java, работающих в режиме использования федерального стандарта обработки информации (FIPS) 140 в соответствии с федеральными стандартами.

### <a name="kerberos-authentication-improvements"></a>Улучшения проверки подлинности Kerberos

Теперь драйвер JDBC поддерживает следующее.

* Метод субъект/пароль для приложений, в которых конфигурация Kerberos не может быть изменена или не может получить новый токен или таблицу ключей. Этот метод можно использовать для проверки подлинности на экземпляре SQL Server, который разрешает только проверку подлинности Kerberos.
* Проверка подлинности между областями с использованием встроенной проверки подлинности Kerberos без явного указания имени участника-службы сервера. Теперь драйвер автоматически вычисляет области, даже если они не указаны.
* Ограниченное делегирование Kerberos путем принятия учетных данных олицетворенного пользователя в качестве объекта учетных данных GSS через источник данных. Эти олицетворенные учетные данные затем используются для установления подключения Kerberos.

### <a name="added-timeouts"></a>Добавлено время ожидания

Теперь драйвер JDBC поддерживает следующие настраиваемые значения времени ожидания. Вы можете изменить их в зависимости от потребностей своего приложения.

* Время ожидания запроса для контроля количества секунд ожидания до истечения времени ожидания при выполнении запроса.
* Время ожидания сокета для указания количества миллисекунд ожидания до истечения времени ожидания чтения или принятия сокета.

## <a name="61"></a>6.1

Номер версии: 6.1.0  
Выпущено: 17 ноября 2016 г.  

Microsoft JDBC Driver версии 6.1 для SQL Server полностью соответствует спецификации JDBC версий 4.1 и 4.2. Это первоначальный выпуск драйвера JDBC с открытым исходным кодом. Исходный код можно найти по [тегу GitHub v6.1.0](https://github.com/microsoft/mssql-jdbc/releases/tag/v6.1.0). Он компилирует файлы mssql-jdbc-6.1.0.jre8.jar и mssql-jdbc-6.1.0.jre7.jar, которые соответствуют совместимости версий Java.

## <a name="60"></a>6,0

**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft JDBC Driver for SQL Server версии 6.0 (самораспаковывающийся EXE-файл)](https://go.microsoft.com/fwlink/?linkid=2122617)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 6.0 (TAR.GZ)](https://go.microsoft.com/fwlink/?linkid=2122714)**  

Номер версии: 6.0.8112  
Выпущено: 17 января 2017 г.

Если необходимо загрузить драйвер на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера в самораспаковывающемся EXE-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122617&clcid=0x40a)  
Для драйвера в TAR.GZ-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122714&clcid=0x40a)  

Microsoft JDBC Driver версии 6.0 для SQL Server полностью соответствует спецификации JDBC версий 4.1 и 4.2. JAR-файлам в пакете 6.0 имена присваиваются в соответствии с их совместимостью с версией API JDBC. Например, файл sqljdbc42.jar из пакета 6.0 совместим с API JDBC 4.2. Аналогичным образом файл sqljdbc41.jar совместим с API JDBC 4.1.

Чтобы убедиться, что у вас есть правильный файл sqljdbc42.jar или sqljdbc41.jar, запустите следующие строки кода. Если выводится сообщение: "Driver version: 6.0.7507.100", значит у вас есть пакет драйвера JDBC 6.0.

```java
Connection conn = DriverManager.getConnection("jdbc:sqlserver://<server>;user=<user>;password=<password>;");
System.out.println("Driver version: " + conn.getMetaData().getDriverVersion());
```

### <a name="always-encrypted"></a>Always Encrypted

Драйвер поддерживает функцию постоянного шифрования в SQL Server 2016. Эта функция гарантирует, что конфиденциальные данные никогда не будут отображаться в виде открытого текста в экземпляре SQL Server. В основе работы Always Encrypted лежит прозрачное шифрование данных в приложении, чтобы SQL Server обрабатывал только зашифрованные данные, а не значения открытого текста. Даже в случае компрометации экземпляра SQL или хост-компьютера злоумышленник получит только зашифрованный текст конфиденциальных данных. Дополнительные сведения см. в статье [Использование функции Always Encrypted с драйвером JDBC](using-always-encrypted-with-the-jdbc-driver.md).

### <a name="internationalized-domain-names"></a>Международное доменное имя (IDN)

Драйвер поддерживает международные доменные имена (IDN) в качестве имен серверов. Дополнительные сведения см. в разделе "Использование международных доменных имен" в статье [Функции поддержки различных языков драйвера JDBC](international-features-of-the-jdbc-driver.md).

### <a name="parameterized-queries"></a>Параметризованные запросы

Драйвер теперь поддерживает получение метаданных параметра с подготовленными инструкциями для сложных запросов, например подзапросов или соединений. Обратите внимание, что это улучшение доступно только при использовании SQL Server 2012 и более новых версий.

### <a name="azure-active-directory"></a>Azure Active Directory

Проверка подлинности Active Directory — это механизм подключения к базе данных Azure SQL версии 12 с помощью удостоверений в Azure AD. Проверка подлинности AAD используется для централизованного управления удостоверениями пользователей базы данных и в качестве альтернативы проверке подлинности SQL Server.

Вы можете использовать драйвер JDBC 6.0, чтобы указать учетные данные AAD в строке подключения JDBC для подключения к базе данных SQL Azure. Дополнительные сведения см. в разделе о свойстве проверки подлинности в статье [Задание свойств соединения](setting-the-connection-properties.md).

### <a name="table-valued-parameters"></a>Возвращающие табличные значения параметры

Параметры, возвращающие табличное значение, упрощают маршалинг нескольких строк данных из клиентского приложения в SQL Server, устраняя потребность в нескольких круговых путях или специальной серверной логике для обработки данных. Параметры, возвращающие табличное значение, можно использовать для инкапсуляции строк данных в клиентском приложении и их отправки на сервер единой параметризованной командой. Входящие строки данных хранятся в переменной таблицы, которой затем можно управлять с помощью Transact-SQL. Дополнительные сведения см. в разделе [Использование параметров, возвращающих табличные значения](using-table-valued-parameters.md).

### <a name="always-on-availability-groups"></a>Группы доступности AlwaysOn

Теперь драйвер поддерживает прозрачное подключение к группам доступности Always On. Драйвер быстро обнаруживает текущую топологию групп доступности AlwaysOn серверной инфраструктуры и прозрачно подключается к текущему активному серверу.

## <a name="42"></a>4.2

**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft JDBC Driver for SQL Server версии 4.2 (самораспаковывающийся EXE-файл)](https://go.microsoft.com/fwlink/?linkid=2122538)**  
**[![Скачать](../../ssms/media/download-icon.png) Скачать Microsoft ODBC Driver for SQL Server версии 4.2 (TAR.GZ)](https://go.microsoft.com/fwlink/?linkid=2122437)**  

Номер версии: 4.2.8112  
Выпущено: 24 августа 2015 г.

Если необходимо загрузить драйвер на языке, отличном от обнаруженного, можно использовать эти прямые ссылки.  
Для драйвера в самораспаковывающемся EXE-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122538&clcid=0x40a)  
Для драйвера в TAR.GZ-файле: [Китайский (упрощенное письмо)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x804) | [Китайский (традиционное письмо)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x404) | [Английский (США)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x409) | [Французский](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x40c) | [Немецкий](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x407) | [Итальянский](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x410) | [Японский](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x411) | [Корейский](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x412) | [Португальский (Бразилия)](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x416) | [Русский](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x419) | [Испанский](https://go.microsoft.com/fwlink/?linkid=2122437&clcid=0x40a)  

Microsoft JDBC Driver версии 4.2 для SQL Server полностью соответствует спецификации JDBC версий 4.1 и 4.2. JAR-файлам в пакете 4.2 имена присваиваются в соответствии с их совместимостью с версией API JDBC. Например, файл sqljdbc42.jar из пакета 4.2 совместим с API JDBC 4.2. Аналогичным образом файл sqljdbc41.jar совместим с API JDBC 4.1.

Чтобы убедиться, что у вас есть правильный файл sqljdbc42.jar или sqljdbc41.jar, запустите следующие строки кода. Если выводится сообщение: "Driver version: 4.2.6420.100", значит у вас есть пакет драйвера JDBC 4.2.

```java
Connection conn = DriverManager.getConnection("jdbc:sqlserver://<server>;user=<user>;password=<password>;");
System.out.println("Driver version: " + conn.getMetaData().getDriverVersion());
```

### <a name="support-for-jdk-8"></a>Поддержка JDK 8

Драйвер поддерживает JDK версии 8.0 в дополнение к версиям 7.0, 6.0 и 5.0.

### <a name="jdbc-41-and-42-compliance"></a>Соответствие JDBC 4.1 и 4.2

Драйвер поддерживает спецификации Java Database Connectivity API 4.1 и 4.2 в дополнение к версии 4.0. Дополнительные сведения см. в статьях [Соответствие JDBC 4.1 для JDBC Driver](jdbc-4-1-compliance-for-the-jdbc-driver.md) и [Соответствие JDBC 4.2 для JDBC Driver](jdbc-4-2-compliance-for-the-jdbc-driver.md).

### <a name="bulk-copy"></a>Массовое копирование

Функция массового копирования используется для быстрого копирования больших объемов данных в таблицы или представления в базах данных SQL Server. Дополнительные сведения см. в статье [Использование массового копирования с драйвером JDBC](using-bulk-copy-with-the-jdbc-driver.md).

### <a name="xa-transaction-rollback-option"></a>Возможность отката транзакций XA

Добавлены новые параметры времени ожидания для существующего автоматического отката неподготовленных транзакций. Дополнительные сведения см. в статье [Основные сведения о транзакциях XA](understanding-xa-transactions.md).

### <a name="new-kerberos-principal-connection-property"></a>Новое свойство соединения участника Kerberos

Драйвер использует новое свойство соединения для улучшения гибкости подключений Kerberos. Дополнительные сведения см. в статье [Использование встроенной проверки подлинности Kerberos для соединения с SQL Server](using-kerberos-integrated-authentication-to-connect-to-sql-server.md).

## <a name="41"></a>4.1

Номер версии: 4.1.8112  
Выпущено: 12 декабря 2014 г.

### <a name="support-for-jdk-7"></a>Поддержка JDK 7

Драйвер поддерживает JDK версии 7.0 в дополнение к версиям 6.0 и 5.0.

## <a name="itanium-not-supported-for-jdbc-driver-applications"></a>Процессор Itanium не поддерживается для приложений JDBC Driver

Приложения Microsoft JDBC Driver for SQL Server не поддерживают выполнение на компьютерах с процессором Itanium.

## <a name="see-also"></a>См. также раздел

[Общие сведения о драйвере JDBC](overview-of-the-jdbc-driver.md)

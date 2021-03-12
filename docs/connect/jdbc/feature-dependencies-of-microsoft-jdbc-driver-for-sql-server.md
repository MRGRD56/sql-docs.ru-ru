---
title: Зависимости компонентов Microsoft JDBC Driver
description: Сведения о зависимостях драйвера Microsoft JDBC Driver for SQL Server и о том, как учитывать их.
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 939a8773-2583-49a4-bf00-6b892fbe39dc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 61542eafadc26a96c01204472c6fd78997787f15
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836961"
---
# <a name="feature-dependencies-of-the-microsoft-jdbc-driver-for-sql-server"></a>Зависимости компонентов Microsoft JDBC Driver для SQL Server

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

В этой статье перечислены библиотеки, от которых зависит Microsoft JDBC Driver для SQL Server. Проект включает в себя следующие зависимости.

## <a name="compile-time"></a>Время компиляции

 - `com.microsoft.azure:azure-keyvault` : поставщик Azure Key Vault для функции Always Encrypted Azure Key Vault. (необязательно).
 - `com.microsoft.azure:azure-identity` : библиотека Azure Active Directory для Java для функции проверки подлинности Azure Active Directory и функции Azure Key Vault. (необязательно).
 - `org.antlr:antlr4-runtime`: среда выполнения ANTLR 4 для функции "useFmtOnly". (необязательно).
 - `org.osgi:org.osgi.core`: основная библиотека OSGi для поддержки платформы OSGi.
 - `org.osgi:org.osgi.compendium`: библиотека-справочник OSGi для поддержки платформы OSGi.
 - `com.google.code.gson`: средство синтаксического анализа JSON для функции Always Encrypted с безопасными анклавами. (необязательно).
 - `org.bouncycastle.bcprov-jdk15on`: поставщик Bouncy Castle для функции Always Encrypted с безопасными анклавами (только при использовании JAVA 8). (необязательно).

## <a name="test-time"></a>Время тестирования

В конкретных проектах, для которых требуется какая-либо из предыдущих функций, необходимо явно объявить соответствующие зависимости в файле POM.

**Например:** при использовании функции проверки подлинности Azure Active Directory, зависимость `adal4j` необходимо повторно объявить в файл POM проекта. См. следующий фрагмент кода.

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>9.2.1.jre11</version>
    <scope>compile</scope>
</dependency>

<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.6.5</version>
</dependency>

<dependency>
    <groupId>com.microsoft.rest</groupId>
    <artifactId>client-runtime</artifactId>
    <version>1.7.4</version>
</dependency>
```

**Например:** при использовании функции Azure Key Vault, необходимо повторно объявить зависимости `azure-keyvault` и `adal4j` в файл POM проекта. См. следующий фрагмент кода.

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>9.2.1.jre11</version>
    <scope>compile</scope>
</dependency>

<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>adal4j</artifactId>
    <version>1.6.5</version>
</dependency>

<dependency>
    <groupId>com.microsoft.rest</groupId>
    <artifactId>client-runtime</artifactId>
    <version>1.7.4</version>
</dependency>

<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-keyvault</artifactId>
    <version>1.2.4</version>
</dependency>
```

## <a name="dependency-requirements-for-the-jdbc-driver"></a>Требования к зависимостям для JDBC Driver

### <a name="working-with-the-azure-key-vault-provider"></a>Использование поставщика Azure Key Vault:

- Драйвер JDBC версии 9.2.1. Версии зависимостей: Azure-keyvault (версия 4.2.1), Azure-identity (версия 1.1.3) и их зависимости ([пример приложения](azure-key-vault-sample-version-9.2.md)).
- Драйвер JDBC версии 8.4.1. Версии зависимостей: Azure-Keyvault (версия 1.2.4), Adal4j (версия 1.6.5), Client-Runtime-for-AutoRest (1.7.4) и их зависимости ([пример приложения](azure-key-vault-sample-version-7.0.md)).
- Драйвер JDBC версии 8.2.2. Версии зависимостей: Azure-Keyvault (версия 1.2.2), Adal4j (версия 1.6.4), Client-Runtime-for-AutoRest (1.7.0) и их зависимости ([пример приложения](azure-key-vault-sample-version-7.0.md)).
- JDBC driver версии 7.4.1. Версии зависимостей: Azure-Keyvault (версия 1.2.1), Adal4j (версия 1.6.4), Client-Runtime-for-AutoRest (1.6.10) и их зависимости ([пример приложения](azure-key-vault-sample-version-7.0.md)).
- JDBC driver версии 7.2.2. Версии зависимостей: Azure-Keyvault (версия 1.2.0), Azure-Keyvault-Webkey (версия 1.2.0), Adal4j (версия 1.6.3), Client-Runtime-for-AutoRest (1.6.5) и их зависимости ([пример приложения](azure-key-vault-sample-version-7.0.md)).
- JDBC driver версии 7.0.0. Версии зависимостей: Azure-Keyvault (версия 1.0.0), Adal4j (версия 1.6.0) и их зависимости ([пример приложения](azure-key-vault-sample-version-7.0.md)).
- JDBC driver версии 6.4.0. Версии зависимостей: Azure-Keyvault (версия 1.0.0), Adal4j (версия 1.4.0) и их зависимости ([пример приложения](azure-key-vault-sample-version-6.2.2.md)).
- JDBC driver версии 6.2.2. Версии зависимостей: Azure-Keyvault (версия 1.0.0), Adal4j (версия 1.4.0) и их зависимости ([пример приложения](azure-key-vault-sample-version-6.2.2.md)).
- JDBC driver версии 6.0.0. Версии зависимостей: Azure-Keyvault (версия 0.9.7), Adal4j (версия 1.3.0) и их зависимости ([пример приложения](azure-key-vault-sample-version-6.0.0.md)).

> [!NOTE]
> В версиях драйвера 6.2.2 и 6.4.0 зависимость azure-keyvault-java была обновлена ​​до версии 1.0.0. Однако новая версия была несовместима с предыдущей версией (0.9.7) и ломает существующую реализацию в драйвере. Новая реализация в драйвере требовала изменений API, что, в свою очередь, нарушало работу клиентских программ, использующих поставщик Azure Key Vault.
>
> Эта проблема решена с помощью последней версии драйвера (версия 7.0.0 и выше). Удаленный конструктор, который использовал механизм обратного вызова для проверки подлинности, добавлен обратно в поставщик Azure Key Vault для обратной совместимости.

### <a name="working-with-azure-active-directory-authentication"></a>Использование проверки подлинности Azure Active Directory:

- Драйвер JDBC версии 9.2.1. Версии зависимостей: Azure-identity (версия 1.1.3) и его зависимости.
- Драйвер JDBC версии 8.4.1. Версии зависимостей: Adal4j (версия 1.6.5), Client-Runtime-for-AutoRest (1.7.4) и их зависимости.
- Драйвер JDBC версии 8.2.2. Версии зависимостей: Adal4j (версия 1.6.4), Client-Runtime-for-AutoRest (1.7.0) и их зависимости. В этой версии драйвера файл sqljdbc_auth.dll был переименован в mssql-jdbc_auth-\<version>-\<arch>.dll.
- JDBC driver версии 7.4.1. Версии зависимостей: Adal4j (версия 1.6.4), Client-Runtime-for-AutoRest (1.6.10) и их зависимости.
- JDBC driver версии 7.2.2. Версии зависимостей: Adal4j (версия 1.6.3), Client-Runtime-for-AutoRest (1.6.5) и их зависимости.
- JDBC driver версии 7.0.0. Версии зависимостей: Adal4j (версия 1.6.0) и его зависимости.
- JDBC driver версии 6.4.0. Версии зависимостей: Adal4j (версия 1.4.0) и его зависимости.
- JDBC driver версии 6.2.2. Версии зависимостей: Adal4j (версия 1.4.0) и его зависимости.
- JDBC driver версии 6.0.0. Версии зависимостей: Adal4j (версия 1.3.0) и его зависимости. В этой версии драйвера вы можете подключиться с помощью режима проверки подлинности _ActiveDirectoryIntegrated_ только в операционной системе Windows с помощью sqljdbc_auth.dll и библиотеки проверки подлинности Active Directory для SQL Server (ADALSQL.DLL).

Начиная с версии драйвера 6.4.0, использование ADALSQL.DLL в операционной системе Windows для приложений не обязательно. Для *операционных систем, отличных от Windows*, для работы с проверкой подлинности ActiveDirectoryIntegrated драйверу требуется билет Kerberos. Дополнительные сведения о подключении к Active Directory с помощью Kerberos см. в разделе [Задать билет Kerberos в Windows, Linux и macOS](connecting-using-azure-active-directory-authentication.md#set-kerberos-ticket-on-windows-linux-and-macos).

Для *операционной системы Windows* драйвер по умолчанию ищет файл sqljdbc_auth.dll и не требует установки билетов Kerberos или зависимостей библиотеки Azure. Если файл sqljdbc_auth.dll недоступен, драйвер ищет билет Kerberos для проверки подлинности в Active Directory, как и в других операционных системах.

Начиная с версии драйвера 8.2.2 и далее файл sqljdbc_auth.dll переименован в mssql-jdbc_auth-\<version>-\<arch>.dll. Пример: 'mssql-jdbc_auth-8.2.2.x64.dll'.

Для встроенной проверки подлинности Active Directory помимо библиотеки **mssql-jdbc_auth-\<version>-\<arch>.dll** (доступна в пакете драйверов JDBC) нужно также установить Библиотеку проверки подлинности Azure Active Directory (**ADAL.DLL**). Библиотеку ADAL можно установить через [Microsoft ODBC Driver for SQL Server](../odbc/download-odbc-driver-for-sql-server.md) или [Microsoft OLE DB Driver for SQL Server](../oledb/download-oledb-driver-for-sql-server.md). Для ADAL.DLL драйвер JDBC поддерживает только версию **1.0.2028.318 и выше**.


Вы можете получить [образец приложения](connecting-using-azure-active-directory-authentication.md), использующего эту функцию.

## <a name="see-also"></a>См. также раздел

[Репозиторий GitHub по JDBC Driver](https://github.com/microsoft/mssql-jdbc)  
[Справочник интерфейса драйвера JDBC](reference/jdbc-driver-api-reference.md)

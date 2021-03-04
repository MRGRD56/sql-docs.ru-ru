---
description: Использование Always Encrypted с безопасными анклавами с драйвером JDBC
title: Использование Always Encrypted с безопасными анклавами с драйвером JDBC | Документация Майкрософт
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 271c0438-8af1-45e5-b96a-4b1cabe32707
author: reneye
ms.author: v-reye
ms.openlocfilehash: 46e812a4234098e49a272be503e52f34d4c78733
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837541"
---
# <a name="using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver"></a>Использование Always Encrypted с безопасными анклавами с драйвером JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Эта страница содержит сведения о том, как разрабатывать приложения Java с использованием [Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/always-encrypted-enclaves.md) и Microsoft JDBC Driver 8.2 (или более поздней версии) для SQL Server.

Безопасные анклавы являются дополнением к существующей функции [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Их назначением является устранение ограничений при работе с данными Always Encrypted. Ранее пользователи могли выполнять применительно к данным Always Encrypted только сравнение на равенство. Для выполнения других операций им приходилось извлекать и расшифровывать данные. Безопасные анклавы устраняют это ограничение, позволяя выполнять вычисления с данными в виде обычного текста внутри безопасного анклава на стороне сервера. Безопасный анклав — это защищенная область памяти в процессе SQL Server. Он также выступает в качестве доверенной среды выполнения для обработки конфиденциальных данных в ядре SQL Server. Безопасный анклав представляет собой "черный ящик" на фоне остальной части SQL Server и других процессов на главном компьютере. Просмотреть данные или код внутри анклава невозможно, даже с помощью отладчика.

## <a name="prerequisites"></a>Предварительные требования
- Убедитесь в том, что на компьютере разработки установлен драйвер Microsoft JDBC Driver 8.2 (или более поздней версии) для SQL Server.
- Убедитесь, что зависимости среды, такие как библиотеки DLL, хранилища ключей и т. д., расположены в соответствующих путях. Always Encrypted с безопасными анклавами — это надстройка для существующего компонента [Always Encrypted](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md) с аналогичными предварительными требованиями.

> [!Note]
> Если вы используете более старую версию JDK 8, может потребоваться скачать и установить файлы политики юрисдикции неограниченной стойкости Java Cryptography Extension (JCE). Обязательно прочитайте содержащийся в ZIP-архиве файл сведений, чтобы получить инструкции по установке и сопутствующие сведения о возможных проблемах экспорта и импорта.  
>
> Файлы политики можно скачать на [странице скачивания файлов политики юрисдикции неограниченной стойкости Java Cryptography Extension (JCE) 8](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html).

## <a name="setting-up-secure-enclaves"></a>Настройка безопасных анклавов
Чтобы начать работу с безопасными анклавами, см. статью [Руководство. Начало работы с Always Encrypted и безопасными анклавами в SQL Server](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) или [Учебник. Начало работы с Always Encrypted и безопасными анклавами в Базе данных SQL Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started). Более подробные сведения см. в статье [Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="connection-string-properties"></a>Свойства строки подключения

Чтобы включить вычисления анклава для подключения к базе данных, необходимо не только включить Always Encrypted, но и задать следующие ключевые слова строки подключения.

- **enclaveAttestationProtocol** — определяет протокол аттестации. 
  - Если вы используете [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] и службу защиты узла (HGS), значение этого ключевого слова должно быть `HGS`.
  - Если вы используете [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] и Аттестацию Microsoft Azure, значение этого ключевого слова должно быть `AAS`.

- **enclaveAttestationUrl:**  — определяет URL-адрес аттестации (конечная точка службы аттестации). Необходимо получить URL-адрес аттестации для имеющейся среды у администратора службы аттестации.
  - Если вы используете [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] и службу защитника узлов (HGS), см. сведения в разделе об [определении и совместном использовании URL-адреса аттестации HGS](../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Если вы используете [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] и Аттестацию Microsoft Azure, см. сведения об [определении URL-адреса аттестации для политики аттестации](../../relational-databases/security/encryption/always-encrypted-enclaves.md?view=sql-server-ver15#secure-enclave-attestation).

Чтобы включить Always Encrypted с безопасными анклавами из [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], пользователям необходимо включить параметр **columnEncryptionSetting** и правильно задать **оба** приведенных выше свойства строки подключения.

## <a name="working-with-secure-enclaves"></a>Работа с безопасными анклавами
Если свойства подключения к анклавам заданы правильно, функция будет работать прозрачно. Драйвер будет определять, должен ли запрос автоматически использовать безопасный анклав. Ниже приведены примеры запросов, активирующих вычисления анклава. Параметры базы данных и таблицы можно найти в статье [Руководство. Начало работы с Always Encrypted и безопасными анклавами в SQL Server](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) или [Учебник. Начало работы с Always Encrypted и безопасными анклавами в Базе данных SQL Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started).


Полнофункциональные запросы инициируют вычисления анклава:
```java
private static final String URL = "jdbc:sqlserver://<server>:<port>;user=<username>;password=<password>;databaseName=ContosoHR;columnEncryptionSetting=enabled;enclaveAttestationUrl=<attestation-url>;enclaveAttestationProtocol=<attestation-protocol>;";
try (Connection c = DriverManager.getConnection(URL)) {
    try (PreparedStatement p = c.prepareStatement("SELECT * FROM Employees WHERE SSN LIKE ?")) {
        p.setString(1, "%6818");
        try (ResultSet rs = p.executeQuery()) {
            while (rs.next()) {
                // Do work with data
            }
        }
    }
    
    try (PreparedStatement p = c.prepareStatement("SELECT * FROM Employees WHERE SALARY > ?")) {
        ((SQLServerPreparedStatement) p).setMoney(1, new BigDecimal(0));
        try (ResultSet rs = p.executeQuery()) {
            while (rs.next()) {
                // Do work with data
            }
        }
    }
}
```

Включение шифрования для столбца также инициирует вычисления анклава:
```java
private static final String URL = "jdbc:sqlserver://<server>:<port>;user=<username>;password=<password>;databaseName=ContosoHR;columnEncryptionSetting=enabled;enclaveAttestationUrl=<attestation-url>;enclaveAttestationProtocol=<attestation-protocol>;";
try (Connection c = DriverManager.getConnection(URL);Statement s = c.createStatement()) {
    s.executeUpdate("ALTER TABLE Employees ALTER COLUMN SSN CHAR(11) NULL WITH (ONLINE = ON)");
}
```

## <a name="java-8-users"></a>Пользователи Java 8
Для этой функции требуется алгоритм подписи RSASSA-PSA. Он был добавлен в JDK 11, но не был портирован в JDK 8. Пользователи, которые хотят использовать эту функцию с версией [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] для JDK 8, должны либо загрузить собственный поставщик, поддерживающий алгоритм подписи RSASSA-PSA, либо включить дополнительную зависимость BouncyCastleProvider. Эта зависимость будет удалена позже, если алгоритм подписи будет портирован в JDK 8 или жизненный цикл поддержки JDK 8 завершится.

## <a name="see-also"></a>См. также раздел
[Использование функции Always Encrypted с JDBC Driver](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md)
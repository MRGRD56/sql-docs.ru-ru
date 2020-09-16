---
description: Использование Always Encrypted с безопасными анклавами с драйвером JDBC
title: Использование Always Encrypted с безопасными анклавами с драйвером JDBC | Документация Майкрософт
ms.custom: ''
ms.date: 03/02/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 271c0438-8af1-45e5-b96a-4b1cabe32707
author: reneye
ms.author: v-reye
ms.openlocfilehash: 8035e1d5890bf51d80341f740436d586053d4e90
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88487926"
---
# <a name="using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver"></a>Использование Always Encrypted с безопасными анклавами с драйвером JDBC
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Эта страница содержит сведения о том, как разрабатывать приложения Java с использованием [Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/always-encrypted-enclaves.md) и Microsoft JDBC Driver 8.2 (или более поздней версии) для SQL Server.

Безопасные анклавы являются дополнением к существующей функции [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Их назначением является устранение ограничений при работе с данными Always Encrypted. Ранее пользователи могли выполнять применительно к данным Always Encrypted только сравнение на равенство. Для выполнения других операций им приходилось извлекать и расшифровывать данные. Безопасные анклавы устраняют это ограничение, позволяя выполнять вычисления с данными в виде обычного текста внутри безопасного анклава на стороне сервера. Безопасный анклав — это защищенная область памяти в процессе SQL Server. Он также выступает в качестве доверенной среды выполнения для обработки конфиденциальных данных в ядре SQL Server. Безопасный анклав представляет собой "черный ящик" на фоне остальной части SQL Server и других процессов на главном компьютере. Просмотреть данные или код внутри анклава извне невозможно, даже с помощью отладчика.

## <a name="prerequisites"></a>Предварительные требования
- Убедитесь в том, что на компьютере разработки установлен драйвер Microsoft JDBC Driver 8.2 (или более поздней версии) для SQL Server.
- Убедитесь, что зависимости среды, такие как библиотеки DLL, хранилища ключей и т. д., расположены в соответствующих путях. Always Encrypted с безопасными анклавами — это надстройка для существующего компонента [Always Encrypted](../../connect/jdbc/using-always-encrypted-with-the-jdbc-driver.md) с аналогичными предварительными требованиями.

> [!Note]
> Если вы используете более старую версию JDK 8, может потребоваться скачать и установить файлы политики юрисдикции неограниченной стойкости Java Cryptography Extension (JCE). Обязательно прочитайте содержащийся в ZIP-архиве файл сведений, чтобы получить инструкции по установке и сопутствующие сведения о возможных проблемах экспорта и импорта.  
>
> Файлы политики можно скачать на [странице скачивания файлов политики юрисдикции неограниченной стойкости Java Cryptography Extension (JCE) 8](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html).

## <a name="setting-up-secure-enclaves"></a>Настройка безопасных анклавов
Чтобы приступить к использованию безопасных анклавов, обратитесь к [этому руководству](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md). Более подробные сведения см. в статье [Always Encrypted с безопасными анклавами](../../relational-databases/security/encryption/always-encrypted-enclaves.md).

## <a name="connection-string-properties"></a>Свойства строки подключения
**enclaveAttestationUrl** — URL-адрес конечной точки службы аттестации.

**enclaveAttestationProtocol** — протокол службы аттестации. В настоящее время поддерживается только одно значение — **HGS** (служба защиты узла).

Чтобы включить Always Encrypted с безопасными анклавами из [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], пользователям необходимо включить параметр **columnEncryptionSetting** и правильно задать **оба** приведенных выше свойства строки подключения.

## <a name="working-with-secure-enclaves"></a>Работа с безопасными анклавами
Если свойства подключения к анклавам заданы правильно, функция будет работать прозрачно. Драйвер будет определять, должен ли запрос автоматически использовать безопасный анклав. Ниже приведены примеры запросов, инициирующих вычисления анклава. Инструкции по настройке базы данных и таблицы см. в статье [Always Encrypted с безопасными анклавами в SSMS](../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md).

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

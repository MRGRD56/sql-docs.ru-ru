---
description: См. примеры подключения с использованием шифрования TLS в приложении Java с помощью драйвера JDBC для SQL Server.
title: Подключение с шифрованием
ms.custom: ''
ms.date: 03/31/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: vanto
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ec91fa8a-ab7e-4c1e-a05a-d7951ddf33b1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fbefb54c4af48706d04e4081d8e3a6c25ec8e21f
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/01/2021
ms.locfileid: "106177055"
---
# <a name="connecting-with-encryption"></a>Подключение с шифрованием

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

В примерах из этой статьи описывается использование свойств строки соединения, которые разрешают приложениям использовать шифрование TLS в приложении Java. Дополнительные сведения об этих новых свойствах строки соединения, например **encrypt**, **trustServerCertificate**, **trustStore**, **trustStorePassword** и **hostNameInCertificate**, см. в разделе [Задание свойств соединения](setting-the-connection-properties.md).

## <a name="configuring-the-connection"></a>Настройка подключения

Если свойство **encrypt** установлено в значение **true**, а свойство **trustServerCertificate** — в значение **true**, то драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] не будет проверять TLS-сертификат [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Этот параметр обычно используется, чтобы разрешить подключения в тестовой среде, например когда у экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] есть только самозаверяющий сертификат.

В следующем примере кода показано, как можно задать свойство **trustServerCertificate** в строке подключения:

```java
String connectionUrl =
    "jdbc:sqlserver://localhost:1433;" +
     "databaseName=AdventureWorks;integratedSecurity=true;" +
     "encrypt=true;trustServerCertificate=true";
```

Если свойство **encrypt** установлено как **true**, а свойство **trustServerCertificate** — как **false**, то драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] будет проверять TLS-сертификат [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Проверка сертификата сервера является частью TLS-подтверждения и гарантирует, что для подключения выбран правильный сервер. Чтобы проверить сертификат сервера, при подключении необходимо явно предоставить доверенный материал с помощью свойств подключения **trustStore** и **trustStorePassword** или неявно, с помощью базового доверенного хранилища JVM по умолчанию.

Свойство **trustStore** указывает путь (включая имя файла) к файлу сертификата trustStore, который содержит список сертификатов, которым доверяет клиент. Свойство **trustStorePassword** задает пароль, используемый для проверки целостности данных trustStore. Дополнительные сведения об использовании доверенного хранилища виртуальной машины Java по умолчанию см. в статье [Настройка шифрования на клиенте](configuring-the-client-for-ssl-encryption.md).

В следующем примере кода показано, как в строке подключения можно задать свойства **trustStore** и **trustStorePassword**:

```java
String connectionUrl =
    "jdbc:sqlserver://localhost:1433;" +
     "databaseName=AdventureWorks;integratedSecurity=true;" +
     "encrypt=true; trustServerCertificate=false;" +
     "trustStore=storeName;trustStorePassword=storePassword";
```

Драйвер JDBC предоставляет дополнительное свойство **hostNameInCertificate**, которое определяет имя узла сервера. Значение этого свойства должно совпадать со значением свойства subject сертификата.

В следующем примере кода показано, как в строке подключения можно использовать свойство **hostNameInCertificate**:

```java
String connectionUrl =
    "jdbc:sqlserver://localhost:1433;" +
     "databaseName=AdventureWorks;integratedSecurity=true;" +
     "encrypt=true; trustServerCertificate=false;" +
     "trustStore=storeName;trustStorePassword=storePassword" +
     "hostNameInCertificate=hostName";
```

> [!NOTE]
> Кроме того, можно задать значение свойству подключения с помощью соответствующих методов **задания**, предоставляемых классом [SQLServerDataSource](reference/sqlserverdatasource-class.md).

Если для свойства **encrypt** установлено значение **true**, для свойство **trustServerCertificate** установлено значение **false**, а имя сервера в строке подключения не соответствует имени сервера в сертификате TLS, возникнет следующая ошибка: `The driver couldn't establish a secure connection to SQL Server by using Secure Sockets Layer (SSL) encryption. Error: "java.security.cert.CertificateException: Failed to validate the server name in a certificate during Secure Sockets Layer (SSL) initialization."`. Начиная с версии 7.2, драйвер поддерживает сопоставление шаблонов с подстановочными знаками в крайней левой части имени сервера в сертификате TLS.

## <a name="see-also"></a>См. также раздел

[Использование шифрования](using-ssl-encryption.md)  
[Защита приложений JDBC Driver](securing-jdbc-driver-applications.md)  

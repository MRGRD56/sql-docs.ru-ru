---
description: Сведения о защите приложений и использовании разрешений политики Java при разработке приложения с помощью драйвера JDBC.
title: Безопасность приложений
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 940879b4-aa0f-41ce-a369-6cfc0e78e01d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f4785c39dc1d3fa9921a7191c9c3d338c7a655c2
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/01/2021
ms.locfileid: "106177023"
---
# <a name="application-security"></a>Безопасность приложения

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

При использовании [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] важно обеспечить безопасность приложения. В следующих разделах приводится информация о том, как можно защитить приложение.

## <a name="using-java-policy-permissions"></a>Использование разрешений политики Java

При использовании [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] важно задать обязательные разрешения политики Java, необходимые драйверу JDBC. Среда выполнения Java (JRE) предоставляет расширенную модель безопасности. Ее можно использовать во время выполнения, чтобы определить, имеет ли поток доступ к ресурсу. Файлы политики безопасности могут управлять этим доступом. Самими файлами политики управляют администратор развертывания и системный администратор контейнера. На работу драйвера JDBC влияют разрешения, перечисленные в этой статье.

Типичное разрешение в файле политики выглядит следующим образом.

```config
// Example policy file entry.
grant [signedBy <signer>,] [codeBase <code source>] {
   permission  <class>  [<name> [, <action list>]];
};
```

 Чтобы предоставить наименьшее число привилегий, следующая база кода должна быть ограничена базой кода драйвера JDBC.

```config
grant codeBase "file:/install_dir/lib/-" {

// Grant access to data source.
permission java.util.PropertyPermission "java.naming.*", "read,write";

// Specify which hosts can be connected to.
permission java.net.socketPermission "host:port", "connect";

// Logger permission to take advantage of logging.
permission java.util.logging.LoggingPermission;

// Grant listen/connect/accept permissions to the driver if
// connecting to a named instance as the client driver.
// This connects to a udp service and listens for a response.
permission java.net.SocketPermission "*", "listen, connect, accept";
};
```

> [!NOTE]
> Код «file:/install_dir/lib/-« ссылается на каталог установки драйвера JDBC.

## <a name="protecting-server-communication"></a>Защита обмена данными с сервером

При использовании драйвера JDBC для взаимодействия с базой данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] важно обеспечить безопасность канала связи. Это можно сделать за счет использования либо протокола IPSEC, либо шифрования TLS, ранее называемого SSL шифрованием. Или можно использовать их оба.

Поддержка протокола TLS обеспечивает дополнительный уровень защиты, помимо IPSEC. Дополнительные сведения об использовании протокола TLS см. в разделе [Использование шифрования](using-ssl-encryption.md).

## <a name="see-also"></a>См. также раздел

[Защита приложений JDBC Driver](securing-jdbc-driver-applications.md)

---
title: Формирование URL-адреса соединения
description: Сведения о формате строки подключения, используемой Microsoft JDBC Driver for SQL Server.
ms.custom: ''
ms.date: 01/29/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 44996746-d373-4f59-9863-a8a20bb8024a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 709855be591f527191028ef3164066f0adb63819
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633265"
---
# <a name="building-the-connection-url"></a>Формирование URL-адреса соединения

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Обычно URL-адреса соединения выглядят следующим образом:

`jdbc:sqlserver://[serverName[\instanceName][:portNumber]][;property=value[;property=value]]`

где:

- **jdbc:sqlserver://** (обязательно) известен как подпротокол и является константой.

- **serverName** (необязательно) является адресом сервера, с которым выполняется соединение. Это может быть DNS, IP-адрес, localhost или адрес 127.0.0.1 локального компьютера. Имя сервера необходимо указать в коллекции свойств, если оно не указано в URL-адресе соединения.

- **instanceName** (необязательно) является экземпляром, с которым выполняется соединение с serverName. Подключение выполняется к экземпляру по умолчанию, если не указано другое.

- **portNumber** (необязательно) является портом, с которым выполняется соединение с serverName. Значение по умолчанию — 1433. Если соединение выполняется с портом по умолчанию, в URL-адресе необязательно указывать порт или символ ':' перед ним.

    > [!NOTE]
    >  Для оптимизации производительности соединения при соединении с именованным экземпляром необходимо указать portNumber. Это позволит избежать дополнительного обращения к серверу для определения номера порта. Если используются portNumber и instanceName, portNumber будет иметь более высокий приоритет, и instanceName будет пропущен.

- **property** (необязательно) представляет собой одно или несколько свойств соединений. Дополнительные сведения см. в статье о [настройке свойств подключения](setting-the-connection-properties.md). Можно указать любое свойство из списка. В качестве разделителей в списке свойств можно использовать точку с запятой (';'), при этом свойства не могут повторяться.

> [!CAUTION]
> В целях безопасности не рекомендуется составлять URL-адрес соединения на основе данных пользователей. В URL-адресе необходимо указывать только имя сервера и драйвер. Для указания значений имени пользователя и пароля следует использовать коллекции свойств соединения. Дополнительные сведения о безопасности в приложениях JDBC см. в статье [Защита приложений JDBC Driver](securing-jdbc-driver-applications.md).

## <a name="connection-examples"></a>Примеры подключений

Подключитесь к базе данных по умолчанию на локальном компьютере с помощью имени пользователя и пароля:

`jdbc:sqlserver://localhost;user=MyUserName;password=*****;`

> [!NOTE]
> Хотя в предыдущем примере в строке подключения указывались имя пользователя и пароль, следует использовать встроенную безопасность, так как она более надежна. Дополнительные сведения см. в подразделе [Соединения с использованием встроенной проверки подлинности](#Connectingintegrated) далее в этом разделе.

Следующая строка подключения дается в качестве примера выполнения соединения с базой данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] со встроенной проверкой подлинности и Kerberos из приложения, работающего в любой операционной системе, поддерживаемой [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]:

```java
jdbc:sqlserver://;servername=server_name;integratedSecurity=true;authenticationScheme=JavaKerberos
```

Подключитесь к базе данных по умолчанию на локальном компьютере с помощью встроенной проверки подлинности:

`jdbc:sqlserver://localhost;integratedSecurity=true;`

Подключитесь к именованной базе данных на удаленном сервере:

`jdbc:sqlserver://localhost;databaseName=AdventureWorks;integratedSecurity=true;`

Подключитесь к удаленному серверу с использованием порта по умолчанию:

`jdbc:sqlserver://localhost:1433;databaseName=AdventureWorks;integratedSecurity=true;`

Подключитесь, указав настраиваемое имя приложения:

`jdbc:sqlserver://localhost;databaseName=AdventureWorks;integratedSecurity=true;applicationName=MyApp;`

## <a name="named-and-multiple-sql-server-instances"></a>Именованные и множественные экземпляры SQL Server

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] обеспечивает возможность установки нескольких экземпляров на одном сервере. Все экземпляры идентифицируются с помощью специального имени. Для соединения с именованным экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно либо указать номер порта именованного экземпляра (рекомендуется), либо указать имя экземпляра в качестве свойства URL-адреса JDBC или свойства **datasource**. Если имя экземпляра или свойство номера порта не указано, создается соединение с экземпляром по умолчанию. См. следующие примеры.

Чтобы указать номер порта, используйте следующий формат:

`jdbc:sqlserver://localhost:1433;integratedSecurity=true;<more properties as required>;`

Чтобы использовать свойство URL-адреса JDBC, используйте следующий формат:

`jdbc:sqlserver://localhost;instanceName=instance1;integratedSecurity=true;<more properties as required>;`

## <a name="escaping-values-in-the-connection-url"></a>Экранирование значений в URL-адресе подключения

Может потребоваться экранировать определенные части значений URL-адреса соединения, если значения содержат специальные символы, такие как пробелы, точки с запятыми и кавычки. Драйвер JDBC поддерживает экранирование этих значений, заключая их в скобки. Например, {;} указывает на преобразование точки с запятой.

Экранированные значения могут содержать специальные символы (особенно '=', ';', '[]' и пробелы), но не могут содержать скобок. Значения, которые необходимо преобразовать, и значения, содержащие скобки, следует добавить к коллекции свойств.

> [!NOTE]
> Пустое пространство внутри скобок является литералом и не усекается.

## <a name="connecting-with-integrated-authentication-on-windows"></a><a name="Connectingintegrated"></a> Подключение с использованием встроенной проверки подлинности Windows

Драйвер JDBC поддерживает использование встроенной проверки подлинности типа 2 в операционных системах Windows с использованием свойства строки подключения `integratedSecurity`. Чтобы использовать встроенную проверку подлинности, скопируйте файл mssql-jdbc_auth-\<version>-\<arch> в системный каталог Windows на компьютере, на котором установлен драйвер JDBC.

Файлы mssql-jdbc_auth-\<version>-\<arch>.dll устанавливаются в следующем местоположении:

\<*installation directory*>\sqljdbc_\<*version*>\\<*язык*>\auth\

Для любой операционной системы, поддерживаемой в [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], в [этой статье](using-kerberos-integrated-authentication-to-connect-to-sql-server.md) перечислены новые компоненты [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)], которые позволяют приложениям соединяться с базой данных через встроенную проверку подлинности по протоколу Kerberos типа 4.

> [!NOTE]
> При использовании 32-разрядной виртуальной машины Java (JVM) следует использовать файл mssql-jdbc_auth-\<version>-\<arch>.dll в папке x86, даже если используется операционная система x64. При использовании 64-разрядной виртуальной машины Java и процессора x64 используйте файл mssql-jdbc_auth-\<version>-\<arch>.dll в папке x64.

Или можно задать системное свойство java.libary.path для указания каталога, в котором содержится файл mssql-jdbc_auth-\<version>-\<arch>.dll. Например, если драйвер JDBC установлен в каталоге по умолчанию, можно указать местоположение файла DLL, использовав следующий аргумент виртуальной машины (VM) при запуске приложения Java:

`-Djava.library.path=C:\Microsoft JDBC Driver 6.4 for SQL Server\sqljdbc_<version>\enu\auth\x86`

## <a name="connecting-with-ipv6-addresses"></a>Подключение с помощью IPv6-адресов

Драйвер JDBC поддерживает использование IPv6-адресов с коллекцией свойств соединения и свойством строки соединения serverName. Исходное значение serverName, такое как jdbc:*sqlserver*://*serverName*, не поддерживается для IPv6-адресов в строках соединения. Использования имени для *serverName* вместо неизмененного IPv6-адреса будет достаточно для всех вариантов соединений. Следующие примеры можно использовать как дополнительные источники сведений.

**Использование свойства serverName**

`jdbc:sqlserver://;serverName=3ffe:8311:eeee:f70f:0:5eae:10.203.31.9\\instance1;integratedSecurity=true;`

**Использование коллекции свойств**

`Properties pro = new Properties();`

`pro.setProperty("serverName", "serverName=3ffe:8311:eeee:f70f:0:5eae:10.203.31.9\\instance1");`

`Connection con = DriverManager.getConnection("jdbc:sqlserver://;integratedSecurity=true;", pro);`

## <a name="see-also"></a>См. также раздел

[Подключение к SQL Server с помощью JDBC Driver](connecting-to-sql-server-with-the-jdbc-driver.md)

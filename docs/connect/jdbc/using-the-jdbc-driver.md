---
title: Использование JDBC Driver | Документация Майкрософт
description: В этом разделе приводятся краткие указания по установлению простого соединения с базой данных SQL Server с использованием драйвера Microsoft JDBC Driver for SQL Server.
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 6faaf05b-8b70-4ed2-9b44-eee5897f1cd0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e74135523d2ba4cbde27e588eee999b3450def37
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837522"
---
# <a name="using-the-jdbc-driver"></a>Использование JDBC Driver

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

В этом разделе приведены краткие указания, как установить простое подключение к базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью драйвера [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]. Прежде чем подключаться к базе данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], необходимо установить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на сервер или локальный компьютер, а также установить на локальный компьютер драйвер JDBC.  
  
## <a name="choosing-the-right-jar-file"></a>Выбор нужного JAR-файла

Microsoft JDBC Driver обеспечивает различные Jar-файлы, которые можно использовать в соответствии с предпочитаемыми параметрами среды выполнения Java (JRE), как указано ниже.

Microsoft JDBC Driver 9.2 для SQL Server содержит следующие файлы библиотеки классов: **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** и **mssql-jdbc-9.2.1.jre15.jar**.

Microsoft JDBC Driver 8.4 для SQL Server содержит следующие файлы библиотеки классов: **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** и **mssql-jdbc-8.4.1.jre14.jar**.

Microsoft JDBC Driver 8.2 для SQL Server содержит следующие файлы библиотеки классов: **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** и **mssql-jdbc-8.2.2.jre13.jar**.

Microsoft JDBC Driver 7.4 для SQL Server включает следующие файлы библиотеки классов: **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** и **mssql-jdbc-7.4.1.jre12.jar**.

Microsoft JDBC Driver 7.2 для SQL Server обеспечивает файлы библиотеки классов **mssql-jdbc-7.2.2.jre8.jar** и **mssql-jdbc-7.2.2.jre11.jar**.

Microsoft JDBC Driver 7.0 для SQL Server обеспечивает файлы библиотеки классов **mssql-jdbc-7.0.0.jre8.jar** и **mssql-jdbc-7.0.0.jre10.jar**.

Microsoft JDBC Driver 6.4 для SQL Server обеспечивает файлы библиотеки классов **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** и **mssql-jdbc-6.4.0.jre9.jar**.

Microsoft JDBC Driver 6.2 для SQL Server обеспечивает файлы библиотеки классов **mssql-jdbc-6.2.2.jre7.jar** и **mssql-jdbc-6.2.2.jre8.jar**.
  
Microsoft JDBC Driver 6.0 и 4.2 для SQL Server обеспечивают файлы библиотеки классов **sqljdbc41.jar** и **sqljdbc42.jar**.
  
Microsoft JDBC Driver 4.1 для SQL Server обеспечивает файл библиотеки класса **sqljdbc41.jar**.

От выбора версии драйвера также зависит набор доступных функций. Дополнительные сведения о выборе JAR-файла см. в описании [требований к системе для JDBC Driver](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  
  
## <a name="setting-the-classpath"></a>Настройка пути к классу

JAR-файлы Microsoft JDBC Driver не являются частью Java SDK и должны быть включены в путь к классу пользовательского приложения.

Если используется JDBC Driver 4.1 или 4.2, настройте путь к классу, чтобы включить файл из соответствующей загрузки драйвера **sqljdbc41.jar** или **sqljdbc42.jar**.

Если используется JDBC Driver 6.2, настройте путь к классу, чтобы включить файл **mssql-jdbc-6.2.2.jre7.jar** или **mssql-jdbc-6.2.2.jre8.jar**.

Если используется JDBC Driver 6.4, настройте путь к классу, чтобы включить следующие файлы: **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** или **mssql-jdbc-6.4.0.jre9.jar**.

Если используется JDBC Driver 7.0, настройте путь к классу, чтобы включить файл **mssql-jdbc-7.0.0.jre8.jar** или **mssql-jdbc-7.0.0.jre10.jar**.

Если используется JDBC Driver 7.2, настройте путь к классу, чтобы включить файл **mssql-jdbc-7.2.2.jre8.jar** или **mssql-jdbc-7.2.2.jre11.jar**.

Если используется JDBC Driver 7.4, настройте путь к классу, чтобы включить следующие файлы: **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** или **mssql-jdbc-7.4.1.jre12.jar**.

Если используется JDBC Driver 8.2, настройте путь к классу, чтобы включить следующие файлы: **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** или **mssql-jdbc-8.2.2.jre13.jar**.

Если используется JDBC Driver 8.4, настройте путь к классу, чтобы включить следующие файлы: **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** или **mssql-jdbc-8.4.1.jre14.jar**.

Если используется JDBC Driver 9.2, настройте путь к классу, чтобы включить следующие файлы: **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** или **mssql-jdbc-9.2.1.jre15.jar**.

Если в пути к классу отсутствует запись для правильного Jar-файла, приложение выдает общее исключение `Class not found`.  

### <a name="for-microsoft-jdbc-driver-92"></a>Для Microsoft JDBC Driver 9.2

Файлы **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** или **mssql-jdbc-9.2.1.jre15.jar** установлены в следующих расположениях:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-9.2.1.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-9.2.1.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-9.2.1.jre15.jar
```

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 9.2 for SQL Server\sqljdbc_9.2\enu\mssql-jdbc-9.2.1.jre11.jar`

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_9.2/enu/mssql-jdbc-9.2.1.jre11.jar`

Убедитесь в том, что в инструкции CLASSPATH содержится только один драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], то есть **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** или **mssql-jdbc-9.2.1.jre15.jar**.


### <a name="for-microsoft-jdbc-driver-84"></a>Для Microsoft JDBC Driver 8.4

Файлы **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** или **mssql-jdbc-8.4.1.jre14.jar** установлены в следующих расположениях:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.4.1.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.4.1.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.4.1.jre14.jar
```

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 8.4 for SQL Server\sqljdbc_8.4\enu\mssql-jdbc-8.4.1.jre11.jar`

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_8.4/enu/mssql-jdbc-8.4.1.jre11.jar`

Убедитесь, что в инструкции CLASSPATH содержится только один драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], то есть **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** или **mssql-jdbc-8.4.1.jre14.jar**.


### <a name="for-microsoft-jdbc-driver-82"></a>Для Microsoft JDBC Driver 8.2

Файлы **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** или **mssql-jdbc-8.2.2.jre13.jar** установлены в следующих расположениях:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.2.2.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.2.2.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-8.2.2.jre13.jar
```

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 8.2 for SQL Server\sqljdbc_8.2\enu\mssql-jdbc-8.2.2.jre11.jar`

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_8.2/enu/mssql-jdbc-8.2.2.jre11.jar`

Убедитесь, что в инструкции CLASSPATH содержится только один драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], то есть **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** или **mssql-jdbc-8.2.2.jre13.jar**.

### <a name="for-microsoft-jdbc-driver-74"></a>Для Microsoft JDBC Driver 7.4

Файлы **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** или **mssql-jdbc-7.4.1.jre12.jar** установлены в следующих расположениях:

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.4.1.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.4.1.jre11.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.4.1.jre12.jar
```

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 7.4 for SQL Server\sqljdbc_7.4\enu\mssql-jdbc-7.4.1.jre11.jar`

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_7.4/enu/mssql-jdbc-7.4.1.jre11.jar`

Убедитесь, что в инструкции CLASSPATH содержится только один драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], то есть **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** или **mssql-jdbc-7.4.1.jre12.jar**.

### <a name="for-microsoft-jdbc-driver-72"></a>Для Microsoft JDBC Driver 7.2

Файлы **mssql-jdbc-7.2.2.jre8.jar** или **mssql-jdbc-7.2.2.jre11.jar** установлены в следующих местах.

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.2.2.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.2.2.jre11.jar
```

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Windows:

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 7.2 for SQL Server\sqljdbc_7.2\enu\mssql-jdbc-7.2.2.jre11.jar`

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Unix/Linux:

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_7.2/enu/mssql-jdbc-7.2.2.jre11.jar`

Убедитесь, что в инструкции CLASSPATH содержится только один драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], то есть либо **mssql-jdbc-7.2.2.jre8.jar**, либо **mssql-jdbc-7.2.2.jre11.jar**.
  
### <a name="for-microsoft-jdbc-driver-70"></a>Для Microsoft JDBC Driver 7.0

Файлы **mssql-jdbc-7.0.0.jre8.jar** или **mssql-jdbc-7.0.0.jre10.jar** установлены в следующих местах.

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.0.0.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-7.0.0.jre10.jar
```

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Windows:  

`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 7.0 for SQL Server\sqljdbc_7.0\enu\mssql-jdbc-7.0.0.jre10.jar`  
  
В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Unix/Linux:  
  
`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_7.0/enu/mssql-jdbc-7.0.0.jre10.jar`  
  
Убедитесь, что в инструкции CLASSPATH содержится только один драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], то есть либо **mssql-jdbc-7.0.0.jre8.jar**, либо **mssql-jdbc-7.0.0.jre10.jar**.

### <a name="for-microsoft-jdbc-driver-64"></a>Для Microsoft JDBC Driver 6.4

Файлы **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar или **mssql-jdbc-6.4.0.jre9.jar** установлены в следующих местах.  

```bash  
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.4.0.jre7.jar
  
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.4.0.jre8.jar

\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.4.0.jre9.jar
```

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Windows:  
  
`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.4 for SQL Server\sqljdbc_6.4\enu\mssql-jdbc-6.4.0.jre9.jar`  
  
В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Unix/Linux:  
  
`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_6.4/enu/mssql-jdbc-6.4.0.jre9.jar`  
  
Убедитесь, что в инструкции CLASSPATH содержится только один драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], то есть **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar или **mssql-jdbc-6.4.0.jre9.jar**.

### <a name="for-microsoft-jdbc-driver-62"></a>Для Microsoft JDBC Driver 6.2

Файлы **mssql-jdbc-6.2.2.jre7.jar** или **mssql-jdbc-6.2.2.jre8.jar** установлены в следующих местах.

```bash
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.2.2.jre7.jar
  
\<installation directory>\sqljdbc_<version>\<language>\mssql-jdbc-6.2.2.jre8.jar
```

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Windows:  
  
`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.2 for SQL Server\sqljdbc_6.2\enu\mssql-jdbc-6.2.2.jre8.jar`  
  
В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Unix/Linux:  
  
`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_6.2/enu/mssql-jdbc-6.2.2.jre8.jar`  
  
Убедитесь, что в инструкции CLASSPATH содержится только один драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], то есть либо mssql-jdbc-6.2.2.jre7.jar, либо mssql-jdbc-6.2.2.jre8.jar.  

### <a name="for-microsoft-jdbc-driver-41-42-and-60"></a>Для Microsoft JDBC Driver 4.1, 4.2 и 6.0

Файл sqljdbc.jar file, sqljdbc4.jar file, sqljdbc41.jar или sqljdbc42.jar устанавливается в следующее расположение:  

```bash
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc.jar  
  
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc4.jar  
  
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc41.jar  
  
\<installation directory>\sqljdbc_<version>\<language>\sqljdbc42.jar  
```

В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Windows:  
  
`CLASSPATH =.;C:\Program Files\Microsoft JDBC Driver 6.0 for SQL Server\sqljdbc_4.2\enu\sqljdbc42.jar`  
  
В следующем фрагменте кода дан пример инструкции CLASSPATH, используемой для приложения Unix/Linux:  

`CLASSPATH =.:/home/usr1/mssqlserverjdbc/Driver/sqljdbc_4.2/enu/sqljdbc42.jar`  
  
Инструкция CLASSPATH должна содержать только один драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], то есть sqljdbc.jar, sqljdbc4.jar, sqljdbc41.jar либо sqljdbc42.jar.  
  
> [!NOTE]  
> В системах Windows имена каталогов, длина которых превышает оговоренный в соглашении об именовании размер 8.3, а также папки, имена которых содержат пробелы, могут вызвать проблемы с путями к классам. Если вы подозреваете такие проблемы, временно переместите файл sqljdbc.jar, sqljdbc4.jar или sqljdbc41.jar в каталог с простым именем, например `C:\Temp`, измените путь к классам и проверьте, устранило ли это проблему.  
  
### <a name="applications-that-are-run-directly-at-the-command-prompt"></a>Приложения, которые выполняются непосредственно в командной строке

Путь к классам настраивается в операционной системе. Добавьте sqljdbc.jar, sqljdbc4.jar или sqljdbc41.jar в путь к классам в системе. Вы также можете указать путь к классам в командной строке Java при запуске приложения, используя параметр `java -classpath`.  
  
### <a name="applications-that-run-in-an-ide"></a>Приложения, выполняющиеся в интегрированной среде разработки  

Каждый поставщик интегрированных сред разработки предоставляет собственный метод установки classpath. Простое задание пути к классам в операционной системе не будет работать. Необходимо добавить sqljdbc.jar, sqljdbc4.jar или sqljdbc41.jar в путь к классам интегрированной среды разработки.  
  
### <a name="servlets-and-jsps"></a>Сервлеты и JSP  

Сервлеты и JSP выполняются в подсистеме сервлетов/JSP, например Tomcat. Путь к классам должен быть задан в соответствии с документацией подсистемы сервлетов и JSP. Простое задание пути к классам в операционной системе не будет работать. Некоторые подсистемы сервлетов/JSP предоставляют экраны настройки, которые можно использовать для задания пути к классам подсистемы. В этом случае необходимо добавить к существующему пути к классу подсистемы нужный JAR-файл драйвера JDBC и перезапустить подсистему. В остальных случаях можно развернуть драйвер, скопировав файл sqljdbc.jar, sqljdbc4.jar или sqljdbc41.jar в определенный каталог (например, lib) во время установки подсистемы. Путь к классам драйвера подсистемы также можно задать в файле конфигурации конкретной подсистемы.  
  
### <a name="enterprise-java-beans"></a>Enterprise Java Beans  

Компоненты Enterprise Java Beans (EJB) выполняются в контейнере EJB. Контейнеры EJB предоставляются различными поставщиками. Java-приложения работают в браузере, но загружаются с веб-сервера. Скопируйте sqljdbc.jar, sqljdbc4.jar или sqljdbc41.jar в корень веб-сервера и укажите имя JAR-файла в параметре "archive" HTML-тега "applet", например: `<applet ... archive=mssql-jdbc-***.jar>`.  
  
## <a name="making-a-simple-connection-to-a-database"></a>Установление простого подключения к базе данных

В случае использования библиотеки классов sqljdbc.jar приложения сначала должны зарегистрировать драйвер следующим образом:  
  
`Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");`  

После загрузки драйвера можно установить соединение, используя URL-адрес соединения и метод getConnection класса DriverManager.

```java
String connectionUrl = "jdbc:sqlserver://localhost:1433;" +  
   "databaseName=AdventureWorks;user=MyUserName;password=*****;";  
Connection con = DriverManager.getConnection(connectionUrl);  
```

Начиная с JDBC API 4.0 метод `DriverManager.getConnection()` усовершенствован и загружает драйверы JDBC автоматически. Поэтому при использовании библиотек JAR драйвера приложениям не обязательно вызывать метод `Class.forName` для регистрации или загрузки драйвера.  
  
При вызове метода getConnection класса DriverManager подходящий драйвер выбирается из набора зарегистрированных драйверов JDBC. Файлы sqljdbc4.JAR, sqljdbc41.jar и sqljdbc42.jar содержат файл META-INF/services/java.sql.Driver, который содержит **com.microsoft.sqlserver.jdbc.SQLServerDriver** в качестве зарегистрированного драйвера. Существующие приложения, которые загружают драйверы с помощью метода Class.forName, продолжат работать, не требуя изменений.  
  
> [!NOTE]  
> Библиотеку классов sqljdbc4.JAR, sqljdbc41.jar или sqljdbc42.jar нельзя использовать с более старыми версиями среды выполнения Java. Список версий JRE-файлов, поддерживаемых [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], см. в описании [требований к системе для JDBC Driver](../../connect/jdbc/system-requirements-for-the-jdbc-driver.md).  

Дополнительные сведения о подключении к источникам данных и использовании URL-адреса подключения см. в руководствах по [созданию URL-адреса подключения](../../connect/jdbc/building-the-connection-url.md) и [настройке свойств подключения](../../connect/jdbc/setting-the-connection-properties.md).  
  
## <a name="see-also"></a>См. также раздел  

[Общие сведения о JDBC Driver](../../connect/jdbc/overview-of-the-jdbc-driver.md)  

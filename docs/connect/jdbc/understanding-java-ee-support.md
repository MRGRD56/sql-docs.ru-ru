---
description: Узнайте, как драйвер JDBC поддерживает Java EE и API JBDC, а также пакеты Java, требуемые для использования этих функций.
title: Основные сведения о поддержке Java EE
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: a9448b80-b7a3-49cf-8bb4-322c73676005
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5b6ec6bb0be6b03c8e72106e231c23be09fa2c9d
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793012"
---
# <a name="understanding-java-ee-support"></a>Основные сведения о поддержке Java EE

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

В следующих разделах описано, как [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] реализует поддержку платформы Java, Enterprise Edition (Java EE) и дополнительные функции API-интерфейсов JDBC 3.0. Образцы исходного кода, собранные в данном разделе справки, позволяют быстро освоить эти функции.

Сначала убедитесь, что среда JAVA (JDK, JRE) включает пакет javax.sql. Это обязательный пакет для любого приложения JDBC, которое использует дополнительный API. JDK 1.5 и более поздние версии уже содержат этот пакет, поэтому устанавливать его отдельно не требуется.

## <a name="driver-name"></a>Имя драйвера

Имя класса драйвера ― **com.microsoft.sqlserver.jdbc.SQLServerDriver**. Для драйверов JDBC 4.1, 4.2 и 6.0 драйвер содержится в файле **sqljdbc.jar**, **sqljdbc4.jar**, **sqljdbc41.jar** или **sqljdbc42.jar**.

Для драйвера JDBC 6.2 драйвер содержится в файле **mssql-jdbc-6.2.2.jre7.jar** или **mssql-jdbc-6.2.2.jre8.jar**.

Для драйвера JDBC 6.4 драйвер содержится в файле **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** или **mssql-jdbc-6.4.0.jre9.jar**.

Для драйвера JDBC 7.0 драйвер содержится в файле **mssql-jdbc-7.0.0.jre8.jar** или **mssql-jdbc-7.0.0.jre10.jar**.

Для драйвера JDBC 7.2 драйвер содержится в файле **mssql-jdbc-7.2.2.jre8.jar** или **mssql-jdbc-7.2.2.jre11.jar**.

Для драйвера JDBC 7.4 драйвер содержится в файле **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** или **mssql-jdbc-7.4.1.jre12.jar**.

Для драйвера JDBC 8.2 драйвер содержится в файле **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** или **mssql-jdbc-8.2.2.jre13.jar**.

Для драйвера JDBC 8.4 драйвер содержится в файле **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** или **mssql-jdbc-8.4.1.jre14.jar**.

Для драйвера JDBC 9.2 драйвер содержится в файле **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** или **mssql-jdbc-9.2.1.jre15.jar**.

Имя класса используется каждый раз, когда вы загружаете драйвер с использованием класса JDBC DriverManager и указываете имя класса драйвера в любой конфигурации драйвера. Например, имя класса драйвера может потребоваться для настройки источника данных на сервере приложений Java EE.

## <a name="data-sources"></a>Источники данных

Драйвер JDBC обеспечивает поддержку для источников данных Java EE/JDBC 3.0. Класс [SQLServerXADataSource](reference/sqlserverxadatasource-class.md) драйвера JDBC реализуется `com.microsoft.sqlserver.jdbc.SQLServerXADataSource`.

### <a name="datasource-names"></a>Имена источников данных

Подключения к базам данных можно создать, используя источники данных. Источники данных, доступные с драйвером JDBC, описываются в следующей таблице.

|Тип источника данных|Имя класса и описание|
|---------------|--------------------------|
|DataSource|`com.microsoft.sqlserver.jdbc.SQLServerDataSource` <br/> <br/> Источник данных без организации пулов.|
|ConnectionPoolDataSource|`com.microsoft.sqlserver.jdbc.SQLServerConnectionPoolDataSource` <br/> <br/> Источник данных для настройки пулов соединений сервера приложений JAVA EE. Обычно используется, когда приложение работает на сервере приложений JAVA EE.|
|XADataSource|`com.microsoft.sqlserver.jdbc.SQLServerXADataSource` <br/> <br/> Источник данных для настройки источников данных JAVA EE XA. Обычно используется, когда приложение работает на сервере приложений JAVA EE и диспетчере транзакций XA.|

### <a name="data-source-properties"></a>Свойства источника данных

Все источники данных поддерживают возможность задания и получения любого свойства, связанного с базовым набором свойств драйвера.

Примеры:

`setServerName("localhost");`  
`setDatabaseName("AdventureWorks");`  

Далее показано, как приложение подключается, используя источник данных.

```java
//initialize JNDI ..
Context ctx = new InitialContext(System.getProperties());
...
DataSource ds = (DataSource) ctx.lookup("MyDataSource");
Connection c = ds.getConnection("user", "pwd");
```

См. о [настройке свойств источника данных](setting-the-data-source-properties.md).

## <a name="see-also"></a>См. также раздел

[Общие сведения о JDBC Driver](overview-of-the-jdbc-driver.md)  

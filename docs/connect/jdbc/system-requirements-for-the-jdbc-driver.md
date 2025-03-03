---
description: Требования к системе для JDBC Driver
title: Требования к системе для JDBC Driver | Документация Майкрософт
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 447792bb-f39b-49b4-9fd0-1ef4154c74ab
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 56ce0af2be925244e4ca2013b40d2b98060bc974
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464812"
---
# <a name="system-requirements-for-the-jdbc-driver"></a>Требования к системе для JDBC Driver

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Чтобы получить доступ к данным из базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] с помощью драйвера [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], на компьютере должны быть установлены следующие компоненты:

- [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] ([скачать](download-microsoft-jdbc-driver-for-sql-server.md))
- Среда выполнения Java

## <a name="java-runtime-environment-requirements"></a>Требования к среде выполнения Java  

 Начиная с версии Microsoft JDBC Driver 9.2 для SQL Server реализована поддержка пакета SDK для Java (JDK) 15.0 и среды выполнения Java (JRE) 15.0.

 Начиная с версии Microsoft JDBC Driver 8.4 для SQL Server реализована поддержка пакета SDK для Java (JDK) 14.0 и среды выполнения Java (JRE) 14.0.

 Начиная с версии Microsoft JDBC Driver 8.2 для SQL Server реализована поддержка пакета SDK для Java (JDK) 13.0 и среды выполнения Java (JRE) 13.0.

 Начиная с версии Microsoft JDBC Driver 7.4 для SQL Server реализована поддержка пакета SDK для Java (JDK) 12.0 и среды выполнения Java (JRE) 12.0.

 Начиная с версии Microsoft JDBC Driver 7.2 для SQL Server реализована поддержка пакета SDK для Java (JDK) 11.0 и среды выполнения Java (JRE) 11.0.

 Начиная с версии Microsoft JDBC Driver 7.0 для SQL Server реализована поддержка пакета SDK для Java (JDK) 10.0 и среды выполнения Java (JRE) 10.0.

 Начиная с версии Microsoft JDBC Driver 6.4 для SQL Server реализована поддержка пакета SDK для Java (JDK) 9.0 и среды выполнения Java (JRE) 9.0.

 Начиная с версии Microsoft JDBC Driver 4.2 для SQL Server реализована поддержка пакета SDK для Java (JDK) 8.0 и среды выполнения Java (JRE) 8.0. Поддержка спецификации API JDBC была расширена за счет включения API JDBC 4.1 и 4.2.
  
 Начиная с версии Microsoft JDBC Driver 4.1 для SQL Server реализована поддержка пакета SDK для Java (JDK) 7.0 и среды выполнения Java (JRE) 7.0.
  
 Начиная с [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)] поддержка драйвера JDBC для спецификации API JDBC была расширена за счет включения API JDBC 4.0. Версия API JDBC 4.0 впервые появилась в составе пакета SDK для Java (JDK) 6.0 и среды выполнения Java (JRE) 6.0. JDBC 4.0 является супермножеством API JDBC 3.0.
  
 При развертывании [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] в операционных системах Windows и UNIX необходимо использовать пакеты установки *sqljdbc_\<version>_enu.exe* и *sqljdbc_\<version>_enu.tar.gz* соответственно. Дополнительные сведения см. в статье о [развертывании JDBC Driver](../../connect/jdbc/deploying-the-jdbc-driver.md).  

**Microsoft JDBC Driver 9.2 для SQL Server:**  

  Драйвер JDBC Driver 9.2 содержит три библиотеки классов JAR в каждом пакете установки: **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** и **mssql-jdbc-9.2.1.jre15.jar**.

  Драйвер JDBC Driver 9.2 рассчитан на совместимость и корректную работу со всеми основными виртуальными машинами Java, но протестирован только в OpenJDK 1.8, OpenJDK 11.0, OpenJDK 15.0, Azul Zulu JRE 1.8, Azul Zulu JRE 11.0 и Azul Zulu JRE 15.0.
  
  На схеме ниже приведены сводные данные о поддержке, которую обеспечивают два JAR-файла в составе Microsoft JDBC Driver 9.2 для SQL Server.  
  
  |JAR|Соответствие версии JDBC|Рекомендуемая версия Java|Описание|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-9.2.1.jre8.jar|4.2|8|Требуется среда выполнения Java (JRE) версии 1.8. При использовании JRE 1.7 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 9.2 относятся: поддержка JDK 15, поддержка интерактивной проверки подлинности Azure Active Directory, поддержка проверки подлинности субъекта-службы Azure Active Directory и поддержка useBulkCopyForBatchInsert для серверов, отличных от Azure Synapse Analytics. |
|mssql-jdbc-9.2.1.jre11.jar|4.3|11|Требуется среда выполнения Java (JRE) 11.0. В случае использования JRE 10.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 9.2 относятся: поддержка JDK 15, поддержка интерактивной проверки подлинности Azure Active Directory, поддержка проверки подлинности субъекта-службы Azure Active Directory и поддержка useBulkCopyForBatchInsert для серверов, отличных от Azure Synapse Analytics. |
|mssql-jdbc-9.2.1.jre15.jar|4.3|15|Требуется среда выполнения Java (JRE) версии 15.0. При использовании JRE 14.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 9.2 относятся: поддержка JDK 15, поддержка интерактивной проверки подлинности Azure Active Directory, поддержка проверки подлинности субъекта-службы Azure Active Directory и поддержка useBulkCopyForBatchInsert для серверов, отличных от Azure Synapse Analytics. |

  Драйвер JDBC Driver 9.2 также доступен в Maven Central Repository и может быть добавлен в проект Maven путем включения в POM.XML следующего кода:  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>9.2.1.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 8.4 для SQL Server**  

  Драйвер Driver JDBC 8.4 содержит три библиотеки классов JAR в каждом пакете установки: **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** и **mssql-jdbc-8.4.1.jre14.jar**.

  Драйвер JDBC Driver 8.4 рассчитан на совместимость и корректную работу со всеми основными виртуальными машинами Java, но протестирован только в OpenJDK 1.8, OpenJDK 11.0, OpenJDK 14.0, Azul Zulu JRE 1.8, Azul Zulu JRE 11.0 и Azul Zulu JRE 14.0.
  
  На диаграмме ниже приведены сводные данные о поддержке, которую обеспечивают два JAR-файла в составе Microsoft JDBC Driver 8.4 для SQL Server.  
  
  |JAR|Соответствие версии JDBC|Рекомендуемая версия Java|Описание|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-8.4.1.jre8.jar|4.2|8|Требуется среда выполнения Java (JRE) версии 1.8. При использовании JRE 1.7 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 8.4 относятся: поддержка JDK 14, поддержка проверки подлинности для Azure Key Vault с использованием управляемого удостоверения, расширенная поддержка массового копирования для хранилища данных Azure, кэширование DNS-сервера Azure SQL, поддержка обратной совместимости для потоковых объектов LOB и аутентификация на основе сертификата клиента для сценариев замыкания на себя. |
|mssql-jdbc-8.4.1.jre11.jar|4.3|11|Требуется среда выполнения Java (JRE) 11.0. В случае использования JRE 10.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 8.4 относятся: поддержка JDK 14, поддержка проверки подлинности для Azure Key Vault с использованием управляемого удостоверения, расширенная поддержка массового копирования для хранилища данных Azure, кэширование DNS-сервера Azure SQL, поддержка обратной совместимости для потоковых объектов LOB и аутентификация на основе сертификата клиента для сценариев замыкания на себя. |
|mssql-jdbc-8.4.1.jre13.jar|4.3|14|Требуется среда выполнения Java (JRE) версии 14.0. При использовании JRE 13.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 8.4 относятся: поддержка JDK 14, поддержка проверки подлинности для Azure Key Vault с использованием управляемого удостоверения, расширенная поддержка массового копирования для хранилища данных Azure, кэширование DNS-сервера Azure SQL, поддержка обратной совместимости для потоковых объектов LOB и аутентификация на основе сертификата клиента для сценариев замыкания на себя. |

  Драйвер Driver JDBC 8.4 также доступен в Maven Central Repository и может быть добавлен в проект Maven путем включения в POM.XML следующего кода:  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>8.4.1.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 8.2 для SQL Server**  

  Драйвер Driver JDBC 8.2 содержит три библиотеки классов JAR в каждом пакете установки: **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** и **mssql-jdbc-8.2.2.jre13.jar**.

  Драйвер JDBC Driver 8.2 рассчитан на совместимость и корректную работу со всеми основными виртуальными машинами Java, но протестирован только в OpenJDK 1.8, OpenJDK 11.0, OpenJDK 13.0, Azul Zulu JRE 1.8, Azul Zulu JRE 11.0 и Azul Zulu JRE 13.0.
  
  На диаграмме ниже приведены сводные данные о поддержке, которую обеспечивают два JAR-файла в составе Microsoft JDBC Driver 8.2 для SQL Server.  
  
  |JAR|Соответствие версии JDBC|Рекомендуемая версия Java|Описание|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-8.2.2.jre8.jar|4.2|8|Требуется среда выполнения Java (JRE) версии 1.8. При использовании JRE 1.7 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 8.2 относятся: поддержка JDK 13, Always Encrypted с безопасными анклавами и временные улучшения производительности типов данных. |
|mssql-jdbc-8.2.2.jre11.jar|4.3|11|Требуется среда выполнения Java (JRE) 11.0. В случае использования JRE 10.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 8.2 относятся: поддержка JDK 13, Always Encrypted с безопасными анклавами и временные улучшения производительности типов данных. |
|mssql-jdbc-8.2.2.jre13.jar|4.3|13|Требуется среда выполнения Java (JRE) версии 13.0. При использовании JRE 11.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 8.2 относятся: поддержка JDK 13, Always Encrypted с безопасными анклавами и временные улучшения производительности типов данных. |

  Драйвер Driver JDBC 8.2 также доступен в Maven Central Repository и может быть добавлен в проект Maven путем включения в POM.XML следующего кода:  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>8.2.2.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 7.4 для SQL Server**  

  Драйвер JDBC 7.4 содержит три библиотеки классов JAR в каждом пакете установки: **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** и **mssql-jdbc-7.4.1.jre12.jar**.

  Драйвер JDBC Driver 7.4 рассчитан на совместимость и корректную работу со всеми основными виртуальными машинами Java, но протестирован только в OpenJDK 1.8, OpenJDK 11.0, OpenJDK 12.0, Azul Zulu JRE 1.8, Azul Zulu JRE 11.0, и Azul Zulu JRE 12.0.
  
  На диаграмме ниже приведены сводные данные о поддержке, которую обеспечивают два JAR-файла в составе Microsoft JDBC Driver 7.4 для SQL Server.  
  
  |JAR|Соответствие версии JDBC|Рекомендуемая версия Java|Описание|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-7.4.1.jre8.jar|4.2|8|Требуется среда выполнения Java (JRE) версии 1.8. При использовании JRE 1.7 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 7.4 относятся: поддержка JDK 12, проверка подлинности NTLM и useFmtOnly. |  
|mssql-jdbc-7.4.1.jre11.jar|4.3|11|Требуется среда выполнения Java (JRE) 11.0. В случае использования JRE 10.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 7.4 относятся: поддержка JDK 12, проверка подлинности NTLM и useFmtOnly. |  
|mssql-jdbc-7.4.1.jre12.jar|4.3|12|Требуется среда выполнения Java (JRE) версии 12.0. При использовании JRE 11.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 7.4 относятся: поддержка JDK 12, проверка подлинности NTLM и useFmtOnly. |  

  Драйвер JDBC 7.4 также доступен в Maven Central Repository и может быть добавлен в проект Maven путем включения в POM.XML следующего кода:  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.4.1.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 7.2 для SQL Server**  

  Драйвер JDBC 7.2 содержит две библиотеки классов JAR в каждом пакете установки: **mssql-jdbc-7.2.2.jre8.jar** и **mssql-jdbc-7.2.2.jre11.jar**.

  Драйвер JDBC Driver 7.2 рассчитан на совместимость и корректную работу со всеми основными виртуальными машинами Java, но протестирован только в OpenJDK 8.0 и 11.0 и Azul Zulu JRE 8.0 и 11.0.
  
  На диаграмме ниже приведены сводные данные о поддержке, которую обеспечивают два JAR-файла в составе Microsoft JDBC Driver 7.2 для SQL Server.  
  
  |JAR|Соответствие версии JDBC|Рекомендуемая версия Java|Описание|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-7.2.2.jre8.jar|4.2|8|Требуется среда выполнения Java (JRE) версии 8.0. В случае использования JRE 7.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 7.2 относятся: поддержка JDK 11, Управляемое удостоверение Active Directory (MSI), поддержка OSGi, API-интерфейсы SQLServerError. |  
|mssql-jdbc-7.2.2.jre11.jar|4.3|10|Требуется среда выполнения Java (JRE) 11.0. В случае использования JRE 10.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 7.2 относятся: поддержка JDK 11, Управляемое удостоверение Active Directory (MSI), поддержка OSGi, API-интерфейсы SQLServerError. |  

  Драйвер JDBC 7.2 также доступен в Maven Central Repository и может быть добавлен в проект Maven путем добавления в POM.XML следующего кода.  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.2.2.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 7.0 для SQL Server**  

  Драйвер JDBC 7.0 содержит две библиотеки классов JAR в каждом пакете установки: **mssql-jdbc-7.0.0.jre8.jar** и **mssql-jdbc-7.0.0.jre10.jar**.

  JDBC Driver 7.0 рассчитан на совместимость и корректную работу со всеми основными виртуальными машинами Java, но протестирован только в OpenJDK 8.0 и 10.0.
  
  На диаграмме ниже приведены сводные данные о поддержке, которую обеспечивают два JAR-файла в составе Microsoft JDBC Driver 7.0 для SQL Server.  
  
  |JAR|Соответствие версии JDBC|Рекомендуемая версия Java|Описание|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-7.0.0.jre8.jar|4.2|8|Требуется среда выполнения Java (JRE) версии 8.0. В случае использования JRE 7.0 или более ранней версии возникает исключение.<br /><br /> В новые функции в версии 7.0 входят: поддержка JDK 10, обновленный по умолчанию уровень совместимости в спецификациях JDBC 4.2, поддержка пространственных типов данных, свойство подключения cancelQueryTimeout, методы границы запросов, свойство подключения useBulkCopyForBatchInsert, информация об обнаружении и классификации данных, расширение возможности UTF-8 и поддержка CityHash. |  
|mssql-jdbc-7.0.0.jre10.jar|4.3|10|Требуется среда выполнения Java (JRE) 10.0. В случае использования JRE 9.0 или более ранней версии возникает исключение.<br /><br /> В новые функции в версии 7.0 входят: поддержка JDK 10, обновленный по умолчанию уровень совместимости в спецификациях JDBC 4.2, поддержка пространственных типов данных, свойство подключения cancelQueryTimeout, методы границы запросов, свойство подключения useBulkCopyForBatchInsert, информация об обнаружении и классификации данных, расширение возможности UTF-8 и поддержка CityHash. |  

  Драйвер JDBC 7.0 также доступен в Maven Central Repository и может быть добавлен в проект Maven путем добавления в POM.XML следующего кода.  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.0.0.jre10</version>
</dependency>
```
  
**Microsoft JDBC Driver 6.4 для SQL Server**  

  Драйвер JDBC 6.4 содержит три библиотеки классов JAR в каждом пакете установки: **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** и **mssql-jdbc-6.4.0.jre9.jar**.

  JDBC Driver 6.4 рассчитан на совместимость и корректную работу со всеми основными виртуальными машинами Java, но протестирован только в OpenJDK 7.0, 8.0 и 9.0.
  
  На диаграмме ниже приведены сводные данные о поддержке, которую обеспечивают три JAR-файла в составе Microsoft JDBC Driver 6.4 для SQL Server.  
  
  |JAR|Соответствие версии JDBC|Рекомендуемая версия Java|Описание|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-6.4.0.jre7.jar|4.1|7|Требуется среда выполнения Java (JRE) версии 7.0. В случае использования JRE 6.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 6.4 относятся: проверка подлинности Azure AD для Linux, метод Principal/Password для Kerberos, автоматическое определение REALM в SPN для проверки подлинности междоменного доступа, ограниченное делегирование Kerberos, время ожидания запроса, время ожидания для сокета и повторное использование дескриптора подготовленной инструкции. |  
|mssql-jdbc-6.4.0.jre8.jar|4.2|8|Требуется среда выполнения Java (JRE) версии 8.0. В случае использования JRE 7.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 6.4 относятся: проверка подлинности Azure AD для Linux, метод Principal/Password для Kerberos, автоматическое определение REALM в SPN для проверки подлинности междоменного доступа, ограниченное делегирование Kerberos, время ожидания запроса, время ожидания для сокета и повторное использование дескриптора подготовленной инструкции. |  
|mssql-jdbc-6.4.0.jre9.jar|4.3|9|Требуется среда выполнения Java (JRE) версии 9.0. В случае использования JRE 8.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 6.4 относятся: проверка подлинности Azure AD для Linux, метод Principal/Password для Kerberos, автоматическое определение REALM в SPN для проверки подлинности междоменного доступа, ограниченное делегирование Kerberos, время ожидания запроса, время ожидания для сокета и повторное использование дескриптора подготовленной инструкции. |

Драйвер JDBC 6.4 также доступен в Maven Central Repository и может быть добавлен в проект Maven путем добавления в POM.XML следующего кода.

 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.4.0.jre9</version>
</dependency>
```

**Microsoft JDBC Driver 6.2 для SQL Server:**  
  
  Драйвер JDBC 6.2 содержит две библиотеки классов JAR: **mssql-jdbc-6.2.2.jre7.jar** и **mssql-jdbc-6.2.2.jre8.jar**.
  
 JDBC Driver 6.2 рассчитан на совместимость и корректную работу со всеми основными виртуальными машинами Java, но протестирован только в Sun JRE 5.0, 6.0, 7.0 и 8.0.
  
 На диаграмме ниже приведены сводные данные о поддержке, которую обеспечивают два JAR-файла в составе Microsoft JDBC Driver 6.0 и 4.2 для SQL Server.  
  
|JAR|Соответствие версии JDBC|Рекомендуемая версия Java|Описание|  
|---------|-----------------------------|----------------------|-----------------|
|mssql-jdbc-6.2.2.jre7.jar|4.1|7|Требуется среда выполнения Java (JRE) версии 7.0. В случае использования JRE 6.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 6.2 относятся: проверка подлинности Azure AD для Linux, метод Principal/Password для Kerberos, автоматическое определение REALM в SPN для проверки подлинности междоменного доступа, ограниченное делегирование Kerberos, время ожидания запроса, время ожидания для сокета и повторное использование дескриптора подготовленной инструкции. |  
|mssql-jdbc-6.2.3.jre8.jar|4.2|8|Требуется среда выполнения Java (JRE) версии 8.0. В случае использования JRE 7.0 или более ранней версии возникает исключение.<br /><br /> К новым функциям в версии 6.2 относятся: проверка подлинности Azure AD для Linux, метод Principal/Password для Kerberos, автоматическое определение REALM в SPN для проверки подлинности междоменного доступа, ограниченное делегирование Kerberos, время ожидания запроса, время ожидания для сокета и повторное использование дескриптора подготовленной инструкции|  

  Драйвер JDBC 6.2 также доступен в Maven Central Repository и может быть добавлен в проект Maven путем добавления в POM.XML следующего кода.
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.2.jre8</version>
</dependency>
```

 **Microsoft JDBC Driver 6.0 и 4.2 для SQL Server:**  
  
  Драйверы JDBC 6.0 и 4.2 включают в себя две библиотеки классов JAR: **sqljdbc41.jar** и **sqljdbc42.jar**.
  
 Драйверы JDBC Driver 6.0 и 4.2 рассчитаны на совместимость и корректную работу со всеми основными виртуальными машинами Java, однако протестированы только в Sun JRE 5.0, 6.0, 7.0 и 8.0.
  
 На диаграмме ниже приведены сводные данные о поддержке, которую обеспечивают два JAR-файла в составе Microsoft JDBC Driver 6.0 и 4.2 для SQL Server.  
  
|JAR|Соответствие версии JDBC|Рекомендуемая версия Java|Описание|  
|---------|-----------------------------|----------------------|-----------------|  
|sqljdbc41.jar|4.1|7|Требуется среда выполнения Java (JRE) версии 7.0. В случае использования JRE 6.0 или более ранней версии возникает исключение.<br /><br /> В число новых возможностей в пакетах 6.0 и 4.2 входит массовое копирование и соответствие JDBC 4.1.<br /><br /> Кроме этого, новые возможности, доступные только в пакете 6.0, включают в себя: Always Encrypted, возвращающие табличные значения параметры, проверку подлинности Azure Active Directory, прозрачные подключения к группам доступности AlwaysOn, улучшение параметров извлечения метаданных для подготовленных запросов и международного доменного имени (IDN)|  
|sqljdbc42.jar|4.2|8|Требуется среда выполнения Java (JRE) версии 8.0. В случае использования JRE 7.0 или более ранней версии возникает исключение.<br /><br /> В число новых возможностей в пакетах 6.0 и 4.2 входит массовое копирование, соответствие JDBC 4.1 и соответствие JDBC 4.2.<br /><br /> Кроме этого, новые возможности, доступные только в пакете 6.0, включают в себя: Always Encrypted, возвращающие табличные значения параметры, проверку подлинности Azure Active Directory, прозрачные подключения к группам доступности AlwaysOn, улучшение параметров извлечения метаданных для подготовленных запросов и международного доменного имени (IDN)|  
  
 **Microsoft JDBC Driver 4.1 для SQL Server:**  
  
 Драйвер JDBC 4.1 включает в себя одну библиотеку классов JAR: **sqljdbc41.jar**.  

|JAR|Описание|  
|---------|-----------------|  
|sqljdbc41.jar|Библиотека классов **sqljdbc41.jar** включает поддержку для API JDBC 4.0. Она включает в себя все функции драйвера JDBC 4.0, а также методы API JDBC 4.0. JDBC 4.1 не поддерживается (выдается исключение SQLFeatureNotSupportedException).<br /><br /> Библиотеке классов **sqljdbc41.jar** требуется среда выполнения Java (JRE) версии 7.0. В случае использования **sqljdbc41.jar** в JRE 6.0 и 5.0 возникает исключение.<br /><br />
  
 JDBC Driver рассчитан на совместимость и корректную работу со всеми основными виртуальными машинами Java, но протестирован только в Sun JRE 5.0, 6.0 и 7.0.
  
 На диаграмме ниже приведены сводные данные о поддержке, которую обеспечивает JAR-файл в составе Microsoft JDBC Driver 4.1 для SQL Server.  
  
|JAR|Версия JDBC|JRE (можно выполнять)|JDK (можно компилировать)|  
|---------|------------------|---------------------|-------------------------|  
|sqljdbc41.jar|4|7|7 6 5|  
  
## <a name="sql-server-requirements"></a>Требования к SQL Server  

 Драйвер JDBC поддерживает подключения к базе данных Azure SQL и SQL Server. Для драйвера Microsoft JDBC 4.2 и 4.1 для SQL Server поддержка начинается с SQL Server 2008.
  
## <a name="operating-system-requirements"></a>Требования к операционной системе  

 Драйвер JDBC разработан для использования с любой операционной системой, поддерживающей использование виртуальной машины Java (JVM). Однако официально протестированы только системы Sun Solaris, SUSE Linux, Ubuntu Linux, CentOS Linux, macOS и Windows.  
  
## <a name="supported-languages"></a>Поддерживаемые языки  

 Драйвер JDBC поддерживает все параметры сортировки столбцов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения о параметрах сортировки, поддерживаемых JDBC Driver, см. в описании [функций поддержки разных языков JDBC Driver](../../connect/jdbc/international-features-of-the-jdbc-driver.md).  
  
 Дополнительные сведения о параметрах сортировки см. в разделе "Использование параметров сортировки" электронной документации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>См. также раздел  

 [Общие сведения о JDBC Driver](../../connect/jdbc/overview-of-the-jdbc-driver.md)  

---
title: Матрица поддержки драйвера Microsoft JDBC Driver for SQL Server
description: В этой статье приведены матрица и политика жизненного цикла поддержки для драйвера Microsoft JDBC Driver for SQL Server.
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: c5769e67-99f7-4bc1-a4fa-8941dad33d35
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2fb84ce512cca3e87f0ec108698928caa2934122
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835364"
---
# <a name="microsoft-jdbc-driver-for-sql-server-support-matrix"></a>Матрица поддержки драйвера Microsoft JDBC Driver for SQL Server

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  В этой статье приведены матрица и политика жизненного цикла поддержки для драйвера Microsoft JDBC Driver for SQL Server.  
  
## <a name="microsoft-jdbc-driver-support-lifecycle-matrix-and-policy"></a>Матрица и политика жизненного цикла поддержки для драйвера Microsoft JDBC  

Политика жизненного цикла поддержки Майкрософт (MSL) предоставляет понятную и предсказуемую информацию о жизненном цикле поддержки продуктов Майкрософт. Основная фаза поддержки драйверов JDBC Driver версий 4.x, 6.x, 7.x, 8.x и 9.x длится пять лет с момента выпуска соответствующей версии. Основная фаза поддержки определена на веб-сайте жизненного цикла поддержки Майкрософт.  
  
Возможность расширенной или настраиваемой поддержки драйвера JDBC не предусмотрена.  

Перечисленные ниже версии драйверов Microsoft JDBC будут поддерживаться до указанной даты окончания поддержки.  
  
|Имя драйвера|Версия пакета драйвера|Применимые JAR(s)|Окончание основной фазы поддержки|
|-|-|-|-|  
|Microsoft JDBC Driver 9.2 для SQL Server|9.2|mssql-jdbc-9.2.1.jre15.jar<br> mssql-jdbc-9.2.1.jre11.jar<br> mssql-jdbc-9.2.1.jre8.jar|29 января 2026 г.|
|Microsoft JDBC Driver 8.4 для SQL Server|8.4|mssql-jdbc-8.4.1.jre14.jar<br> mssql-jdbc-8.4.1.jre11.jar<br> mssql-jdbc-8.4.1.jre8.jar|31 июля 2025 г.|
|Microsoft JDBC Driver 8.2 для SQL Server|8.2|mssql-jdbc-8.2.2.jre13.jar<br> mssql-jdbc-8.2.2.jre11.jar<br> mssql-jdbc-8.2.2.jre8.jar|31 января 2025 г.|
|Microsoft JDBC Driver 7.4 для SQL Server|7.4|mssql-jdbc-7.4.1.jre12.jar<br> mssql-jdbc-7.4.1.jre11.jar<br> mssql-jdbc-7.4.1.jre8.jar|31 июля 2024 г.|
|Microsoft JDBC Driver 7.2 для SQL Server|7.2|mssql-jdbc-7.2.2.jre11.jar<br> mssql-jdbc-7.2.2.jre8.jar|31 января 2024 г.|
|Драйвер Microsoft JDBC 7.0 для SQL Server|7.0|mssql-jdbc-7.0.0.jre10.jar<br> mssql-jdbc-7.0.0.jre8.jar|31 июля 2023 г.|
|Драйвер Microsoft JDBC 6.4 для SQL Server|6.4|mssql-jdbc-6.4.0.jre9.jar<br> mssql-jdbc-6.4.0.jre8.jar<br> mssql-jdbc-6.4.0.jre7.jar|27 февраля 2023 г.|
|Microsoft JDBC Driver 6.2 для SQL Server|6.2|mssql-jdbc-6.2.2.jre8.jar<br> mssql-jdbc-6.2.2.jre7.jar|30 июня 2022 г.|
|Драйвер Microsoft JDBC Driver 6.0 для SQL Server|6,0|sqljdbc42.jar<br>sqljdbc41.jar|14 июля 2021 г.|
  
 Следующие драйверы Microsoft JDBC больше не поддерживаются.  

|Имя драйвера|Версия пакета драйвера|Окончание основной фазы поддержки|  
|-|-|-|
|Microsoft JDBC Driver 4.2 for SQL Server|4.2|24 августа 2020 г.|
|Microsoft JDBC Driver 4.1 for SQL Server|4.1|12 декабря 2019 г.|  
|Microsoft JDBC Driver 4.0 for SQL Server|4,0|6 марта 2017 г.|  
|Драйвер JDBC 3.0 для Microsoft SQL Server|3.0|23 апреля 2015 г.|  
|Драйвер JDBC 2.0 для Microsoft SQL Server|2.0|31 декабря 2012 г.|  
|Драйвер JDBC 1.2 для Microsoft SQL Server 2005|1.2|25 июня 2011 г.|  
|Драйвер JDBC 1.1 для Microsoft SQL Server 2005|1,1|25 июня 2011 г.|  
|Драйвер JDBC 1.0 для Microsoft SQL Server 2005|1.0|25 июня 2011 г.|  
|Драйвер JDBC для Microsoft SQL Server 2000|2000|9 июля 2010 г.|  
  
## <a name="sql-version-compatibility"></a>Совместимость с версиями SQL  
  
|Версия базы данных&nbsp;&#8594;<br />&#8595; Версия драйвера|База данных SQL Azure|Azure Synapse Analytics|Управляемый экземпляр SQL Azure|SQL Server 2019|SQL Server 2017|SQL Server 2016|SQL Server 2014|SQL Server 2012|PDW 2008R2 AU3<sup>4</sup>|SQL Server 2008 R2|SQL Server 2008|
|---|---|---|---|---|---|---|---|---|---|---|---|
|9.2|Да|Да|Да|Да|Да|Да|Да|Да|   |   |   |
|8.4|Да|Да|Да|Да|Да|Да|Да|Да|Да|   |   |
|8.2|Да|Да|Да|Да|Да|Да|Да|Да|Да|   |   |
|7.4|Да|Да|Да|Да|Да|Да|Да|Да|Да|   |   |
|7.2|Да|Да|Да|   |Да|Да|Да|Да|Да|Да|   |
|7.0|Да|Да|Да|   |Да|Да|Да|Да|Да|Да|   |
|6.4|Да|Да|Да|   |Да|Да|Да|Да|Да|Да|   |
|6.2|Да|Да|   |   |Да|Да|Да|Да|Да|Да|Да|
|6.1|Да|   |   |   |   |Да|Да|Да|Да|Да|Да|
|6,0|Да|   |   |   |   |Да|Да|Да|Да|Да|Да|
|4.2|Да|   |   |   |   |Да|Да|Да|Да|Да|Да|
|4.1|Да|   |   |   |   |Да|Да|Да|Да|Да|Да|
|4,0|Да|   |   |   |   |Да|Да|Да|Да|Да|Да|
|3.0|Да<sup>2</sup>|   |   |   |   |   |Да<sup>5</sup>|Да<sup>1</sup>|   |Да|Да|
|2.0|   |   |   |   |   |   |   |   |   |Да<sup>3</sup>|Да<sup>3</sup>|
|1.2|   |   |   |   |   |   |   |   |   |   |Да<sup>3</sup>|

 <sup>1</sup> Microsoft JDBC Driver для SQL Server версии 3.0 может подключаться SQL Server 2012 в качестве клиента нижнего уровня.  
  
 <sup>2</sup> В версии 3.0 в форме исправления добавлена поддержка базы данных SQL Azure. Мы рекомендуем пользователям баз данных SQL Azure использовать последнюю версию драйвера.  
  
 <sup>3</sup> Microsoft JDBC Driver для SQL Server версии 2.0 и Microsoft JDBC Driver для SQL Server 2005 версии 1.2 могут подключаться к SQL Server 2008 в качестве клиента нижнего уровня. Когда допускается подключение к клиенту более низкого уровня с последующим преобразованием данных, приложения могут выполнять запросы и выполнять обновления для таких новых типов данных SQL Server 2008: time, date, datetime2, datetimeoffset и FILESTREAM. Дополнительные сведения о том, как использовать эти новые типы данных с помощью драйвера JDBC, см. в статьях  [Working with SQL Server 2008 Date/Time Data Types using v1.2 JDBC driver](/archive/blogs/jdbcteam/) (Использование типов данных Date и Time в SQL Server 2008 с помощью драйвера JDBC 1.2) и  [Working with SQL Server 2008 Filestream using v1.2 JDBC driver](/archive/blogs/jdbcteam/)(Использование типа данных Filestream в SQL Server 2008 с помощью драйвера JDBC). Дополнительные сведения об обратной совместимости новых типов данных см. в статьях  [Использование данных даты и времени](/previous-versions/sql/sql-server-2008-r2/ms180878(v=sql.105))и  [Поддержка FILESTREAM](../../relational-databases/native-client/features/filestream-support.md) в электронной документации по SQL Server.  
  
 <sup>4</sup> Поддержка подключений между Microsoft JDBC Driver и хранилищем Parallel Data Warehouse была впервые реализована в Microsoft JDBC Driver 4.0 для SQL Server и обновлении 3 для устройства Parallel Data Warehouse в Microsoft SQL Server 2008 R2.  
  
 <sup>5</sup> Microsoft JDBC Driver для SQL Server версии 3.0 может подключаться SQL Server 2014 в качестве клиента нижнего уровня.  
  
## <a name="java-and-jdbc-specification-support"></a>Поддержка спецификаций Java и JDBC
  
|Версия драйвера JDBC|Версия JRE|Версия API JDBC|
|-|-|-|
|[9.2](release-notes-for-the-jdbc-driver.md#92)|1.8, 11, 15|4.2, 4.3 (частично)|
|[8.4](release-notes-for-the-jdbc-driver.md#84)|1.8, 11, 14|4.2, 4.3 (частично)|
|[8.2](release-notes-for-the-jdbc-driver.md#82)|1.8, 11, 13|4.2, 4.3 (частично)|
|[7.4](release-notes-for-the-jdbc-driver.md#74)|1.8, 11, 12|4.2, 4.3 (частично)|
|[7.2](release-notes-for-the-jdbc-driver.md#72)|1.8, 11|4.2, 4.3 (частично)|
|[7.0](release-notes-for-the-jdbc-driver.md#70)|1.8, 10|4.2, 4.3 (частично)|
|[6.4](release-notes-for-the-jdbc-driver.md#64)|1.7, 1.8, 9|4.1, 4.2, 4.3 (частично)|
|[6.2](release-notes-for-the-jdbc-driver.md#62)|1.7, 1.8|4.1, 4.2|
|[6.1](release-notes-for-the-jdbc-driver.md#61)|1.7, 1.8|4.1, 4.2|
|[6.0](release-notes-for-the-jdbc-driver.md#60)|1.7, 1.8|4.1, 4.2|
|4.2|1.7, 1.8|4.1, 4.2|
|4.1|1.7|4,0|
|4,0|1.5, 1.6, 1.7|3.0, 4.0|
|3.0|1.5, 1.6,|3.0, 4.0|
|2.0|1.5, 1.6|3.0, 4.0|
|1.2|1.4, 1.5, 1.6|3.0|
|1,1|1.4|3.0|
|1.0|1.4|3.0|
|2000|1.4|3.0|

## <a name="supported-operating-systems"></a>Поддерживаемые операционные системы

Драйвер JDBC может работать со всеми операционными системами, поддерживающими виртуальную машину Java (JVM). Вот некоторые из наиболее распространенных систем: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2008 R2, Linux, Unix, AIX, macOS и др.  

Разработчики JDBC тестируют драйвер в операционных системах Windows, Sun Solaris, SUSE Linux, Ubuntu Linux, CentOS Linux и macOS.

## <a name="application-server-support"></a>Поддержка сервера приложений

Драйвер Microsoft JDBC Driver for SQL Server тестируется на совместимость с различными серверами приложений.  Обратитесь к поставщику вашего сервера приложений, чтобы узнать, какая версия драйвера совместима с их продуктом.
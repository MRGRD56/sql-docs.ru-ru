---
description: Сведения о соответствии JDBC Driver для SQL Server спецификации JDBC 4.1.
title: Соответствие JDBC 4.1 для JDBC Driver
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: f087fd40-8451-478e-b465-43112c711515
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 60fef7e2a6340e8d450732b814035ff8e92714c8
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633201"
---
# <a name="jdbc-41-compliance-for-the-jdbc-driver"></a>Соответствие JDBC 4.1 для JDBC Driver

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

> [!NOTE]
> Версии Microsoft JDBC Driver для SQL Server до 4.2 соответствуют спецификациям Java Database Connectivity API 4.0. Этот раздел не применяется к версиям до 4.2.

Спецификация Java Database Connectivity API 4.1 поддерживается драйвером Microsoft JDBC Driver 4.2 для SQL Server со следующими методами API.

## <a name="sqlserverconnection-class"></a>SQLServerConnection, класс

|Новый метод|Описание|Реализация драйвера JDBC|
|----------------|-----------------|--------------------------------|
|void abort(Executor executor)|Завершает открытое соединение с SQL Server.|Реализовано, как описано в интерфейсе java.sql.Connection. Дополнительные сведения см. на странице о [java.sql.Connection](https://docs.oracle.com/javase/7/docs/api/java/sql/Connection.html).|
|void setSchema(String schema)|Задает схему для текущего соединения.|SQL Server не поддерживает установку схемы для текущего сеанса. Драйвер автоматически записывает в журнал предупреждение, если этот метод вызывается. Дополнительные сведения см. на странице о [java.sql.Connection](https://docs.oracle.com/javase/7/docs/api/java/sql/Connection.html).|
|String getSchema()|Возвращает имя схемы для текущего соединения.|Поскольку SQL Server не поддерживает установку схемы для текущего соединения, драйвер возвращает пользовательскую схему по умолчанию. Дополнительные сведения см. на странице о [java.sql.Connection](https://docs.oracle.com/javase/7/docs/api/java/sql/Connection.html).|

## <a name="sqlserverdatabasemetadata-class"></a>SQLServerDatabaseMetaData, класс

|Новый метод|Описание|Реализация драйвера JDBC|
|----------------|-----------------|--------------------------------|
|boolean generatedKeyAlwaysReturned()|Возвращает значение true, если драйвер поддерживает получение созданных ключей.|Реализовано, как описано в java.sql. Интерфейс DatabaseMetaData. Дополнительные сведения см. на странице о [java.sql.DatabaseMetaData](https://docs.oracle.com/javase/7/docs/api/java/sql/DatabaseMetaData.html).|
|ResultSet getPseudoColumns(String catalog, String schemaPattern, String tableNamePattern, String columnNamePattern)|Извлекает описание псевдостолбцов или скрытых столбцов.|Возвращает пустой результирующий набор, так как в SQL Server нет формального понятия псевдостолбцов. Дополнительные сведения см. на странице о [java.sql.DatabaseMetaData](https://docs.oracle.com/javase/7/docs/api/java/sql/DatabaseMetaData.html).|

## <a name="sqlserverstatement-class"></a>SQLServerStatement, класс

|Новый метод|Описание|Реализация драйвера JDBC|
|----------------|-----------------|--------------------------------|
|void closeOnCompletion()|Указывает, что эта инструкция будет закрыта после закрытия всех зависимых результирующих наборов.|Реализовано, как описано в интерфейсе java.sql.Statement. Дополнительные сведения см. на странице о [java.sql.Statement](https://docs.oracle.com/javase/7/docs/api/java/sql/Statement.html).|
|boolean isCloseOnCompletion()|Возвращает значение, указывающее, будет ли эта инструкция закрываться при закрытии всех зависимых результирующих наборов.|Реализовано, как описано в интерфейсе java.sql.Statement. Дополнительные сведения см. на странице о [java.sql.Statement](https://docs.oracle.com/javase/7/docs/api/java/sql/Statement.html).|

 Спецификация Java Database Connectivity API 4.1 поддерживается драйвером Microsoft JDBC 4.2 для SQL Server со следующими возможностями.

|Новая функция|Описание|
|-----------------|-----------------|
|Новая функция Escape<br /><br /> Ограниченная функция Escape возврата строк|Поддерживается частично<br /><br /> Escape-синтаксис: LIMIT \<rows> [OFFSET <смещение_строк>](using-sql-escape-sequences.md).|

Спецификация Java Database Connectivity API 4.1 поддерживается драйвером Microsoft JDBC 4.2 для SQL Server со следующими сопоставлениями типов данных.

|Сопоставление типов данных|Описание|
|------------------------|-----------------|
|В методах PreparedStatement.setObject() и PreparedStatement.setNull() теперь поддерживаются новые сопоставления типов данных.|1. Новое сопоставление типов Java с JDBC<br /><br /> (а) java.math.BigInteger с JDBC BIGINT<br /><br /> (б) java.util.Date и java.util.Calendar с JDBC TIMESTAMP<br /><br /> 2. Новые преобразования типов данных:<br /><br /> (а) java.math.BigInteger с CHAR, VARCHAR, LONGVARCHAR и BIGINT<br /><br /> (б) java.util.Date и java.util.Calendar с CHAR, VARCHAR, LONGVARCHAR, DATE, TIME и TIMESTAMP<br /><br /> Дополнительные сведения см. в спецификации JDBC 4.1.|

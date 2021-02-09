---
title: API массового копирования для операции пакетной вставки в JDBC
description: Microsoft JDBC Driver for SQL Server поддерживает использование массового копирования для пакетных операций вставки, что позволяет быстрее загружать данные в базу данных.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4a769d73f799b8ca0b4b806a3e656517377e23ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161572"
---
# <a name="using-bulk-copy-api-for-batch-insert-operation"></a>Использование API массового копирования для операции пакетной вставки

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Microsoft JDBC Driver for SQL Server версии 9.2 и более поздних поддерживает использование интерфейса API массового копирования для пакетных операций вставки. Эта функция позволяет активировать в драйвере выполнение операции массового копирования при выполнении операций пакетной вставки. Драйвер позволяет повысить производительность вставки аналогичного объема данных по сравнению с обычной операцией пакетной вставки. Драйвер анализирует SQL-запрос пользователя, используя интерфейс API массового копирования вместо обычной операции пакетной вставки. Ниже приведены различные способы включения интерфейс API массового копирования, а также список ограничений. На этой странице также содержится небольшой пример кода, демонстрирующий использование и увеличение производительности.

Эта функция применима только к API-интерфейсам `executeBatch()` & `executeLargeBatch()` объектов PreparedStatement и CallableStatement.

## <a name="prerequisites"></a>Предварительные требования

Предварительные требования для включения интерфейса API для пакетной вставки.

* Требуется запрос INSERT (запрос может содержать комментарии, но должен начинаться с ключевого слова INSERT, чтобы эта функция действовала).

## <a name="enabling-bulk-copy-api-for-batch-insert"></a>Включение API массового копирования для операции пакетной вставки

Существует три способа включения API-интерфейса массового копирования для пакетной вставки.

### <a name="1-enabling-with-connection-property"></a>1. Включение с помощью свойства подключения

Эту функцию можно включить, добавив параметр **useBulkCopyForBatchInsert=true;** в строку подключения.

```java
Connection connection = DriverManager.getConnection("jdbc:sqlserver://<server>:<port>;userName=<user>;password=<password>;database=<database>;useBulkCopyForBatchInsert=true;");
```

### <a name="2-enabling-with-setusebulkcopyforbatchinsert-method-from-sqlserverconnection-object"></a>2. Включение с помощью метода setUseBulkCopyForBatchInsert() из объекта SQLServerConnection

Эту функцию можно включить с помощью вызова **SQLServerConnection.setUseBulkCopyForBatchInsert(true)** .

Вызов **SQLServerConnection.getUseBulkCopyForBatchInsert()** извлекает текущее значение свойства подключения **useBulkCopyForBatchInsert**.

Значение параметра **useBulkCopyForBatchInsert** остается постоянным для каждой инструкции PreparedStatement во время инициализации. Последующие вызовы **SQLServerConnection.setUseBulkCopyForBatchInsert()** не влияют на уже созданное значение PreparedStatement.

### <a name="3-enabling-with-setusebulkcopyforbatchinsert-method-from-sqlserverdatasource-object"></a>3. Включение с помощью метода setUseBulkCopyForBatchInsert() из объекта SQLServerDataSource

Выполняется аналогично приведенному выше, но объект SQLServerConnection создается с помощью класса SQLServerDataSource. Оба метода дают одинаковый результат.

## <a name="known-limitations"></a>Известные ограничения

В настоящее время к этой функции применяются следующие ограничения.

* Запросы вставки, содержащие непараметризованные значения (например, `INSERT INTO TABLE VALUES (?, 2`), не поддерживаются. Подстановочные знаки (?) — единственные поддерживаемые параметры для этой функции.
* Запросы вставки, содержащие выражения INSERT-SELECT (например, `INSERT INTO TABLE SELECT * FROM TABLE2`), не поддерживаются.
* Запросы вставки, содержащие несколько выражений VALUE (например, `INSERT INTO TABLE VALUES (1, 2) (3, 4)`), не поддерживаются.
* Запросы вставки, за которыми следует предложение OPTION, объединенное с несколькими таблицами, или за которыми следует другой запрос, не поддерживаются.
* Из-за ограничений, связанных с API массового копирования, типы данных `MONEY`, `SMALLMONEY`, `DATE`, `DATETIME`, `DATETIMEOFFSET`, `SMALLDATETIME`, `TIME`, `GEOMETRY` и `GEOGRAPHY` в настоящее время не поддерживаются для этой функции.

Если запрос не выполняется из-за ошибок, не связанных с SQL Server, драйвер регистрирует сообщение об ошибке и возвращает исходную логику для пакетной вставки.

## <a name="example"></a>Пример

Этот пример демонстрирует вариант использования операции пакетной вставки с тысячами строк для обоих сценариев (обычная вставка и API массового копирования).

```java
    public static void main(String[] args) throws Exception
    {
        String tableName = "batchTest";
        String tableNameBulkCopyAPI = "batchTestBulk";

        String connectionUrl = "jdbc:sqlserver://<server>:<port>;databaseName=<database>;user=<user>;password=<password>";

        try (Connection con = DriverManager.getConnection(connectionUrl);
                Statement stmt = con.createStatement();
                PreparedStatement pstmt = con.prepareStatement("insert into " + tableName + " values (?, ?)");) {

            String dropSql = "if exists (select * from dbo.sysobjects where id = object_id(N'[dbo].[" + tableName + "]') and OBJECTPROPERTY(id, N'IsUserTable') = 1) DROP TABLE [" + tableName + "]";
            stmt.execute(dropSql);

            String createSql = "create table " + tableName + " (c1 int, c2 varchar(20))";
            stmt.execute(createSql);

            System.out.println("Starting batch operation using regular batch insert operation.");
            long start = System.currentTimeMillis();
            for (int i = 0; i < 1000; i++) {
                pstmt.setInt(1, i);
                pstmt.setString(2, "test" + i);
                pstmt.addBatch();
            }
            pstmt.executeBatch();

            long end = System.currentTimeMillis();

            System.out.println("Finished. Time taken : " + (end - start) + " milliseconds.");
        }

        try (Connection con = DriverManager.getConnection(connectionUrl + ";useBulkCopyForBatchInsert=true");
                Statement stmt = con.createStatement();
                PreparedStatement pstmt = con.prepareStatement("insert into " + tableNameBulkCopyAPI + " values (?, ?)");) {

            String dropSql = "if exists (select * from dbo.sysobjects where id = object_id(N'[dbo].[" + tableNameBulkCopyAPI + "]') and OBJECTPROPERTY(id, N'IsUserTable') = 1) DROP TABLE [" + tableNameBulkCopyAPI + "]";
            stmt.execute(dropSql);

            String createSql = "create table " + tableNameBulkCopyAPI + " (c1 int, c2 varchar(20))";
            stmt.execute(createSql);

            System.out.println("Starting batch operation using Bulk Copy API.");
            long start = System.currentTimeMillis();
            for (int i = 0; i < 1000; i++) {
                pstmt.setInt(1, i);
                pstmt.setString(2, "test" + i);
                pstmt.addBatch();
            }
            pstmt.executeBatch();

            long end = System.currentTimeMillis();

            System.out.println("Finished. Time taken : " + (end - start) + " milliseconds.");
        }
    }
```

Результат:

```bash
Starting batch operation using regular batch insert operation.
Finished. Time taken : 104132 milliseconds.
Starting batch operation using Bulk Copy API.
Finished. Time taken : 1058 milliseconds.
```

## <a name="see-also"></a>См. также раздел

[Повышение производительности и надежности с помощью JDBC Driver](improving-performance-and-reliability-with-the-jdbc-driver.md)

---
description: Узнайте о полной обработке результатов, включая несколько результирующих наборов, из выполнения запроса в драйвере JDBC.
title: Анализ результатов
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: rene-ye
ms.author: v-reye
manager: kenvh
ms.openlocfilehash: 1605fd65414a81acc0912d774a24ff4ae10a1c9c
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793029"
---
# <a name="parsing-the-results"></a>Анализ результатов

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

В этой статье описывается, как SQL Server ожидает от пользователей полной обработки результатов, возвращаемых по любому запросу.

## <a name="update-counts-and-result-sets"></a>Счетчик обновлений и результирующие наборы

В этом разделе будут рассмотрены два наиболее распространенных результата, возвращаемых с SQL Server: Счетчик обновлений и ResultSet. Как правило, любой выполняемый пользователем запрос приводит к возврату одного из этих результатов. Ожидается, что при обработке результатов пользователи будут обрабатывать оба эти результата.

Следующий код является примером того, как пользователь может выполнить итерацию по всем результатам, полученным с сервера:

```java
try (Connection connection = DriverManager.getConnection(URL); Statement statement = connection.createStatement()) {
    boolean resultsAvailable = statement.execute(USER_SQL);
    int updateCount = -2;
    while (true) {
        updateCount = statement.getUpdateCount();
        if (!resultsAvailable && updateCount == -1)
            break;
        if (resultsAvailable) {
            // handle ResultSet
        } else {
            // handle Update Count
        }
        resultsAvailable = statement.getMoreResults();
    }
}
```

## <a name="exceptions"></a>Исключения

При выполнении инструкции, которая приводит к отображению ошибки или информационного сообщения, SQL Server будет отвечать по-разному. Например, если создать план выполнения не удается, сообщение об ошибке будет создано немедленно в `execute()`. Если ошибка связана с обработкой инструкций, приложению необходимо проанализировать результирующий набор, чтобы увидеть исключение.

Исключение возникает немедленно, если SQL Server не удается создать план выполнения.

```java
String SQL = "SELECT * FROM nonexistentTable;";
try (Statement statement = connection.createStatement();) {
    statement.execute(SQL);
} catch (SQLException e) {
    e.printStackTrace();
}
```

Когда SQL Server возвращает сообщение об ошибке в результирующем наборе, этот набор необходимо обработать, чтобы получить исключение.

```java
String SQL = "SELECT 1/0;";
try (Statement statement = connection.createStatement();) {
    boolean hasResult = statement.execute(SQL);
    if (hasResult) {
        try (ResultSet rs = statement.getResultSet()) {
            // Exception is thrown on next().
            while (rs.next()) {}
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

Если при выполнении инструкции создается несколько результирующих наборов, каждый из них необходимо обрабатывать до обнаружения набора, содержащего исключение.

```java
String SQL = "SELECT 1; SELECT * FROM nonexistentTable;";
try (Statement statement = connection.createStatement();) {
    // Does not throw an exception on execute().
    boolean hasResult = statement.execute(SQL);
    while (hasResult) {
        try (ResultSet rs = statement.getResultSet()) {
            while (rs.next()) {
                System.out.println(rs.getString(1));
            }
        }
        // Moves the next result set that generates the exception.
        hasResult = statement.getMoreResults();
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

Например, `String SQL = "SELECT * FROM nonexistentTable; SELECT 1;";` создает исключение немедленно в `execute()` и `SELECT 1` не выполняется вообще.

Если ошибка от SQL Server имеет уровень серьезности от `0` до `9`, она считается информационным сообщением и возвращается в виде `SQLWarning`.

```java
String SQL = "RAISERROR ('WarningLevel5', 5, 2);";
try (Statement statement = connection.createStatement();) {
    boolean hasResult = statement.execute(SQL);
    SQLWarning warning = statement.getWarnings();
    System.out.println(warning);
}
```

## <a name="see-also"></a>См. также раздел

[Общие сведения о JDBC Driver](overview-of-the-jdbc-driver.md)

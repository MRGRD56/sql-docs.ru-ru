---
description: Сведения о типе данных sql_variant в драйвере JDBC и его использовании для поддержки параметров, возвращающих табличные значения (TVP), и операций массового копирования.
title: Использование данных типа sql_variant
ms.custom: ''
ms.date: 03/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dff69a149a8acf50aafa02653a6a083354dfa993
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633297"
---
# <a name="using-sql_variant-data-type"></a>Использование данных типа sql_variant

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Начиная с версии 6.3.0, драйвер JDBC поддерживает тип данных sql_variant. Sql_variant также поддерживается при использовании таких функций, как возвращающие табличные значения параметры и BulkCopy с некоторыми ограничениями. В типе данных sql_variant можно хранить не все типы данных. Список поддерживаемых типов данных с sql_variant см. в статье о [sql_variant (Transact-SQL)](../../t-sql/data-types/sql-variant-transact-sql.md).

## <a name="populating-and-retrieving-a-table"></a>Заполнение и получение таблицы

Предположим, что у одного столбца есть таблица со столбцом sql_variant:

```sql
CREATE TABLE sampleTable (col1 sql_variant)
```

Пример скрипта для вставки значений с помощью оператора:

```java
try (Statement stmt = connection.createStatement()){
    stmt.execute("insert into sampleTable values (1)");
}
```

Вставка значения с помощью подготовленного оператора:

```java
try (PreparedStatement preparedStatement = con.prepareStatement("insert into sampleTable values (?)")) {
    preparedStatement.setObject(1, 1);
    preparedStatement.execute();
}
```

Если известен базовый тип передаваемых данных, можно использовать соответствующий метод задания. Например, при вставке значения с целым числом можно использовать `preparedStatement.setInt()`.

```java
try (PreparedStatement preparedStatement = con.prepareStatement("insert into table values (?)")) {
    preparedStatement.setInt (1, 1);
    preparedStatement.execute();
}
```

Для чтения значений из таблицы можно использовать соответствующие методы получения. Например, если поступающие от сервера значения известны, можно использовать метод `getInt()` или `getString()`:

```java
try (SQLServerResultSet resultSet = (SQLServerResultSet) stmt.executeQuery("select * from sampleTable ")) {
    resultSet.next();
    resultSet.getInt(1); //or rs.getString(1); or rs.getObject(1);
}
```

## <a name="using-stored-procedures-with-sql_variant"></a>Использование хранимых процедур с sql_variant

При наличии хранимой процедуры, например:

```java
String sql = "CREATE PROCEDURE " + inputProc + " @p0 sql_variant OUTPUT AS SELECT TOP 1 @p0=col1 FROM sampleTable ";
```

Выходные параметры должны быть зарегистрированы:

```java
try (CallableStatement callableStatement = con.prepareCall(" {call " + inputProc + " (?) }")) {
    callableStatement.registerOutParameter(1, microsoft.sql.Types.SQL_VARIANT);
    callableStatement.execute();
}
```

## <a name="limitations-of-sql_variant"></a>Ограничения sql_variant

- При использовании TVP для заполнения таблицы `datetime`/`smalldatetime`/`date` значения, хранящегося в sql_variant, вызов `getDateTime()`/`getSmallDateTime()`/`getDate()` в ResultSet не будет работать и вызовет следующее исключение:

    `Java.lang.String cannot be cast to java.sql.Timestamp`

    Обходное решение: вместо этого используйте `getString()` или `getObject()`.

- Использование TVP для заполнения таблицы и отправка нулевого значения в sql_variant не поддерживается. В противном случае вызывается исключение:

    `Inserting null value with column type sql_variant in TVP is not supported.`

## <a name="see-also"></a>См. также раздел

[Основные сведения о типах данных JDBC Driver](understanding-the-jdbc-driver-data-types.md)

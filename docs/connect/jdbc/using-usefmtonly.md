---
description: Получение ParameterMetaData через useFmtOnly
title: Получение ParameterMetaData через useFmtOnly | Документация Майкрософт
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ''
caps.latest.revision: ''
author: rene-ye
ms.author: v-reye
manager: kenvh
ms.openlocfilehash: a88707cc4679343c2e29a86ecc3a9efb8a9719dd
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837455"
---
# <a name="retrieving-parametermetadata-via-usefmtonly"></a>Получение ParameterMetaData через useFmtOnly
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Драйвер Microsoft JDBC для SQL Server содержит альтернативный способ запроса метаданных параметров из сервера — **useFmtOnly**. Эта функция впервые появилась в драйвере версии 7.4 и является обязательной для решения известных проблем в `sp_describe_undeclared_parameters`.
  
  В основном для запроса метаданных параметр "драйвер" использует хранимую процедуру `sp_describe_undeclared_parameters`, так как в большинстве случаев это рекомендуемый подход к получению метаданных параметров. Однако выполнение хранимой процедуры завершается ошибкой в следующих случаях:
  
-   Для столбцов Always Encrypted
  
-   Для временных таблиц и табличных переменных
  
-   Для представлений 
  
  Предлагаемое решение для этих вариантов использования — анализ SQL-запроса пользователя для параметров и таблиц целевых объектов, а затем выполнение запроса `SELECT` с включенным `FMTONLY`. Следующий фрагмент кода поможет визуализировать эту функцию.
  
```sql
--create a normal table 'Foo' and a temporary table 'Bar'
CREATE TABLE Foo(c1 int);
CREATE TABLE #Bar(c1 int);

EXEC sp_describe_undeclared_parameters N'SELECT * FROM Foo WHERE c1 = @p0' --works fine
EXEC sp_describe_undeclared_parameters N'SELECT * FROM #Bar WHERE c1 = @p0' --fails with "Invalid object name '#Bar'"

SET FMTONLY ON;
SELECT c1 FROM Foo; --works
SET FMTONLY OFF;
SET FMTONLY ON;
SELECT c1 FROM #Bar; --works
SET FMTONLY OFF;
```
 
## <a name="turning-the-feature-onoff"></a>Включение или выключение компонента 
 Компонент **useFmtOnly** отключен по умолчанию. Пользователи могут включить эту функцию в строке подключения, указав `useFmtOnly=true`. Например: `jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;useFmtOnly=true;`.
 
 Кроме того, эта функция доступна через `SQLServerDataSource`.
 ```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName(<server>);
ds.setPortNumber(<port>);
ds.setDatabaseName("<databaseName>");
ds.setUser("<user>");
ds.setPassword("<password>");
ds.setUseFmtOnly(true);
try (Connection c = ds.getConnection()) {
    // do work with connection
}
 ```
 
 Эта функция также доступна на уровне инструкции. Пользователи могут включить или отключить этот компонент с помощью `PreparedStatement.setUseFmtOnly(boolean)`.
> [!NOTE]  
>  Драйвер будет определять приоритет свойства уровня инструкции для свойства уровня соединения.

## <a name="using-the-feature"></a>Использование компонента
  После включения драйвер будет использовать новый компонент вместо `sp_describe_undeclared_parameters`, при запросе метаданных параметров. Пользователю не требуется предпринимать никаких действий.
```java
final String sql = "INSERT INTO #Bar VALUES (?)";
try (Connection c = DriverManager.getConnection(URL, USERNAME, PASSWORD)) {
    try (Statement s = c.createStatement()) {
        s.execute("CREATE TABLE #Bar(c1 int)");
    }
    try (PreparedStatement p1 = c.prepareStatement(sql); PreparedStatement p2 = c.prepareStatement(sql)) {
        ((SQLServerPreparedStatement) p1).setUseFmtOnly(true);
        ParameterMetaData pmd1 = p1.getParameterMetaData();
        System.out.println(pmd1.getParameterTypeName(1)); // prints int
        ParameterMetaData pmd2 = p2.getParameterMetaData(); // throws exception, Invalid object name '#Bar'
    }
}
```
> [!NOTE]  
>  Этот компонент поддерживает только запросы `SELECT/INSERT/UPDATE/DELETE`. Запросы должны начинаться с одного из 4 поддерживаемых ключевых слов или [обобщенного табличного выражения](../../t-sql/queries/with-common-table-expression-transact-sql.md), за которым следует один из поддерживаемых запросов. Параметры в обобщенных табличных выражениях не поддерживаются.

## <a name="known-issues"></a>Известные проблемы
  С этой функцией сейчас связаны некоторые проблемы из-за недостатков в логике синтаксического анализа SQL. Эти проблемы могут быть решены в будущем обновлении компонентов, и они описаны ниже вместе с предложениями по решению проблемы.
  
A. Использование псевдонима "forward declared"
```sql
CREATE TABLE Foo(c1 int)

DELETE fooAlias FROM Foo fooAlias WHERE c1 > ?; --Invalid object name 'fooAlias'

--Workaround #1: Specify AS keyword
DELETE fooAlias FROM Foo AS fooAlias WHERE c1 > ?;
--Workaround #2: Use the table name
DELETE Foo FROM Foo fooAlias WHERE c1 > ?;
```

Б. Неоднозначное имя столбца, если у таблиц есть имена общих столбцов
```sql
CREATE TABLE Foo(c1 int, c2 int, c3 int)
CREATE TABLE Bar(c1 int, c2 int, c3 int)

SELECT c1,c2 FROM Foo WHERE c3 IN (SELECT c3 FROM Bar WHERE c1 > ? and c2 < ? and c3 = ?); --Ambiguous Column Name

--Workaround: Use aliases
SELECT c1,c2 FROM Foo WHERE c3 IN (SELECT c3 FROM Bar b WHERE b.c1 = ? and b.c2 = ? and b.c3 = ?);
```

В. ВЫБОР из вложенного запроса с параметрами
```sql

CREATE TABLE Foo(c1 int)

SELECT * FROM (SELECT * FROM Foo WHERE c1 = ?) WHERE c1 = ?; --Incorrect syntax near '?'

--Workaround: N/A
```

Г. Вложенные запросы в предложении SET
```sql
CREATE TABLE Foo(c1 int)

UPDATE Foo SET c1 = (SELECT c1 FROM Foo) WHERE c1 = ?; --Incorrect syntax near ')'

--Workaround: Add a 'delimiting' condition
UPDATE Foo SET c1 = (SELECT c1 FROM Foo HAVING (HASH JOIN)) WHERE c1 = ?;
```

## <a name="see-also"></a>См. также раздел  
 [Настройка свойств подключения](../../connect/jdbc/setting-the-connection-properties.md)  
  

---
description: Узнайте, как создавать прокси-интерфейсы и программы-оболочки, которые предоставляют доступ к расширениям API JDBC.
title: Оболочки и интерфейсы
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 27fc9b72-9f21-4728-abcb-5c015f28a6ab
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 653cb21216c123a342f26134b3288b9f54b68b67
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793003"
---
# <a name="wrappers-and-interfaces"></a>Оболочки и интерфейсы

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] поддерживает интерфейсы, позволяющие создавать классы-посредники и оболочки для доступа к расширениям API JDBC, определяемым драйвером [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], через интерфейс-посредник.

## <a name="wrappers"></a>Оболочки

Драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] поддерживает интерфейс java.sql.Wrapper. Этот интерфейс обеспечивает механизм доступа к расширениям API JDBC, определяемым драйвером [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], через интерфейс-посредник.

Интерфейс Java. SQL. оберток определяет два метода: **isWrapperFor** и **Unwrap**. Метод **isWrapperFor** проверяет, реализует ли указанный объект ввода данный интерфейс. Метод **unwrap** возвращает объект, в котором реализован указанный интерфейс, для обеспечения доступа к методам, определяемым драйвером [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].

Методы **isWrapperFor** и **unwrap** предоставляются следующим образом:

- [Метод isWrapperFor (SQLServerCallableStatement)](reference/iswrapperfor-method-sqlservercallablestatement.md)
- [Метод unwrap (SQLServerCallableStatement)](reference/unwrap-method-sqlservercallablestatement.md)
- [Метод isWrapperFor &#40;SQLServerConnectionPoolDataSource&#41;](reference/iswrapperfor-method-sqlserverconnectionpooldatasource.md)
- [Метод unwrap (SQLServerConnectionPoolDataSource)](reference/unwrap-method-sqlserverconnectionpooldatasource.md)
- [Метод isWrapperFor (SQLServerDataSource)](reference/iswrapperfor-method-sqlserverdatasource.md)
- [Метод unwrap (SQLServerDataSource)](reference/unwrap-method-sqlserverdatasource.md)
- [Метод isWrapperFor (SQLServerPreparedStatement)](reference/iswrapperfor-method-sqlserverpreparedstatement.md)
- [Метод unwrap (SQLServerPreparedStatement)](reference/unwrap-method-sqlserverpreparedstatement.md)
- [Метод isWrapperFor (SQLServerStatement)](reference/iswrapperfor-method-sqlserverstatement.md)
- [Метод unwrap (SQLServerStatement)](reference/unwrap-method-sqlserverstatement.md)
- [Метод isWrapperFor &#40;SQLServerXADataSource&#41;](reference/iswrapperfor-method-sqlserverxadatasource.md)
- [Метод unwrap (SQLServerXADataSource)](reference/unwrap-method-sqlserverxadatasource.md)

## <a name="interfaces"></a>Интерфейсы

Начиная с версии 3.0 драйвера JDBC для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], можно использовать интерфейсы для сервера приложений, которые предоставляют доступ к методу, определяемому драйвером, из связанного класса. Сервер приложений может поместить класс в оболочку, создав класс-посредник, обеспечивающий определяемые драйвером [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] функции через интерфейс. Драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] поддерживает интерфейсы, имеющие методы и константы, определяемые драйвером [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], что позволяет серверу приложений создать для класса класс-посредник.

Эти интерфейсы являются производными от стандартных интерфейсов Java, поэтому тот же объект может быть использован и после получения из оболочки для доступа к функциям, определяемым драйвером, либо к стандартным функциям драйвера [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)].

Добавлены следующие интерфейсы:

- [ISQLServerCallableStatement](reference/isqlservercallablestatement-interface.md)
- [ISQLServerConnection](reference/isqlserverconnection-interface.md)
- [ISQLServerDataSource](reference/isqlserverdatasource-interface.md)
- [ISQLServerPreparedStatement](reference/isqlserverpreparedstatement-interface.md)
- [ISQLServerResultSet](reference/isqlserverresultset-interface.md)
- [ISQLServerStatement](reference/isqlserverstatement-interface.md)

## <a name="example"></a>Пример

### <a name="description"></a>Описание

В образце показано, как получить доступ к функции, определяемой драйвером [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], через объект DataSource. Класс DataSource может быть помещен в оболочку сервером приложений. Для доступа к функции или константе, определяемой драйвером JDBC, можно снять оболочку с datasource для интерфейса ISQLServerDataSource и использовать функции, объявленные в данном интерфейсе.

### <a name="code"></a>Код

```java
import javax.sql.*;
import java.sql.*;
import com.microsoft.sqlserver.jdbc.*;

public class UnWrapTest {
   public static void main(String[] args) {
      // This is a test.  This DataSource object could be something from an appserver
      // which has wrapped the real SQLServerDataSource with its own wrapper
      SQLServerDataSource ds = new SQLServerDataSource();
      checkSendStringParametersAsUnicode(ds);
   }

   // Unwrap to the ISQLServerDataSource interface to access the getSendStringParametersAsUnicode function
   static void checkSendStringParametersAsUnicode(DataSource ds) {
      try {
         final ISQLServerDataSource sqlServerDataSource = ds.unwrap(ISQLServerDataSource.class);
         boolean sendStringParametersAsUnicode = sqlServerDataSource.getSendStringParametersAsUnicode();

         System.out.println("Send string as parameter value is:-" + sendStringParametersAsUnicode);

      } catch (SQLException sqlE) {
         System.out.println("Exception:-" + sqlE);
      }
   }
}
```

## <a name="see-also"></a>См. также раздел

[Основные сведения о типах данных JDBC Driver](understanding-the-jdbc-driver-data-types.md)

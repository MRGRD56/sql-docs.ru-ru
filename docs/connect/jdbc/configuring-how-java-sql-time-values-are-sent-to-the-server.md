---
description: Узнайте, как настроить отправку значений java.sql.Time на сервер с помощью параметра подключения sendTimeAsDatetime.
title: Настройка способа отправки значений java.sql.Time на сервер
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 07eb00dd-621a-46f9-a5a5-8cab4d6058b5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3982d18398a77a28113292f3a8c8300a94a5de47
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793063"
---
# <a name="configuring-how-javasqltime-values-are-sent-to-the-server"></a>Настройка способа отправки значений java.sql.Time на сервер

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Если для задания параметра используется объект java.sql.Time или тип JDBC java.sql.Types.TIME, вы можете настроить метод отправки значения java.sql.Time на сервер: в виде типа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **time** или типа **datetime**.

Этот сценарий применяется в случае использования одного из следующих методов:

- [SQLServerCallableStatement.registerOutParameter(int, int)](reference/registeroutparameter-method-int-int.md)
- [SQLServerCallableStatement.registerOutParameter(int, int, int)](reference/registeroutparameter-method-int-int-int.md)
- [SQLServerCallableStatement.setTime](reference/settime-method-sqlservercallablestatement.md)
- [SQLServerPreparedStatement.setTime](reference/settime-method-sqlserverpreparedstatement.md)
- [SQLServerCallableStatement.setObject](reference/setobject-method-sqlservercallablestatement.md)
- [SQLServerPreparedStatement.setObject](reference/setobject-method-sqlserverpreparedstatement.md)

Порядок отправки значения java.sql.Time можно настроить с помощью свойства соединения **sendTimeAsDatetime**. Дополнительные сведения: [Задание свойств соединения](setting-the-connection-properties.md).

Значение свойства соединения **sendTimeAsDatetime** можно изменить программным образом с помощью [SQLServerDataSource.setSendTimeAsDatetime](reference/setsendtimeasdatetime-method-sqlserverdatasource.md).

Более ранние версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], чем [!INCLUDE[ssKatmai](../../includes/sskatmai_md.md)], не поддерживают тип данных **time**, поэтому приложения, использующие java.sql.Time, обычно хранят значения java.sql.Time как типы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **DateTime** или **smalldatetime**.

Если для работы со значениями java.sql.Time вы хотите использовать типы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**datetime** и **smalldatetime**, то для свойства подключения **sendTimeAsDatetime** следует задать значение **true**. Если для работы со значениями java.sql.Time вы хотите использовать тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **time**, то для свойства подключения **sendTimeAsDatetime** следует задать значение **false**.

Если значения java.sql.Time передаются в параметр, тип данных которого также поддерживает хранение дат, эти даты по умолчанию будут отличаться в зависимости от того, как отправляется значение java.sql.Time: как **datetime** (01.01.1970) или как **time** (01.01.1900). Дополнительные сведения о преобразовании данных при отправке на [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в разделе [Использование даты и времени](/previous-versions/sql/sql-server-2008-r2/ms180878(v=sql.105)).

В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] JDBC Driver 3.0, параметр **sendTimeAsDatetime** по умолчанию имеет значение true. В следующей версии свойство соединения **sendTimeAsDatetime** может иметь значение false по умолчанию.

Чтобы гарантировать, что приложение будет работать ожидаемым образом независимо от значения по умолчанию для свойства соединения **sendTimeAsDatetime**, можно выполнить следующие действия.

- Использовать java.sql.Time при работе с типом данных **time**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Используйте java.sql.Timestamp при работе с типами данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**datetime**, **smalldatetime** и **datetime2**.

SendTimeAsDatetime должен иметь значение FALSE для зашифрованных столбцов, так как они не поддерживают преобразование типа time в datetime. Начиная с Microsoft JDBC Driver 6.0 для SQL Server, класс SQLServerConnection содержит два следующих метода для задания и получения значений свойства sendTimeAsDatetime.

```java
  public boolean getSendTimeAsDatetime()
  public void setSendTimeAsDatetime(boolean sendTimeAsDateTimeValue)
```

## <a name="see-also"></a>См. также раздел

[Основные сведения о типах данных JDBC Driver](understanding-the-jdbc-driver-data-types.md)

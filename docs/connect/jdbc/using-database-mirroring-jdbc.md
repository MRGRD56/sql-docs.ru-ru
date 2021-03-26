---
description: Узнайте, как использовать зеркальное отображение базы данных с JDBC Driver для SQL Server и что нужно учитывать при отработке отказа.
title: Использование зеркального отображения базы данных (JDBC)
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 4ff59218-0d3b-4274-b647-9839c4955865
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 369c31309ab439bb3de5362c42c6563b99130b66
ms.sourcegitcommit: bacd45c349d1b33abef66db47e5aa809218af4ea
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2021
ms.locfileid: "104793049"
---
# <a name="using-database-mirroring-jdbc"></a>Использование зеркального отображения базы данных (JDBC)

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Зеркальное отображение базы данных представляет решение по повышению доступности базы данных и резервированию данных, реализуемое в основном программными средствами. Драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] реализует неявную поддержку зеркального отображения базы данных, поэтому разработчику не придется писать код или выполнять другие действия, если для базы данных настроено зеркальное отображение.

Зеркальное отображение базы данных, реализованное отдельно для каждой базы данных, хранит копию рабочей базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на резервном сервере. Это или «горячий», или «теплый» резервный сервер, в зависимости от конфигурации и состояния сеанса зеркального отображения базы данных. Сервер горячей замены, поддерживающий быструю отработку отказа без потери зафиксированных транзакций. Резервный сервер поддерживает вынужденное обслуживание с возможной потерей данных.

Рабочая база данных называется _основной_, а резервная копия — _зеркальной_ базой данных. Основная и зеркальная базы данных должны находиться в разных экземплярах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (экземплярах сервера), а по возможности — и на разных компьютерах.

Экземпляр рабочего сервера (основной сервер) обменивается данными с экземпляром резервного сервера (зеркальный сервер). Основной и зеркальный серверы выступают в роли участников в сеансе зеркального отображения базы данных. Если основной сервер дает сбой, зеркальный сервер делает свою базу данных основной, выполнив _отработку отказа_. Например, имеется два сервера-участника, Partner_A and Partner_B, при этом основная база данных изначально находится на сервере Partner_A (основной сервер), а зеркальная база данных — на сервере Partner_B (зеркальный сервер). Если сервер Partner_A переходит в режим вне сети, база данных на сервере Partner_B становится текущей основной базой данных. Когда сервер Partner_A возвращается в сеанс зеркального отображения, он становится зеркальным сервером, а его база данных — зеркальной базой данных.

Если сервер Partner_A выходит из строя без возможности восстановления, сервер Partner_C можно перевести в подключенный режим, чтобы использовать в качестве зеркального сервера для сервера Partner_B, который становится основным. Однако в этой ситуации клиентское приложение должно содержать логику программирования, обеспечивающую обновление свойств строк соединения с учетом новых имен серверов, используемых в конфигурации зеркального отображения базы данных. В противном случае соединение с серверами может завершиться ошибкой.

Другие конфигурации зеркального отображения баз данных имеют разные уровни производительности и безопасности данных, а также поддерживают разные формы отработки отказа. Дополнительные сведения см. в разделе "Общие сведения о зеркальном отображении базы данных" электронной документации по [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="programming-considerations"></a>Замечания по программированию

Если сервер, на котором размещается основная база данных, дает сбой, в ответ на вызовы API клиентское приложение получает ошибки, которые указывают на потерю соединения с базой данных. В этом случае утрачиваются все незафиксированные изменения в базе данных и выполняется откат текущей транзакции. В этом сценарии приложение должно закрыть подключение (или освободить объект источника данных), а затем снова открыть его. После подключения новое соединение автоматически направляется на зеркальную базу данных, которая теперь играет роль основного сервера, и клиенту не нужно изменять строку подключения или объект источника данных.

Во время первоначального установления соединения основной сервер отправляет удостоверение партнера по обеспечению обработки отказа на клиент, который будет использоваться при отработке отказа. Когда приложение пытается установить первоначальное соединение с основным сервером в состоянии сбоя, клиенту неизвестно удостоверение партнера по обеспечению обработки отказа. Чтобы дать клиентам возможность выхода из такой ситуации, предоставляются свойство строки подключения failoverPartner и необязательный метод источника данных [setFailoverPartner](../../connect/jdbc/reference/setfailoverpartner-method-sqlserverdatasource.md), с помощью которых клиент может самостоятельно указать удостоверение партнера по обеспечению отработки отказа. Свойство клиента используется только в этом сценарии и не используется, если доступен основной сервер.

> [!NOTE]
> Если свойство failoverPartner указывается в строке соединения или в объекте источника данных, то также необходимо установить свойство databaseName. В противном случае будет вызвано исключение. Если свойства failoverPartner и databaseName не заданы явно, то приложение не будет запускать отработку отказа в случае отказа основного сервера базы данных. Иначе говоря, автоматическое перенаправление работает только для соединений, где явно указаны свойства failoverPartner и databaseName. Дополнительные сведения о свойстве failoverPartner и других свойствах строки соединения см. в статье о [настройке свойств подключения](../../connect/jdbc/setting-the-connection-properties.md).

Если сервер, указанный клиентом как партнер по обеспечению обработки отказа, не используется в роли такого партнера для указанной базы данных и если указанный сервер (база данных) участвует в зеркальном отображении, подключение будет отклонено сервером. Класс [SQLServerDataSource](../../connect/jdbc/reference/sqlserverdatasource-class.md) содержит метод [getFailoverPartner](../../connect/jdbc/reference/getfailoverpartner-method-sqlserverdatasource.md), но этот метод возвращает только имя партнера по обеспечению отработки отказа, указанное в строке подключения или с помощью метода setFailoverPartner. Получить имя фактического партнера по обеспечению отработки отказа можно с помощью следующей инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)]:

```sql
SELECT m.mirroring_role_DESC, m.mirroring_state_DESC,
m.mirroring_partner_instance FROM sys.databases as db,
sys.database_mirroring AS m WHERE db.name = 'MirroringDBName'
AND db.database_id = m.database_id
```

> [!NOTE]
> Эту инструкцию нужно изменить в соответствии с именем зеркальной базы данных.

Рекомендуется кэшировать сведения об участниках для обновления строки подключения или разработать стратегию повторных попыток на случай, если первая попытка установить подключение завершается ошибкой.

## <a name="example"></a>Пример

В следующем примере сначала выполняется попытка подключения к основному серверу. Если эта попытка завершается созданием исключения, выполняется попытка подключения к зеркальному серверу, который мог стать новым основным сервером. Заметьте, что в строке соединения используется свойство failoverPartner.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class ClientFailover {
    public static void main(String[] args) {

        String connectionUrl = "jdbc:sqlserver://serverA:1433;"
                + "databaseName=AdventureWorks;integratedSecurity=true;"
                + "failoverPartner=serverB";

        // Establish the connection to the principal server.
        try (Connection con = DriverManager.getConnection(connectionUrl);
                Statement stmt = con.createStatement();) {
            System.out.println("Connected to the principal server.");

            // Note that if a failover of serverA occurs here, then an
            // exception will be thrown and the failover partner will
            // be used in the first catch block below.

            // Execute a SQL statement that inserts some data.

            // Note that the following statement assumes that the
            // TestTable table has been created in the AdventureWorks
            // sample database.
            stmt.executeUpdate("INSERT INTO TestTable (Col2, Col3) VALUES ('a', 10)");
        }
        catch (SQLException se) {
            System.out.println("Connection to principal server failed, " + "trying the mirror server.");
            // The connection to the principal server failed,
            // try the mirror server which may now be the new
            // principal server.
            try (Connection con = DriverManager.getConnection(connectionUrl);
                    Statement stmt = con.createStatement();) {
                System.out.println("Connected to the new principal server.");
                stmt.executeUpdate("INSERT INTO TestTable (Col2, Col3) VALUES ('a', 10)");
            }
            // Handle any errors that may have occurred.
            catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## <a name="see-also"></a>См. также раздел

[Подключение к SQL Server с помощью JDBC Driver](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)

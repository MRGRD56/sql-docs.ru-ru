---
title: Основные сведения о транзакциях XA
description: Microsoft JDBC Driver для SQL Server обеспечивает поддержку дополнительных распределенных транзакций на платформе Java, Enterprise Edition/JDBC 2.0.
ms.custom: ''
ms.date: 09/29/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 574e326f-0520-4003-bdf1-62d92c3db457
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7fa4598fc332a2f84a02b428e786ed381e388a1e
ms.sourcegitcommit: a7af7bead92044595556b8687e640a0eab0bc455
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/02/2021
ms.locfileid: "106179884"
---
# <a name="understanding-xa-transactions"></a>Основные сведения о транзакциях XA

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] обеспечивает поддержку дополнительных распределенных транзакций на платформе Java, Enterprise Edition/JDBC 2.0. Соединения JDBC, получаемые с помощью класса [SQLServerXADataSource](../../connect/jdbc/reference/sqlserverxadatasource-class.md), можно использовать в стандартных средах обработки распределенных транзакций, например на серверах приложений платформы Java Enterprise Edition (Java EE).  

В этой статье XA обозначает расширенную архитектуру.

> [!WARNING]  
> Драйвер Microsoft JDBC Driver 4.2 для SQL (и более поздних версий) включает новые параметры времени ожидания для автоматического отката неподготовленных транзакций существующих компонентов. Дополнительные сведения см. в разделе [Настройка параметров времени ожидания сервера для автоматического отката неподготовленных транзакций](../../connect/jdbc/understanding-xa-transactions.md#BKMK_ServerSide) в этой статье.  

## <a name="remarks"></a>Remarks

Далее представлены классы для реализации распределенных транзакций.  
  
| Class                                              | Реализации                      | Описание                                       |
| -------------------------------------------------- | ------------------------------- | ------------------------------------------------- |
| com.microsoft.sqlserver.jdbc.SQLServerXADataSource | javax.sql.XADataSource          | Фабрика класса для распределенных соединений.    |
| com.microsoft.sqlserver.jdbc.SQLServerXAResource   | javax.transaction.xa.XAResource | Адаптер ресурсов для диспетчера транзакций. |
  
> [!NOTE]  
> Для соединений распределенных транзакций XA по умолчанию устанавливается уровень изоляции Read Committed.  
  
## <a name="guidelines-and-limitations-when-using-xa-transactions"></a>Правила и ограничения, связанные с использованием транзакций XA  

К сильно связанным транзакциям относятся следующие дополнительные рекомендации.  

- Если транзакции XA используются вместе с координатором распределенных транзакций (Майкрософт) (MS DTC), то текущая версия MS DTC может не поддерживать сильно связанные ветви XA. Например, в MS DTC действует взаимно однозначное сопоставление между идентификатором ветви транзакции XA (XID) и идентификатором транзакции MS DTC, и работа, выполняемая слабосвязанными ветвями XA, изолируется.  
  
- MSDTC также обеспечивает поддержку сильно связанных ветвей XA, когда несколько ветвей XA с одним глобальным идентификатором транзакции (GTRID) сопоставляются с одним идентификатором транзакции MS DTC. В результате в каждой из нескольких сильно связанных ветвей XA в диспетчере ресурсов (например, в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) можно просматривать изменения, выполненные другой ветвью.
  
- Флаг [SSTRANSTIGHTLYCPLD](../../connect/jdbc/reference/sstranstightlycpld-field-sqlserverxaresource.md) разрешает приложениям использовать тесно связанные транзакции XA, имеющие различные идентификаторы ветвей транзакции (BQUAL), но одинаковые глобальные идентификаторы транзакции (GTRID) и идентификатор формата (FormatID). Для использования этой функции необходимо задать значение [SSTRANSTIGHTLYCPLD](../../connect/jdbc/reference/sstranstightlycpld-field-sqlserverxaresource.md) в параметре флагов метода XAResource.start:
  
    ```java
    xaRes.start(xid, SQLServerXAResource.SSTRANSTIGHTLYCPLD);  
    ```  
  
## <a name="configuration-instructions"></a>Инструкции по настройке

Если источники данных XA нужно использовать вместе с координатором распределенных транзакций (Майкрософт) (MS DTC) для обработки распределенных транзакций, необходимо выполнить следующие действия.  

> [!NOTE]  
> Компоненты распределенных транзакций JDBC находятся в каталоге XA в каталоге установки драйвера JDBC. К этим компонентам относятся файлы xa_install.sql и sqljdbc_xa.dll. Если на разных клиентах установлены разные версии драйвера JDBC, рекомендуется использовать последнюю версию sqljdbc_xa.dll на сервере.  

> [!NOTE]  
> Начиная с накопительного пакета обновления 16 SQL Server 2017 и в SQL Server 2019, компоненты распределенных транзакций JDBC XA включены в ядро SQL Server, и их можно включить или отключить с помощью системной хранимой процедуры. Файл sqjdbc_xa.dll из драйвера не требуется. Рекомендуется включить серверные компоненты вместо этих версий сервера.
> Чтобы необходимые компоненты могли выполнять распределенные транзакции XA с помощью драйвера JDBC, выполните следующую сохраненную процедуру.
>
> EXEC sp_sqljdbc_xa_install
>
> Чтобы отключить ранее установленные компоненты, выполните следующую сохраненную процедуру.
>
> EXEC sp_sqljdbc_xa_uninstall

### <a name="running-the-ms-dtc-service"></a>Запуск службы MS DTC

Для службы MS DTC следует указать в диспетчере служб тип запуска **Авто**, чтобы обеспечить ее запуск во время запуска службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Чтобы включить MS DTC для транзакций XA, необходимо выполнить следующие действия.  
  
В Windows Vista и более поздних версиях:  
  
1. Нажмите кнопку **Пуск**, введите **dcomcnfg** в поле **Начать поиск** и нажмите клавишу ВВОД, чтобы открыть окно **Службы компонентов**. Также можно ввести %windir%\system32\comexp.msc в поле **Начать поиск**, чтобы открыть окно **Службы компонентов**.  
  
2. Разверните узлы "Службы компонентов", "Компьютеры", "Мой компьютер" и "Координатор распределенных транзакций".  
  
3. Щелкните правой кнопкой мыши узел **Локальная DTC** и выберите пункт **Свойства**.  
  
4. В диалоговом окне **Свойства: локальная DTC** перейдите на вкладку **Безопасность**.  
  
5. Установите флажок **Включить транзакции XA** и нажмите кнопку **ОК**. Это приведет к перезапуску службы MS DTC.
  
6. Еще раз нажмите кнопку **ОК**, чтобы закрыть диалоговое окно **Свойства**, а затем закройте окно **Службы компонентов**.  
  
7. Остановите и перезапустите [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], чтобы обеспечить синхронизацию с изменениями MS DTC.  

### <a name="configuring-the-jdbc-distributed-transaction-components"></a>Настройка компонентов распределенных транзакций JDBC  

Чтобы настроить компоненты распределенных транзакций драйвера JDBC, можно выполнить следующие действия  
  
1. Скопируйте новый файл sqljdbc_xa.dll из каталога установки драйвера JDBC в каталог Binn на каждом компьютере [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], который будет участвовать в распределенных транзакциях.  
  
    > [!NOTE]  
    > Если транзакции XA используются с 32-разрядной версией [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], используйте файл sqljdbc_xa.dll из папки x86, даже если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] установлен на компьютере с процессором x64. Если транзакции XA используются с 64-разрядной версией [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на компьютере с процессором x64, используйте файл sqljdbc_xa.dll из папки x64.  
  
2. Выполните скрипт базы данных xa_install.sql в каждом экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], который будет участвовать в распределенных транзакциях. Этот скрипт устанавливает расширенные хранимые процедуры, которые вызываются из sqljdbc_xa.dll. В этих расширенных хранимых процедурах реализована поддержка распределенных транзакций и XA для драйвера [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)]. Этот скрипт необходимо запускать от имени администратора экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3. Чтобы предоставить определенному пользователю разрешения для участия в распределенных транзакциях через драйвер JDBC, его необходимо включить в роль SqlJDBCXAUser.  
  
В каждый момент времени в каждом экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно настроить только одну версию сборки sqljdbc_xa.dll. Приложениям могут понадобиться различные версии драйвера JDBC для подключения к одному экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью соединения XA. В этом случае в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] необходимо установить файл sqljdbc_xa.dll, который содержит новейшую версию драйвера JDBC.  
  
Существует три способа, чтобы проверить версию файла sqljdbc_xa.dll, установленную в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
1. Откройте каталог LOG на компьютере [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], который будет участвовать в распределенных транзакциях. Выберите и откройте файл [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ERRORLOG. Найдите в файле ERRORLOG фразу "Используется SQLJDBC_XA.dll версии...".  
  
2. Откройте каталог Binn на компьютере [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], который будет участвовать в распределенных транзакциях. Выберите сборку sqljdbc_xa.dll.

    - В Windows Vista и более поздних версиях: щелкните правой кнопкой мыши файл sqljdbc_xa.dll и выберите пункт "Свойства". Перейдите на вкладку **Подробно**. В поле **Версия файла** показана версия файла sqljdbc_xa.dll, которая в настоящий момент установлена в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3. Настройте ведение журнала, как показано в примере кода в следующем разделе. Найдите в выходном файле журнала фразу «Версия XA DLL на сервере:...».  

### <a name="configuring-server-side-timeout-settings-for-automatic-rollback-of-unprepared-transactions"></a><a name="BKMK_ServerSide"></a> Настройка параметров времени ожидания сервера для автоматического отката неподготовленных транзакций  

> [!WARNING]  
> Этот серверный параметр появился в версии Microsoft JDBC Driver 4.2 для SQL Server (и более поздних версиях). Чтобы это обновленное поведение работало, убедитесь, что файл sqljdbc_xa.dll на сервере обновлен. Дополнительные сведения о настройке времени ожидания на стороне клиента см. в описании [XAResource.setTransactionTimeout()](https://docs.oracle.com/javase/8/docs/api/javax/transaction/xa/XAResource.html).  

Существует два параметра реестра (типа DWORD) для управления временем ожидания распределенных транзакций.  
  
- **XADefaultTimeout** (в секундах): значение времени ожидания по умолчанию, которое применяется, когда пользователь не указывает время ожидания. Значение по умолчанию равно 0.  
  
- **XAMaxTimeout** (в секундах): максимальное значение времени ожидания, которое может задать пользователь. Значение по умолчанию равно 0.  
  
Эти параметры относятся к экземпляру SQL Server и должны быть созданы в следующем разделе реестра:  

```bash
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL<version>.<instance_name>\XATimeout  
```

> [!NOTE]  
> Для 32-разрядной версии SQL Server на 64-разрядных компьютерах необходимо создать параметры реестра в следующем разделе: `HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Microsoft SQL Server\MSSQL<version>.<instance_name>\XATimeout`
  
Время ожидания задается для каждой транзакции при ее запуске. По истечении времени ожидания выполняется откат транзакции в SQL Server. Время ожидания зависит от параметров реестра и указанных пользователем параметров в XAResource.setTransactionTimeout(). Вот несколько примеров интерпретации таких значений времени ожидания.  
  
- `XADefaultTimeout = 0`, `XAMaxTimeout = 0`
  
     Время ожидания по умолчанию не будет использоваться, максимальное время ожидания не будет применяться к клиентам. В этом случае у транзакций будет время ожидания, только если клиент устанавливает время ожидания с помощью XAResource.setTransactionTimeout.  
  
- `XADefaultTimeout = 60`, `XAMaxTimeout = 0`
  
     Время ожидания всех транзакций будет равно 60 секундам, если клиент не указывает время ожидания. Если клиент указывает время ожидания, будет использоваться это значение времени ожидания. Максимальное значение времени ожидания не применяется.  
  
- `XADefaultTimeout = 30`, `XAMaxTimeout = 60`
  
     Время ожидания всех транзакций будет равно 30 секундам, если клиент не указывает время ожидания. Если клиент указывает время ожидания, будет использоваться время ожидания клиента, если оно менее 60 секунд (максимальное значение).  
  
- `XADefaultTimeout = 0`, `XAMaxTimeout = 30`
  
     Время ожидания всех транзакций будет равно 30 секундам (максимальное значение), если клиент не указывает время ожидания. Если клиент указывает время ожидания, будет использоваться время ожидания клиента, если оно менее 30 секунд (максимальное значение).  
  
### <a name="upgrading-sqljdbc_xadll"></a>Обновление файла sqljdbc_xa.dll

При установке новой версии драйвера JDBC также следует обновить файл sqljdbc_xa.dll, расположенный на сервере, с помощью файла sqljdbc_xa.dll из этой новой версии.  
  
> [!IMPORTANT]  
> Файл sqljdbc_xa.dll следует обновлять во время планового обслуживания либо в случае, если не выполняется ни одной транзакции MS DTC.
  
1. Загрузите файл sqljdbc_xa.dll с помощью команды [!INCLUDE[tsql](../../includes/tsql-md.md)] **DBCC sqljdbc_xa (FREE)** .  
  
2. Скопируйте новый файл sqljdbc_xa.dll из каталога установки драйвера JDBC в каталог Binn на каждом компьютере [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], который будет участвовать в распределенных транзакциях.  
  
    Новый файл DLL будет загружаться при вызове расширенной процедуры в sqljdbc_xa.dll. Для загрузки новых определений перезапускать [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не требуется.  
  
### <a name="configuring-the-user-defined-roles"></a>Настройка определяемых пользователем ролей

Чтобы предоставить определенному пользователю разрешения для участия в распределенных транзакциях через драйвер JDBC, его необходимо включить в роль SqlJDBCXAUser. Например, следующий код [!INCLUDE[tsql](../../includes/tsql-md.md)] позволяет добавить пользователя с именем shelly (стандартное имя входа пользователя SQL) в роль SqlJDBCXAUser:  

```sql
USE master  
GO  
EXEC sp_grantdbaccess 'shelly', 'shelly'  
GO  
EXEC sp_addrolemember [SqlJDBCXAUser], 'shelly'  
```

Определяемые пользователем роли SQL определяются в рамках базы данных. Чтобы создать собственную роль в целях безопасности, необходимо определить роль в каждой базе данных и добавлять пользователей отдельно для каждой базы данных. Роль SqlJDBCXAUser строго определена в базе данных master, так как она используется для предоставления доступа к расширенным хранимым процедурам SQL JDBC, находящимся в базе данных master. Сначала необходимо предоставить отдельным пользователям доступ к базе данных master, а затем выполнить вход в базу данных master и предоставить этим пользователям доступ к роли SqlJDBCXAUser.  

## <a name="example"></a>Пример  

```java
import java.net.Inet4Address;
import java.sql.*;
import java.util.Random;
import javax.sql.XAConnection;
import javax.transaction.xa.*;
import com.microsoft.sqlserver.jdbc.*;


public class testXA {

    public static void main(String[] args) throws Exception {

        // Create variables for the connection string.
        String prefix = "jdbc:sqlserver://";
        String serverName = "localhost";
        int portNumber = 1433;
        String databaseName = "AdventureWorks";
        String user = "UserName";
        String password = "*****";

        String connectionUrl = prefix + serverName + ":" + portNumber + ";databaseName=" + databaseName + ";user="
                + user + ";password=" + password;

        Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");

        try (Connection con = DriverManager.getConnection(connectionUrl); Statement stmt = con.createStatement()) {
            stmt.executeUpdate("CREATE TABLE XAMin (f1 int, f2 varchar(max))");

        }
        // Create the XA data source and XA ready connection.
        SQLServerXADataSource ds = new SQLServerXADataSource();
        ds.setUser(user);
        ds.setPassword(password);
        ds.setServerName(serverName);
        ds.setPortNumber(portNumber);
        ds.setDatabaseName(databaseName);

        XAConnection xaCon = ds.getXAConnection();
        try (Connection con = xaCon.getConnection()) {

            // Get a unique Xid object for testing.
            XAResource xaRes = null;
            Xid xid = null;
            xid = XidImpl.getUniqueXid(1);

            // Get the XAResource object and set the timeout value.
            xaRes = xaCon.getXAResource();
            xaRes.setTransactionTimeout(0);

            // Perform the XA transaction.
            System.out.println("Write -> xid = " + xid.toString());
            xaRes.start(xid, XAResource.TMNOFLAGS);
            PreparedStatement pstmt = con.prepareStatement("INSERT INTO XAMin (f1,f2) VALUES (?, ?)");
            pstmt.setInt(1, 1);
            pstmt.setString(2, xid.toString());
            pstmt.executeUpdate();

            // Commit the transaction.
            xaRes.end(xid, XAResource.TMSUCCESS);
            xaRes.commit(xid, true);
        }
        xaCon.close();

        // Open a new connection and read back the record to verify that it worked.
        try (Connection con = DriverManager.getConnection(connectionUrl); Statement stmt = con.createStatement();
                ResultSet rs = stmt.executeQuery("SELECT * FROM XAMin")) {
            rs.next();
            System.out.println("Read -> xid = " + rs.getString(2));
            stmt.executeUpdate("DROP TABLE XAMin");
        }
    }
}


class XidImpl implements Xid {

    public int formatId;
    public byte[] gtrid;
    public byte[] bqual;

    public byte[] getGlobalTransactionId() {
        return gtrid;
    }

    public byte[] getBranchQualifier() {
        return bqual;
    }

    public int getFormatId() {
        return formatId;
    }

    XidImpl(int formatId, byte[] gtrid, byte[] bqual) {
        this.formatId = formatId;
        this.gtrid = gtrid;
        this.bqual = bqual;
    }

    public String toString() {
        int hexVal;
        StringBuffer sb = new StringBuffer(512);
        sb.append("formatId=" + formatId);
        sb.append(" gtrid(" + gtrid.length + ")={0x");
        for (int i = 0; i < gtrid.length; i++) {
            hexVal = gtrid[i] & 0xFF;
            if (hexVal < 0x10)
                sb.append("0" + Integer.toHexString(gtrid[i] & 0xFF));
            else
                sb.append(Integer.toHexString(gtrid[i] & 0xFF));
        }
        sb.append("} bqual(" + bqual.length + ")={0x");
        for (int i = 0; i < bqual.length; i++) {
            hexVal = bqual[i] & 0xFF;
            if (hexVal < 0x10)
                sb.append("0" + Integer.toHexString(bqual[i] & 0xFF));
            else
                sb.append(Integer.toHexString(bqual[i] & 0xFF));
        }
        sb.append("}");
        return sb.toString();
    }

    // Returns a globally unique transaction id.
    static byte[] localIP = null;
    static int txnUniqueID = 0;

    static Xid getUniqueXid(int tid) {

        Random rnd = new Random(System.currentTimeMillis());
        txnUniqueID++;
        int txnUID = txnUniqueID;
        int tidID = tid;
        int randID = rnd.nextInt();
        byte[] gtrid = new byte[64];
        byte[] bqual = new byte[64];
        if (null == localIP) {
            try {
                localIP = Inet4Address.getLocalHost().getAddress();
            } catch (Exception ex) {
                localIP = new byte[] {0x01, 0x02, 0x03, 0x04};
            }
        }
        System.arraycopy(localIP, 0, gtrid, 0, 4);
        System.arraycopy(localIP, 0, bqual, 0, 4);

        // Bytes 4 -> 7 - unique transaction id.
        // Bytes 8 ->11 - thread id.
        // Bytes 12->15 - random number generated by using seed from current time in milliseconds.
        for (int i = 0; i <= 3; i++) {
            gtrid[i + 4] = (byte) (txnUID % 0x100);
            bqual[i + 4] = (byte) (txnUID % 0x100);
            txnUID >>= 8;
            gtrid[i + 8] = (byte) (tidID % 0x100);
            bqual[i + 8] = (byte) (tidID % 0x100);
            tidID >>= 8;
            gtrid[i + 12] = (byte) (randID % 0x100);
            bqual[i + 12] = (byte) (randID % 0x100);
            randID >>= 8;
        }
        return new XidImpl(0x1234, gtrid, bqual);
    }
}

```

## <a name="see-also"></a>См. также раздел  

[Выполнение транзакций с помощью JDBC Driver](../../connect/jdbc/performing-transactions-with-the-jdbc-driver.md)  

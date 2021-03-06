---
title: Устойчивость соединения в режиме ожидания
description: Узнайте, что такое устойчивость соединения в режиме ожидания и как она работает в драйверах Майкрософт для PHP для SQL Server.
ms.date: 03/04/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 07223e87c82a64fe17cd1b2969b5c93227c70af2
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247490"
---
# <a name="idle-connection-resiliency"></a>Устойчивость соединения в режиме ожидания
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[Устойчивость подключения](../odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md) является принципом, при котором разорванное подключение может быть восстановлено в пределах некоторых ограничений. В случае сбоя подключения к Microsoft SQL Server устойчивость подключения позволяет клиенту автоматически выполнять попытку восстановления подключения. Устойчивость подключения является свойством источника данных. Она поддерживается только в SQL Server 2014 и более поздних версий и в Базе данных SQL Azure.

Устойчивость подключения реализуется с помощью двух ключевых слов подключения, которые можно добавить в строки подключения: **ConnectRetryCount** и **ConnectRetryInterval**.

|Ключевое слово|Значения|По умолчанию|Описание|
|-|-|-|-|
|**ConnectRetryCount**| Целое число от 0 до 255 (включительно).|1|Максимальное число попыток повторного установления разорванного подключения, при достижении которого их следует прекратить. По умолчанию выполняется одна попытка восстановить разорванное подключение. Значение 0 означает, что повторное подключение не будет устанавливаться.|
|**ConnectRetryInterval**| Целое число от 1 до 60 (включительно).|10| Время в секундах между попытками восстановить подключение. Приложение попытается повторно подключиться сразу после обнаружения разорванного подключения, а затем будет ждать заданное в параметре **ConnectRetryInterval** количество секунд, прежде чем повторить попытку. Это ключевое слово игнорируется, если **ConnectRetryCount** равен 0.

Если результат **ConnectRetryCount**, умноженный на **ConnectRetryInterval**, превышает значение **LoginTimeout**, то клиент прекращает попытки подключиться при достижении **LoginTimeout**. В противном случае он будет продолжать попытки повторного подключения до тех пор, пока не будет достигнуто **ConnectRetryCount**.

#### <a name="remarks"></a>Remarks

Устойчивость подключения применяется, когда подключение бездействует. Например, сбои, возникающие при выполнении транзакции, не приведут к попыткам установки повторного подключения. Они завершатся ошибкой. В следующих ситуациях, известных как невосстанавливаемые состояния сеанса, попытки повторного подключения не активируются:

* Временные таблицы
* глобальные и локальные курсоры;
* контекст транзакции и блокировки транзакций на уровне сеанса;
* Блокировки приложений
* контекст безопасности EXECUTE AS/REVERT;
* дескрипторы OLE-автоматизации;
* подготовленные дескрипторы XML;
* Флаги трассировки

## <a name="example"></a>Пример

Следующий код подключается к базе данных и выполняет запрос. Подключение прервано из-за завершения сеанса, и предпринята попытка выполнить новый запрос с использованием разорванного подключения. В примере используется образец базы данных [AdventureWorks](/previous-versions/sql/sql-server-2008/ms124501(v=sql.100)).

В этом примере мы указываем буферизованный курсор перед прерыванием подключения. Если не указать буферизованный курсор, подключение не будет восстановлено из-за наличия активного серверного курсора, поэтому подключение не будет бездействующим во время прерывания. Однако в этом случае перед прерыванием подключения можно вызвать sqlsrv_free_stmt () для освобождения курсора, и подключение будет успешно восстановлено.

```php
<?php
// This function breaks the connection by determining its session ID and killing it.
// A separate connection is used to break the main connection because a session
// cannot kill itself. The sleep() function ensures enough time has passed for KILL
// to finish ending the session.
function BreakConnection( $conn, $conn_break )
{
    $stmt1 = sqlsrv_query( $conn, "SELECT @@SPID" );
    if ( sqlsrv_fetch( $stmt1 ) )
    {
        $spid=sqlsrv_get_field( $stmt1, 0 );
    }

    $stmt2 = sqlsrv_prepare( $conn_break, "KILL ".$spid );
    sqlsrv_execute( $stmt2 );
    sleep(1);
}

// Connect to the local server using Windows authentication and specify
// AdventureWorks as the database in use. Specify values for
// ConnectRetryCount and ConnectRetryInterval as well.
$databaseName = 'AdventureWorks2014';
$serverName = '(local)';
$connectionInfo = array( "Database"=>$databaseName, "ConnectRetryCount"=>10, "ConnectRetryInterval"=>10 );

$conn = sqlsrv_connect( $serverName, $connectionInfo );
if( $conn === false)  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}

// A separate connection that will be used to break the main connection $conn
$conn_break = sqlsrv_connect( $serverName, array( "Database"=>$databaseName) );

// Create a statement to retrieve the contents of a table
$stmt1 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Employee",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt1 === false )
{
     echo "Error in statement 1.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 1 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt1 );
    echo $rowcount." rows in result set.\n";
}

// Now break the connection $conn
BreakConnection( $conn, $conn_break );

// Create another statement. The connection will be reestablished.
$stmt2 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Department",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt2 === false )
{
     echo "Error in statement 2.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 2 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt2 );
    echo $rowcount." rows in result set.\n";
}

sqlsrv_close( $conn );
sqlsrv_close( $conn_break );
?>
```
Ожидаемые выходные данные:
```
Statement 1 successful.
290 rows in result set.
Statement 2 successful.
16 rows in result set.
```

## <a name="see-also"></a>См. также:
[Устойчивость подключения в драйвере ODBC в Windows](../odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)

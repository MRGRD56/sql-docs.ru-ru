---
title: Практическое руководство. Отправка данных в виде потока
description: Узнайте, как использовать потоки для отправки больших объектов в базу данных с помощью драйверов Microsoft SQLSRV и PDO_SQLSRV для PHP для SQL Server.
ms.custom: ''
ms.date: 02/28/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- updating data
- streaming data
ms.assetid: ab6b95d6-b6e6-4bd7-a18c-50f2918f7532
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c2d986336a369698b107b2437beae322d91508fb
ms.sourcegitcommit: 620a868e623134ad6ced6728ce9d03d7d0038fe0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/29/2020
ms.locfileid: "87411441"
---
# <a name="how-to-send-data-as-a-stream"></a>Практическое руководство. Отправка данных в виде потока
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] использует преимущества потоков PHP для отправки больших объектов на сервер. Примеры в этой статье демонстрируют процедуру отправки данных в виде потока. Первый пример использует драйвер SQLSRV, чтобы продемонстрировать поведение по умолчанию, которое заключается в отправке всех потоковых данных во время выполнения запроса. Второй пример демонстрирует использование драйвера SQLSRV для отправки на сервер потоковых данных объемом до восьми килобайт (8 КБ) за раз.  
  
Третий пример показывает, как отправить потоковые данные на сервер с помощью драйвера PDO_SQLSRV.  
  
## <a name="example-sending-stream-data-at-execution"></a>Пример Отправка потоковых данных во время выполнения
Следующий пример вставляет строку в таблицу *Production.ProductReview* базы данных AdventureWorks. Комментарии клиента ( *$comments*) открываются в виде потока с помощью функции PHP [fopen](https://php.net/manual/en/function.fopen.php), а затем передаются в потоковом режиме на сервер при выполнении запроса.  
  
В примере предполагается, что SQL Server и базы данных [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) установлены на локальном компьютере. Все выходные данные выводятся в консоль.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if ($conn === false) {
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Set up the Transact-SQL query. */  
$tsql = "INSERT INTO Production.ProductReview (ProductID,   
                                               ReviewerName,  
                                               ReviewDate,  
                                               EmailAddress,  
                                               Rating,  
                                               Comments)  
         VALUES (?, ?, ?, ?, ?, ?)";  
  
/* Set the parameter values and put them in an array.  
Note that $comments is opened as a stream. */  
$productID = '709';  
$name = 'Customer Name';  
$date = date("Y-m-d");  
$email = 'customer@name.com';  
$rating = 3;  
$data = 'Insert any lengthy comment here.';
$comments = fopen('data:text/plain,'.urlencode($data), 'r');
$params = array($productID, $name, $date, $email, $rating, $comments);
  
/* Execute the query. All stream data is sent upon execution.*/  
$stmt = sqlsrv_query($conn, $tsql, $params);  
if ($stmt === false) {
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
} else {
     echo "The query was successfully executed.";  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="example-sending-stream-data-using-sqlsrv_send_stream_data"></a>Пример Отправка потоковых данных с помощью sqlsrv_send_stream_data
Следующий пример такой же, как предыдущий, однако поведение по умолчанию, заключающееся в отправке всех потоковых данных при выполнении, отключено. Пример использует [sqlsrv_send_stream_data](../../connect/php/sqlsrv-send-stream-data.md) для отправки потоковых данных на сервер. При каждом вызове **sqlsrv_send_stream_data** отправляется до восьми килобайт (8 КБ) данных. Скрипт подсчитывает количество вызовов, сделанных **sqlsrv_send_stream_data** , и выводит это количество в консоль.  
  
В примере предполагается, что SQL Server и базы данных [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) установлены на локальном компьютере. Все выходные данные выводятся в консоль.  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if ($conn === false) {
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Set up the Transact-SQL query. */  
$tsql = "INSERT INTO Production.ProductReview (ProductID,   
                                               ReviewerName,  
                                               ReviewDate,  
                                               EmailAddress,  
                                               Rating,  
                                               Comments)  
         VALUES (?, ?, ?, ?, ?, ?)";  
  
/* Set the parameter values and put them in an array.  
Note that $comments is opened as a stream. */  
$productID = '709';  
$name = 'Customer Name';  
$date = date("Y-m-d");  
$email = 'customer@name.com';  
$rating = 3;  
$data = 'Insert any lengthy comment here.';
$comments = fopen('data:text/plain,'.urlencode($data), 'r');
$params = array($productID, $name, $date, $email, $rating, $comments);  
  
/* Turn off the default behavior of sending all stream data at  
execution. */  
$options = array("SendStreamParamsAtExec" => 0);  
  
/* Execute the query. */  
$stmt = sqlsrv_query($conn, $tsql, $params, $options);  
if ($stmt === false) {
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Send up to 8K of parameter data to the server with each call to  
sqlsrv_send_stream_data. Count the calls. */  
$i = 1;  
while (sqlsrv_send_stream_data($stmt)) {
     echo "$i call(s) made.\n";  
     $i++;  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
Хотя примеры в этой статье отправляют на сервер символьные данные, в виде потока можно передавать данные в любом формате. Например, методы, описанные в этой статье, можно также использовать для отправки изображений в двоичном формате в виде потоков.  
  
## <a name="example-sending-an-image-as-a-stream"></a>Пример Отправка изображения в виде потока 
  
```  
<?php  
   $server = "(local)";   
   $database = "Test";  
   $conn = new PDO( "sqlsrv:server=$server;Database = $database", "", "");  
  
   $binary_source = fopen( "data://text/plain,", "r");  
  
   $stmt = $conn->prepare("insert into binaries (imagedata) values (?)");  
   $stmt->bindParam(1, $binary_source, PDO::PARAM_LOB);   
  
   $conn->beginTransaction();  
   $stmt->execute();  
   $conn->commit();  
?>  
```  
  
## <a name="see-also"></a>См. также:  
[Обновление данных (драйверы Майкрософт для PHP для SQL Server)](../../connect/php/updating-data-microsoft-drivers-for-php-for-sql-server.md)

[Извлечение данных в виде потока с помощью драйвера SQLSRV](../../connect/php/retrieving-data-as-a-stream-using-the-sqlsrv-driver.md)

[Информация о примерах кода в документации](../../connect/php/about-code-examples-in-the-documentation.md)  
  

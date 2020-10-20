---
title: Руководство. Подключение с помощью проверки подлинности Windows
description: Узнайте, что означает подключение с помощью встроенной проверки подлинности Windows через драйверы для PHP для SQL Server.
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connecting to the server, Windows Authentication
ms.assetid: f403a4e0-b0a8-4939-9dc1-e1209626367e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 62334a277bc169350af4db1c2961595178e733a6
ms.sourcegitcommit: 7eb80038c86acfef1d8e7bfd5f4e30e94aed3a75
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2020
ms.locfileid: "92081763"
---
# <a name="how-to-connect-using-windows-authentication"></a>Руководство. Подключение с помощью проверки подлинности Windows
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

По умолчанию [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] использует для подключения к серверу проверку подлинности Windows. Следует отметить, что в большинстве случаев это значит, что для подключения к серверу используется удостоверение процесса или удостоверение потока веб-сервера (если веб-сервер использует олицетворение), а не удостоверение конечного пользователя.  
  
При использовании проверки подлинности Windows для подключения к SQL Server необходимо учитывать следующие аспекты.  
  
-   Для установки соединения учетные данные, с которыми выполняется процесс (или поток) веб-сервера, должны соответствовать допустимому имени входа SQL Server.  
  
-   Если SQL Server и веб-сервер находятся на разных компьютерах, необходимо настроить SQL Server для поддержки удаленных подключений.  
  
> [!NOTE]  
> Атрибуты соединения, такие как *Database* и *ConnectionPooling* , можно задать при установке соединения. Полный список поддерживаемых атрибутов соединения см. в статье [Connection Options](connection-options.md).  
  
Для подключения к SQL Server следует по возможности использовать проверку подлинности Windows по следующим причинам.  
  
-   Во время проверки подлинности по сети не передаются никакие учетные данные; имена пользователей и пароли не внедряются в строку подключения базы данных. Это означает, что злоумышленники или пользователи-злоумышленники не смогут получить учетные данные путем мониторинга сети или просмотра строк подключения в файлах конфигурации.  
  
-   На пользователей распространяется централизованное управление учетными записями, применяются политики безопасности, касающиеся, например, сроков действия паролей, минимальной длины паролей и блокировки учетных записей после нескольких неудачных запросов на вход.  
  
Если использование проверки подлинности Windows не является целесообразным, см. [практическое руководство по подключению с использованием проверки подлинности SQL Server](how-to-connect-using-sql-server-authentication.md).  
  
## <a name="sqlsrv-example"></a>Пример SQLSRV  
Благодаря драйверу SQLSRV [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]следующий пример использует проверку подлинности Windows для подключения к локальному экземпляру SQL Server. После установки соединения на сервер отправляет запрос имени входа пользователя, осуществляющего доступ к базе данных.  
  
В примере предполагается, что SQL Server и базы данных [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) установлены на локальном компьютере. При выполнении примера в браузере все выходные данные выводятся в браузер.  
  
```  
<?php  
/* Specify the server and connection string attributes. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
  
/* Connect using Windows Authentication. */  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Unable to connect.</br>";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Query SQL Server for the login of the user accessing the  
database. */  
$tsql = "SELECT CONVERT(varchar(32), SUSER_SNAME())";  
$stmt = sqlsrv_query( $conn, $tsql);  
if( $stmt === false )  
{  
     echo "Error in executing query.</br>";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Retrieve and display the results of the query. */  
$row = sqlsrv_fetch_array($stmt);  
echo "User login: ".$row[0]."</br>";  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="pdo_sqlsrv-example"></a>Пример PDO_SQLSRV  
Следующий пример использует драйвер PDO_SQLSRV для выполнения той же задачи, что и в предыдущем примере.  
  
```  
<?php  
try {  
   $conn = new PDO( "sqlsrv:Server=(local);Database=AdventureWorks", NULL, NULL);   
   $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );  
}  
  
catch( PDOException $e ) {  
   die( "Error connecting to SQL Server" );   
}  
  
echo "Connected to SQL Server\n";  
  
$query = 'select * from Person.ContactType';   
$stmt = $conn->query( $query );   
while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){   
   print_r( $row );   
}  
?>  
```  
  
## <a name="see-also"></a>См. также:  
[Руководство. Connect Using SQL Server Authentication](how-to-connect-using-sql-server-authentication.md) (Как установить подключение с использованием проверки подлинности SQL Server)

[Руководство по программированию драйверов Microsoft для PHP для SQL Server](programming-guide-for-php-sql-driver.md)

[Информация о примерах кода в документации](about-code-examples-in-the-documentation.md)

[Руководство. Создание имени входа SQL Server](../../relational-databases/security/authentication-access/create-a-login.md)

[Руководство. Создание пользователя базы данных](../../relational-databases/security/authentication-access/create-a-database-user.md)

[Управление пользователями, ролями и именами входа](../../relational-databases/server-management-objects-smo/tasks/managing-users-roles-and-logins.md)

[Отделение пользователей от схем](../../relational-databases/server-management-objects-smo/tasks/managing-users-roles-and-logins.md)

[Предоставление разрешений для объекта (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)  
  

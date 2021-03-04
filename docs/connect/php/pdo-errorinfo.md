---
title: PDO::errorInfo
description: Справочник по API для функции PDO::errorInfo в драйвере Microsoft PDO_SQLSRV для PHP для SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 9d5481d5-13bc-4388-b3aa-78676c0fc709
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 62ad5c26342ce03d3098d5a835776d193df4b88c
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837236"
---
# <a name="pdoerrorinfo"></a>PDO::errorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Извлекает расширенные сведения об ошибке для последней операции с дескриптором базы данных.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
array PDO::errorInfo();  
```  
  
## <a name="return-value"></a>Возвращаемое значение  
Массив сведений об ошибке для последней операции с дескриптором базы данных. Этот массив состоит из следующих полей:  
  
-   Код ошибки SQLSTATE.  
  
-   Код ошибки, относящийся к драйверу.  
  
-   Сообщение об ошибке, относящееся к драйверу.  
  
Если ошибка отсутствует или не задано SQLSTATE, то поля, относящиеся к драйверу, будут иметь значение NULL.  
  
## <a name="remarks"></a>Remarks  
PDO::errorInfo возвращает только сведения об ошибках для операций, выполненных непосредственно в базе данных. Используйте PDOStatement::errorInfo при создании экземпляра PDOStatement с помощью PDO::prepare или PDO::query.  
  
Поддержка PDO была добавлена в версии 2.0 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a>Пример  
В этом примере неправильно указано имя столбца (`Cityx` вместо `City`), что вызывает ошибку, о которой затем сообщается.  
  
```php
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
echo "\n";  
print_r ($conn->errorInfo());  
?>  
```  

## <a name="additional-odbc-messages"></a>Дополнительные сообщения ODBC

При возникновении исключения драйвер ODBC может вернуть несколько ошибок, которые помогают в диагностике проблем. Однако PDO::errorInfo всегда выдает только первую ошибку. В ответ на этот [отчет об ошибке](https://bugs.php.net/bug.php?id=78196) функции [PDO::errorInfo](https://www.php.net/manual/en/pdo.errorinfo.php) и [PDOStatement::errorInfo](https://www.php.net/manual/en/pdostatement.errorinfo.php) были обновлены: теперь драйверы должны выводить *по крайней мере* следующие три поля:
```
0   SQLSTATE error code (a five characters alphanumeric identifier defined in the ANSI SQL standard).
1   Driver specific error code.
2   Driver specific error message.
```

Начиная с версии 5.9.0 функция PDO::errorInfo по умолчанию выводит дополнительные ошибки ODBC при их наличии. Пример.

```php
<?php  
try {
    $conn = new PDO("sqlsrv:server=$server;", $uid, $pwd);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    $stmt = $conn->prepare("SET NOCOUNT ON; USE $database; SELECT 1/0 AS col1");
    $stmt->execute();
} catch (PDOException $e) {
    var_dump($e->errorInfo);
}
?>  
```  

При выполнении приведенного выше скрипта должно было произойти исключение, и выходные данные должны выглядеть так:

```
array(6) {
  [0]=>
  string(5) "01000"
  [1]=>
  int(5701)
  [2]=>
  string(91) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Changed database context to 'tempdb'."
  [3]=>
  string(5) "22012"
  [4]=>
  int(8134)
  [5]=>
  string(87) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Divide by zero error encountered."
}
```

Если пользователь предпочитает прежнее поведение, отключить новое можно с помощью параметра конфигурации `pdo_sqlsrv.report_additional_errors`. Просто добавьте следующую строку в начало любого скрипта PHP:

```
ini_set('pdo_sqlsrv.report_additional_errors', 0);
```

В этом случае при выполнении того же примера скрипта будут показаны следующие сведения об ошибках:

```
array(3) {
  [0]=>
  string(5) "01000"
  [1]=>
  int(5701)
  [2]=>
  string(91) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Changed database context to 'tempdb'."
}
```

При необходимости пользователь может добавить следующую строку в файл php.ini, чтобы отключить эту функцию во всех скриптах PHP:

```
pdo_sqlsrv.report_additional_errors = 0
```

## <a name="warnings-and-errors"></a>Предупреждения и ошибки

Начиная с версии 5.9.0 предупреждения ODBC больше не будут регистрироваться как ошибки. То есть [коды ошибок](../../odbc/reference/appendixes/appendix-a-odbc-error-codes.md) с префиксом 01 регистрируются как предупреждения. Иными словами, если вы хотите регистрировать только ошибки, измените файл php.ini следующим образом:

```
[pdo_sqlsrv]  
pdo_sqlsrv.log_severity = 1
```

В этом случае в файле журнала не будет сообщений с предупреждениями. Ознакомьтесь с тем, как [ведется журнал](./logging-activity.md#logging-activity-using-the-pdo_sqlsrv-driver) для пользователей pdo_sqlsrv.

## <a name="see-also"></a>См. также:  
[Класс PDO](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  

[PDOStatement::errorInfo](../../connect/php/pdostatement-errorinfo.md)
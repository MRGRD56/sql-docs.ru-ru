---
title: PDOStatement::getColumnMeta
description: Справочник по API для функции PDOStatement::getColumnMeta в драйвере Microsoft PDO_SQLSRV для PHP для SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: c92a21cc-8e53-43d0-a4bf-542c77c100c9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bbfddcbf39dbbba3b63c6853ea7700e6a27a4614
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837223"
---
# <a name="pdostatementgetcolumnmeta"></a>PDOStatement::getColumnMeta
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Извлекает метаданные для столбца.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
array PDOStatement::getColumnMeta ( $column );  
```  
  
#### <a name="parameters"></a>Параметры  
*$conn*: отсчитываемый от нуля номер столбца (целое число), метаданные которого требуется извлечь.  
  
## <a name="return-value"></a>Возвращаемое значение  
Ассоциативный массив (ключ и значение), содержащий метаданные для столбца. Описание полей в массиве см. в разделе "Примечания".  
  
## <a name="remarks"></a>Remarks  
В следующей таблице перечислены поля в массиве, возвращенном getColumnMeta.  
  
|ИМЯ|VALUES|  
|--------|----------|  
|native_type|Указывает тип PHP для столбца. Всегда строка.|  
|driver:decl_type|Указывает тип SQL, используемый для представления значения столбца в базе данных. Если столбец в результирующем наборе является результатом функции, это значение не возвращается PDOStatement::getColumnMeta.|  
|flags|Указывает флаги, установленные для этого столбца. Всегда равно 0.|  
|name|Указывает имя столбца в базе данных.|  
|table|Указывает имя таблицы, содержащей столбец в базе данных. Всегда пусто.|  
|len|Указывает длину столбца.|  
|точность|Указывает числовую точность этого столбца.|  
|pdo_type|Указывает тип этого столбца, представленный константами PDO::PARAM_*. Всегда PDO::PARAM_STR (2).|  
  
Поддержка PDO была добавлена в версии 2.0 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
  
## <a name="example"></a>Пример  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$stmt = $conn->query("select * from Person.ContactType");  
$metadata = $stmt->getColumnMeta(2);  
var_dump($metadata);  
  
print $metadata['sqlsrv:decl_type'] . "\n";  
print $metadata['native_type'] . "\n";  
print $metadata['name'];  
?>  
```  
  
## <a name="sensitivity-data-classification-metadata"></a>Метаданные классификации данных о конфиденциальности

Начиная с версии 5.8.0, пользователям доступен новый атрибут инструкции `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION`, который позволяет получать [метаданные классификации данных о конфиденциальности](../../relational-databases/security/sql-data-discovery-and-classification.md) из Microsoft SQL Server 2019 с помощью `PDOStatement::getColumnMeta`. Для этого требуется драйвер Microsoft ODBC Driver версии 17.4.2 или выше.

Обратите внимание, что атрибут `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` по умолчанию имеет значение `false`, но вы можете указать значение `true`, и тогда массив упомянутого выше поля `flags` будет заполнен метаданными классификации данных о конфиденциальности, если они существуют. 

В качестве примера рассмотрим таблицу Patients:

```
CREATE TABLE Patients 
      [PatientId] int identity,
      [SSN] char(11),
      [FirstName] nvarchar(50),
      [LastName] nvarchar(50),
      [BirthDate] date)
```

Столбцы SSN и BirthDate можно классифицировать следующим образом:

```
ADD SENSITIVITY CLASSIFICATION TO [Patients].SSN WITH (LABEL = 'Highly Confidential - secure privacy', INFORMATION_TYPE = 'Credentials')
ADD SENSITIVITY CLASSIFICATION TO [Patients].BirthDate WITH (LABEL = 'Confidential Personal Data', INFORMATION_TYPE = 'Birthdays')
```

Чтобы получить доступ к метаданным, установите для `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` значение true и примените `PDOStatement::getColumnMeta`, как показано в следующем фрагменте кода:

```
$options = array(PDO::SQLSRV_ATTR_DATA_CLASSIFICATION => true);
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = $conn->prepare($tsql, $options);
$stmt->execute();
$numCol = $stmt->columnCount();

for ($i = 0; $i < $numCol; $i++) {
    $metadata = $stmt->getColumnMeta($i);
    $jstr = json_encode($metadata);
    echo $jstr . PHP_EOL;
}
```

Выходные данные содержат метаданные для всех столбцов:

```
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"int identity","native_type":"string","table":"","pdo_type":2,"name":"PatientId","len":10,"precision":0}
{"flags":{"Data Classification":[{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""}}]},"sqlsrv:decl_type":"char","native_type":"string","table":"","pdo_type":2,"name":"SSN","len":11,"precision":0}
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"FirstName","len":50,"precision":0}
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"LastName","len":50,"precision":0}
{"flags":{"Data Classification":[{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""}}]},"sqlsrv:decl_type":"date","native_type":"string","table":"","pdo_type":2,"name":"BirthDate","len":10,"precision":0}
```

Если мы изменим приведенный выше фрагмент кода так, чтобы параметр `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` имел значение `false` (вариант по умолчанию), поле `flags` снова будет получать значение `0`, как показано ниже:

```
{"flags":0,"sqlsrv:decl_type":"int identity","native_type":"string","table":"","pdo_type":2,"name":"PatientId","len":10,"precision":0}
{"flags":0,"sqlsrv:decl_type":"char","native_type":"string","table":"","pdo_type":2,"name":"SSN","len":11,"precision":0}
{"flags":0,"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"FirstName","len":50,"precision":0}
{"flags":0,"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"LastName","len":50,"precision":0}
{"flags":0,"sqlsrv:decl_type":"date","native_type":"string","table":"","pdo_type":2,"name":"BirthDate","len":10,"precision":0}
```

## <a name="sensitivity-rank-using-a-predefined-set-of-values"></a>Ранг конфиденциальности на основе предопределенного набора значений

Начиная с версии 5.9.0 в драйверах PHP появилась возможность извлечения ранга классификации при использовании ODBC Driver 17.4.2 или более поздней версии. Пользователь может определить ранг при использовании [ADD SENSITIVITY CLASSIFICATION](../../t-sql/statements/add-sensitivity-classification-transact-sql.md) для классификации любого столбца данных. 

Например, если пользователь назначает `NONE` и `LOW` столбцам BirthDate и SSN соответственно, представление JSON будет выглядеть следующим образом:

```
{"0":{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""},"rank":0},"rank":0}
{"0":{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""},"rank":10},"rank":10}
```

Как видно из [классификации конфиденциальности](../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md), ранги имеют следующие числовые значения:

```
0 for NONE
10 for LOW
20 for MEDIUM
30 for HIGH
40 for CRITICAL
```

Таким образом, если вместо `RANK=NONE` пользователь задает `RANK=CRITICAL` при классификации столбца BirthDate, метаданные классификации будут следующими:

```
array(1) {
  ["Data Classification"]=>
  array(2) {
    [0]=>
    array(3) {
      ["Label"]=>
      array(2) {
        ["name"]=>
        string(26) "Confidential Personal Data"
        ["id"]=>
        string(0) ""
      }
      ["Information Type"]=>
      array(2) {
        ["name"]=>
        string(9) "Birthdays"
        ["id"]=>
        string(0) ""
      }
      ["rank"]=>
      int(40)
    }
    ["rank"]=>
    int(40)
  }
}
```

Обновленное представление JSON выглядит так:

```
{"0":{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""},"rank":40},"rank":40}
```

## <a name="see-also"></a>См. также:  
[Класс PDOStatement](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)
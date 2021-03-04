---
description: sqlsrv_field_metadata
title: sqlsrv_field_metadata | Документация Майкрософт
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_field_metadata
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_field_metadata
- sqlsrv_field_metadata
ms.assetid: c02f6942-0484-4567-a78e-fe8aa2053536
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 82e1ad1147867abd9308dc89517855df830a38e6
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837157"
---
# <a name="sqlsrv_field_metadata"></a>sqlsrv_field_metadata
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Извлекает метаданные для полей подготовленной инструкции. Дополнительные сведения о подготовке инструкции см. в статье [sqlsrv_query](../../connect/php/sqlsrv-query.md) или [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md). Обратите внимание, что **sqlsrv_field_metadata** можно вызвать для любой подготовленной инструкции до или после выполнения.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sqlsrv_field_metadata( resource $stmt)  
```  
  
#### <a name="parameters"></a>Параметры  
*$stmt*: ресурс инструкции, для полей которого выполняется поиск метаданных.  
  
## <a name="return-value"></a>Возвращаемое значение  
**Массив** массивов или значение **false**. Массив содержит один массив для каждого поля в результирующем наборе. Каждый подмассив имеет ключи, как описано в следующей таблице. Если при извлечении метаданных полей возникает ошибка, возвращается значение **false** .  
  
|Клавиши|Описание|  
|-------|---------------|  
|name|Имя столбца, которому соответствует поле.|  
|Тип|Числовое значение, соответствующее типу SQL.|  
|Размер|Число символов для полей символьного типа (char(n), varchar(n), nchar(n), nvarchar(n), XML). Число байтов для полей двоичного типа (binary(n), varbinary(n), UDT). Значение **NULL** для других типов данных SQL Server.|  
|Точность|Точность для типов переменной точности (real, numeric, decimal, datetime2, datetimeoffset и time). Значение **NULL** для других типов данных SQL Server.|  
|Масштабирование|Масштаб для типов переменного масштаба (numeric, decimal, datetime2, datetimeoffset и time). Значение **NULL** для других типов данных SQL Server.|  
|Допускает значения NULL|Перечисляемое значение, указывающее, что столбец допускает значение NULL (**SQLSRV_NULLABLE_YES**), столбец не допускает значение NULL (**SQLSRV_NULLABLE_NO**), либо неизвестно, допускает ли столбец значение NULL (**SQLSRV_NULLABLE_UNKNOWN**).|  
  
Следующая таблица содержит дополнительные сведения о ключах для каждого подмассива (дополнительные сведения об этих типах см. в документации по SQL Server):  
  
|Тип данных SQL Server 2008|Тип|Минимальная/максимальная точность|Минимальный/максимальный масштаб|Размер|  
|-----------------------------|--------|----------------------|------------------|--------|  
|BIGINT|SQL_BIGINT (-5)|||8|  
|binary|SQL_BINARY (-2)|||0 < *n* < 8000 <sup>1</sup>|  
|bit|SQL_BIT (-7)||||  
|char|SQL_CHAR (1)|||0 < *n* < 8000 <sup>1</sup>|  
|Дата|SQL_TYPE_DATE (91)|10/10|0/0||  
|DATETIME|SQL_TYPE_TIMESTAMP (93)|23/23|3/3||  
|datetime2|SQL_TYPE_TIMESTAMP (93)|19/27|0/7||  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET (-155)|26/34|0/7||  
|Decimal|SQL_DECIMAL (3)|1/38|0/значение точности||  
|FLOAT|SQL_FLOAT (6)|4/8|||  
|Изображение|SQL_LONGVARBINARY (-4)|||2 ГБ|  
|INT|SQL_INTEGER (4)||||  
|money|SQL_DECIMAL (3)|19/19|4/4||  
|nchar|SQL_WCHAR (-8)|||0 < *n* < 4000 <sup>1</sup>|  
|ntext|SQL_WLONGVARCHAR (-10)|||1 ГБ|  
|NUMERIC|SQL_NUMERIC (2)|1/38|0/значение точности||  
|nvarchar|SQL_WVARCHAR (-9)|||0 < *n* < 4000 <sup>1</sup>|  
|real|SQL_REAL (7)|4/4|||  
|smalldatetime|SQL_TYPE_TIMESTAMP (93)|16/16|0/0||  
|smallint|SQL_SMALLINT (5)|||2 байта|  
|Smallmoney|SQL_DECIMAL (3)|10/10|4/4||  
|sql_variant|SQL_SS_VARIANT (-150)|||Переменная|  
|текст|SQL_LONGVARCHAR (-1)|||2 ГБ|  
|time|SQL_SS_TIME2 (-154)|8/16|0/7||  
|TIMESTAMP|SQL_BINARY (-2)|||8 байт|  
|tinyint|SQL_TINYINT (-6)|||1 байт|  
|определяемый пользователем тип|SQL_SS_UDT (-151)|||Переменная|  
|UNIQUEIDENTIFIER|SQL_GUID (-11)|||16|  
|varbinary|SQL_VARBINARY (-3)|||0 < *n* < 8000 <sup>1</sup>|  
|varchar|SQL_VARCHAR (12)|||0 < *n* < 8000 <sup>1</sup>|  
|Xml|SQL_SS_XML (-152)|||0|  
  
(1) Нуль (0) указывает, что разрешен максимальный размер.  
  
Ключ Nullable может иметь значение yes или no.  
  
## <a name="example"></a>Пример  
Следующий пример создает ресурс инструкции, а затем извлекает и отображает метаданные полей. В примере предполагается, что SQL Server и базы данных [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) установлены на локальном компьютере. При выполнении примера из командной строки все выходные данные выводятся в консоль.  
  
```
<?php
/* Connect to the local server using Windows Authentication and
specify the AdventureWorks database as the database in use. */
$serverName = "(local)";
$connectionInfo = array("Database"=>"AdventureWorks");
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
    echo "Could not connect.\n";
    die( print_r( sqlsrv_errors(), true));
}

/* Prepare the statement. */
$tsql = "SELECT ReviewerName, Comments FROM Production.ProductReview";
$stmt = sqlsrv_prepare( $conn, $tsql);
  
/* Get and display field metadata. */
foreach( sqlsrv_field_metadata( $stmt) as $fieldMetadata) {
    foreach( $fieldMetadata as $name => $value) {
        echo "$name: $value\n";
    }  
    echo "\n";
}  
  
/* Note: sqlsrv_field_metadata can be called on any statement
resource, pre- or post-execution. */
  
/* Free statement and connection resources. */
sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);
?>
```

## <a name="sensitivity-data-classification-metadata"></a>Метаданные классификации данных о конфиденциальности

В версии 5.8.0 появилась новая возможность `DataClassification`, которая позволяет пользователям получать [метаданные классификации данных о конфиденциальности](../../relational-databases/security/sql-data-discovery-and-classification.md) из Microsoft SQL Server 2019 с помощью `sqlsrv_field_metadata`. Для этого требуется драйвер Microsoft ODBC Driver версии 17.4.2 или выше. 

По умолчанию что атрибут `DataClassification` имеет значение `false`, но вы можете указать значение `true`, и тогда возвращаемый из `sqlsrv_field_metadata` массив будет заполнен метаданными классификации данных о конфиденциальности, если они существуют. 

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

Чтобы получить доступ к метаданным, примените `sqlsrv_field_metadata`, как показано в следующем фрагменте кода:

```
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = sqlsrv_prepare($conn, $tsql, array(), array('DataClassification' => true));
if (sqlsrv_execute($stmt)) {
    $fieldmeta = sqlsrv_field_metadata($stmt);

    foreach ($fieldmeta as $f) {
        if (count($f['Data Classification']) > 0) {
            echo $f['Name'] . ": \n";
            print_r($f['Data Classification']); 
        }
    }
}
```

Выходные данные должны выглядеть следующим образом:

```
SSN: 
Array
(
    [0] => Array
        (
            [Label] => Array
                (
                    [name] => Highly Confidential - secure privacy
                    [id] => 
                )

            [Information Type] => Array
                (
                    [name] => Credentials
                    [id] => 
                )

        )

)
BirthDate: 
Array
(
    [0] => Array
        (
            [Label] => Array
                (
                    [name] => Confidential Personal Data
                    [id] => 
                )

            [Information Type] => Array
                (
                    [name] => Birthdays
                    [id] => 
                )

        )

)
```

Если вместо `sqlsrv_prepare` используется `sqlsrv_query`, приведенный выше фрагмент можно изменить следующим образом:

```
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = sqlsrv_query($conn, $tsql, array(), array('DataClassification' => true));
$fieldmeta = sqlsrv_field_metadata($stmt);

foreach ($fieldmeta as $f) {
    $jstr = json_encode($f);
    echo $jstr . PHP_EOL;
}
```

Как видно в представлении JSON ниже, здесь отображаются метаданные классификации данных, если таковые связаны со столбцами:

```
{"Name":"PatientId","Type":4,"Size":null,"Precision":10,"Scale":null,"Nullable":0,"Data Classification":[]}
{"Name":"SSN","Type":1,"Size":11,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""}}]}
{"Name":"FirstName","Type":-9,"Size":50,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[]}
{"Name":"LastName","Type":-9,"Size":50,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[]}
{"Name":"BirthDate","Type":91,"Size":null,"Precision":10,"Scale":0,"Nullable":1,"Data Classification":[{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""}}]}
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
  array(7) {
    ["Name"]=>
    string(9) "BirthDate"
    ["Type"]=>
    int(91)
    ["Size"]=>
    NULL
    ["Precision"]=>
    int(10)
    ["Scale"]=>
    int(0)
    ["Nullable"]=>
    int(1)
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
[Справочник по API для драйвера SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  

[Константы (драйверы Майкрософт для PHP для SQL Server)](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)  

[Информация о примерах кода в документации](../../connect/php/about-code-examples-in-the-documentation.md)
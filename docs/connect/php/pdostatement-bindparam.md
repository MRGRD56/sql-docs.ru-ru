---
title: PDOStatement::bindParam
description: Справочник по API для функции PDOStatement::bindParam в драйвере Microsoft PDO_SQLSRV для PHP для SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 65212058-2632-47a4-ba7d-2206883abf09
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dd2d1feb1ae156d685dbd18595447a248836eba9
ms.sourcegitcommit: 7eb80038c86acfef1d8e7bfd5f4e30e94aed3a75
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2020
ms.locfileid: "92081423"
---
# <a name="pdostatementbindparam"></a>PDOStatement::bindParam
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Привязывает параметр к именованному заполнителю или заполнителю с вопросительным знаком в инструкции SQL.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
bool PDOStatement::bindParam($parameter, &$variable[, $data_type[, $length[, $driver_options]]]);  
```  
  
#### <a name="parameters"></a>Параметры  
$*parameter*: идентификатор параметра (смешанные значения). Для инструкции, использующей именованные заполнители, это имя параметра (:name). Для подготовленной инструкции, использующей синтаксис с вопросительным знаком, это индекс параметра, идущий от единицы.  
  
&$*variable*: имя (смешанные значения) переменной PHP, привязываемой к параметру инструкции SQL.  
  
$*data_type*: необязательная константа PDO::PARAM_* (целое число). Значение по умолчанию — PDO::PARAM_STR.  
  
$*length*: необязательная длина типа данных (целое число). С помощью PDO::SQLSRV_PARAM_OUT_DEFAULT_SIZE вы можете указать размер по умолчанию при использовании PDO::PARAM_INT или PDO::PARAM_BOOL в $*data_type*.  
  
$*driver_options*: необязательные параметры драйвера (смешанное значение). Например, можно указать PDO::SQLSRV_ENCODING_UTF8 для привязки столбца к переменной в виде строки с кодировкой UTF-8.  
  
## <a name="return-value"></a>Возвращаемое значение  
Значение TRUE в случае успеха, в противном случае — значение FALSE.  
  
## <a name="remarks"></a>Remarks  
Если вы привязываете данные NULL к серверным столбцам типа varbinary, binary или varbinary(max), необходимо указать двоичную кодировку (PDO::SQLSRV_ENCODING_BINARY) с помощью $*driver_options*. Дополнительные сведения о кодировании констант: [Константы](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md).  
  
Поддержка PDO была добавлена в версии 2.0 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  

## <a name="parameter-example"></a>Пример параметра  
Этот пример кода показывает, что после привязки значения $contact к параметру изменение этого значения приводит к изменению значения, передаваемого в запрос.  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  
  
$contact = "Sales Agent";  
$stmt = $conn->prepare("select * from Person.ContactType where name = ?");  
$stmt->bindParam(1, $contact);  
$contact = "Owner";  
$stmt->execute();  
  
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {  
   print "$row[Name]\n\n";  
}  
  
$stmt = null;  
$contact = "Sales Agent";  
$stmt = $conn->prepare("select * from Person.ContactType where name = :contact");  
$stmt->bindParam(':contact', $contact);  
$contact = "Owner";  
$stmt->execute();  
  
while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {  
   print "$row[Name]\n\n";  
}  
?>  
```  
  
## <a name="output-parameter-example"></a>Пример параметра вывода  
Этот пример кода показывает, как получить доступ к параметру вывода.  
  
```  
<?php  
$database = "Test";  
$server = "(local)";  
$conn = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  
  
$input1 = 'bb';  
  
$stmt = $conn->prepare("select ? = count(*) from Sys.tables");  
$stmt->bindParam(1, $input1, PDO::PARAM_STR, 10);  
$stmt->execute();  
echo $input1;  
?>  
```  
  
> [!NOTE]
> При привязке выходного параметра к типу bigint, если значение может находиться вне диапазона для типа [integer](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md), использование PDO::PARAM_INT в сочетании с PDO::SQLSRV_PARAM_OUT_DEFAULT_SIZE может привести к исключению "значение вне диапазона". Поэтому здесь лучше использовать вариант по умолчанию PDO::PARAM_STR и задать размер результирующей строки не более 21. Это максимальное число цифр, включая минус, для любого значения bigint. 

## <a name="inputoutput-example"></a>Пример входных и выходных данных  
Этот пример кода показывает, как получить доступ к параметру ввода/вывода.  
  
```  
<?php  
   $database = "AdventureWorks";  
   $server = "(local)";  
   $dbh = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  
  
   $dbh->query("IF OBJECT_ID('dbo.sp_ReverseString', 'P') IS NOT NULL DROP PROCEDURE dbo.sp_ReverseString");  
   $dbh->query("CREATE PROCEDURE dbo.sp_ReverseString @String as VARCHAR(2048) OUTPUT as SELECT @String = REVERSE(@String)");  
   $stmt = $dbh->prepare("EXEC dbo.sp_ReverseString ?");  
   $string = "123456789";  
   $stmt->bindParam(1, $string, PDO::PARAM_STR | PDO::PARAM_INPUT_OUTPUT, 2048);  
   $stmt->execute();  
   print $string;   // Expect 987654321  
?>  
```  

> [!NOTE]
> Рекомендуется использовать строки в качестве входных данных при привязке значений к [десятичным или числовым столбцам](../../t-sql/data-types/decimal-and-numeric-transact-sql.md), чтобы обеспечить точность и правильность, поскольку PHP имеет ограниченную точность для [чисел с плавающей запятой](https://php.net/manual/en/language.types.float.php). То же касается и столбцов bigint, особенно в том случае, если значения выходят за пределы диапазона [целых чисел](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md).

## <a name="decimal-input-example"></a>Пример входного значения десятичного числа  
В этом примере кода показано, как привязать десятичное значение в качестве входного параметра.  

```
<?php  
$database = "Test";  
$server = "(local)";  
$conn = new PDO("sqlsrv:server=$server ; Database = $database", "", "");  

// Assume TestTable exists with a decimal field 
$input = "9223372036854.80000";
$stmt = $conn->prepare("INSERT INTO TestTable (DecimalCol) VALUES (?)");
// by default it is PDO::PARAM_STR, rounding of a large input value may
// occur if PDO::PARAM_INT is specified
$stmt->bindParam(1, $input, PDO::PARAM_STR);
$stmt->execute();
```


## <a name="see-also"></a>См. также:  
[Класс PDOStatement](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  

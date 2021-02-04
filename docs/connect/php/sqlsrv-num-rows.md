---
title: sqlsrv_num_rows
description: Справочник по API для функции sqlsrv_num_rows в драйвере SQLSRV для PHP для SQL Server.
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- API Reference, sqlsrv_num_rows
- sqlsrv_num_rows
ms.assetid: c832210e-bb2a-47b5-a505-160b02d1d95e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fe5599045702f2fec458df608d5fb25124732cff
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99153589"
---
# <a name="sqlsrv_num_rows"></a>sqlsrv_num_rows
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Сообщает число строк в результирующем наборе.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sqlsrv_num_rows( resource $stmt )  
```  
  
#### <a name="parameters"></a>Параметры  
*$stmt*: результирующий набор, для которого требуется подсчитать строки.  
  
## <a name="return-value"></a>Возвращаемое значение  
Значение **false** , если произошла ошибка при вычислении количества строк. В противном случае возвращает число строк в результирующем наборе.  
  
## <a name="remarks"></a>Remarks  
Инструкция sqlsrv_num_rows требует использовать курсор CLIENT, STATIC или KEYSET и возвращает значение **false** (ложь), если используется курсор FORWARD или DYNAMIC. (По умолчанию используется однонаправленный курсор.) Дополнительные сведения о курсорах см. в статьях [sqlsrv_query](../../connect/php/sqlsrv-query.md) и [Типы курсоров &#40;драйвер SQLSRV&#41;](../../connect/php/cursor-types-sqlsrv-driver.md).  
  
## <a name="example"></a>Пример  
  
```  
<?php  
   $server = "server_name";  
   $conn = sqlsrv_connect( $server, array( 'Database' => 'Northwind' ) );  
  
   $stmt = sqlsrv_query( $conn, "select * from orders where CustomerID = 'VINET'" , array(), array( "Scrollable" => SQLSRV_CURSOR_KEYSET ));  
  
   $row_count = sqlsrv_num_rows( $stmt );  
  
   if ($row_count === false)  
      echo "\nerror\n";  
   else if ($row_count >=0)  
      echo "\n$row_count\n";  
?>  
```  
  
В следующем примере показано, что при наличии более чем одного результирующего набора (пакетного запроса) число строк доступно только при использовании клиентского курсора.  
  
```  
<?php  
$serverName = "(local)";  
$connectionInfo = array("Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
  
$tsql = "select * from HumanResources.Department";  
  
// Client-side cursor and batch statements  
$tsql = "select top 2 * from HumanResources.Employee;Select top 3 * from HumanResources.EmployeeAddress";  
  
// works  
$stmt = sqlsrv_query($conn, $tsql, array(), array("Scrollable"=>"buffered"));  
  
// fails  
// $stmt = sqlsrv_query($conn, $tsql);  
// $stmt = sqlsrv_query($conn, $tsql, array(), array("Scrollable"=>"forward"));  
// $stmt = sqlsrv_query($conn, $tsql, array(), array("Scrollable"=>"static"));  
// $stmt = sqlsrv_query($conn, $tsql, array(), array("Scrollable"=>"keyset"));  
// $stmt = sqlsrv_query($conn, $tsql, array(), array("Scrollable"=>"dynamic"));  
  
$row_count = sqlsrv_num_rows( $stmt );  
echo "\nRow count for first result set = $row_count\n";  
  
sqlsrv_next_result($stmt);  
  
$row_count = sqlsrv_num_rows( $stmt );  
echo "\nRow count for second result set = $row_count\n";  
?>  
```  
  
## <a name="see-also"></a>См. также:  
[Справочник по API для драйвера SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  
  

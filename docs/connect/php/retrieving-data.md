---
description: Извлечение данных
title: Получение данных | Документы Майкрософт
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 3414992c-61c0-4e7d-b509-72517e52c1bb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 35a9d9131b7c1cb382ab4e64baa26809d58c90d3
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88414430"
---
# <a name="retrieving-data"></a>Извлечение данных
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Эта статья и статьи в данном разделе посвящены извлечению данных.  
  
## <a name="sqlsrv-driver"></a>Драйвер SQLSRV  
Драйвер SQLSRV [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] предоставляет следующие возможности для извлечения данных из результирующего набора:  
  
-   [sqlsrv_fetch_array](../../connect/php/sqlsrv-fetch-array.md)  
  
-   [sqlsrv_fetch_object](../../connect/php/sqlsrv-fetch-object.md)  
  
-   [sqlsrv_fetch](../../connect/php/sqlsrv-fetch.md)/[sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md)  
  
> [!NOTE]  
> При использовании любых из упомянутых выше функций, избегайте использования сравнений со значением NULL в качестве критерия для выхода из циклов. Поскольку функции **sqlsrv** возвращают значение false, когда происходит ошибка, следующий код может привести к бесконечному циклу при возникновении ошибки в [sqlsrv_fetch_array](../../connect/php/sqlsrv-fetch-array.md):  
>   
> `/*``This code could result in an infinite loop. It is recommended that`  
>   
> `you do NOT use null comparisons as the criterion for exiting loops,`  
>   
> `as is done here. */`  
>   
> `do{`  
>   
> `$result = sqlsrv_fetch_array($stmt);`  
>   
> `} while( !is_null($result));`  
  
Если запрос извлекает больше одного результирующего набора, можно перейти к следующему результирующему набору с помощью [sqlsrv_next_result](../../connect/php/sqlsrv-next-result.md).  
  
Начиная с версии 1.1 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] можно использовать [sqlsrv_has_rows](../../connect/php/sqlsrv-has-rows.md), чтобы определить наличие строк в результирующем наборе.  
  
## <a name="pdo_sqlsrv-driver"></a>Драйвер PDO_SQLSRV  
Драйвер PDO_SQLSRV [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] предоставляет следующие возможности для извлечения данных из результирующего набора:  
  
-   [PDOStatement::fetch](../../connect/php/pdostatement-fetch.md)  
  
-   [PDOStatement::fetchAll](../../connect/php/pdostatement-fetchall.md)  
  
-   [PDOStatement::fetchColumn](../../connect/php/pdostatement-fetchcolumn.md)  
  
-   [PDOStatement::fetchObject](../../connect/php/pdostatement-fetchobject.md)  
  
Если запрос извлекает больше одного результирующего набора, можно перейти к следующему результирующему набору с помощью [PDOStatement::nextRowset](../../connect/php/pdostatement-nextrowset.md).  
  
Можно узнать, сколько строк содержит результирующий набор, если задать прокручиваемый курсор, а затем вызвать [PDOStatement::rowCount](../../connect/php/pdostatement-rowcount.md).  
  
[PDO::prepare](../../connect/php/pdo-prepare.md) позволяет указать тип курсора. Затем с помощью [PDOStatement::fetch](../../connect/php/pdostatement-fetch.md) можно выбрать строку. Дополнительные сведения и пример см. в статье [PDO::prepare](../../connect/php/pdo-prepare.md) .  
  
## <a name="in-this-section"></a>в этом разделе  
  
|Раздел|Описание|  
|---------|---------------|  
|[Извлечение данных в виде потока](../../connect/php/retrieving-data-as-a-stream-using-the-sqlsrv-driver.md)|Содержит общие сведения о потоковой передаче данных с сервера, а также ссылки на конкретные варианты использования.|  
|[Использование параметров направления](../../connect/php/using-directional-parameters.md)|Описывает, как использовать параметры направления при вызове хранимой процедуры.|  
|[Указание типа курсора и выбор строк](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md)|Демонстрирует, как создать результирующий набор со строками, доступ к которым можно осуществлять в любом порядке.|  
|[Практическое руководство. Получение типов даты и времени в виде строк с помощью драйвера SQLSRV](../../connect/php/how-to-retrieve-date-and-time-type-as-strings-using-the-sqlsrv-driver.md)|Здесь объясняется, как получить типы даты и времени в виде строк с помощью драйвера SQLSRV.|  
|[Как извлечь типы даты и времени в виде объектов даты и времени PHP с помощью драйвера PDO_SQLSRV](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md)|Описано, как получать типы даты и времени в виде объектов с помощью драйвера PDO_SQLSRV.|  
|[Форматирование десятичных строк и денежных значений (драйвер SQLSRV)](../../connect/php/formatting-decimals-sqlsrv-driver.md)|Сведения о том, как форматировать десятичные и денежные значения с помощью драйвера SQLSRV.|  
|[Форматирование десятичных строк и денежных значений (драйвер PDO_SQLSRV)](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md)|Сведения о том, как форматировать десятичные и денежные значения с помощью драйвера PDO_SQLSRV.|  
  
## <a name="related-sections"></a>Связанные разделы  
[Практическое руководство. Указание типов данных PHP](../../connect/php/how-to-specify-php-data-types.md)  
  
## <a name="see-also"></a>См. также:  
[Руководство по программированию драйверов Microsoft для PHP для SQL Server](../../connect/php/programming-guide-for-php-sql-driver.md)

[Извлечение данных](../../connect/php/retrieving-data.md)  
  

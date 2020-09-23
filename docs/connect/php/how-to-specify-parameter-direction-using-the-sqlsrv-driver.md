---
title: Практическое руководство по настройке направления параметров с помощью драйвера SQLSRV
description: Узнайте, как указать направление параметра при вызове хранимой процедуры с помощью драйвера Microsoft SQLSRV для PHP для SQL Server.
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- stored procedure support
ms.assetid: 1209eeca-df75-4283-96dc-714f39956b95
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f085fc40ded15310b81d6a447f30676ed011e7f8
ms.sourcegitcommit: d1051f05a7db81ec62d9785bb6af572408f3d4e0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/20/2020
ms.locfileid: "88680259"
---
# <a name="how-to-specify-parameter-direction-using-the-sqlsrv-driver"></a>Практическое руководство. Указание направления параметров с помощью драйвера SQLSRV
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

Эта статья описывает использование драйвера SQLSRV для указания направления параметров при вызове хранимой процедуры. Направление параметров указывается при создании массива параметров (шаг 3), который передается [sqlsrv_query](../../connect/php/sqlsrv-query.md) или [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md).  
  
### <a name="to-specify-parameter-direction"></a>Порядок указания направления параметров  
  
1.  Определите запрос Transact-SQL, который вызывает хранимую процедуру. Используйте вопросительные знаки (?) вместо параметров, передаваемых в хранимую процедуру. Например, эта строка вызывает хранимую процедуру (UpdateVacationHours), которая принимает два параметра:  
  
    ```  
    $tsql = "{call UpdateVacationHours(?, ?)}";  
    ```  
  
    > [!NOTE]  
    > Рекомендуется вызывать хранимые процедуры с использованием канонического синтаксиса. Дополнительные сведения о каноническом синтаксисе см. в статье [Вызов хранимой процедуры](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md).  
  
2.  Инициализируйте или обновите переменные PHP, которые соответствуют заполнителям в запросе Transact-SQL. Например, следующий код инициализирует два параметра для хранимой процедуры UpdateVacationHours:  
  
    ```  
    $employeeId = 101;  
    $usedVacationHours = 8;  
    ```  
  
    > [!NOTE]  
    >  Переменные, которые инициализируются или обновляются с использованием **null**, **DateTime**или типов потоков, нельзя использовать в качестве параметров вывода.  
  
3.  Используйте переменные PHP из шага 2, чтобы создать или обновить массив значений параметров, порядок которых соответствует заполнителям параметров в строке Transact-SQL. Укажите направление для каждого параметра в массиве. Направление каждого параметра определяется одним из двух способов: по умолчанию (для параметров ввода) или с помощью констант **SQLSRV_PARAM_\*** (для параметров вывода и двунаправленных параметров). Например, следующий код задает параметр *$employeeId* в качестве параметра ввода и параметр *$usedVacationHours* в качестве двунаправленного параметра:  
  
    ```  
    $params = array(  
                     array($employeeId, SQLSRV_PARAM_IN),  
                     array($usedVacationHours, SQLSRV_PARAM_INOUT)  
                    );  
    ```  
  
    Чтобы лучше понять синтаксис для указания направления параметра, предположим, что *$var1*, *$var2*и *$var3* соответствуют параметру ввода, параметру вывода и двунаправленному параметру. Направление параметров можно указать любым из следующих способов:  
  
    -   Укажите параметр ввода неявным образом, параметр вывода явным образом и двунаправленный параметр явным образом:  
  
        ```  
        array(   
               array($var1),  
               array($var2, SQLSRV_PARAM_OUT),  
               array($var3, SQLSRV_PARAM_INOUT)  
               );  
        ```  
  
    -   Укажите параметр ввода явным образом, параметр вывода явным образом и двунаправленный параметр явным образом:  
  
        ```  
        array(   
               array($var1, SQLSRV_PARAM_IN),  
               array($var2, SQLSRV_PARAM_OUT),  
               array($var3, SQLSRV_PARAM_INOUT)  
               );  
        ```  
  
4.  Выполните запрос с использованием [sqlsrv_query](../../connect/php/sqlsrv-query.md) или [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md) и [sqlsrv_execute](../../connect/php/sqlsrv-execute.md). Например, следующий код использует соединение *$conn* для выполнения запроса *$tsql* со значениями параметров, указанными в *$params*:  
  
    ```  
    sqlsrv_query($conn, $tsql, $params);  
    ```  
  
## <a name="see-also"></a>См. также:  
[Практическое руководство. Извлечение параметров вывода с помощью драйвера SQLSRV](../../connect/php/how-to-retrieve-output-parameters-using-the-sqlsrv-driver.md)

[Практическое руководство. Извлечение параметров ввода и вывода с помощью драйвера SQLSRV](../../connect/php/how-to-retrieve-input-and-output-parameters-using-the-sqlsrv-driver.md)  
  

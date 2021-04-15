---
title: Использование функций преобразования в запросах XPath (SQLXML)
description: Узнайте, как указать явные функции преобразования String () и Number () в запросах типа SQLXML 4,0 XPath.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- explicit conversion functions [SQLXML]
- string function
- number function
- XPath queries [SQLXML], explicit conversion functions
ms.assetid: 1111cb5d-2bd9-4bdb-8de2-dc0e47452dd6
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 33e71474d1bee154028d78385d12ecbdac3ee69f
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490497"
---
# <a name="specifying-explicit-conversion-functions-in-xpath-queries-sqlxml-40"></a>Определение явных функций преобразования в запросах XPath (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Следующие примеры показывают, как явные функции преобразования указываются в запросах XPath. В данных примерах запросы XPath определены в соответствии со схемой сопоставления, которая содержится в файле SampleSchema1.xml. Дополнительные сведения об этом образце схемы см. в разделе [Пример схемы XSD с заметками для XPath-примеров &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/sample-annotated-xsd-schema-for-xpath-examples-sqlxml-4-0.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-use-the-number-explicit-conversion-function"></a>A. Используйте функцию явного преобразования number()  
 Функция **Number ()** Преобразует аргумент в число.  
  
 Если значение параметра **ContactID** не является числовым, следующий запрос преобразует идентификатор **ContactID** в число и сравнивает его со значением 4. Затем запрос возвращает все **\<Employee>** дочерние элементы узла контекста с атрибутом **ContactID** , имеющим числовое значение 4:  
  
```  
/child::Contact[number(attribute::ContactID)= 4]  
```  
  
 Можно указать ярлык оси **атрибута** (@), а так как **дочерняя** ось является значением по умолчанию, ее можно опустить в запросе:  
  
```  
/Contact[number(@ContactID) = 4]  
```  
  
 В реляционных терминах запрос возвращает сотрудника с параметром **ContactID** , равным 4.  
  
##### <a name="to-test-the-xpath-query-against-the-mapping-schema"></a>Проверка запроса XPath к схеме сопоставления  
  
1.  Скопируйте [пример кода схемы](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/sample-annotated-xsd-schema-for-xpath-examples-sqlxml-4-0.md) и вставьте его в текстовый файл. Сохраните файл с именем SampleSchema1.xml.  
  
2.  Создайте следующий шаблон (ExplicitConversionA.xml) и сохраните его в каталоге, в котором сохранен файл SampleSchema1.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="SampleSchema1.xml">  
        /Contact[number(@ContactID)=4]  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Путь к каталогу схемы сопоставления (файл SampleSchema1.xml) задается относительно каталога, в котором сохранен шаблон. Можно также задать абсолютный путь, например:  
  
    ```  
    mapping-schema="C:\MyDir\SampleSchema1.xml"  
    ```  
  
3.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Результирующий набор для выполнения этого шаблона:  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Contact ContactID="4" LastName="Acevedo" FirstName="Humberto" Title="Sr." />   
</ROOT>  
```  
  
### <a name="b-use-the-string-explicit-conversion-function"></a>Б. Используйте функцию явного преобразования string()  
 Функция **String ()** Преобразует аргумент в строку.  
  
 Следующий запрос преобразует **ContactID** в строку и сравнивает его со строковым значением "4". Запрос возвращает все **\<Employee>** дочерние элементы узла контекста с параметром **ContactID** со строковым значением "4":  
  
```  
/child::Contact[string(attribute::ContactID)="4"]  
```  
  
 Можно указать ярлык оси **атрибута** (@), а так как **дочерняя** ось является значением по умолчанию, ее можно опустить в запросе:  
  
```  
/Contact[string(@ContactID)="4"]  
```  
  
 С функциональной точки зрения этот запрос возвращает те же результаты, что и предыдущий пример запроса, хотя выполняется сопоставление со строковым значением, а не числовым значением (то есть числом 4).  
  
##### <a name="to-test-the-xpath-query-against-the-mapping-schema"></a>Проверка запроса XPath к схеме сопоставления  
  
1.  Скопируйте [пример кода схемы](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/samples/sample-annotated-xsd-schema-for-xpath-examples-sqlxml-4-0.md) и вставьте его в текстовый файл. Сохраните файл с именем SampleSchema1.xml.  
  
2.  Создайте следующий шаблон (ExplicitConversionB.xml) и сохраните его в каталоге, в котором сохранен файл SampleSchema1.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="SampleSchema1.xml">  
        Contact[string(@ContactID)="4"]  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Путь к каталогу схемы сопоставления (файл SampleSchema1.xml) задается относительно каталога, в котором сохранен шаблон. Можно также задать абсолютный путь, например:  
  
    ```  
    mapping-schema="C:\MyDir\SampleSchema1.xml"  
    ```  
  
3.  Создайте и запустите тестовый скрипт SQLXML 4.0 (Sqlxml4test.vbs), чтобы выполнить шаблон.  
  
     Дополнительные сведения см. [в разделе Использование ADO для выполнения запросов SQLXML 4,0](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Далее приведен результирующий набор, полученный в результате выполнения этого шаблона.  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <Contact ContactID="4" LastName="Acevedo" FirstName="Humberto" Title="Sr." />   
</ROOT>  
```  
  
  

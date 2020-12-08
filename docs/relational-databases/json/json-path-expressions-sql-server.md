---
description: Выражения пути JSON (SQL Server)
title: Выражения пути JSON
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- JSON, path expressions
- path expressions (JSON)
ms.assetid: 25ea679c-84cc-4977-867c-2cbe9d192553
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 558f11fdac7b8475054500cf5d002bfec107cb7f
ms.sourcegitcommit: 28fecbf61ae7b53405ca378e2f5f90badb1a296a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2020
ms.locfileid: "96595029"
---
# <a name="json-path-expressions-sql-server"></a>Выражения пути JSON (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

 Используйте выражения пути JSON для создания ссылок на свойства объектов JSON.  
  
 Выражение пути нужно указывать при вызове следующих функций.  
  
-   При вызове **OPENJSON** для создания реляционного представления данных JSON. Дополнительные сведения см. в разделе [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md).  
  
-   При вызове **JSON_VALUE** с целью извлечения значения из текста JSON. Дополнительные сведения см. в разделе [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md).  
  
-   При вызове **JSON_QUERY** для извлечения объекта JSON или массива. Дополнительные сведения см. в разделе [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md).  
  
-   При вызове **JSON_MODIFY** для обновления значения свойства в строке JSON. Дополнительные сведения см. в разделе [JSON_MODIFY (Transact-SQL)](../../t-sql/functions/json-modify-transact-sql.md).  

## <a name="parts-of-a-path-expression"></a>Элементы выражения пути
 Выражение пути состоит из двух компонентов.  
  
1.  Необязательный компонент [path mode](#PATHMODE) со значением **lax** или **strict**.  
  
2.  Сам [путь](#PATH) .  

##  <a name="path-mode"></a><a name="PATHMODE"></a> Path mode  
 В начале выражения пути можно при необходимости объявить режим пути, указав ключевое слово **lax** или **strict**. По умолчанию **lax**.  
  
-   В режиме **lax** функция возвращает пустые значения, если выражение пути содержит ошибку. Например, если вы запрашиваете значение **$.name**, а текст JSON не содержит ключ **name**, функция вернет значение NULL, но ошибку это не вызовет.  
  
-   В режиме **strict** функция возвращает ошибку, если выражение пути содержит ошибку.  

В следующем запросе в выражении пути явно задан режим `lax`.

```sql  
DECLARE @json NVARCHAR(MAX);
SET @json=N'{ ... }';

SELECT * FROM OPENJSON(@json, N'lax $.info');
```  
  
##  <a name="path"></a><a name="PATH"></a> Path  
 Объявив необязательный режим пути, укажите сам путь.  
  
-   Знак доллара (`$`) представляет элемент контекста.  
  
-   Путь свойства — это набор действий пути. Действия пути могут содержать следующие элементы и операторы.  
  
    -   Имена ключей. Например, `$.name` и `$."first name"`. Если имя ключа начинается со знака доллара или содержит специальные символы, например пробелы, заключите его в кавычки.   
  
    -   Элементы массива. Например, `$.product[3]`. Массивы отсчитываются от нуля.  
  
    -   Оператор "точка" (`.`) указывает на элемент объекта. Например, в `$.people[1].surname``surname` является дочерним элементом `people`.
  
## <a name="examples"></a>Примеры  
 В примерах этого раздела используется следующий текст JSON.  
  
```json  
{
    "people": [{
        "name": "John",
        "surname": "Doe"
    }, {
        "name": "Jane",
        "surname": null,
        "active": true
    }]
}
```  
  
 В таблице ниже приведены некоторые примеры выражений пути.  
  
|Выражение пути|Значение|  
|---------------------|-----------|  
|$.people[0].name|Джон|  
|$.people[1]|{ "name": "Jane", "surname": null, "active": true }|  
|$.people[1].surname|null|  
|$|{ "people": [ { "name": "John",  "surname": "Doe" },<br />   { "name": "Jane",  "surname": null, "active": true } ] }|  
  
## <a name="how-built-in-functions-handle-duplicate-paths"></a>Как встроенные функции обрабатывают повторяющиеся пути  
 Если текст JSON содержит повторяющиеся свойства (например, два ключа с одним и тем же именем на одном уровне), функции **JSON_VALUE** и **JSON_QUERY** возвращают первое значение, которое соответствует пути. Чтобы проанализировать объект JSON, который содержит повторяющиеся ключи, и получить все значения, используйте функцию **OPENJSON**, как показано в следующем примере.  
  
```sql  
DECLARE @json NVARCHAR(MAX);
SET @json=N'{"person":{"info":{"name":"John", "name":"Jack"}}}';

SELECT value
  FROM OPENJSON(@json,'$.person.info');
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Дополнительные сведения о JSON в SQL Server и базе данных SQL Azure  
  
### <a name="microsoft-videos"></a>Видео Майкрософт

Наглядные инструкции по встроенной поддержке JSON в SQL Server и базе данных SQL Azure см. в следующих видео.

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support) (SQL Server 2016 и поддержка JSON)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database) (Использование JSON в SQL Server 2016 и базе данных SQL Azure)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) (JSON как мост между NoSQL и реляционными решениями)
  
## <a name="see-also"></a>См. также:  
 [OPENJSON (Transact-SQL)](../../t-sql/functions/openjson-transact-sql.md)   
 [JSON_VALUE (Transact-SQL)](../../t-sql/functions/json-value-transact-sql.md)   
 [JSON_QUERY (Transact-SQL)](../../t-sql/functions/json-query-transact-sql.md)  
  
  

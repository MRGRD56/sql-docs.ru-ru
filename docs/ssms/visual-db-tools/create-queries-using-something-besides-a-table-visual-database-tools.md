---
description: Создание запросов, использующих не только таблицу (визуальные инструменты для баз данных)
title: Создание запросов, использующих не только таблицу
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- user-defined functions [SQL Server], queries
- queries [SQL Server], creating
ms.assetid: 8e4a1f0a-8a42-4733-be8d-e21d6dbddb33
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 95a432ca8912b4d306538906d11bb2a3d5464e5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350448"
---
# <a name="create-queries-using-something-besides-a-table-visual-database-tools"></a>Создание запросов, использующих не только таблицу (визуальные инструменты для баз данных)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
При написании запроса разработчик указывает, какие требуются столбцы, как отбираются строки и откуда обработчик запросов получает исходные данные. Обычно исходные данные поступают из таблицы или нескольких таблиц, участвующих в соединении. Однако исходные данные могут поступать не только из таблиц. Источниками данных могут служить представления, запросы, синонимы или определяемые пользователем функции, которые возвращают таблицу.  
  
## <a name="using-a-view-in-place-of-a-table"></a>Использование представления вместо таблицы  
Допускается выбор строк из представления. Например, предположим, что база данных содержит представление с именем «ExpensiveBooks», строки в котором описывают книги с ценой, превышающей 19,99. Определение представления может выглядеть следующим образом:  
  
```  
SELECT *  
FROM titles  
WHERE price > 19.99  
```  
  
Можно отобрать дорогие книги по психологии, выбирая их из представления ExpensiveBooks. Конечный код SQL может выглядеть следующим образом:  
  
```  
SELECT *  
FROM ExpensiveBooks  
WHERE type = 'psychology'  
```  
  
Допускается включение представления в операцию JOIN. Например, можно получить данные по продажам дорогих книг, соединив таблицу продаж с представлением ExpensiveBooks. Конечный код SQL может выглядеть следующим образом:  
  
```  
SELECT *  
FROM sales   
         INNER JOIN   
         ExpensiveBooks   
         ON sales.title_id   
         =  ExpensiveBooks.title_id  
```  
  
Дополнительные сведения о добавлении представления в запрос см. в разделе [Добавление таблиц в запросы (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md).  
  
## <a name="using-a-query-in-place-of-a-table"></a>Использование запроса вместо таблицы  
Допускается выбор строк из запроса. Предположим, что имеется запрос, возвращающий названия и идентификаторы для книг, написанных соавторами, т. е. имеющих более одного автора. Код SQL может выглядеть следующим образом:  
  
```  
SELECT   
     titles.title_id, title, type  
FROM   
     titleauthor   
         INNER JOIN  
         titles   
         ON titleauthor.title_id   
         =  titles.title_id   
GROUP BY   
     titles.title_id, title, type  
HAVING COUNT(*) > 1  
```  
  
После этого можно написать другой запрос, использующий этот результат. Например, запрос, возвращающий книги по психологии, написанные соавторами, будет использовать существующий запрос как источник данных. Конечный код SQL может выглядеть следующим образом:  
  
```  
SELECT   
    title  
FROM   
    (  
    SELECT   
        titles.title_id,   
        title,   
        type  
    FROM   
        titleauthor   
            INNER JOIN  
            titles   
            ON titleauthor.title_id   
            =  titles.title_id   
    GROUP BY   
        titles.title_id,   
        title,   
        type  
    HAVING COUNT(*) > 1  
    )   
    co_authored_books  
WHERE     type = 'psychology'  
```  
  
Полужирным шрифтом выделен существующий запрос, используемый как источник данных нового запроса. Следует отметить, что в новом запросе для существующего запроса используется псевдоним (co_authored_books). Дополнительные сведения о псевдонимах см. в разделах [Создание псевдонимов таблицы (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/create-table-aliases-visual-database-tools.md) и [Создание псевдонимов столбцов (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/create-column-aliases-visual-database-tools.md).  
  
Допускается включение запроса в операцию JOIN. Например, можно получить данные по продажам дорогих книг, написанных соавторами, соединив представление ExpensiveBooks с существующим запросом. Конечный код SQL может выглядеть следующим образом:  
  
```  
SELECT   
    ExpensiveBooks.title  
FROM   
    ExpensiveBooks   
        INNER JOIN  
        (  
        SELECT   
            titles.title_id,   
            title,   
            type  
        FROM   
            titleauthor   
                INNER JOIN  
                titles   
                ON titleauthor.title_id   
                =  titles.title_id   
        GROUP BY   
            titles.title_id,   
            title,   
            type  
        HAVING COUNT(*) > 1  
        )  
```  
  
Дополнительные сведения о добавлении запроса в запрос см. в разделе [Добавление таблиц в запросы (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md).  
  
## <a name="using-a-user-defined-function-in-place-of-a-table"></a>Использование определяемых пользователем функций вместо таблицы  
В SQL Server 2000 или более поздних версиях поддерживается создание определяемой пользователем функции, возвращающей таблицу. Такие функции полезны при использовании сложной или процедурной логики.  
  
Предположим, что таблица сотрудников содержит дополнительный столбец employee.manager_emp_id и что существует внешний ключ от столбца manager_emp_id к столбцу employee.emp_id. В каждой строке таблицы сотрудников столбец manager_emp_id указывает начальника конкретного сотрудника. Точнее, указывается код emp_id начальника конкретного сотрудника. Можно создать определяемую пользователем функцию, которая возвращает таблицу, содержащую одну строку для каждого сотрудника, работающего в иерархии подчиненности для руководителя высшего уровня. Функцию можно назвать fn_GetWholeTeam и определить так, чтобы входной переменной был идентификатор руководителя, сведения о подчиненных которого требуется получить.  
  
Затем можно написать запрос, использующий функцию fn_GetWholeTeam как источник данных. Конечный код SQL может выглядеть следующим образом:  
  
```  
SELECT *   
FROM   
     fn_GetWholeTeam ('VPA30890F')  
```  
  
«VPA30890F» представляет код emp_id руководителя, сведения о подчиненных которого требуется получить. Дополнительные сведения о добавлении пользовательской функции в запрос см. в разделе [Добавление таблиц в запросы (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/add-tables-to-queries-visual-database-tools.md). Подробное описание определяемых пользователем функций см. в разделе [Определяемые пользователем функции](../../relational-databases/user-defined-functions/user-defined-functions.md).  

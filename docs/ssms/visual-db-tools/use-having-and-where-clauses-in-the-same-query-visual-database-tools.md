---
description: Использование предложения HAVING и WHERE в одном запросе (визуальные инструменты для баз данных)
title: Использование предложений HAVING и WHERE в одном запросе
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- search criteria [SQL Server], excluding rows
- search criteria [SQL Server], WHERE clause
- search conditions [SQL Server], HAVING clause
- row excluded in search [SQL Server]
- search criteria [SQL Server], HAVING clause
- HAVING clause, search criteria
- search conditions [SQL Server], WHERE clause
- WHERE clause, search criteria
- excluding rows
ms.assetid: 1e07cf56-b4b7-4c49-8ddd-c276812a7148
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: cdaf2096589b5a1b16753c79c7eac6cda08fff68
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350440"
---
# <a name="use-having-and-where-clauses-in-the-same-query-visual-database-tools"></a>Использование предложения HAVING и WHERE в одном запросе (визуальные инструменты для баз данных)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
В некоторых экземплярах может понадобиться исключить отдельные строки из групп (с использованием предложения WHERE) до того, как применять условие к группе как к целому (с использованием предложения HAVING).  
  
Предложение HAVING подобно предложению WHERE, но применимо только к целым группам (то есть к строкам в результирующем наборе, представляющим собой группы), тогда как предложение WHERE применимо к отдельным строкам. В запросе могут содержаться оба предложения: WHERE и HAVING. В этом случае:  
  
-   Предложение WHERE применяется сначала к отдельным строкам таблиц или возвращающих табличное значение объектов на панели диаграмм. Группируются только строки, которые удовлетворяют условиям в предложении WHERE.  
  
-   Затем предложение HAVING применяется к строкам в результирующем наборе. Только строки, которые удовлетворяют условиям HAVING, появляются в результирующем запросе. Можно применить предложение HAVING только к тем столбцам, которые появляются в предложении GROUP BY или агрегатной функции.  
  
Например, соединяются таблицы `titles` и `publishers` для создания запроса, в котором показана средняя цена книги для группы издателей. Требуется средняя цена книги только определенной группы издателей — например, только издателей в штате Калифорния. При этом нужно показать только те средние цены, которые превышают $10,00.  
  
Первое условие можно задать с помощью предложения WHERE, которое устраняет всех издателей не из Калифорнии перед тем, как начать вычисление средней цены. Второе условие требует предложения HAVING, так как условие основано на результатах группирования и сводных данных. Конечная инструкция SQL может выглядеть следующим образом:  
  
```  
SELECT titles.pub_id, AVG(titles.price)  
FROM titles INNER JOIN publishers  
   ON titles.pub_id = publishers.pub_id  
WHERE publishers.state = 'CA'  
GROUP BY titles.pub_id  
HAVING AVG(price) > 10  
```  
  
Можно создать оба предложения HAVING и WHERE на панели критериев. По умолчанию любое заданное условие поиска для столбца становится частью предложения HAVING. Однако можно изменить условие, сделав его предложением WHERE.  
  
Можно создать предложение WHERE и HAVING для одного и того же столбца. Для этого необходимо дважды добавить столбец на панели критериев, затем указать один экземпляр как часть предложения HAVING и другой экземпляр как часть предложения WHERE.  
  
### <a name="to-specify-a-where-condition-in-an-aggregate-query"></a>Задание условия WHERE в статистическом запросе  
  
1.  Укажите группы для запроса. Дополнительные сведения см. в статье о [группировании строк в результатах запроса](../../ssms/visual-db-tools/group-rows-in-query-results-visual-database-tools.md).  
  
2.  Если столбец, на котором основывается условие WHERE, находится не на панели критериев, то добавьте его на панель критериев.  
  
3.  Очистите столбец **Вывод** , если столбец данных не является частью предложения GROUP BY или не входит в агрегатную функцию.  
  
4.  В столбце **Фильтр** укажите условие WHERE. Конструктор запросов и представлений добавляет условие в предложение HAVING инструкции SQL.  
  
    > [!NOTE]  
    > В качестве примера данной процедуры показан запрос, соединяющий две таблицы `titles` и `publishers`.  
  
    В этой точке в запросе инструкции SQL содержится предложение HAVING:  
  
    ```  
    SELECT titles.pub_id, AVG(titles.price)  
    FROM titles INNER JOIN publishers   
       ON titles.pub_id = publishers.pub_id  
    GROUP BY titles.pub_id  
    HAVING publishers.state = 'CA'  
    ```  
  
5.  В столбце **Группировать** выберите **Where** из списка параметров группировки и сводки. Конструктор запросов и представлений удаляет условие из предложения HAVING инструкции SQL и добавляет его в предложение WHERE.  
  
    Инструкция SQL изменяется, теперь она содержит предложение WHERE:  
  
    ```  
    SELECT titles.pub_id, AVG(titles.price)  
    FROM titles INNER JOIN publishers   
       ON titles.pub_id = publishers.pub_id  
    WHERE publishers.state = 'CA'  
    GROUP BY titles.pub_id  
    ```  
  
## <a name="see-also"></a>См. также:  
[Сортировка и группировка результатов запроса](../../ssms/visual-db-tools/sort-and-group-query-results-visual-database-tools.md)  
[Резюмирование результатов запросов](../../ssms/visual-db-tools/summarize-query-results-visual-database-tools.md)  
  

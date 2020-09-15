---
description: Указание условий поиска (визуальные инструменты для баз данных)
title: Указание условий поиска
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- choosing search criteria
- search conditions [SQL Server], specifying
- search criteria [SQL Server], specifying conditions
ms.assetid: 18e2c759-68ec-4efe-b208-2f73418cd9bd
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: d5f300b04c3085628ccf624b7f03fca611bc0d63
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88369020"
---
# <a name="specify-search-conditions-visual-database-tools"></a>Указание условий поиска (визуальные инструменты для баз данных)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Можно указать строки данных, которые появятся в результате запроса, задав условия поиска. Например, при выполнении запроса к таблице `employee` можно указать, что хотите найти только работников, которые работают в определенной области.  
  
Условия поиска указываются с помощью выражений. Наиболее часто используемое выражение состоит из оператора и искомого значения. Например, чтобы найти работников в определенной области продаж, можно указать следующий критерий поиска для столбца `region` :  
  
```  
='UK'  
```  
  
> [!NOTE]  
> При работе с несколькими таблицами конструктор запросов и представлений проверяет каждое условие поиска, чтобы определить, будет ли соединение результатом сравнения. Если да, то конструктор запросов и представлений автоматически преобразует условие поиска в соединение. Дополнительные сведения см. в статье [Автоматическое соединение таблиц (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/join-tables-automatically-visual-database-tools.md).  
  
### <a name="to-specify-search-conditions"></a>Указание условий поиска  
  
1.  Если это еще не сделано, нужно добавить на панели критериев столбцы или выражения, которые необходимо использовать в условиях поиска.  
  
    Если при создании запроса SELECT нежелательно, чтобы столбцы поиска или выражения появлялись в выходных данных запроса, очистите столбец **Выход** для каждого столбца или выражения, чтобы убрать их из числа выходных столбцов.  
  
2.  Найдите строку, содержащую столбец данных или выражение поиска, и в столбце **Фильтр** введите условие поиска.  
  
    > [!NOTE]  
    > Если не ввести ни одного оператора, конструктор запросов и представлений автоматически вставит оператор равенства «=».  
  
Конструктор запросов и представлений обновляет инструкцию SQL на [панели SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md) , добавляя или изменяя предложение WHERE.  
  
## <a name="see-also"></a>См. также:  
[Правила ввода значений для поиска (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/rules-for-entering-search-values-visual-database-tools.md)  
[Определение критериев поиска (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  

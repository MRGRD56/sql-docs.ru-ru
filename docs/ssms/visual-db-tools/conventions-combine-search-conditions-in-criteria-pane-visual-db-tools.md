---
description: Обозначения для условий комбинированного поиска на панели критериев (визуальные инструменты для баз данных)
title: Обозначения для объединения условий поиска в области условий
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- search conditions [SQL Server], combining
- precedence [SQL Server], Criteria pane
- search criteria [SQL Server], combining conditions
- multiple OR clauses
- combining search conditions
- OR operator
- AND, Criteria pane
- multiple AND clauses
ms.assetid: d4859be5-ff5b-48b2-a101-ad40c6dbcc68
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: fe9c75b0b173bf2f72437233d2c06305b5041132
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355205"
---
# <a name="conventions-for-combining-search-conditions-in-the-criteria-pane-visual-database-tools"></a>Обозначения для условий комбинированного поиска на панели критериев (визуальные инструменты для баз данных)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Предусмотрена возможность создания запросов, включающих любое количество условий поиска, соединенных любым количеством операторов AND и OR. Запрос с сочетанием предложений AND и OR может стать сложным, поэтому важно знать, как такой запрос интерпретируется при выполнении и представляется на [панели критериев](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md) и [панели SQL](../../ssms/visual-db-tools/sql-pane-visual-database-tools.md).  
  
> [!NOTE]  
> Дополнительные сведения об условиях поиска, которые содержат только один оператор AND или OR, см. в разделах [Указание нескольких условий поиска для одного столбца (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-one-column-visual-database-tools.md) и [Указание нескольких условий поиска для нескольких столбцов (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/specify-multiple-search-conditions-for-multiple-columns-visual-database-tools.md).  
  
Ниже содержатся сведения о следующем:  
  
-   Приоритет AND и OR в запросах, содержащих их оба.  
  
-   Как условия в предложениях AND и OR логически связаны друг с другом.  
  
-   Как конструктор запросов и представлений отображает на панели критериев запросы, содержащие AND и OR.  
  
Для простоты понимания нижеследующего представьте, что работаете с таблицей `employee` , содержащей столбцы `hire_date`, `job_lvl`и `status`. В примерах предполагается, что пользователи должны знать такие сведения, как дата найма сотрудника, какой вид работы он выполняет (его должность), а также его состояние (например, уволенный).  
  
## <a name="precedence-of-and-and-or"></a>Приоритет AND и OR  
При выполнении запроса сначала проверяются предложения, соединенные AND, а затем — соединенные OR.  
  
> [!NOTE]  
> Оператор NOT имеет приоритет над AND и над OR.  
  
Например, чтобы найти либо сотрудников, которые проработали в компании более пяти лет на младших должностях, либо сотрудников среднего звена безотносительно к дате найма, можно сконструировать следующее предложение WHERE:  
  
```  
WHERE   
   hire_date < '01/01/95' AND   
   job_lvl = 100 OR  
   job_lvl = 200  
```  
  
Для переопределения приоритета AND над OR, используемого по умолчанию, можно поместить определенные условия на панели SQL в круглые скобки. Условия в круглых скобках всегда вычисляются первыми. Например, чтобы найти всех сотрудников, которые проработали в компании на младших или средних должностях более пяти лет, можно сконструировать следующее предложение WHERE:  
  
```  
WHERE   
   hire_date < '01/01/95' AND   
   (job_lvl = 100 OR job_lvl = 200)  
```  
  
> [!TIP]  
> Рекомендуется для ясности всегда задавать круглые скобки при сочетании предложений AND и OR вместо использования приоритета по умолчанию.  
  
## <a name="how-and-works-with-multiple-or-clauses"></a>Работа AND с несколькими предложениями OR  
Понимание того, как работают предложения AND и OR при объединении, поможет при составлении и изучении сложных запросов в конструкторе запросов и представлений.  
  
При соединении нескольких условий с помощью AND первый набор условий, соединенных AND, применяется ко всем условиям из второго набора. Другими словами, условие, соединенное AND с другим условием, распространяется на все условия второго набора. Например, следующее схематичное представление иллюстрирует условие AND, соединенное с набором условий OR:  
  
```  
A AND (B OR C)  
```  
  
Вышеприведенное представление логически идентично следующему схематичному представлению, иллюстрирующему распространение условия AND на второй набор условий:  
  
```  
(A AND B) OR (A AND C)  
```  
  
Этот принцип распространения влияет на использование конструктора запросов и представлений. Например, пользователь ищет всех сотрудников, работающих в компании более пяти лет на младших или средних должностях. Следующее предложение WHERE добавляется в инструкцию на панели SQL:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
   (job_lvl = 100 OR job_lvl = 200)  
```  
  
Предложение, соединенное AND, применяется к обоим предложениям, соединенным OR. Явно это можно выразить посредством повторения условия AND по одному разу для каждого условия в предложении OR. Следующая инструкция более точная (и длинная), чем предыдущая, но логически идентична ей:  
  
```  
WHERE    (hire_date < '01/01/95' ) AND  
  (job_lvl = 100) OR   
  (hire_date < '01/01/95' ) AND   
  (job_lvl = 200)  
```  
  
Принцип распространения предложений AND на соединенные предложения OR применяется вне зависимости от количества отдельных учитываемых условий. Например, пусть требуется найти сотрудников высокого или среднего звена, проработавших в компании более пяти лет либо уволенных. Предложение WHERE может выглядеть так:  
  
```  
WHERE   
   (job_lvl = 200 OR job_lvl = 300) AND  
   (hire_date < '01/01/95' ) OR (status = 'R')  
```  
  
После распространения условий, соединенных AND, предложение WHERE выглядит так:  
  
```  
WHERE   
   (job_lvl = 200 AND hire_date < '01/01/95' ) OR  
   (job_lvl = 200 AND status = 'R') OR  
   (job_lvl = 300 AND hire_date < '01/01/95' ) OR  
   (job_lvl = 300 AND status = 'R')  
```  
  
## <a name="how-multiple-and-and-or-clauses-are-represented-in-the-criteria-pane"></a>Как несколько предложений AND и OR представляются на панели критериев  
Конструктор запросов и представлений выводит условия поиска на [панель критериев](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md). Однако в некоторых случаях при использовании нескольких предложений, соединенных AND и OR, представление на панели критериев может отличаться от ожидаемого. Кроме того, при редактировании запроса на панели критериев или [панели диаграммы](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md)можно обнаружить, что инструкция SQL изменилась по сравнению с ранее заданной.  
  
Эти правила определяют то, как предложения AND и OR отображаются на панели критериев:  
  
-   Все условия, соединенные AND, выводятся в столбце сетки **Фильтр** или в одном столбце **Или...** .  
  
-   Все условия, соединенные OR, выводятся в отдельных столбцах **Или...** .  
  
-   Если логический результат объединения предложений AND и OR заключается в том, что AND распространяется на несколько предложений OR, панель критериев представляет это в явной форме, повторяя предложение AND столько раз, сколько требуется.  
  
Например, на панели SQL можно создать нижеприведенное условие поиска, в котором два предложения, соединенные AND, имеют приоритет над третьим, соединенным OR:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  (job_lvl = 100) OR   
  (status = 'R')  
```  
  
Конструктор запросов и представлений отображает данное предложение WHERE на панели критериев следующим образом:  
  
![Очередность предложений OR в области условий](../../ssms/visual-db-tools/media/vs_criteriapane1.gif "Очередность предложений OR в области условий")  
  
Однако если связанные предложения OR имеют приоритет над предложением AND, последнее повторяется для каждого предложения OR. В результате предложение AND распространяется на каждое предложение OR. Например, на панели SQL было создано предложение WHERE, подобное следующему:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  ( (job_lvl = 100) OR   
  (status = 'R') )  
```  
  
Конструктор запросов и представлений отображает данное предложение WHERE на панели критериев следующим образом:  
  
![Несколько предложений AND и OR в области условий](../../ssms/visual-db-tools/media/vs_criteriapane2.gif "Несколько предложений AND и OR в области условий")  
  
Если соединенные предложения OR включают только один столбец данных, конструктор запросов и представлений может поместить все предложение OR в одну ячейку сетки, устраняя необходимость повторения предложения AND. Например, на панели SQL было создано предложение WHERE, подобное следующему:  
  
```  
WHERE (hire_date < '01/01/95' ) AND   
  ((status = 'R') OR (status = 'A'))  
```  
  
Конструктор запросов и представлений отображает данное предложение WHERE на панели критериев следующим образом:  
  
![Связанные предложения OR, определенные в области условий](../../ssms/visual-db-tools/media/vs_criteriapane3.gif "Связанные предложения OR, определенные в области условий")  
  
При внесении изменений в запрос (например при редактировании одного из значений на панели критериев), конструктор запросов и представлений повторно создает инструкцию SQL на панели SQL. Воссозданная инструкция SQL аналогична представлению панели критериев, а не исходной инструкции. Например, если панель критериев содержит распределенные предложения AND, результирующая инструкция на панели SQL создается повторно с явно распределенными предложениями AND. Дополнительные сведения см. в разделе «Работа AND с несколькими предложениями OR» ранее в этом подразделе.  
  
## <a name="see-also"></a>См. также:  
[Определение критериев поиска (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/specify-search-criteria-visual-database-tools.md)  
  

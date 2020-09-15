---
description: Резюмирование результатов запросов (визуальные инструменты для баз данных)
title: Резюмирование результатов запросов
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- summarizing query results
- queries [SQL Server], results
- aggregate queries [SQL Server]
ms.assetid: c9e15350-ed57-4d95-814d-815fbebfd86b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 5bd8a8f85fd0d26f206f8c6c3b352eb883d3aa61
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88312280"
---
# <a name="summarize-query-results-visual-database-tools"></a>Резюмирование результатов запросов (визуальные инструменты для баз данных)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
К созданию статистических запросов следует применять определенные логические принципы. Например, невозможно вывести содержимое отдельных строк в сводном запросе. Конструктор запросов и представлений помогает соблюсти эти принципы — такое поведение заложено в [панель диаграммы](../../ssms/visual-db-tools/diagram-pane-visual-database-tools.md) и [панель критериев](../../ssms/visual-db-tools/criteria-pane-visual-database-tools.md) .  
  
Поняв принципы статистических запросов и поведение конструктора запросов и представлений, можно создавать логически безошибочные статистические запросы. Важнейший принцип гласит, что статистические запросы могут выдавать только сводные данные. Таким образом, большинство остальных принципов описывают способы создания в статистическом запросе ссылок на отдельные столбцы данных.  
  
## <a name="in-this-section"></a>в этом разделе  
[Работа со столбцами в статистических запросах (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/work-with-columns-in-aggregate-queries-visual-database-tools.md)  
Описывает концепции группирования и суммирования столбцов при помощи предложений GROUP BY, WHERE и HAVING.  
  
[Подсчет строк в таблице (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/count-rows-in-a-table-visual-database-tools.md)  
Пошаговые инструкции для подсчета количества строк в таблице или количество строк в таблице, отвечающих набору критериев.  
  
[Получение суммарных или статистических значений для всех строк в таблице (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/summarize-or-aggregate-values-for-all-rows-in-a-table-visual-database-tools.md)  
Пошаговые инструкции относительно суммирования всех строк, а не набора сгруппированных строк.  
  
[Суммирование значения или выполнение статистической обработки с помощью пользовательских выражений (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/summarize-or-aggregate-values-using-custom-expressions-visual-database-tools.md)  
Пошаговые инструкции по использованию выражений для суммирования и выполнения статистической обработки вместо стандартных предложений.  
  
## <a name="related-sections"></a>Связанные разделы  
[Разделы по конструированию запросов и представлений (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
Предоставляет ссылки на разделы, объясняющие как использовать конструктор запросов и представлений.  
  

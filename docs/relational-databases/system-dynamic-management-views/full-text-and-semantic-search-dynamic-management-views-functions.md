---
description: Динамические административные представления Full-Text и семантического поиска — функции
title: Динамические административные представления Full-Text и семантического поиска — функции | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- dynamic management objects [SQL Server], full-text search
- full-text search [SQL Server], dynamic management views
ms.assetid: 199dbd5a-29f6-4ef0-8e65-86e32c0aaa3a
author: pmasl
ms.author: pelopes
ms.openlocfilehash: cd9e072fa2c4aafceaad2d4aad57eb9dcc4bb38b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201358"
---
# <a name="full-text-and-semantic-search-dynamic-management-views---functions"></a>Динамические административные представления Full-Text и семантического поиска — функции
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  В этом разделе описываются следующие динамические административные представления и функции, которые связаны с полнотекстовым и семантическим поиском.  
  
## <a name="full-text-search-dynamic-management-views-and-functions"></a>Динамические административные представления и функции компонента Full-Text Search  
 [sys.dm_fts_active_catalogs (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-active-catalogs-transact-sql.md)  
 Возвращает сведения о полнотекстовых каталогах, для которых на сервере выполняются те или иные операции по заполнению.  
  
 [sys.dm_fts_fdhosts](../../relational-databases/system-dynamic-management-views/sys-dm-fts-fdhosts-transact-sql.md)  
 Возвращает сведения о текущем действии узла управляющей программы фильтрации или узлов на экземпляре сервера.  
  
 [sys.dm_fts_index_keywords](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
 Возвращает сведения о содержимом полнотекстового индекса для указанной таблицы.  
  
 [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
 Возвращает сведения о содержимом полнотекстового индекса на уровне документа для указанной таблицы. Данное ключевое слово может встречаться в нескольких документах.  
  
 [sys.dm_fts_index_keywords_by_property](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)  
 Возвращает данные, связанные со свойством, в полнотекстовом индексе данной таблицы. Сюда включаются все данные, принадлежащие любому свойству, зарегистрированному списком свойств поиска, связанным с этим полнотекстовым индексом.  
  
 sys.dm_fts_index_keywords_position_by_document  
 Возвращает расположение ключевых слов в документе.  
  
 [sys.dm_fts_index_population](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)  
 Возвращает сведения о выполняющихся в настоящий момент процессах заполнения полнотекстовых индексов.  
  
 [sys.dm_fts_memory_buffers](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)  
 Возвращает данные о буферах памяти, принадлежащих конкретному пулу памяти, используемому в качестве части полнотекстового сканирования или диапазона полнотекстового сканирования.  
  
 [sys.dm_fts_memory_pools](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)  
 Возвращает сведения о пулах общей памяти, доступных компоненту полнотекстового сборщика данных для полнотекстового сканирования или диапазона полнотекстового сканирования.  
  
 [sys.dm_fts_outstanding_batches](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md)  
 Возвращает данные о каждом пакете полнотекстового индексирования.  
  
 [sys.dm_fts_parser](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md)  
 Возвращает итоговый результат разметки после применения сочетания данного средства разбиения по словам, тезауруса и списка стоп-слов к строковым входным данным запроса. Входные данные эквивалентны выходным, если указанная строка запроса была передана средству полнотекстового поиска.  
  
 [sys.dm_fts_population_ranges](../../relational-databases/system-dynamic-management-views/sys-dm-fts-population-ranges-transact-sql.md)  
 Возвращает сведения о конкретных диапазонах, соответствующих выполняемому в настоящий момент заполнению полнотекстового индекса.  
  
## <a name="semantic-search-dynamic-management-views-and-functions"></a>Динамические административные представления и функции семантического поиска  
 [sys.dm_fts_semantic_similarity_population (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-semantic-similarity-population-transact-sql.md)  
 Возвращает по одной строке сведений о состоянии заполнения индекса подобия документов для каждого индекса подобия в каждой таблице, имеющей связанный семантический индекс.  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Системные представления &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)  
  

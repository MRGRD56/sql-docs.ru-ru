---
description: Полнотекстовый поиск и хранимые процедуры семантического поиска (Transact-SQL)
title: Хранимые процедуры поиска Full-Text и семантического поиска (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- full-text indexes [SQL Server], stored procedures
- full-text search [SQL Server], stored procedures
- full-text catalogs [SQL Server], stored procedures
- system stored procedures [SQL Server], full-text search
ms.assetid: 0d185a16-2b16-4958-884f-efe675e2e551
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39e13966219ed8f08c5a5095c505a661e6923a8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165202"
---
# <a name="full-text-search-and-semantic-search-stored-procedures-transact-sql"></a>Полнотекстовый поиск и хранимые процедуры семантического поиска (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает следующие системные хранимые процедуры, используемые для реализации и запроса полнотекстовых и семантических индексов.  
  
## <a name="full-text-search-stored-procedures"></a>Хранимые процедуры для работы с полнотекстовым поиском  
 [sp_fulltext_catalog](../../relational-databases/system-stored-procedures/sp-fulltext-catalog-transact-sql.md)  
 Создает и удаляет полнотекстовый каталог, запускает и останавливает действие индексирования для каталога. Для каждой базы данных можно создать несколько полнотекстовых каталогов.  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Вместо этого используйте инструкцию [CREATE FULLTEXT Catalog](../../t-sql/statements/create-fulltext-catalog-transact-sql.md), [ALTER FULLTEXT Catalog](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)и [DROP FULLTEXT Catalog](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md) .  
  
 [sp_fulltext_column](../../relational-databases/system-stored-procedures/sp-fulltext-column-transact-sql.md)  
 Определяет, должен ли конкретный столбец таблицы использоваться в полнотекстовом индексировании.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Используйте вместо него [инструкцию ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) .  
  
 [sp_fulltext_database, хранимая процедура](../../relational-databases/system-stored-procedures/sp-fulltext-database-transact-sql.md)  
 Не влияет на полнотекстовые каталоги в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версиях и поддерживается только для обеспечения обратной совместимости.  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 [sp_fulltext_keymappings](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)  
 Возвращает сопоставления между идентификаторами документов (**DocId** s) и значениями полнотекстового ключа.  
  
 [sp_fulltext_load_thesaurus_file, хранимая процедура](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md)  
 Выполняет синтаксический анализ и загрузку данных из обновленного файла тезауруса, который соответствует коду языка, и запускает повторную компиляцию полнотекстовых запросов, использующих этот тезаурус.  
  
 [sp_fulltext_pendingchanges](../../relational-databases/system-stored-procedures/sp-fulltext-pendingchanges-transact-sql.md)  
 Возвращает необработанные изменения, например ожидающие выполнения операции вставки, обновления и удаления, для указанной таблицы, в которой отслеживаются изменения.  
  
 [sp_fulltext_service, хранимая процедура](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)  
 Изменяет серверные свойства полнотекстового поиска для SQL Server.  
  
 [sp_fulltext_table](../../relational-databases/system-stored-procedures/sp-fulltext-table-transact-sql.md)  
 Отмечает таблицу для полнотекстового индексирования или снимает эту отметку.  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Вместо этого используйте [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md), [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md)и [DROP FULLTEXT INDEX](../../t-sql/statements/drop-fulltext-index-transact-sql.md) .  
  
 [sp_help_fulltext_catalog_components](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalog-components-transact-sql.md)  
 Возвращает список всех компонентов (фильтров, разделителей слов и обработчиков протоколов), используемых для всех полнотекстовых каталогов в текущей базе данных.  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 [sp_help_fulltext_catalogs](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)  
 Возвращает идентификатор, имя, корневой каталог, состояние и число таблиц с полнотекстовым индексом для заданного полнотекстового каталога.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) представление каталога.  
  
 [sp_help_fulltext_catalogs_cursor](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-cursor-transact-sql.md)  
 Использует курсор для возвращения идентификатора, имени, корневого каталога, состояния и числа полнотекстовых индексированных таблиц для заданного полнотекстового каталога.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) представление каталога.  
  
 [sp_help_fulltext_columns](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-transact-sql.md)  
 Возвращает столбцы, предназначенные для полнотекстового индексирования.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) представление каталога.  
  
 [sp_help_fulltext_columns_cursor](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-cursor-transact-sql.md)  
 Использует курсор для возврата столбцов, назначенных для полнотекстового индексирования.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) представление каталога.  
  
 [sp_help_fulltext_system_components](../../relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md)  
 Возвращает сведения о зарегистрированных средствах разбиения по словам, фильтрах и обработчиках протоколов, а также список идентификаторов баз данных и полнотекстовых каталогов, которые использовали указанный компонент.  
  
 [sp_help_fulltext_tables](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)  
 Возвращает список таблиц, зарегистрированных для полнотекстового индексирования.  
  
 [sp_help_fulltext_tables_cursor](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)  
 Возвращает список таблиц, зарегистрированных для полнотекстового индексирования.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md) представление каталога.  
  
## <a name="semantic-search-stored-procedures"></a>Хранимые процедуры семантического поиска  
 [sp_fulltext_semantic_register_language_statistics_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-semantic-register-language-statistics-db-transact-sql.md)  
 Регистрирует предварительно заполненную базу данных семантической статистики языка в текущем экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [sp_fulltext_semantic_unregister_language_statistics_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-semantic-unregister-language-statistics-db-transact-sql.md)  
 Отменяет регистрацию существующей базы данных со статистикой семантики языка на текущем экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и удаляет все связанные метаданные.  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога полнотекстового поиска и семантического поиска &#40;языке Transact-SQL&#41;](../../relational-databases/system-catalog-views/full-text-search-and-semantic-search-catalog-views-transact-sql.md)   
 [Динамические административные представления и функции полнотекстового поиска и семантического поиска &#40;языке Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Полнотекстовый поиск](../../relational-databases/search/full-text-search.md)  
  
  

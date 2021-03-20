---
description: sp_fulltext_table (Transact-SQL)
title: sp_fulltext_table (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_table_TSQL
- sp_fulltext_table
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_table
ms.assetid: a765f311-07fc-4af3-b74c-e9a027fbecce
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7184d4d9fae78821c0558ae193e7a830f6f28f28
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749964"
---
# <a name="sp_fulltext_table-transact-sql"></a>sp_fulltext_table (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  Отмечает таблицу для полнотекстового индексирования или снимает эту отметку.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md), [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md)и [DROP FULLTEXT INDEX](../../t-sql/statements/drop-fulltext-index-transact-sql.md) .  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_fulltext_table   
   [ @tabname= ] 'qualified_table_name'           
   , [ @action= ] 'action'   
   [   
   , [ @ftcat= ] 'fulltext_catalog_name'           
   , [ @keyname= ] 'unique_index_name'   
   ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @tabname = ] 'qualified_table_name'` Имя таблицы из одной или двух частей. Таблица должна существовать в текущей базе данных. *qualified_table_name* имеет тип **nvarchar (517)** и не имеет значения по умолчанию.  
  
`[ @action = ] 'action'` Действие, которое необходимо выполнить. *Action* имеет тип **nvarchar (50)**, не имеет значения по умолчанию и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**Создание**|Создает метаданные для полнотекстового индекса для таблицы, на которую ссылается *qualified_table_name* , и указывает, что данные полнотекстового индекса для этой таблицы должны располагаться в *fulltext_catalog_name*. Это действие также обозначает использование *unique_index_name* в качестве полнотекстового ключевого столбца. Этот уникальный индекс уже должен существовать и быть определенным для одного из столбцов таблицы.<br /><br /> Полнотекстовый поиск по этой таблице не будет выполняться, пока полнотекстовый каталог не будет заполнен.|  
|**Тени**|Удаляет метаданные полнотекстового индекса для *qualified_table_name*. Если при этом производится полнотекстовое индексирование, оно автоматически прекращается перед удалением метаданных. Перед удалением полнотекстового индекса необязательно удалять столбцы.|  
|**Активировать**|Активирует возможность сбора данных в полнотекстовом индексе для *qualified_table_name* после деактивации. Для активации в полнотекстовый индекс должен входить хотя бы один столбец.<br /><br /> Полнотекстовый индекс автоматически становится активным (для заполнения) как только добавляется первый столбец для индексирования. Если из индекса удален последний столбец, индекс становится неактивным. Если включено отслеживание изменений, неактивный индекс начинает новое заполнение.<br /><br /> Обратите внимание, что это не заполняет полнотекстовый индекс, но просто регистрирует таблицу в полнотекстовом каталоге в файловой системе, чтобы строки из *qualified_table_name* можно было извлечь во время следующего заполнения полнотекстового индекса.|  
|**Отключение**|Деактивирует полнотекстовый индекс для *qualified_table_name* , чтобы данные полнотекстового индекса не могли быть собраны для *qualified_table_name*. Метаданные полнотекстового индекса сохраняются, и индексирование может быть активировано снова.<br /><br /> Если включено отслеживание изменений, деактивация активного индекса закрепляет состояние индекса: любое текущее заполнение прекращается, и новые изменения в индекс не попадают.|  
|**start_change_tracking**|Начинает добавочное заполнение полнотекстового индекса. Если у таблицы нет отметки времени, начать полное заполнение полнотекстового индекса. Начать отслеживать изменения таблицы.<br /><br /> Полнотекстовое отслеживание изменений не отслеживает операции WRITETEXT или UPDATETEXT, выполненные над столбцами с полнотекстовым индексом типа **Image**, **Text** или **ntext**.|  
|**stop_change_tracking**|Прекращает отслеживать изменения таблицы.|  
|**update_index**|Внести текущий набор изменений таблицы в полнотекстовый индекс.|  
|**start_background_updateindex**|Начинает внесение изменений таблицы в полнотекстовый индекс по мере их появления.|  
|**stop_background_updateindex**|Прекращает внесение изменений таблицы в полнотекстовый индекс по мере их появления.|  
|**start_full**|Выполняет полное заполнение полнотекстового индекса.|  
|**start_incremental**|Начинает добавочное заполнение полнотекстового индекса.|  
|**Остановить**|Прекращает добавочное заполнение.|  
  
`[ @ftcat = ] 'fulltext_catalog_name'` Допустимое имя существующего полнотекстового каталога для действия **создания** . Для всех других действий этот параметр должен быть равен NULL. Аргумент *fulltext_catalog_name* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @keyname = ] 'unique_index_name'` Является допустимым индексом с одним ключом и уникальным значением, не допускающим значение null, для *qualified_table_name* действия **CREATE** . Для всех других действий этот параметр должен быть равен NULL. Аргумент *unique_index_name* имеет тип **sysname** и значение по умолчанию NULL.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="remarks"></a>Remarks  
 После деактивации полнотекстового индекса для определенной таблицы существующий полнотекстовый индекс остается на месте до следующего полного заполнения. Однако этот индекс не используется, так как [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] блокирует запросы к деактивированным таблицам.  
  
 Если таблица активирована вновь, но индекс не заполняется снова, старый индекс доступен для запросов к остававшимся, но не новым столбцам, для которых включено полнотекстовое индексирование. Данные из удаленных столбцов используются в запросах поиска по всем столбцам полнотекстового индекса.  
  
 После того как таблица будет определена для полнотекстового индексирования, Переключение полнотекстового уникального ключевого столбца из одного типа данных в другой путем изменения типа данных этого столбца или изменения полнотекстового уникального ключа из одного столбца в другой без полного повторного заполнения может привести к сбою во время последующего запроса и возврату сообщения об ошибке: "преобразование в тип *data_type* неудачное значение ключа полнотекстового поиска *key_value*". Чтобы избежать этого, удалите полнотекстовое определение для этой таблицы с помощью действия **drop** **sp_fulltext_table** и переопределите его с помощью **sp_fulltext_table** и **sp_fulltext_column**.  
  
 Полнотекстовый ключевой столбец должен содержать значения размером не более 900 байт. Из соображений производительности рекомендуется делать размер ключевого столбца как можно меньшим.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** , **db_owner** и **db_ddladmin** предопределенных ролей базы данных, или пользователь со ссылочными разрешениями на полнотекстовый каталог могут выполнять **sp_fulltext_table**.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-enabling-a-table-for-full-text-indexing"></a>A. Включение полнотекстового индексирования таблицы  
 В следующем примере создаются метаданные полнотекстового индекса для таблицы `Document` базы данных `AdventureWorks`. `Cat_Desc` — полнотекстовый каталог. `PK_Document_DocumentID` — уникальный индекс по одиночному столбцу таблицы `Document`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'create', 'Cat_Desc', 'PK_Document_DocumentID';  
--Add some columns  
EXEC sp_fulltext_column 'Production.Document','DocumentSummary','add';  
-- Activate the full-text index  
EXEC sp_fulltext_table 'Production.Document','activate';  
GO  
```  
  
### <a name="b-activating-and-propagating-track-changes"></a>Б. Активация отслеживания изменений и их внесение в индекс  
 В следующем примере будет начато отслеживание изменений таблицы и их внесение в полнотекстовый индекс по мере появления.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'Start_change_tracking';  
EXEC sp_fulltext_table 'Production.Document', 'Start_background_updateindex';  
GO  
```  
  
### <a name="c-removing-a-full-text-index"></a>В. Удаление полнотекстового индекса  
 В этом примере удаляются метаданные полнотекстового индекса таблицы `Document` базы данных `AdventureWorks`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'drop';  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [INDEXPROPERTY (Transact-SQL)](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_help_fulltext_tables &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [sp_helpindex &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Хранимые процедуры полнотекстового поиска и семантического поиска &#40;языке Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  

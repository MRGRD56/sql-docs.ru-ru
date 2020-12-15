---
description: sys.dm_fts_index_keywords_by_document (Transact-SQL)
title: sys.dm_fts_index_keywords_by_document (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_fts_index_keywords_by_document_TSQL
- dm_fts_index_keywords_by_document_TSQL
- sys.dm_fts_index_keywords_by_document
- dm_fts_index_keywords_by_document
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], troubleshooting
- sys.dm_fts_index_keywords_by_document dynamic management function
- full-text search [SQL Server], viewing keywords
ms.assetid: 793b978b-c8a1-428c-90c2-a3e49d81b5c9
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 353fcb321549546ff1b44956d5ebc20826e6c0e3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484706"
---
# <a name="sysdm_fts_index_keywords_by_document-transact-sql"></a>sys.dm_fts_index_keywords_by_document (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Возвращает сведения о содержимом полнотекстового индекса на уровне документа, связанного с указанной таблицей.  
  
 Функция sys.dm_fts_index_keywords_by_document является функцией динамического управления.  
  
 **Просмотр сведений более высокого уровня о полнотекстовом индексе**  
  
-   [sys.dm_fts_index_keywords (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
  
 **Просмотр сведений о содержимом уровня свойств, связанным со свойством документа**  
  
-   [sys.dm_fts_index_keywords_by_property (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.dm_fts_index_keywords_by_document  
(   
    DB_ID('database_name'),     OBJECT_ID('table_name')   
)  
```  
  
## <a name="arguments"></a>Аргументы  
 db_id ("*database_name*")  
 Вызов функции [DB_ID ()](../../t-sql/functions/db-id-transact-sql.md) . Эта функция принимает имя базы данных и возвращает идентификатор базы данных, который затем используется функцией sys.dm_fts_index_keywords_by_document для поиска указанной базы данных. Если аргумент *database_name* не указан, возвращается идентификатор текущей базы данных.  
  
 object_id ("*table_name*")  
 Вызов функции [object_id ()](../../t-sql/functions/object-id-transact-sql.md) . Эта функция принимает имя таблицы и возвращает идентификатор таблицы, содержащей полнотекстовый индекс для проверки.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Столбец|Тип данных|Описание|  
|------------|---------------|-----------------|  
|ключевое слово|**nvarchar(4000)**|Шестнадцатеричное представление ключевого слова, которое хранится в полнотекстовом индексе.<br /><br /> Примечание. Оксфф представляет специальный символ, указывающий на конец файла или набора данных.|  
|display_term|**nvarchar(4000)**|Ключевое слово в понятном формате. Этот формат является производным от внутреннего формата хранения полнотекстового индекса.<br /><br /> Примечание. Оксфф представляет специальный символ, указывающий на конец файла или набора данных.|  
|column_id|**int**|Идентификатор столбца, содержащий данное ключевое слово, индексированное полнотекстовым индексом.|  
|document_id|**int**|Идентификатор документа или строки, содержащей текущий термин, индексированный полнотекстовым индексом. Данный идентификатор соответствует значению полнотекстового ключа этого документа или строки.|  
|occurrence_count|**int**|Количество вхождений текущего ключевого слова в документе или строке, которое указывается **document_id**. Если указан параметр "*search_property_name*", occurrence_count отображает только число вхождений текущего ключевого слова в указанном свойстве поиска в документе или строке.|  
  
## <a name="remarks"></a>Комментарии  
 Данные, возвращаемые функцией sys.dm_fts_index_keywords_by_document, позволяют, в частности, выяснить следующее:  
  
-   общее число ключевых слов, содержащихся в полнотекстовом индексе;  
  
-   является ли ключевое слово частью данного документа или строки;  
  
-   сколько раз ключевое слово встречается во всем полнотекстовом индексе, а именно:  
  
     ([Sum](../../t-sql/functions/sum-transact-sql.md)(**occurrence_count**) с **ключевым словом** = *keyword_value* )  
  
-   сколько раз ключевое слово встречается в данном документе или строке;  
  
-   сколько ключевых слов содержит данный документ или строка.  
  
 Кроме того, с помощью данных, предоставляемых функцией sys.dm_fts_index_keywords_by_document, можно получить все ключевые слова, относящиеся к данному документу или строке.  
  
 Если полнотекстовый ключевой столбец, как и рекомендовано, имеет тип данных integer, значение document_id прямо сопоставляется со значением полнотекстового ключа базовой таблицы.  
  
 Напротив, если полнотекстовый ключевой столбец имеет тип данных, отличный от integer, значение document_id не представляет значение полнотекстового ключа базовой таблицы. В этом случае для обнаружения строки в базовой таблице, возвращаемой dm_fts_index_keywords_by_document, необходимо присоединить это представление к результатам, возвращаемым [sp_fulltext_keymappings](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md). Чтобы выполнить соединение, нужно сохранить выход хранимой процедуры во временной таблице. После этого можно соединить столбец document_id, возвращенный функцией dm_fts_index_keywords_by_document, со столбцом DocId, возвращенным хранимой процедурой sp_fulltext_keymappings. Обратите внимание, что столбец **отметок времени** не может принимать значения во время вставки, так как они создаются автоматически [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Поэтому столбец **timestamp** должен быть преобразован в столбцы типа **varbinary (8)** . Следующий пример показывает эти шаги. В этом примере *table_id* — идентификатор таблицы, *database_name* — имя базы данных, а *table_name* — имя таблицы.  
  
```  
USE database_name;  
GO  
CREATE TABLE #MyTempTable   
   (  
      docid INT PRIMARY KEY ,  
      [key] INT NOT NULL  
   );  
DECLARE @db_id int = db_id(N'database_name');  
DECLARE @table_id int = OBJECT_ID(N'table_name');  
INSERT INTO #MyTempTable EXEC sp_fulltext_keymappings @table_id;  
SELECT * FROM sys.dm_fts_index_keywords_by_document   
   ( @db_id, @table_id ) kbd  
   INNER JOIN #MyTempTable tt ON tt.[docid]=kbd.document_id;  
GO  
  
```  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение CREATE FULLTEXT CATALOG, а также разрешение SELECT на столбцы, включенные в полнотекстовый индекс.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-displaying-full-text-index-content-at-the-document-level"></a>A. Отображение содержимого полнотекстового индекса на уровне документа  
 В следующем примере отображается содержимое полнотекстового индекса на уровне документа в таблице `HumanResources.JobCandidate` образца базы данных `AdventureWorks2012`.  
  
> [!NOTE]  
>  Этот индекс можно создать, выполнив пример, приведенный для `HumanResources.JobCandidate` таблицы в [инструкции CREATE FULLTEXT INDEX &#40;TRANSACT-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
```  
SELECT * FROM sys.dm_fts_index_keywords_by_document(db_id('AdventureWorks'),   
object_id('HumanResources.JobCandidate'));  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции полнотекстового поиска и семантического поиска &#40;языке Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Компонент Full-text Search](../../relational-databases/search/full-text-search.md)   
 [sys.dm_fts_index_keywords &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_property (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)   
 [sp_fulltext_keymappings &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)   
 [Повышение производительности полнотекстовых индексов](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)  
  
  

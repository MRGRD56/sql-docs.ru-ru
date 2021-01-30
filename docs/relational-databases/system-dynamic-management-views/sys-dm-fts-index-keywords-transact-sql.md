---
description: sys.dm_fts_index_keywords (Transact-SQL)
title: sys.dm_fts_index_keywords (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_index_keywords
- sys.dm_fts_index_keywords
- sys.dm_fts_index_keywords_TSQL
- dm_fts_index_keywords_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_keywords dynamic management function
- full-text search [SQL Server], viewing keywords
- troubleshooting [SQL Server], full-text search
ms.assetid: fce7b2a1-7e74-4769-86a8-c77c7628decd
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 55bd43c5d4704d145618401230be62e59f3cd3c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211212"
---
# <a name="sysdm_fts_index_keywords-transact-sql"></a>sys.dm_fts_index_keywords (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает сведения о содержимом полнотекстового индекса для указанной таблицы.  
  
 **sys.dm_fts_index_keywords** является функцией динамического управления.  
  
> [!NOTE]  
>  Чтобы просмотреть сведения о полнотекстовом индексе нижнего уровня, используйте функцию динамического управления [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md) на уровне документа.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sys.dm_fts_index_keywords( DB_ID('database_name'), OBJECT_ID('table_name') )  
```  
  
## <a name="arguments"></a>Аргументы  
 db_id ("*database_name*")  
 Вызов функции [DB_ID ()](../../t-sql/functions/db-id-transact-sql.md) . Эта функция принимает имя базы данных и возвращает идентификатор базы данных, который **sys.dm_fts_index_keywords** использует для поиска указанной базы данных. Если аргумент *database_name* не указан, возвращается идентификатор текущей базы данных.  
  
 object_id ("*table_name*")  
 Вызов функции [object_id ()](../../t-sql/functions/object-id-transact-sql.md) . Эта функция принимает имя таблицы и возвращает идентификатор таблицы, содержащей полнотекстовый индекс для проверки.  
  
## <a name="table-returned"></a>Возвращаемая таблица  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**This**|**nvarchar(4000)**|Шестнадцатеричное представление ключевого слова, которое хранится в полнотекстовом индексе.<br /><br /> Примечание. Оксфф представляет специальный символ, указывающий на конец файла или набора данных.|  
|**display_term**|**nvarchar(4000)**|Ключевое слово в понятном формате. Этот формат является производным от шестнадцатеричного формата.<br /><br /> Примечание. **display_term** значение для оксфф — "конец файла".|  
|**column_id**|**int**|Идентификатор столбца, содержащий данное ключевое слово, индексированное полнотекстовым индексом.|  
|**document_count**|**int**|Число документов или строк, содержащих текущий термин.|  
  
## <a name="remarks"></a>Замечания  
 Информация, возвращаемая **sys.dm_fts_index_keywords** , полезна для того, чтобы найти следующее, помимо прочего:  
  
-   является ли ключевое слово частью полнотекстового индекса;  
  
-   сколько документов или строк содержат данное ключевое слово;  
  
-   какое ключевое слово наиболее часто встречается в полнотекстовом индексе:  
  
    -   **document_count** каждого *keyword_value* по сравнению с общим **document_count**, число документов, равное 0xFF.  
  
    -   Как правило, наиболее часто встречающиеся ключевые слова пригодны для объявления в качестве стоп-слов.  
  
> [!NOTE]  
>  **Document_count** , возвращаемые **sys.dm_fts_index_keywords** , могут быть менее точными для конкретного документа, чем количество, возвращенное **sys.dm_fts_index_keywords_by_document** или запросом **Contains** . Согласно проведенной оценке, эта возможная неточность не превышает 1%. Такая неточность может возникать из-за того, что **document_id** может быть считано дважды, когда она будет продолжаться в нескольких строках фрагмента индекса или когда она встречается в одной строке несколько раз. Чтобы получить более точное число для конкретного документа, используйте **sys.dm_fts_index_keywords_by_document** или запрос **Contains** .  
  
## <a name="permissions"></a>Разрешения  
 Необходимо членство в предопределенной роли сервера **sysadmin** .  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-displaying-high-level-full-text-index-content"></a>A. Отображение содержимого полнотекстового индекса высокого уровня  
 В следующем примере отображаются сведения о высокоуровневом содержимом полнотекстового индекса в таблице `HumanResources.JobCandidate`.  
  
```  
SELECT * FROM sys.dm_fts_index_keywords(db_id('AdventureWorks2012'), object_id('HumanResources.JobCandidate'))  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Динамические административные представления и функции полнотекстового поиска и семантического поиска &#40;языке Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Компонент Full-text Search](../../relational-databases/search/full-text-search.md)   
 [sys.dm_fts_index_keywords_by_document (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
  

---
description: sp_help_fulltext_tables (Transact-SQL)
title: sp_help_fulltext_tables (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_fulltext_tables
- sp_help_fulltext_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_tables
ms.assetid: 86e24a5f-a869-43f6-b83e-c52b7b01b5ff
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ac2e0711ba1c18fef41d18a41a4a8155160d2c02
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200181"
---
# <a name="sp_help_fulltext_tables-transact-sql"></a>sp_help_fulltext_tables (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает список таблиц, зарегистрированных для полнотекстового индексирования.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Вместо этого используйте **sys.fulltext_indexes** представление каталога. Дополнительные сведения см. в разделе [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_help_fulltext_tables [ [ @fulltext_catalog_name = ] 'fulltext_catalog_name' ]   
     [ , [ @table_name = ] 'table_name' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @fulltext_catalog_name = ] 'fulltext_catalog_name'` Имя полнотекстового каталога. Аргумент *fulltext_catalog_name* имеет тип **sysname** и значение по умолчанию NULL. Если параметр *fulltext_catalog_name* опущен или имеет значение null, возвращаются все таблицы с полнотекстовым индексом, связанные с базой данных. Если указано *fulltext_catalog_name* , но *table_name* ОПУЩЕНО или имеет значение null, сведения о полнотекстовом индексе извлекаются для каждой таблицы с полнотекстовым индексом, связанной с этим каталогом. Если указаны и *fulltext_catalog_name* , и *table_name* , то возвращается строка, если *table_name* связана с *fulltext_catalog_name*; в противном случае возникает ошибка.  
  
`[ @table_name = ] 'table_name'` Имя таблицы из одной или двух частей, для которой запрашиваются полнотекстовые метаданные. *table_name* имеет тип **nvarchar (517)** и значение по умолчанию NULL. Если указан только *table_name* , то возвращается только строка, соответствующая *table_name* .  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**TABLE_OWNER**|**sysname**|Владелец таблицы. Это имя пользователя базы данных, создавшего таблицу.|  
|**TABLE_NAME**|**sysname**|Имя таблицы.|  
|**FULLTEXT_KEY_INDEX_NAME**|**sysname**|Индекс, налагающий ограничение UNIQUE на столбец, помеченный как уникальный ключевой столбец.|  
|**FULLTEXT_KEY_COLID**|**int**|Идентификатор столбца уникального индекса, задаваемого параметром FULLTEXT_KEY_NAME.|  
|**FULLTEXT_INDEX_ACTIVE**|**int**|Указывает, можно ли использовать в запросах столбцы, помеченные для полнотекстового индексирования в этой таблице.<br /><br /> 0 = неактивно.<br /><br /> 1= активно.|  
|**FULLTEXT_CATALOG_NAME**|**sysname**|Полнотекстовый каталог, в котором хранятся данные полнотекстового индекса.|  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию разрешения на выполнение предоставлены членам роли **public** .  
  
## <a name="examples"></a>Примеры  
 В следующем примере показано, как вернуть имена таблиц с полнотекстовым индексом, связанных с полнотекстовым каталогом `Cat_Desc`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_help_fulltext_tables 'Cat_Desc';  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [INDEXPROPERTY (Transact-SQL)](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_fulltext_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-table-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

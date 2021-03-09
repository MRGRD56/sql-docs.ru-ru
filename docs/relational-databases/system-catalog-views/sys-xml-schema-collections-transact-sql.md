---
description: sys.xml_schema_collections (Transact-SQL)
title: sys.xml_schema_collections (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.xml_schema_collections_TSQL
- sys.xml_schema_collections
- xml_schema_collections
- xml_schema_collections_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_collections catalog view
ms.assetid: f3f7f3dc-029f-4942-ab3c-75fa9814e40f
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9fb972784e2e05d60bc5df687b80aa561a9536e1
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464066"
---
# <a name="sysxml_schema_collections-transact-sql"></a>sys.xml_schema_collections (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает по одной строке для каждой коллекции схем XML. Коллекция схем XML представляет собой именованный набор XSD-определений. Коллекция схем XML содержится в реляционной схеме и определяется именем языка [!INCLUDE[tsql](../../includes/tsql-md.md)] в области схемы. Следующие кортежи являются уникальными: xml_collection_id, schema_id и Name.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|xml_collection_id|**int**|Идентификатор коллекции схем XML. Уникально в пределах базы данных.|  
|schema_id|**int**|Идентификатор реляционной схемы, содержащей эту коллекцию схем XML.|  
|principal_id|**int**|Идентификатор непосредственного владельца, если он отличается от владельца схемы. По умолчанию содержащиеся в схеме объекты принадлежат владельцу схемы. Однако с помощью инструкции ALTER AUTHORIZATION можно изменить право собственности и указать альтернативного владельца.<br /><br /> NULL = альтернативного владельца нет.|  
|name|**sysname**|Имя коллекции схем XML.|  
|create_date|**datetime**|Дата создания коллекции схем XML.|  
|modify_date|**datetime**|Дата последнего изменения коллекции схем XML.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Схемы XML &#40;представления каталога системы типов XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)   
 [Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  

---
description: sys.foreign_key_columns (Transact-SQL)
title: sys.foreign_key_columns (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.foreign_key_columns
- foreign_key_columns
- sys.foreign_key_columns_TSQL
- foreign_key_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.foreign_key_columns catalog view
ms.assetid: 7247f065-5441-4bcf-9f25-c84a03290dc6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7d7f54d407acedac963983e2f4eb0bc95b87d8d6
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465176"
---
# <a name="sysforeign_key_columns-transact-sql"></a>sys.foreign_key_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Содержит строку для каждого столбца или набора столбцов, представляющих внешний ключ.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**constraint_object_id**|**int**|Идентификатор ограничения FOREIGN KEY.|  
|**constraint_column_id**|**int**|Идентификатор столбца или набора столбцов, составляющих внешний ключ (*1.. n* , где n = число столбцов).|  
|**parent_object_id**|**int**|Идентификатор родителя ограничения, ссылающегося на объект.|  
|**parent_column_id**|**int**|Идентификатор родительского столбца, являющегося ссылающимся столбцом.|  
|**referenced_object_id**|**int**|Идентификатор объекта ссылки с потенциальным ключом.|  
|**referenced_column_id**|**int**|Идентификатор столбца ссылки (потенциальный ключевой столбец).|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  

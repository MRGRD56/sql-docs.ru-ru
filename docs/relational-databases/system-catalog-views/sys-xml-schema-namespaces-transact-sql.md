---
description: sys.xml_schema_namespaces (Transact-SQL)
title: sys.xml_schema_namespaces (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.xml_schema_namespaces_TSQL
- sys.xml_schema_namespaces
- xml_schema_namespaces
- xml_schema_namespaces_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_namespaces catalog view
ms.assetid: 3ed42dd6-929a-41de-80e8-d3a0a488bc7a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 096c665e371293e2a8bc8516989d3c72df5c2591
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199832"
---
# <a name="sysxml_schema_namespaces-transact-sql"></a>sys.xml_schema_namespaces (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает по одной строке для каждого пространства имен XML, определенного в XSD-схеме. Следующие кортежи являются уникальными: **collection_id**, **namespace_id**, **collection_id** и **Name**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**xml_collection_id**|**int**|Идентификатор коллекции XML-схемы, содержащей это пространство имен.|  
|**name**|**nvarchar(4000)**|Имя пространства имен XML. Пустое **имя** указывает на отсутствие целевого пространства имен.|  
|**xml_namespace_id**|**int**|Порядковое числительное, начинающееся с 1, которое уникальным образом идентифицирует пространство имен XML в базе данных.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Схемы XML &#40;представления каталога системы типов XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  

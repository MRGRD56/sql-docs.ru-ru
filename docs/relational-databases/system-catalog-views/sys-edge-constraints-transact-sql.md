---
description: sys.edge_constraints (Transact-SQL)
title: sys.edge_constraints (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 09/17/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.edge_constraints
- edge_constraints
- SQL Graph
- edge_constraints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.edge_constraints catalog view
ms.assetid: 0f782d2f-7126-46ab-85b7-bcba44862231
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e0c07810f26bcb125ac0fc699c8c943a52ca819b
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464962"
---
# <a name="sysedge_constraints-transact-sql"></a>sys.edge_constraints (Transact-SQL)
[!INCLUDE[sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Содержит по одной строке для каждого объекта, который является ограничением границы. 
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.objects>**||Список столбцов, наследуемых этим представлением, см. в разделе [sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**is_disabled**|**bit**|1 = ограничение границы — отключены.<br /><br /> 0 = ограничение границы включено.|  
|**is_not_trusted**|**bit**|1 = ограничение границы не было проверено системой.<br /><br /> 0 = ограничение ребра проверено системой.|  
|**delete_referential_action**|**tinyint**|Ссылочное действие, определенное для этого ограничения границы.<br /><br />0 = нет действий.|  
|**delete_referential_action_desc**|**nvarchar(60)**|Описание ссылочного действия, определенного для этого ограничения границы.<br /><br />NO_ACTION|  
|**is_system_named**|**bit**|1 = имя ограничения ребра создано системой.<br /><br />0 = имя ограничения границы предоставлено пользователем.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Часто задаваемые вопросы о запросах к системному каталогу сервера SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  

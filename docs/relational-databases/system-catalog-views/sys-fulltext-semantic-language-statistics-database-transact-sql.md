---
description: sys.fulltext_semantic_language_statistics_database (Transact-SQL)
title: sys.fulltext_semantic_language_statistics_database (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_semantic_language_statistics_database_TSQL
- fulltext_semantic_language_statistics_database_TSQL
- fulltext_semantic_language_statistics_database
- sys.fulltext_semantic_language_statistics_database
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fulltext_semantic_language_statistics_database catalog view
ms.assetid: 32e95614-ed88-4068-8c37-1e21544717bc
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 9a2cf710611007080c9b00932b59141553151131
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054678"
---
# <a name="sysfulltext_semantic_language_statistics_database-transact-sql"></a>sys.fulltext_semantic_language_statistics_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает строку о базе данных статистики семантики языка, установленной на текущем экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Можно выполнить запрос к этому представлению для сбора сведений о компоненте статистической семантики языка, необходимых для семантической обработки.  
   
  
|**Имя столбца**|**Тип**|**Описание**|  
|-|-|-|  
|**database_id**|**int**|Идентификатор базы данных, уникальный внутри экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**register_date**|**datetime**|Дата регистрации базы данных для семантической обработки.|  
|**registered_by**|**int**|Идентификатор участника на уровне сервера, зарегистрировавшего базу данных для семантической обработки.|  
|**version**|**nvarchar(128)**|Сведения о последней версии, характерные для базы данных статистики семантики языка.|  
  
## <a name="general-remarks"></a>Общие замечания  
 Дополнительные сведения см. в разделе [Установка и настройка семантического поиска](../../relational-databases/search/install-and-configure-semantic-search.md).  
  
## <a name="metadata"></a>Метаданные  
 Для получения сведений о языках, поддерживаемых для семантического индексирования, запросите представление каталога [sys.fulltext_semantic_languages &#40;&#41;Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-semantic-languages-transact-sql.md).  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Видимость метаданных в представлениях каталогов ограничивается защищаемыми объектами, которыми пользователь владеет или на которые ему были предоставлены разрешения.  
  
## <a name="examples"></a>Примеры  
 В следующем примере показано, как запросить **sys.fulltext_semantic_language_statistics_database** , чтобы получить сведения о базе данных семантической статистики языка, зарегистрированной в текущем экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
```  
SELECT * FROM sys.fulltext_semantic_language_statistics_database;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Установка и настройка семантического поиска](../../relational-databases/search/install-and-configure-semantic-search.md)  
  
  

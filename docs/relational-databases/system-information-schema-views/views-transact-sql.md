---
description: VIEWS (Transact-SQL)
title: ПРЕДСТАВЛЕНИЯ (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- VIEWS_TSQL
- VIEWS
dev_langs:
- TSQL
helpviewer_keywords:
- VIEWS view
- INFORMATION_SCHEMA.VIEWS view
ms.assetid: 6119bc94-0b22-45d4-a34b-967afd810a9d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9ac17172022563318667954be53382f7ed22b484
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754074"
---
# <a name="views-transact-sql"></a>VIEWS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает одну строку для представлений, доступ к которым имеет текущий пользователь в текущей базе данных.  
  
 Чтобы получить сведения из этих представлений, укажите полное имя **INFORMATION_SCHEMA.** _view_name_.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Квалификатор представления.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Имя схемы, содержащей представление.<br /><br /> **&#42;&#42; важно &#42;&#42;**  только надежный способ найти схему объекта — запросить представление каталога sys. objects.|  
|**TABLE_NAME**|**nvarchar (** 128 **)**|Имя представления.|  
|**VIEW_DEFINITION**|**nvarchar (** 4000 **)**|Если длина определения больше **nvarchar (** 4000 **)**, этот столбец имеет значение null. В противном случае этот столбец является текстом определения представления.|  
|**CHECK_OPTION**|**varchar (** 7 **)**|Тип инструкции WITH CHECK OPTION. CASCADE, если первоначальное представление было создано с помощью инструкции WITH CHECK OPTION. Иначе возвращается значение NONE.|  
|**IS_UPDATABLE**|**varchar (** 2 **)**|Указывает, можно ли обновлять это представление. Всегда возвращает NO.|  
  
## <a name="see-also"></a>См. также:  
 [Системные представления &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Представления информационной схемы &#40;&#41;Transact-SQL ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.views (Transact-SQL)](../../relational-databases/system-catalog-views/sys-views-transact-sql.md)  
  

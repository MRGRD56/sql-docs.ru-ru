---
description: sp_fulltext_database (Transact-SQL)
title: sp_fulltext_database (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_database_TSQL
- sp_fulltext_database
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_database
ms.assetid: eeb1e151-eb00-484c-8fd1-5641e621ffc6
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5f180dceedb5473ec3b7518a03f6ea86f0659f8e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185430"
---
# <a name="sp_fulltext_database-transact-sql"></a>sp_fulltext_database (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Не влияет на полнотекстовые каталоги в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версиях и поддерживается только для обеспечения обратной совместимости. **sp_fulltext_database** не отключает подсистему Full-Text для данной базы данных. В [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] полнотекстовое индексирование всегда включено для всех баз данных, созданных пользователем.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Вместо этого используйте [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_fulltext_database [@action=] 'action'  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @action = ] 'action'` Действие, которое необходимо выполнить. **Action** имеет тип **varchar (20)** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**enable**|Поддерживается только для обеспечения обратной совместимости. Не действует на полнотекстовые каталоги в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версиях.|  
|**disable**|Поддерживается только для обеспечения обратной совместимости. Не действует на полнотекстовые каталоги в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версиях.|  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="remarks"></a>Remarks  
 В [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версиях полнотекстовое индексирование отключить нельзя. Отключение полнотекстового индексирования не приводит к удалению строк из **таблицы sysfulltextcatalogs** и не указывает на то, что таблицы с поддержкой полнотекстового индексирования больше не отмечаются для полнотекстовой индексации. Все определения полнотекстовых метаданных продолжают храниться в системных таблицах.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** и **db_owner** предопределенной роли базы данных могут выполнять **sp_fulltext_database**.  
  
## <a name="see-also"></a>См. также:  
 [DATABASEPROPERTYEX (Transact-SQL)](../../t-sql/functions/databasepropertyex-transact-sql.md)   
 [FULLTEXTSERVICEPROPERTY (Transact-SQL)](../../t-sql/functions/fulltextserviceproperty-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

---
description: sp_addtabletocontents (Transact-SQL)
title: sp_addtabletocontents (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addtabletocontents_TSQL
- sp_addtabletocontents
helpviewer_keywords:
- sp_addtabletocontents
ms.assetid: 2ea27001-74f4-463e-bf1b-b6b5a86b9219
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 469d9255813e1a072539e6aa8ca7035378ce2b92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198412"
---
# <a name="sp_addtabletocontents-transact-sql"></a>sp_addtabletocontents (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Вставляет ссылки в таблицы отслеживания слияния для любых строк в исходной таблице, которые не включены в таблицы отслеживания. Используйте этот параметр, если вы используете **bcp**, который не запускает триггеры отслеживания слияния, при наличии большого объема данных. Эта хранимая процедура выполняется на издателе в базе данных публикации.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_addtabletocontents [ @table_name = ] 'table_name'  
    [ , [ @owner_name = ] 'owner_name' ]  
    [ , [ @filter_clause = ] 'filter_clause' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @table_name = ] 'table_name'` Имя таблицы. Аргумент *table_name* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @owner_name = ] 'owner_name'` Имя владельца таблицы. Аргумент *owner_name* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @filter_clause = ] 'filter_clause'` Указывает предложение фильтра, которое управляет тем, какие строки недавно загруженных данных должны быть добавлены в таблицы отслеживания слияния. *filter_clause* имеет тип **nvarchar (4000)** и значение по умолчанию NULL. Если *filter_clause* имеет **значение NULL**, добавляются все строки с множественной загрузкой.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_addtabletocontents** используется только в репликации слиянием.  
  
 Строки в *table_name* называются их **ROWGUIDCOL** , а ссылки добавляются в таблицы отслеживания слияния. **sp_addtabletocontents** следует использовать после выполнения операции с массовым копированием данных в таблицу, опубликованную с помощью репликации слиянием. Хранимая процедура инициирует отслеживание строк, которые были скопированы, и обеспечивает их участие в следующей синхронизации.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_addtabletocontents**.  
  
## <a name="see-also"></a>См. также:  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

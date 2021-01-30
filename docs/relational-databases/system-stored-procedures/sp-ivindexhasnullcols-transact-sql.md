---
description: sp_ivindexhasnullcols (Transact-SQL)
title: sp_ivindexhasnullcols (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_ivindexhasnullcols
- sp_ivindexhasnullcols_TSQL
helpviewer_keywords:
- sp_ivindexhasnullcols
ms.assetid: ed2cde63-37e1-43cf-b6ba-3b6114a0f797
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9fc065d30bc8fac8195a3a389de1b316645bcd57
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183408"
---
# <a name="sp_ivindexhasnullcols-transact-sql"></a>sp_ivindexhasnullcols (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Проверяет уникальность кластеризованного индекса индексированного представления и отсутствие в нем столбцов, которые могут содержать значение NULL в момент, когда индексированное представление должно использоваться для создания публикации транзакций. Эта хранимая процедура выполняется на издателе в базе данных публикации.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_ivindexhasnullcols [ @viewname = ] 'view_name'  
        , [ @fhasnullcols= ] field_has_null_columns OUTPUT  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @viewname = ] 'view_name'` Имя проверяемого представления. Аргумент *view_name* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @fhasnullcols = ] field_has_null_columns OUTPUT` Флаг, указывающий, содержит ли индекс представления столбцы, допускающие значение NULL. Аргумент *view_name* имеет тип **sysname** и не имеет значения по умолчанию. Возвращает значение **1** , если индекс представления содержит столбцы, ДОПУСКАЮЩИЕ значение null. Возвращает значение **0** , если представление не содержит столбцов, ДОПУСКАЮЩИХ значения NULL.  
  
> [!NOTE]  
>  Если хранимая процедура сама возвращает код возврата **1**, то есть выполнение хранимой процедуры завершилось сбоем, это значение равно **0** , и его следует игнорировать.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_ivindexhasnullcols** используется репликацией транзакций.  
  
 По умолчанию, статьи индексированного представления в публикации создаются как таблицы на подписчиках. Однако если индексированные столбцы допускают значения NULL, индексированное представление создается на подписчике как индексированное представление, а не как таблица. Выполнив данную хранимую процедуру, можно предупредить пользователя о существовании (или отсутствии) данной проблемы в текущем индексированном представлении.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_ivindexhasnullcols**.  
  
## <a name="see-also"></a>См. также:  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

---
description: sp_schemafilter (Transact-SQL)
title: sp_schemafilter (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_schemafilter_TSQL
- sp_schemafilter
helpviewer_keywords:
- sp_schemafilter
ms.assetid: 199e869b-2cd2-44ee-b2ee-69edb06a1bc4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f5b05b9976525109f6361c68f7acf7ee309f8b85
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189995"
---
# <a name="sp_schemafilter-transact-sql"></a>sp_schemafilter (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Изменяет и отображает сведения о схеме, исключенной при построении списка таблиц Oracle, подходящих для публикации.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_schemafilter [ @publisher = ] 'publisher'   
   [ , [ @schema = ] 'schema' ]   
   [ , [ @operation = ] 'operation' ]   
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publisher = ] 'publisher'` Имя издателя, не являющегося [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] издателем. параметр *Publisher* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @schema = ] 'schema'` Имя схемы. *Schema* имеет тип **sysname** и значение по умолчанию NULL.  
  
`[ @operation = ] 'operation'` Действие, выполняемое с этой схемой. *Операция* имеет тип **nvarchar (4)** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**add**|Добавляет указанную схему в список схем, не подходящих для публикации.|  
|**тени**|Удаляет указанную схему из списка схем, не подходящих для публикации.|  
|**help**|Возвращает список схем, которые не подходят для публикации.|  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**SchemaName**|**sysname**|Имя схемы, не подходящей для публикации.|  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_schemafilter** следует использовать только для разнородных издателей.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** на распространителе могут выполнять **sp_schemafilter**.  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры репликации (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  

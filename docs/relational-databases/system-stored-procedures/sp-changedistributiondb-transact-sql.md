---
description: sp_changedistributiondb (Transact-SQL)
title: sp_changedistributiondb (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changedistributiondb_TSQL
- sp_changedistributiondb
helpviewer_keywords:
- sp_changedistributiondb
ms.assetid: 66f73185-ea9e-43f9-86ed-9dd933cee2f6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 958b76e23e52a6f36d54acf9d596cec92aadefba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159017"
---
# <a name="sp_changedistributiondb-transact-sql"></a>sp_changedistributiondb (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Изменяет свойства базы данных распространителя. Эта хранимая процедура выполняется на распространителе в любой базе данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_changedistributiondb [ @database= ] 'database'   
    [ , [ @property= ] 'property' ]   
    [ , [ @value= ] 'value' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @database = ] 'database'` Имя базы данных распространителя. Аргумент *Database* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @property = ] 'property'` Свойство, которое необходимо изменить для данной базы данных. Аргумент *Property* имеет тип **sysname** и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**history_retention**|Срок хранения таблицы журнала.|  
|**max_distretention**|Максимальный срок хранения распространения.|  
|**min_distretention**|Минимальный срок хранения распространения.|  
|NULL (по умолчанию)|Выводятся все доступные значения *свойств* .|  
  
`[ @value = ] 'value'` Новое значение для указанного свойства. *value* имеет тип **nvarchar (255)** и значение по умолчанию NULL.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_changedistributiondb** используется во всех типах репликации.  
  
## <a name="example"></a>Пример  
 [!code-sql[HowTo#sp_changedistributiondb](../../relational-databases/replication/codesnippet/tsql/sp-changedistributiondb-_1.sql)]  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** могут выполнять **sp_changedistributiondb**.  
  
## <a name="see-also"></a>См. также  
 [Просмотр и изменение свойств издателя и распространителя](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md)   
 [sp_dropdistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropdistributiondb-transact-sql.md)   
 [sp_helpdistributiondb (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistributiondb-transact-sql.md)   
 [Хранимые процедуры репликации (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  

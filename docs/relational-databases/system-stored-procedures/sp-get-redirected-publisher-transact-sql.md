---
description: sp_get_redirected_publisher (Transact-SQL)
title: sp_get_redirected_publisher (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_get_redirected_publisher_TSQL
- sp_get_redirected_publisher
ms.assetid: d47a9ab5-f2cc-42a8-8be9-a33895ce44f0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fa1a1b8f7cffe3f98435ebd48a404fb76df34c8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204755"
---
# <a name="sp_get_redirected_publisher-transact-sql"></a>sp_get_redirected_publisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Используется агентами репликация для запроса распространителя, чтобы определить, был ли перенаправлен первоначальный издатель.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_get_redirected_publisher   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name',   
    [ @bypass_publisher_validation = ] [0 | 1 ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @original_publisher = ] 'original_publisher'` Имя экземпляра SQL Server, который первоначально опубликовал базу данных. Аргумент *original_publisher* имеет тип **sysname** и не имеет значения по умолчанию.
  
`[ @publisher_db = ] 'publisher_db'` Имя публикуемой базы данных. Аргумент *publisher_db* имеет тип **sysname** и не имеет значения по умолчанию.  
  
`[ @bypass_publisher_validation = ] [0 | 1 ]` Используется для обхода проверки перенаправленного издателя. Если значение равно 0, выполняется проверка. Если значение равно 1, проверка не выполнена. *bypass_publisher_validation* имеет **бит** и значение по умолчанию 0.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**redirected_publisher**|**sysname**|Имя издателя после перенаправления.|  
|**error_number**|**int**|Номер ошибки проверки.|  
|**error_severity**|**int**|Серьезность ошибки проверки.|  
|**error_message**|**nvarchar(4000)**|Текст сообщения ошибки проверки.|  
  
## <a name="remarks"></a>Замечания  
 *redirected_publisher* возвращает имя текущего издателя. Возвращает значение null, если издатель и публикация баз данных не были перенаправлены с помощью **sp_redirect_publisher**.  
  
 Если проверка не запрошена или если для издателя и базы данных публикации не существует записи, *ERROR_NUMBER* и *ERROR_SEVERITY* возвращают 0, а *ERROR_MESSAGE* возвращает значение null.  
  
 При запросе проверки вызывается хранимая процедура проверки [sp_validate_redirected_publisher &#40;вызова Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md) , чтобы убедиться, что целевой объект перенаправления является подходящим узлом для базы данных публикации. Если проверка завершается с ошибкой, **sp_get_redirected_publisher** возвращает имя перенаправленного издателя, 0 для столбцов *ERROR_NUMBER* и *ERROR_SEVERITY* и значение NULL в столбце *ERROR_MESSAGE* .  
  
 Если проверка запрошена и завершилась неудачей, имя перенаправленного издателя возвращается вместе с информацией об ошибке.  
  
## <a name="permissions"></a>Разрешения  
 Участник должен быть членом предопределенной роли сервера **sysadmin** , **db_owner** предопределенной роли базы данных для базы данных распространителя или членом списка доступа к публикации для определенной публикации, связанной с базой данных издателя.  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры репликации (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)   
 [sp_redirect_publisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-redirect-publisher-transact-sql.md)   
 [sp_validate_replica_hosts_as_publishers &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-validate-replica-hosts-as-publishers-transact-sql.md)  
  
  

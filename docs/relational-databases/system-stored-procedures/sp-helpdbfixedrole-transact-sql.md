---
description: sp_helpdbfixedrole (Transact-SQL)
title: sp_helpdbfixedrole (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpdbfixedrole
- sp_helpdbfixedrole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpdbfixedrole
ms.assetid: ad87e9a0-b901-4e37-9950-aa517d680fc3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 817947901ca7f35ba2972004d23ed8d7f85215ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176423"
---
# <a name="sp_helpdbfixedrole-transact-sql"></a>sp_helpdbfixedrole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает список всех предопределенных ролей базы данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_helpdbfixedrole [ [ @rolename = ] 'role' ]   
```  
  
## <a name="arguments"></a>Аргументы  
`[ @rolename = ] 'role'` Имя предопределенной роли базы данных. Аргумент *Role* имеет тип **sysname** и значение по умолчанию NULL. Если указан параметр *Role* , возвращаются только сведения об этой роли. в противном случае возвращается список и описание всех предопределенных ролей базы данных.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**DbFixedRole**|**sysname**|Имя предопределенной роли базы данных.|  
|**Описание**|**nvarchar (70)**|Описание **дбфикседроле.**|  
  
## <a name="remarks"></a>Замечания  
 Предопределенные роли базы данных, как показано в таблице, определены на уровне базы данных и обладают специальными разрешениями для выполнения определенных административных функций. Предопределенные роли базы данных не могут быть добавлены или удалены. Нельзя изменить разрешения, предоставленные предопределенной роли базы данных.  
  
|Предопределенная роль базы данных|Описание|  
|-------------------------|-----------------|  
|**db_owner**|Владельцы базы данных|  
|**db_accessadmin**|Администраторы доступа к базе данных|  
|**db_securityadmin**|Администраторы безопасности базы данных|  
|**db_ddladmin**|Администраторы DDL базы данных|  
|**db_backupoperator**|Операторы резервного копирования базы данных|  
|**db_datareader**|Модули чтения данных из базы данных|  
|**db_datawriter**|Модули записи данных в базу данных|  
|**db_denydatareader**|Модули чтения данных из базы данных, которым отказано в доступе|  
|**db_denydatawriter**|Модули записи данных в базу данных, которым отказано в доступе|  
  
 Следующая таблица показывает хранимые процедуры, которые используются для изменения ролей базы данных.  
  
|Хранимая процедура|Действие|  
|----------------------|------------|  
|**sp_addrolemember**|Добавляет пользователя базы данных к предопределенной роли базы данных.|  
|**sp_helprole**|Возвращает список всех членов предопределенной роли базы данных.|  
|**sp_droprolemember, хранимая процедура**|Удаляет член из предопределенной роли базы данных.|  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
 Полученные данные подлежат ограничениям на доступ к метаданным. Сущности, на которые участник не имеет разрешения, не показаны. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Примеры  
 Следующий пример иллюстрирует получение списка всех предопределенных ролей базы данных.  
  
```  
EXEC sp_helpdbfixedrole;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [Хранимые процедуры безопасности (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Хранимая процедура sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_dbfixedrolepermission &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbfixedrolepermission-transact-sql.md)   
 [sp_droprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

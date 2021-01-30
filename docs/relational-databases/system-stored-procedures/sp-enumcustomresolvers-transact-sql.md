---
description: sp_enumcustomresolvers (Transact-SQL)
title: sp_enumcustomresolvers (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_enumcustomresolvers
- sp_enumcustomresolvers_TSQL
helpviewer_keywords:
- sp_enumcustomresolvers
ms.assetid: 81bd0d3a-48dc-42b1-b662-c630f61fc630
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8972aa1aaa4e567c60efb9653a75e6fa196211b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99156780"
---
# <a name="sp_enumcustomresolvers-transact-sql"></a>sp_enumcustomresolvers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает список всех доступных обработчиков бизнес-логики и пользовательских сопоставителей, зарегистрированных на распространителе. Эта хранимая процедура выполняется на подписчике в любой базе данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_enumcustomresolvers [ [ @distributor =] 'distributor']  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @distributor = ] 'distributor'` Имя распространителя, в котором находится пользовательский сопоставитель. Аргумент *распространитель* имеет тип **sysname** и значение по умолчанию NULL. *Этот аргумент является устаревшим и будет удален в следующем выпуске.*  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**article_resolver**|**nvarchar(255)**|Понятное имя обработчика бизнес-логики или сопоставителя конфликтов.|  
|**resolver_clsid**|**nvarchar(50)**|Идентификатор класса (CLSID) сопоставителя в архитектуре COM. Этот столбец содержит нулевое значение CLSID для обработчика бизнес-логики.|  
|**is_dotnet_assembly**|**bit**|Указывает, предназначена ли регистрация для обработчика бизнес-логики:<br /><br /> **0** = сопоставитель конфликтов на основе COM<br /><br /> **1** = обработчик бизнес-логики|  
|**dotnet_assembly_name**|**nvarchar(255)**|Имя сборки платформы [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework, в которой реализован обработчик бизнес-логики.|  
|**dotnet_class_name**|**nvarchar(255)**|Имя класса, который замещает класс <xref:Microsoft.SqlServer.Replication.BusinessLogicSupport.BusinessLogicModule> для реализации обработчика бизнес-логики.|  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_enumcustomresolvers** используется в репликации слиянием.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** и **db_owner** предопределенной роли базы данных могут выполнять **sp_enumcustomresolvers**.  
  
## <a name="see-also"></a>См. также:  
 [Реализация обработчика бизнес-логики для статьи публикации слиянием](../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)   
 [Реализация пользовательского сопоставителя конфликтов для статьи публикации слиянием](../../relational-databases/replication/implement-a-custom-conflict-resolver-for-a-merge-article.md)   
 [sp_lookupcustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lookupcustomresolver-transact-sql.md)   
 [sp_unregistercustomresolver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-unregistercustomresolver-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

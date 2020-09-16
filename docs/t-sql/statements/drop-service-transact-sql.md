---
description: DROP SERVICE (Transact-SQL)
title: DROP SERVICE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_SERVICE_TSQL
- DROP SERVICE
dev_langs:
- TSQL
helpviewer_keywords:
- deleting services
- services [Service Broker], removing
- dropping services
- DROP SERVICE statement
- removing services
ms.assetid: 2351bba7-0f2a-4cda-b3b2-6a88b8747c53
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f4c717421fcee62984f7df03cd99dc0af1e8f1ef
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/08/2020
ms.locfileid: "89549304"
---
# <a name="drop-service-transact-sql"></a>DROP SERVICE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Удаляет существующую службу.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
DROP SERVICE service_name  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *service_name*  
 Имя удаляемой службы. Не могут быть указаны имена сервера, базы данных и схемы.  
  
## <a name="remarks"></a>Комментарии  
 Нельзя удалить службу, если на нее ссылаются какие-либо приоритеты диалогов.  
  
 При завершении работы службы из очереди, используемой службой, удаляются все сообщения, относящиеся к этой службе. Компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] отправляет сообщение об ошибке на удаленную сторону всех открытых диалогов, которые используют службу.  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию разрешением на удаление службы обладает владелец службы, члены предопределенной роли базы данных db_ddladmin или db_owner и члены предопределенной роли сервера sysadmin.  
  
## <a name="examples"></a>Примеры  
 В следующем примере удаляется служба `//Adventure-Works.com/Expenses`.  
  
```  
DROP SERVICE [//Adventure-Works.com/Expenses] ;  
```  
  
## <a name="see-also"></a>См. также  
 [ALTER BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/alter-broker-priority-transact-sql.md)   
 [ALTER SERVICE (Transact-SQL)](../../t-sql/statements/alter-service-transact-sql.md)   
 [CREATE SERVICE (Transact-SQL)](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

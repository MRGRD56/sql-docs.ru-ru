---
description: DROP CONTRACT (Transact-SQL)
title: DROP CONTRACT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_CONTRACT_TSQL
- DROP CONTRACT
dev_langs:
- TSQL
helpviewer_keywords:
- dropping contracts
- removing contracts
- deleting contracts
- contracts [Service Broker], dropping
- DROP CONTRACT statement
ms.assetid: fdd0f81e-3c22-4cdf-9416-b4977a6ac3b6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e0de044e06a6a78d9b0b8e7bb3f7020d5a356cc0
ms.sourcegitcommit: 197a6ffb643f93592edf9e90b04810a18be61133
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2020
ms.locfileid: "91380149"
---
# <a name="drop-contract-transact-sql"></a>DROP CONTRACT (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаляет существующий контракт из базы данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DROP CONTRACT contract_name   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *contract_name*  
 Имя удаляемого контракта. Не могут быть указаны имена сервера, базы данных и схемы.  
  
## <a name="remarks"></a>Комментарии  
 Нельзя удалить контракт, если на него ссылаются какие-либо службы или приоритеты диалога.  
  
 При удалении контракта компонент [!INCLUDE[ssSB](../../includes/sssb-md.md)] завершает с ошибкой любой диалог, использующий этот контракт.  
  
## <a name="permissions"></a>Разрешения  
 По умолчанию разрешением на удаление контракта обладает владелец контракта, члены предопределенной роли базы данных db_ddladmin или db_owner и члены предопределенной роли сервера sysadmin.  
  
## <a name="examples"></a>Примеры  
 В следующем примере контракт `//Adventure-Works.com/Expenses/ExpenseSubmission` удаляется из текущей базы данных.  
  
```sql  
DROP CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission] ;  
```  
  
## <a name="see-also"></a>См. также  
 [ALTER BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/alter-broker-priority-transact-sql.md)   
 [ALTER SERVICE (Transact-SQL)](../../t-sql/statements/alter-service-transact-sql.md)   
 [CREATE CONTRACT (Transact-SQL)](../../t-sql/statements/create-contract-transact-sql.md)   
 [DROP BROKER PRIORITY (Transact-SQL)](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [DROP SERVICE (Transact-SQL)](../../t-sql/statements/drop-service-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

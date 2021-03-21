---
description: SESSION_CONTEXT (Transact-SQL)
title: SESSION_CONTEXT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SESSION_CONTEXT
- sys.SESSION_CONTEXT
- SESSION_CONTEXT_TSQL
- sys.SESSION_CONTEXT_TSQL
helpviewer_keywords:
- SESSION_CONTEXT function
ms.assetid: b6bdbc54-331a-43cc-ab3d-3872d6a12100
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: af4cf829168f50162d08d448cfc4f0b0f89caa9b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749214"
---
# <a name="session_context-transact-sql"></a>SESSION_CONTEXT (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Возвращает значение указанного ключа в контексте текущего сеанса. Значение задается с помощью процедуры [sp_set_session_context (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
SESSION_CONTEXT(N'key')  
```  
  
## <a name="arguments"></a>Аргументы
 'key'  
 Ключ (типа sysname) извлекаемого значения.  
  
## <a name="return-type"></a>Тип возвращаемых данных  
 **sql_variant**  
  
## <a name="return-value"></a>Возвращаемое значение  
 Значение, связанное с указанным ключом в контексте сеанса, или NULL, если значение для этого ключа не задано.  
  
## <a name="permissions"></a>Разрешения  
 Любой пользователь может считывать контекст своего сеанса.  
  
## <a name="remarks"></a>Комментарии  
 Поведение функции MARS для SESSION_CONTEXT аналогично ее поведению для CONTEXT_INFO. Если пакет MARS задает пару "ключ-значение", новое значение не будет возвращаться в других пакетах MARS по тому же соединению, если они были запущены до того, как завершилось выполнение пакета, задавшего это значение. Если в соединении имеется несколько активных пакетов MARS, значения не могут устанавливаться как доступные только для чтения. Это позволяет избежать состояний гонки и неопределенности в отношении того, какое значение является победителем.  
  
## <a name="examples"></a>Примеры  
 В приведенном ниже простом примере значение контекста сеанса для ключа `user_id` устанавливается в 4, а затем используется функция **SESSION_CONTEXT** для получения значения.  
  
```sql  
EXEC sp_set_session_context 'user_id', 4;  
SELECT SESSION_CONTEXT(N'user_id');  
```  
  
## <a name="see-also"></a>См. также  
 [sp_set_session_context (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-set-session-context-transact-sql.md)   
 [CURRENT_TRANSACTION_ID (Transact-SQL)](../../t-sql/functions/current-transaction-id-transact-sql.md)   
 [Безопасность на уровне строк](../../relational-databases/security/row-level-security.md)   
 [CONTEXT_INFO  (Transact-SQL)](../../t-sql/functions/context-info-transact-sql.md)   
 [SET CONTEXT_INFO (Transact-SQL)](../../t-sql/statements/set-context-info-transact-sql.md)  
  
  

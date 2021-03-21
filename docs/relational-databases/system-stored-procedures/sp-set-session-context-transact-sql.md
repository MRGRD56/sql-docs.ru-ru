---
description: sp_set_session_context (Transact-SQL)
title: sp_set_session_context (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 05/14/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_set_session_context
- sp_set_session_context_TSQL
- sys.sp_set_session_context
- sys.sp_set_session_context_TSQL
helpviewer_keywords:
- sp_set_session_context
ms.assetid: 7a3a3b2a-1408-4767-a376-c690e3c1fc5b
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ff9f50aa953b0a7388156a9793d3ab1cbe819dc7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754634"
---
# <a name="sp_set_session_context-transact-sql"></a>sp_set_session_context (Transact-SQL)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

Задает пару "ключ-значение" в контексте сеанса.  
  

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
sp_set_session_context [ @key= ] N'key', [ @value= ] 'value'  
    [ , [ @read_only = ] { 0 | 1 } ]  
[ ; ]  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @key =] Н'кэй '  
 Заданный ключ типа **sysname**. Максимальный размер ключа составляет 128 байт.  
  
 [ @value =] "значение"  
 Значение для указанного ключа типа **sql_variant**. При установке значения NULL освобождается память. Максимальный размер 8 000 байт.  
  
 [ @read_only =] {0 | 1}  
 Флаг типа **bit**. Если значение равно 1, то в этом логическом подключении нельзя снова изменить значения для указанного ключа. При значении 0 (по умолчанию) значение может быть изменено.  
  
## <a name="permissions"></a>Разрешения  
 Любой пользователь может задать контекст сеанса для своего сеанса.  
  
## <a name="remarks"></a>Remarks  
 Как и в случае с другими хранимыми процедурами, в качестве параметров могут передаваться только литералы и переменные (не выражения и вызовы функций).  
  
 Общий размер контекста сеанса ограничен 1 МБ. Если задать значение, которое приводит к превышению этого ограничения, выполнение инструкции завершится ошибкой. Вы можете наблюдать за общим использованием памяти в [sys.dm_os_memory_objects &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-objects-transact-sql.md).  
  
 Вы можете наблюдать за общим использованием памяти, выполняя запрос [sys.dm_os_memory_cache_counters &#40;&#41;Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-counters-transact-sql.md) следующим образом: `SELECT * FROM sys.dm_os_memory_cache_counters WHERE type = 'CACHESTORE_SESSION_CONTEXT';`  
  
## <a name="examples"></a>Примеры  
A. В следующем примере показано, как задать и затем вернуть контекстный ключ сеансов с именем Language со значением английский.  
  
```  
EXEC sys.sp_set_session_context @key = N'language', @value = 'English';  
SELECT SESSION_CONTEXT(N'language');  
```  
  
 В следующем примере показано использование необязательного флага только для чтения.  
  
```  
EXEC sys.sp_set_session_context @key = N'user_id', @value = 4, @read_only = 1;  
```  

Б. В следующем примере показано, как задать и получить ключ контекста сеанса с именем client_correlation_id со значением 12323ad.
```
-- set value
EXEC sp_set_session_context 'client_correlation_id', '12323ad'; 

--check value
SELECT SESSION_CONTEXT(N'client_correlation_id');

```

## <a name="see-also"></a>См. также:  
 [CURRENT_TRANSACTION_ID (Transact-SQL)](../../t-sql/functions/current-transaction-id-transact-sql.md)   
 [SESSION_CONTEXT &#40;Transact-SQL&#41;](../../t-sql/functions/session-context-transact-sql.md)   
 [Безопасность на уровне строк](../../relational-databases/security/row-level-security.md)   
 [CONTEXT_INFO  (Transact-SQL)](../../t-sql/functions/context-info-transact-sql.md)   
 [SET CONTEXT_INFO (Transact-SQL)](../../t-sql/statements/set-context-info-transact-sql.md)  
  
  

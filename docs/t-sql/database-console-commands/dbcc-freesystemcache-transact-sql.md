---
description: DBCC FREESYSTEMCACHE (Transact-SQL)
title: DBCC FREESYSTEMCACHE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 02/25/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FREESYSTEMCACHE_TSQL
- DBCC_FREESYSTEMCACHE_TSQL
- DBCC FREESYSTEMCACHE
- FREESYSTEMCACHE
dev_langs:
- TSQL
helpviewer_keywords:
- clearing unused cache entries
- DBCC FREESYSTEMCACHE statement
- unused cache entries
- releasing unused cache entries
- freeing unused cache entries
- cleaning unused cache entries
ms.assetid: 4b5c460b-e4ad-404a-b4ca-d65aba38ebbb
author: pmasl
ms.author: umajay
ms.openlocfilehash: f99d6e50aed43273dbcaa659f95a8bb8a1fe73d3
ms.sourcegitcommit: 544706f6725ec6cdca59da3a0ead12b99accb2cc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "92638987"
---
# <a name="dbcc-freesystemcache-transact-sql"></a>DBCC FREESYSTEMCACHE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Удаляет все неиспользуемые элементы из всех кэшей. Компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] заранее автоматически очищает неиспользуемые элементы кэша в фоновом режиме, освобождая память для текущих записей. Но можно использовать эту команду, чтобы вручную удалить неиспользуемые записи из каждого кэша или из указанного кэша пула Resource Governor.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
```syntaxsql
DBCC FREESYSTEMCACHE   
    ( 'ALL' [, pool_name ] )   
    [WITH   
    { [ MARK_IN_USE_FOR_REMOVAL ] , [ NO_INFOMSGS ]  }  
    ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
( 'ALL' [, _pool\_name_ ] )  
Ключевое слово ALL указывает все поддерживаемые кэши.  
Аргумент _pool\_name_ указывает кэш пула Resource Governor. Освобождены будут только записи, связанные с этим пулом.  
  
MARK_IN_USE_FOR_REMOVAL  
Асинхронно освобождает текущие используемые элементы из соответствующих кэшей после того, как они перестают использоваться. Новые элементы, созданные в кэше после выполнения DBCC FREESYSTEMCACHE WITH MARK_IN_USE_FOR_REMOVAL, не будут затронуты.  
  
NO_INFOMSGS  
Подавляет вывод всех информационных сообщений.  
  
## <a name="remarks"></a>Remarks  
При выполнении инструкции DBCC FREESYSTEMCACHE очищается кэш планов для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Очистка кэша планов становится причиной перекомпиляции всех предстоящих планов выполнения и приводит к непредвиденному временному снижению производительности обработки запросов. Для каждого удаленного хранилища кэша в кэше планов журнал ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] содержит следующее информационное сообщение: 

>`SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to 'DBCC FREEPROCCACHE' or 'DBCC FREESYSTEMCACHE' operations.`

 Это сообщение добавляется в журнал каждые пять минут при сбросе кэша в течение этого интервала времени.

## <a name="result-sets"></a>Наборы результатов  
Инструкция DBCC FREESYSTEMCACHE возвращает следующее сообщение: "Выполнение инструкции DBCC завершено. Если инструкция DBCC выдает сообщения об ошибках, обратитесь к системному администратору».
  
## <a name="permissions"></a>Разрешения  
Требует разрешения ALTER SERVER STATE на сервере.
  
## <a name="examples"></a>Примеры  
  
### <a name="a-releasing-unused-cache-entries-from-a-resource-governor-pool-cache"></a>A. Освобождение неиспользуемых записей кэша из кэша пула регулятора ресурсов  
В следующем примере показывается, как очищать кэши, выделенные указанному пулу ресурсов регулятора ресурсов.
  
```sql
-- Clean all the caches with entries specific to the resource pool named "default".  
DBCC FREESYSTEMCACHE ('ALL', default);  
```  
  
### <a name="b-releasing-entries-from-their-respective-caches-after-they-become-unused"></a>Б. Освобождение записей из кэшей после прекращения их использования  
В следующем примере используется предложение MARK_IN_USE_FOR_REMOVAL, чтобы освободить записи из всех текущих кэшей, когда записи становятся неиспользуемыми.
  
```sql
DBCC FREESYSTEMCACHE ('ALL') WITH MARK_IN_USE_FOR_REMOVAL;  
```  
  
## <a name="see-also"></a>См. также:  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC FREEPROCCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md)  
[DBCC FREESESSIONCACHE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-freesessioncache-transact-sql.md)  
[Регулятор ресурсов](../../relational-databases/resource-governor/resource-governor.md)
  
  

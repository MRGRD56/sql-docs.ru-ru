---
description: DBCC DROPCLEANBUFFERS (Transact-SQL)
title: DBCC DROPCLEANBUFFERS (Transact-SQL)
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: synapse-analytics, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROPCLEANBUFFERS
- DBCC_DROPCLEANBUFFERS_TSQL
- DROPCLEANBUFFERS_TSQL
- DBCC DROPCLEANBUFFERS
dev_langs:
- TSQL
helpviewer_keywords:
- clean buffers
- cold buffer cache
- buffer pools [SQL Server]
- dropping buffers
- removing buffers
- DBCC DROPCLEANBUFFERS statement
ms.assetid: a4121927-f2ce-4926-aa2c-9b1519dac048
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7f653c83158a513f1c2c00f59bb18c0dcd459eff
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735534"
---
# <a name="dbcc-dropcleanbuffers-transact-sql"></a>DBCC DROPCLEANBUFFERS (Transact-SQL)

[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

Удаляет все чистые буферы из буферного пула и объекты columnstore из пула объектов columnstore.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис

Синтаксис для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и :[!INCLUDE[ssSOD](../../includes/sssodfull-md.md)]

```syntaxsql
DBCC DROPCLEANBUFFERS [ WITH NO_INFOMSGS ]  
```  
Синтаксис для [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и :[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

```syntaxsql  
DBCC DROPCLEANBUFFERS ( COMPUTE | ALL ) [ WITH NO_INFOMSGS ]  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 WITH NO_INFOMSGS  
 Подавляет вывод всех информационных сообщений. Информационные сообщения всегда блокируются в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
 COMPUTE  
 Очистить кэш данных в памяти в каждом вычислительном узле.  
  
 ALL  
 Очистить кэш данных в памяти в каждом вычислительном узле и в управляющем узле. Если значение не указано, это значение по умолчанию.  
  
## <a name="remarks"></a>Remarks  
С помощью инструкции DBCC DROPCLEANBUFFERS можно выполнить проверку запроса при холодном буферном кэше, не выключая и не перезапуская сервер.
Чтобы удалить чистые буферы из буферного пула и объекты columnstore из пула объектов columnstore, необходимо сначала воспользоваться инструкцией CHECKPOINT для обеспечения холодного буферного кэша. Это вызовет принудительную запись всех «грязных» страниц текущей базы данных на диск и очистит буферы. После этого можно выполнить команду DBCC DROPCLEANBUFFERS, которая удалит все буферы из буферного пула.
  
## <a name="result-sets"></a>Результирующие наборы  
Инструкция DBCC DROPCLEANBUFFERS в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает следующее сообщение.
  
```
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Разрешения  
Требуется членство в предопределенной роли сервера `sysadmin` для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
Требуется членство в предопределенной роли сервера `DB_OWNER` для [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  
  
## <a name="see-also"></a>См. также:  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[CHECKPOINT (Transact-SQL)](../../t-sql/language-elements/checkpoint-transact-sql.md)  
  
  

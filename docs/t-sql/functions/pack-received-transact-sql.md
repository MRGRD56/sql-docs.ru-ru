---
description: '&#x40;&#x40;PACK_RECEIVED (Transact-SQL)'
title: '@@PACK_RECEIVED (Transact-SQL) | Документы Майкрософт'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@PACK_RECEIVED_TSQL'
- '@@PACK_RECEIVED'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@PACK_RECEIVED function'
- number of packets read
- packets [SQL Server], number read
ms.assetid: 5c0b3d36-bfad-4f0b-abb8-e8f6391b32cd
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 7b3bb37af363727d1eb41922979ee6f78a9fd008
ms.sourcegitcommit: 197a6ffb643f93592edf9e90b04810a18be61133
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2020
ms.locfileid: "91380826"
---
# <a name="x40x40pack_received-transact-sql"></a>&#x40;&#x40;PACK_RECEIVED (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Возвращает количество входных пакетов, считанных из сети сервером [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с момента его последнего запуска.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
@@PACK_RECEIVED  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 **integer**  
  
## <a name="remarks"></a>Комментарии  
 Для отображения отчета, содержащего статистику по нескольким экземплярам [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], включая перечень полученных и переданных пакетов, выполните процедуру **sp_monitor**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере показано использование функции `@@PACK_RECEIVED`.  
  
```sql  
SELECT @@PACK_RECEIVED AS 'Packets Received';   
```  
  
 Ниже приводится образец результирующего набора.  
  
```  
Packets Received  
----------------  
128  
```  
  
## <a name="see-also"></a>См. также  
 [@@PACK_SENT](../../t-sql/functions/pack-sent-transact-sql.md)   
 [sp_monitor](../../relational-databases/system-stored-procedures/sp-monitor-transact-sql.md)   
 [Системные статистические функции](../../t-sql/functions/system-statistical-functions-transact-sql.md)  
  
  

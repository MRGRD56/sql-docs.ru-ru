---
description: sp_prepexecrpc (Transact-SQL)
title: sp_prepexecrpc (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepexecrpc
- sp_cursor_prepexecrpc_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_prepexecrpc
ms.assetid: 35d686f2-ef31-4eaa-baa9-9cef5d6c87c2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c3190e97f177811bc5c90e770544d9c4da1f104b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183091"
---
# <a name="sp_prepexecrpc-transact-sql"></a>sp_prepexecrpc (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Подготавливает и вызывает параметризованную хранимую процедуру, заданную с помощью идентификатора RPC. sp_prepexecrpc вызывается по ID = 14 в пакете потока табличных данных (TDS).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_prepexecrpc handle OUTPUT, RPCCall  
    [ , bound_param ] [ ,...n]]  
```  
  
## <a name="arguments"></a>Аргументы  
 *справиться*  
 Сформированный системой [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] идентификатор подготовленного дескриптора. *Handle* является обязательным параметром с возвращаемым значением **int** .  
  
 *рпккалл*  
 Определяет вызов хранимой процедуры с использованием канонического синтаксиса ODBC. *Рпккалл* — это обязательный параметр, который вызывает входное строковое значение типа **ntext** .  
  
 *bound_param*  
 Означает необязательное использование дополнительных параметров. *bound_param* вызывает входное значение любого типа данных для обозначения дополнительных используемых параметров.  
  
## <a name="see-also"></a>См. также:  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

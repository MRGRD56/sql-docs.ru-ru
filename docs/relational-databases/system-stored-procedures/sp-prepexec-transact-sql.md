---
description: sp_prepexec (Transact-SQL)
title: sp_prepexec (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepexec
- sp_cursor_prepexec_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_prepexec
ms.assetid: f9141850-a62b-43bf-8e46-b2f92b75ca56
author: markingmyname
ms.author: maghan
ms.openlocfilehash: df6a482aeca510fde3d89dd74ddafc0ebe713287
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196009"
---
# <a name="sp_prepexec-transact-sql"></a>sp_prepexec (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Подготавливает и выполняет параметризованную [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкцию. sp_prepexec объединяет функции sp_prepare и sp_execute. Это действие вызывается по ИДЕНТИФИКАТОРу = 13 в пакете потока табличных данных (TDS).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_prepexec handle OUTPUT, params , stmt  
    [ , bound param ] [ ,...n]]  
```  
  
## <a name="arguments"></a>Аргументы  
 *справиться*  
 — Это [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] созданный идентификатор *маркера* . *Handle* является обязательным параметром с возвращаемым значением **int** .  
  
 *params*  
 Указывает параметризованные инструкции. Определение переменных *params* подставляется для маркеров параметров в инструкции. *params* — это обязательный параметр, который вызывает для входного значения **ntext**, **nchar** или **nvarchar** . Если инструкция не параметризована, необходимо ввести значение NULL.  
  
 *stmt*  
 Определяет результирующий набор курсора. Параметр *stmt* является обязательным и вызывает для входного значения **ntext**, **nchar** или **nvarchar** .  
  
 *bound_param*  
 Означает необязательное использование дополнительных параметров. *bound_param* вызывает входное значение любого типа данных для обозначения дополнительных используемых параметров.  
  
## <a name="examples"></a>Примеры  
 В следующем примере выполняется подготовка и выполнение простого оператора:  
  
```  
Declare @Out int;  
EXEC sp_prepexec @Out output,   
    N'@P1 nvarchar(128), @P2 nvarchar(100)',  
    N'SELECT database_id, name  
      FROM sys.databases  
      WHERE name=@P1 AND state_desc = @P2',   
          @P1 = 'tempdb', @P2 = 'ONLINE';   
EXEC sp_unprepare @Out;  
```  
  
## <a name="see-also"></a>См. также раздел  
 [sp_prepare &#40;Transact SQL&#41;](../../relational-databases/system-stored-procedures/sp-prepare-transact-sql.md)   
 [sp_execute &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-execute-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  

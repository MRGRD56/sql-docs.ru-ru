---
description: sp_helpextendedproc (Transact-SQL)
title: sp_helpextendedproc (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpextendedproc
- sp_helpextendedproc_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpextendedproc
ms.assetid: 7e1f017e-c898-4225-b375-6a73ef9aac7b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e8256d22c8565b56b328119e6b5618ca4f8d226b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197614"
---
# <a name="sp_helpextendedproc-transact-sql"></a>sp_helpextendedproc (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Сообщает сведения о расширенных хранимых процедурах, определенных в настоящий момент, и имя динамически подключаемой библиотеки (DLL), которой принадлежит эта процедура (функция).  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Используйте вместо этого [интеграцию со средой CLR](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md) .  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_helpextendedproc [ [@funcname = ] 'procedure' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @funcname = ] 'procedure'` Имя расширенной хранимой процедуры, для которой сообщается информация. Аргумент *PROCEDURE* имеет тип **sysname** и значение по умолчанию NULL.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Имя расширенной хранимой процедуры.|  
|**компоновки**|**nvarchar(255)**|Имя DLL.|  
  
## <a name="remarks"></a>Замечания  
 Если указана *процедура* , **sp_helpextendedproc** отчеты по указанной расширенной хранимой процедуре. Если этот параметр не указан, **sp_helpextendedproc** возвращает все имена расширенных хранимых процедур и имена библиотек DLL, к которым принадлежит каждая расширенная хранимая процедура.  
  
## <a name="permissions"></a>Разрешения  
 Разрешение на выполнение **sp_helpextendedproc** предоставляется **общедоступному**.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-reporting-help-on-all-extended-stored-procedures"></a>A. Справочные сведения обо всех расширенных хранимых процедурах  
 В следующем примере сообщается обо всех расширенных хранимых процедурах.  
  
```  
USE master;  
GO  
EXEC sp_helpextendedproc;  
GO  
```  
  
### <a name="b-reporting-help-on-a-single-extended-stored-procedure"></a>Б. Справочные сведения об одной расширенной хранимой процедуре  
 Следующий пример сообщает о `xp_cmdshell` расширенной хранимой процедуре.  
  
```  
USE master;  
GO  
EXEC sp_helpextendedproc xp_cmdshell;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [sp_addextendedproc &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addextendedproc-transact-sql.md)   
 [sp_dropextendedproc &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropextendedproc-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

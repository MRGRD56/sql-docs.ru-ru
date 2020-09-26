---
description: '&#x40;&#x40;ROWCOUNT (Transact-SQL)'
title: '@@ROWCOUNT (Transact-SQL) | Документы Майкрософт'
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- '@@ROWCOUNT_TSQL'
- '@@ROWCOUNT'
dev_langs:
- TSQL
helpviewer_keywords:
- '@@ROWCOUNT function'
- number of rows affected by statement
- row affected by statements [SQL Server]
- statements [SQL Server], last statement
- counting rows
ms.assetid: 97a47998-81d9-4331-a244-9eb8b6fe4a56
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: e0a85b458990042e524b8e8382e75ce7c4cf27b1
ms.sourcegitcommit: 197a6ffb643f93592edf9e90b04810a18be61133
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2020
ms.locfileid: "91379969"
---
# <a name="x40x40rowcount-transact-sql"></a>&#x40;&#x40;ROWCOUNT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает число строк, затронутых при выполнении последней инструкции. Если число строк превышает 2 миллиарда, используйте инструкцию [ROWCOUNT_BIG](../../t-sql/functions/rowcount-big-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
@@ROWCOUNT  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 **int**  
  
## <a name="remarks"></a>Remarks  
 Инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] могут устанавливать значение в @@ROWCOUNT указанными ниже способами.  
  
-   Установка значения @@ROWCOUNT в число считанных или измененных строк. Строки могут быть отосланы или не отосланы клиенту.  
  
-   Сохранение значения @@ROWCOUNT из предыдущего выполнения инструкции.  
  
-   Сброс значения @@ROWCOUNT в 0 без возврата значения клиенту.  
  
 Инструкции, которые выполняют простые присваивания, всегда устанавливают значение @@ROWCOUNT равным 1. Строки не отправляются клиенту. Примерами таких инструкций являются: SET @*локальная_переменная*, RETURN, READTEXT и инструкции SELECT без запроса, такие как SELECT GETDATE() или SELECT **'***Обычный текст***'** .  
  
 Инструкции, которые осуществляют присвоение в запросе или используют RETURN, устанавливают значение функции @@ROWCOUNT в число строк, задействованных или считанных запросом, например SELECT @*локальная_переменная* = c1 FROM t1.  
  
 Инструкции языка обработки данных DML задают значение @@ROWCOUNT равным числу строк, задействованных в запросе, и возвращают это значение клиенту. DML-инструкции могут не отправлять строки клиенту.  
  
 Инструкции DECLARE CURSOR и FETCH задают значение @@ROWCOUNT равным 1.  
  
 Инструкции EXECUTE сохраняют предыдущее значение @@ROWCOUNT.  
  
 Такие инструкции, как USE, SET \<option>, DEALLOCATE CURSOR, CLOSE CURSOR, PRINT, RAISERROR, BEGIN TRANSACTION или COMMIT TRANSACTION, сбрасывают значение ROWCOUNT в 0.  
  
 Скомпилированные в собственном коде хранимые процедуры сохраняют предыдущее значение @@ROWCOUNT. Инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)], находящиеся внутри скомпилированных в собственном коде хранимых процедур, не устанавливают значение @@ROWCOUNT. Дополнительные сведения см. в статье [Хранимые процедуры, скомпилированные в собственном коде](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures.md).  
  
## <a name="examples"></a>Примеры  
 Следующий пример выполняет инструкцию `UPDATE` и использует `@@ROWCOUNT` для определения того, были ли изменены строки.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE HumanResources.Employee   
SET JobTitle = N'Executive'  
WHERE NationalIDNumber = 123456789  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were updated';  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Системные функции (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [SET ROWCOUNT (Transact-SQL)](../../t-sql/statements/set-rowcount-transact-sql.md)  
  
  

---
description: TRIGGER_NESTLEVEL (Transact-SQL)
title: TRIGGER_NESTLEVEL (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- TRIGGER_NESTLEVEL
- TRIGGER_NESTLEVEL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- triggers [SQL Server], number executed
- number of triggers
- TRIGGER_NESTLEVEL function
ms.assetid: 6a33e74a-0cf9-4ae1-a1e4-4a137a3ea39d
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 86554070c90b2c946e79359b561da324abbf700a
ms.sourcegitcommit: 197a6ffb643f93592edf9e90b04810a18be61133
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2020
ms.locfileid: "91379539"
---
# <a name="trigger_nestlevel-transact-sql"></a>TRIGGER_NESTLEVEL (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает число обработчиков, выполненных при срабатывании триггера инструкции. Функция TRIGGER_NESTLEVEL используется в триггерах DML и DDL для определения текущего уровня вложенности.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
TRIGGER_NESTLEVEL ( [ object_id ] , [ 'trigger_type' ] , [ 'trigger_event_category' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *object_id*  
 Идентификатор объекта триггера. Если указан аргумент *object_id*, возвращается количество запусков этого триггера для данной инструкции. Если аргумент *object_id* не указан, возвращается число срабатываний всех триггеров для данной инструкции.  
  
 **'** *trigger_type* **'**  
 Указывает, к каким триггерам применяется функция TRIGGER_NESTLEVEL — AFTER или INSTEAD OF. Для триггеров AFTER следует указать **AFTER**. Для триггеров INSTEAD OF укажите **IOT**. Если указан аргумент *trigger_type*, необходимо также указать *trigger_event_category*.  
  
 **'** *trigger_event_category* **'**  
 Указывает, к каким триггерам применять функцию TRIGGER_NESTLEVEL — DML или DDL. Для триггеров DML следует указать **DML**, для триггеров DDL — **DDL**. Если указан аргумент *trigger_event_category*, необходимо также указать *trigger_type*. При использовании значения **DDL** значение предыдущего аргумента может быть только **AFTER**, так как триггеры DDL всегда являются триггерами AFTER.  
  
## <a name="remarks"></a>Комментарии  
 Если не задано никаких аргументов, функция TRIGGER_NESTLEVEL возвращает общее число триггеров в стеке вызова. Этот список также включает и саму функцию TRIGGER_NESTLEVEL. Пропуск аргументов может наблюдаться в случае, когда триггер выполняет команды, приводящие к запуску другого триггера, или создает последовательность запускаемых триггеров.  
  
 Для возвращения общего числа триггеров в стеке вызовов для конкретного типа триггеров и категории событий следует указать аргумент *object_id* = 0.  
  
 Функция TRIGGER_NESTLEVEL возвращает 0 в случае выполнения вне триггера, если значение хотя бы одного из аргументов не равно NULL.  
  
 Если каким-либо аргументам явно задано значение NULL, то оно возвращается независимо от того, внутри или вне триггера использовалась функция TRIGGER_NESTLEVEL.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-testing-the-nesting-level-of-a-specific-dml-trigger"></a>A. Проверка уровня вложенности конкретного триггера DML  
  
```sql
IF ( (SELECT TRIGGER_NESTLEVEL( OBJECT_ID('xyz') , 'AFTER' , 'DML' ) ) > 5 )  
   RAISERROR('Trigger xyz nested more than 5 levels.',16,-1)  
```  
  
### <a name="b-testing-the-nesting-level-of-a-specific-ddl-trigger"></a>Б. Проверка уровня вложенности конкретного триггера DDL  
  
```sql
IF ( ( SELECT TRIGGER_NESTLEVEL ( ( SELECT object_id FROM sys.triggers  
WHERE name = 'abc' ), 'AFTER' , 'DDL' ) ) > 5 )  
   RAISERROR ('Trigger abc nested more than 5 levels.',16,-1)  
```  
  
### <a name="c-testing-the-nesting-level-of-all-triggers-executed"></a>В. Проверка уровня вложенности всех сработавших триггеров  
  
```sql
IF ( (SELECT trigger_nestlevel() ) > 5 )  
   RAISERROR  
      ('This statement nested over 5 levels of triggers.',16,-1)  
```  
  
## <a name="see-also"></a>См. также  
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)  
  
  

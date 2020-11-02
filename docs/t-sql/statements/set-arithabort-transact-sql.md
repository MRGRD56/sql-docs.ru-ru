---
description: SET ARITHABORT (Transact-SQL)
title: SET ARITHABORT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 12/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ARITHABORT_TSQL
- ARITHABORT
- SET ARITHABORT
- SET_ARITHABORT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- terminating queries
- queries [SQL Server], terminating
- overflow errors [SQL Server]
- ARITHABORT option
- divide-by-zero errors
- SET ARITHABORT statement
- ending queries [SQL Server]
- stopping queries
ms.assetid: f938a666-fdd1-4233-b97f-719f27b1a0e6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 14ff75ebf7d8498ed35d5e2a0e5ef55cec14831c
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2020
ms.locfileid: "92679068"
---
# <a name="set-arithabort-transact-sql"></a>SET ARITHABORT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Завершает запрос, если во время выполнения запроса возникает ошибка переполнения или деления на ноль.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database
  
SET ARITHABORT { ON | OFF }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse

SET ARITHABORT ON
```
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks
В сеансах входа в систему всегда необходимо устанавливать для параметра ARITHABORT значение ON. Если для параметра ARITHABORT установлено значение OFF, это может отрицательно повлиять на оптимизацию запросов и привести к проблемам с производительностью.  
  
> [!WARNING]  
>  Настройка по умолчанию ARITHABORT для [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] — ON. Клиентские приложения, в которых для параметра ARITHABORT установлено значение OFF, могут получать разные планы запроса, что осложняет диагностику плохо выполняемых запросов. Иными словами, один и тот же запрос может выполняться быстро в среде Management Studio, но медленно в приложении. При диагностике запросов с [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] всегда сопоставляйте параметр ARITHABORT клиента.  
  
Когда для параметров SET ARITHABORT и SET ANSI WARNINGS установлено значение ON, эти условия ошибок приведут к завершению запроса.  
  
Когда для параметра SET ARITHABORT установлено значение ON, а для параметра SET ANSI WARNINGS — значение OFF, эти условия ошибок приведут к завершению пакета. Если при исполнении транзакции произошли ошибки, то для этой транзакции выполняется откат. Когда для параметра SET ARITHABORT установлено значение OFF и возникает одна из этих ошибок, то появляется предупреждающее сообщение, а результату арифметической операции присваивается значение NULL.  
  
Если параметры SET ARITHABORT и SET ANSI WARNINGS имеют значение OFF и возникает одна из этих ошибок, появится предупреждающее сообщение, а результат арифметической операции примет значение NULL.  
  
> [!NOTE]  
>  Если ни для одного из параметров SET ARITHABORT и SET ARITHIGNORE не установлено значение ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает значение NULL и появляется предупреждающее сообщение после выполнения запроса.  
  
Если ANSI_WARNINGS имеет значение ON, а уровень совместимости базы данных установлен как 90 или выше, ARITHABORT неявно настраивается как ON вне зависимости от значения параметра. Если уровень совместимости базы данных установлен в состояние 80 или более раннее, то параметр ARITHABORT необходимо явным образом установить в состояние ON.  
  
Если для вычисления выражения для параметра SET ARITHABORT установлено значение OFF, а в инструкциях INSERT, UPDATE или DELETE происходит арифметическая ошибка, ошибка переполнения, деления на ноль или ошибка домена, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] вставляет или обновляет значение NULL. Если целевой столбец не пустой, вставка или обновление не осуществляются и пользователь получает ошибку.  
  
Когда для одного из параметров SET ARITHABORT или SET ARITHIGNORE установлено значение OFF, а для параметра SET ANSI_WARNINGS — значение ON, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает сообщение об ошибке при обнаружении ошибок деления на ноль или переполнения.  
  
Когда для параметра SET ARITHABORT установлено значение OFF и возникают аварийные ошибки во время вычисления логических условий в инструкции IF, то исполняется ветвь FALSE.
  
Значение параметра SET ARITHABORT должно быть ON, если создаются или изменяются индексы на вычисляемых столбцах или индексированных представлениях. Если для параметра SET ARITHABORT установлено значение OFF, то действия инструкций CREATE, UPDATE, INSERT и DELETE в таблицах с индексами на вычисляемых столбцах или на индексированных представлениях завершаются ошибкой.
  
Установка параметра SET ARITHABORT происходит во время выполнения, но не во время синтаксического анализа.  
  
Чтобы просмотреть текущее значение параметра SET ARITHABORT, выполните следующий запрос:
  
```sql  
DECLARE @ARITHABORT VARCHAR(3) = 'OFF';  
IF ( (64 & @@OPTIONS) = 64 ) SET @ARITHABORT = 'ON';  
SELECT @ARITHABORT AS ARITHABORT;  
  
```  
  
## <a name="permissions"></a>Разрешения  
Необходимо быть членом роли **public** .  
  
## <a name="examples"></a>Примеры  
Следующий пример демонстрирует ошибки деления на ноль и переполнения при настройках параметра `SET ARITHABORT`.  
  
```sql  
-- SET ARITHABORT  
-------------------------------------------------------------------------------  
-- Create tables t1 and t2 and insert data values.  
CREATE TABLE t1 (  
   a TINYINT,   
   b TINYINT  
);  
CREATE TABLE t2 (  
   a TINYINT  
);  
GO  
INSERT INTO t1   
VALUES (1, 0);  
INSERT INTO t1   
VALUES (255, 1);  
GO  
  
PRINT '**_ SET ARITHABORT ON';  
GO  
-- SET ARITHABORT ON and testing.  
SET ARITHABORT ON;  
GO  
  
PRINT '_*_ Testing divide by zero during SELECT';  
GO  
SELECT a / b AS ab   
FROM t1;  
GO  
  
PRINT '_*_ Testing divide by zero during INSERT';  
GO  
INSERT INTO t2  
SELECT a / b AS ab    
FROM t1;  
GO  
  
PRINT '_*_ Testing tinyint overflow';  
GO  
INSERT INTO t2  
SELECT a + b AS ab   
FROM t1;  
GO  
  
PRINT '_*_ Resulting data - should be no data';  
GO  
SELECT _   
FROM t2;  
GO  
  
-- Truncate table t2.  
TRUNCATE TABLE t2;  
GO  
  
-- SET ARITHABORT OFF and testing.  
PRINT '*** SET ARITHABORT OFF';  
GO  
SET ARITHABORT OFF;  
GO  
  
-- This works properly.  
PRINT '**_ Testing divide by zero during SELECT';  
GO  
SELECT a / b AS ab    
FROM t1;  
GO  
  
-- This works as if SET ARITHABORT was ON.  
PRINT '_*_ Testing divide by zero during INSERT';  
GO  
INSERT INTO t2  
SELECT a / b AS ab    
FROM t1;  
GO  
PRINT '_*_ Testing tinyint overflow';  
GO  
INSERT INTO t2  
SELECT a + b AS ab   
FROM t1;  
GO  
  
PRINT '_*_ Resulting data - should be 0 rows';  
GO  
SELECT _   
FROM t2;  
GO  
  
-- Drop tables t1 and t2.  
DROP TABLE t1;  
DROP TABLE t2;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Инструкции SET (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET ARITHIGNORE (Transact-SQL)](../../t-sql/statements/set-arithignore-transact-sql.md)   
 [SESSIONPROPERTY (Transact-SQL)](../../t-sql/functions/sessionproperty-transact-sql.md)  
  
  

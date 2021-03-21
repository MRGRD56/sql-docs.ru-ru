---
description: sp_prepare (Transact SQL)
title: sp_prepare (Transact SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 02/28/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_cursor_prepare_TSQL
- sp_cursor_prepare
dev_langs:
- TSQL
helpviewer_keywords:
- sp_prepare
ms.assetid: f328c9eb-8211-4863-bafa-347e1bf7bb3f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 017009d5dd3d22c8bba1eed67f5a8ebe01d87ed7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754984"
---
# <a name="sp_prepare-transact-sql"></a>sp_prepare (Transact SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

Подготавливает параметризованную [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкцию и возвращает *маркер* инструкции для выполнения.  `sp_prepare`Для вызова хранимой процедуры  необходимо указать ID = 11 в пакете потока табличных данных (TDS).  
  
 ![Значок ссылки на статью](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
sp_prepare handle OUTPUT, params, stmt, options  
```  
  
## <a name="arguments"></a>Аргументы  
 *справиться*  
 — Это созданный SQL Server идентификатор *подготовленного обработчика* . *Handle* является обязательным параметром с возвращаемым значением **int** .  
  
 *params*  
 Указывает параметризованные инструкции. Определение переменных *params* подставляется для маркеров параметров в инструкции. *params* — это обязательный параметр, который вызывает для входного значения **ntext**, **nchar** или **nvarchar** . Если инструкция не параметризована, необходимо ввести значение NULL.  
  
 *stmt*  
 Определяет результирующий набор курсора. Параметр *stmt* является обязательным и вызывает для входного значения **ntext**, **nchar** или **nvarchar** .  
  
 *options*  
 Необязательный параметр, возвращающий описание столбцов результирующего набора курсора. для *параметров* требуется следующее входное значение int:  
  
|Значение|Описание|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
## <a name="examples"></a>Примеры  
A. В следующем примере подготавливается и выполняется простая инструкция.  
  
```sql  
DECLARE @P1 INT;  
EXEC sp_prepare @P1 OUTPUT,   
    N'@P1 NVARCHAR(128), @P2 NVARCHAR(100)',  
    N'SELECT database_id, name FROM sys.databases WHERE name=@P1 AND state_desc = @P2';  
EXEC sp_execute @P1, N'tempdb', N'ONLINE';  
EXEC sp_unprepare @P1;  
```

Б. В следующем примере подготавливается инструкция в базе данных AdventureWorks2016, а позднее она выполняется с помощью маркера.

```sql
-- Prepare query
DECLARE @P1 INT;  
EXEC sp_prepare @P1 OUTPUT,   
    N'@Param INT',  
    N'SELECT *
FROM Sales.SalesOrderDetail AS sod
INNER JOIN Production.Product AS p ON sod.ProductID = p.ProductID
WHERE SalesOrderID = @Param
ORDER BY Style DESC;';  

-- Return handle for calling application
SELECT @P1;
GO
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```
-----------
1

(1 row affected)
```

Затем приложение выполняет запрос дважды, используя значение маркера 1, перед отменой подготовленного плана.

```sql
EXEC sp_execute 1, 49879;  
GO

EXEC sp_execute 1, 48766;
GO

EXEC sp_unprepare 1; 
GO
```
  
## <a name="see-also"></a>См. также:  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  


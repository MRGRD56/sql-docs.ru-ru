---
title: int, bigint, smallint и tinyint (Transact-SQL) | Документы Майкрософт
description: Справочник по Transact-SQL для типов данных int, bigint, smallint и tinyint. Эти типы данных используются для представления целочисленных данных.
ms.date: 09/08/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- bigint_TSQL
- smallint
- bigint
- smallint_TSQL
- tinyint_TSQL
- int_TSQL
- int
- tinyint
dev_langs:
- TSQL
helpviewer_keywords:
- exact numeric data [SQL Server]
- numeric data
- tinyint data type
- int data type
- smallint data type
ms.assetid: 9bda5b0b-2380-4931-a1c8-f362fdefa99b
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4ccaf291bbb51fa23f6bd697929e5fe5e373630c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737054"
---
# <a name="int-bigint-smallint-and-tinyint-transact-sql"></a>int, bigint, smallint и tinyint (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Типы точных числовых данных, использующие целые значения. Для экономии места в базе данных используйте тип данных наименьшего размера, который гарантирует возможность хранения всех возможных значений. Например, типа tinyint достаточно для хранения возраста людей, так как он не может превышать 255 лет. Однако типа tinyint будет недостаточно для возраста зданий, так как они могут быть старше 255 лет.
  
|Тип данных|Диапазон|Память|  
|---|---|---|
|**bigint**|от -2^63 (-9 223 372 036 854 775 808) до 2^63-1 (9 223 372 036 854 775 807)|8 байт|  
|**int**|от -2^31 (-2 147 483 648) до 2^31-1 (2 147 483 647)|4 байта|  
|**smallint**|от -2^15 (-32 768) до 2^15-1 (32 767)|2 байта|  
|**tinyint**|От 0 до 255|1 байт|  
  
## <a name="remarks"></a>Remarks  
Тип данных **int** является основным типом целочисленных данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Тип данных **bigint** используется для хранения значений, выходящих за диапазон, поддерживаемый типом данных **int**.
  
В таблице приоритетов типов данных тип **bigint** располагается между **smallmoney** и **int**.
  
Функции возвращают **bigint** только в случае, если выражение параметра имеет тип **bigint**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не выполняет автоматического продвижения других целочисленных типов данных (**tinyint**, **smallint** и **int**) до **bigint**.
  
> [!CAUTION]  
>  При использовании таких арифметических операторов, как +, –, \*, / или %, для явного или неявного преобразования констант типа **int**, **smallint**, **tinyint** или **bigint** в значения типа **float**, **real**, **decimal** или **numeric** в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используются различные правила определения типов данных и точности результата, зависящие от наличия автоматической параметризации запроса.  
>   
>  Поэтому одинаковые выражения в различных запросах могут иногда возвращать различные результаты. В случае отсутствия в запросе автоматической параметризации константа сначала преобразуется в значение типа **numeric**, точности которого хватает для ее хранения, а затем происходит преобразование в заданный тип данных. Например, константа 1 преобразуется в **numeric (1, 0)** , а константа 250 — в **numeric (3, 0)** .  
>   
>  При наличии в запросе автоматической параметризации константа всегда сначала преобразуется в значение типа **numeric (10, 0)** , а затем в данные конечного типа. При использовании оператора «/» могут различаться как точность, так и само значение результата. Например, результат автопараметризованного запроса, включающего в себя выражение `SELECT CAST (1.0 / 7 AS float)`, отличается от аналогичного запроса без автоматической параметризации, так как результаты выполнения автопараметризованного запроса усекаются до значений, соответствующих типу данных **numeric (10, 0)** .  
  
## <a name="converting-integer-data"></a>Преобразование целочисленных данных
При неявном преобразовании данных типа integer в данные типа character, если число слишком большое для символьного поля, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] вставляет символ с кодом ASCII 42 — звездочку (*).
  
Целочисленные константы, превышающие 2 147 483 647, преобразуются в тип данных **decimal**, а не в **bigint**. В приведенном ниже примере демонстрируется изменение типа данных результата с **int** на **decimal** при превышении порогового значения.
  
```sql
SELECT 2147483647 / 2 AS Result1, 2147483649 / 2 AS Result2 ;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
Result1      Result2  
1073741823   1073741824.500000  
```  
  
## <a name="examples"></a>Примеры  
В приведенном ниже примере создается таблица, в которой используются типы данных **bigint**, **int**, **smallint** и **tinyint**. Значения вставляются в каждый столбец и возвращаются в инструкции SELECT.
  
```sql
CREATE TABLE dbo.MyTable  
(  
  MyBigIntColumn BIGINT  
,MyIntColumn  INT
,MySmallIntColumn SMALLINT
,MyTinyIntColumn TINYINT
);  
  
GO  
  
INSERT INTO dbo.MyTable VALUES (9223372036854775807, 2147483647,32767,255);  
 GO  
SELECT MyBigIntColumn, MyIntColumn, MySmallIntColumn, MyTinyIntColumn  
FROM dbo.MyTable;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
MyBigIntColumn       MyIntColumn MySmallIntColumn MyTinyIntColumn  
-------------------- ----------- ---------------- ---------------  
9223372036854775807  2147483647  32767            255  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также раздел
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[sys.types (Transact-SQL)](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
  

---
description: SWITCHOFFSET (Transact-SQL)
title: SWITCHOFFSET (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 12/02/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SWITCHTZ
- SWITCHTZ_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dates [SQL Server], functions
- functions [SQL Server], time
- functions [SQL Server], date and time
- SWITCHOFFSET function [SQL Server]
- time [SQL Server], functions
- date and time [SQL Server], SWITCHOFFSET
- time zones [SQL Server]
ms.assetid: 32a48e36-0aa4-4260-9fe9-cae9197d16c5
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8ba376927810521381a3c1a838e7438cdd7acdac
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622895"
---
# <a name="switchoffset-transact-sql"></a>SWITCHOFFSET (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает значение смещения часового пояса с типом данных **datetimeoffset**, изменившееся с хранящегося на новое заданное смещение часового пояса.  
  
 Обзор всех типов данных и функций даты и времени в языке [!INCLUDE[tsql](../../includes/tsql-md.md)] см. в статье [Типы данных и функции даты и времени &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
SWITCHOFFSET ( datetimeoffset_expression, timezoneoffset_expression )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *datetimeoffset_expression*  
 Выражение, которое можно привести к значению типа **datetimeoffset(n)**.  
  
 *timezoneoffset_expression*  
 Выражение в формате [+|-]TZH:TZM или целочисленное значение со знаком (в минутах), представляющее смещение часового пояса. Предполагается, что оно настроено и учитывает переход на летнее время.  
  
## <a name="return-type"></a>Тип возвращаемых данных  
 Значение **datetimeoffset** с точностью в долях секунд, заданной в аргументе *datetimeoffset_expression*.  
  
## <a name="remarks"></a>Комментарии  
 SWITCHOFFSET используется для выбора значения **datetimeoffset** в смещении часового пояса, отличающегося от первоначально сохраненного смещения часового пояса. SWITCHOFFSET не обновляет хранимое значение *time_zone*.  
  
 Функция SWITCHOFFSET может использоваться для обновления столбца **datetimeoffset**.  
  
 Использование SWITCHOFFSET с функцией GETDATE() может привести к тому, что запрос будет выполняться медленно. Это происходит потому, что оптимизатор запросов не может получить точные оценки количества элементов для значений даты и времени. Чтобы устранить эту проблему, используйте указание запроса OPTION (RECOMPILE), чтобы заставить оптимизатор запросов перекомпилировать план запроса при следующем выполнении этого же запроса. Он будет иметь точные оценки количества элементов и сформирует более эффективный план запроса. Дополнительные сведения об указании запроса RECOMPILE см. в статье [Указания запросов (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md).  
  
```sql
DECLARE @dt datetimeoffset = switchoffset (CONVERT(datetimeoffset, GETDATE()), '-04:00');   
SELECT * FROM t    
WHERE c1 > @dt OPTION (RECOMPILE);  
```  
  
## <a name="examples"></a>Примеры  
 В следующем примере функция `SWITCHOFFSET` используется для вывода смещения часового пояса, отличающегося от значения, которое хранится в базе данных.  
  
```sql  
CREATE TABLE dbo.test   
    (  
    ColDatetimeoffset datetimeoffset  
    );  
GO  
INSERT INTO dbo.test   
VALUES ('1998-09-20 7:45:50.71345 -5:00');  
GO  
SELECT SWITCHOFFSET (ColDatetimeoffset, '-08:00')   
FROM dbo.test;  
GO  
--Returns: 1998-09-20 04:45:50.7134500 -08:00  
SELECT ColDatetimeoffset  
FROM dbo.test;  
--Returns: 1998-09-20 07:45:50.7134500 -05:00  
```  
  
## <a name="see-also"></a>См. также  
 [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [AT TIME ZONE (Transact-SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)  
  
  



---
description: DATETIMEFROMPARTS (Transact-SQL)
title: DATETIMEFROMPARTS (Transact-SQL)
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATETIMEFROMPARTS_TSQL
- DATETIMEFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATETIMEFROMPARTS function
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8cc7cb964f1cd8b56cbb06aa5bccc97aef76a7b4
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237913"
---
# <a name="datetimefromparts-transact-sql"></a>DATETIMEFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция возвращает значение **datetime** для указанных аргументов даты и времени.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DATETIMEFROMPARTS ( year, month, day, hour, minute, seconds, milliseconds )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*year*  
Целочисленное выражение, задающее год.
  
*month*  
Целочисленное выражение, задающее месяц.
  
*day*  
Целочисленное выражение, задающее день.
  
*hour*  
Целочисленное выражение, задающее часы.
  
*minute*  
Целочисленное выражение, задающее минуты.
  
*секунд*  
Целочисленное выражение, задающее секунды.
  
*milliseconds*  
Целочисленное выражение, задающее миллисекунды.
  
## <a name="return-types"></a>Типы возвращаемых данных
**datetime**
  
## <a name="remarks"></a>Remarks  
Функция `DATETIMEFROMPARTS` возвращает полностью инициализированное значение типа **datetime**. Функция `DATETIMEFROMPARTS` вызывает ошибку, если по крайней мере один обязательный аргумент имеет недопустимое значение. Функция `DATETIMEFROMPARTS` возвращает NULL, если по крайней мере один обязательный аргумент имеет значение NULL.
  
Для серверов [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)] и выше данная функция может быть удаленной. Она не может быть удаленной для серверов с версией ниже [!INCLUDE[sssql11-md](../../includes/sssql11-md.md)].  
  
## <a name="examples"></a>Примеры  
  
```sql
SELECT DATETIMEFROMPARTS ( 2010, 12, 31, 23, 59, 59, 0 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
---------------------------  
2010-12-31 23:59:59.000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также
[datetime (Transact-SQL)](../../t-sql/data-types/datetime-transact-sql.md)
  
  


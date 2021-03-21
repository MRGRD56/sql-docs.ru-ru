---
description: ATN2 (Transact-SQL)
title: ATN2 (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ATN2
- ATN2_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- arctangent
- tangent
- ATN2 function
ms.assetid: 014b291e-7cd7-4c39-b20d-5db3a9f0505d
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 055ae0039a97bc5fe617685a3373428e6f56afb6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104740475"
---
# <a name="atn2-transact-sql"></a>ATN2 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Возвращает угол в радианах между положительным направлением оси X и лучом, проведенным из начала координат в точку (y, x), где x и y — значения двух указанных выражений с плавающей запятой.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ATN2 ( float_expression , float_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*float_expression*  
[Выражение](../../t-sql/language-elements/expressions-transact-sql.md) типа данных **float**.
  
## <a name="return-types"></a>Типы возвращаемых данных
**float**
  
## <a name="examples"></a>Примеры  
В следующем примере `ATN2` рассчитывается для указанных компонентов `x` и `y`.
  
```sql
DECLARE @x FLOAT = 35.175643, @y FLOAT = 129.44;  
SELECT 'The ATN2 of the angle is: ' + CONVERT(VARCHAR, ATN2(@y, @x));  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
The ATN2 of the angle is: 1.30545                         
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также раздел
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Типы данных float и real (Transact-SQL)](../../t-sql/data-types/float-and-real-transact-sql.md)  
[Математические функции (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)
  
  


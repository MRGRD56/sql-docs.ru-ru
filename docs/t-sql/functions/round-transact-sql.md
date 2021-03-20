---
description: ROUND (Transact-SQL)
title: ROUND (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 12/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ROUND_TSQL
- ROUND
dev_langs:
- TSQL
helpviewer_keywords:
- rounding expressions
- ROUND function [Transact-SQL]
ms.assetid: 23921ed6-dd6a-4c9e-8c32-91c0d44fe4b7
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: efcf7dbbfebaf00bcbba94b0b28cae0cdfcc700a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104734563"
---
# <a name="round-transact-sql"></a>ROUND (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Возвращает числовое значение, округленное до указанной длины или точности.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ROUND ( numeric_expression , length [ ,function ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *numeric_expression*  
 [Выражение](../../t-sql/language-elements/expressions-transact-sql.md) категории точного числового или приблизительного числового типа данных, за исключением типа данных **bit**.  
  
 *length*  
 Точность, с которой должно быть округлено значение *numeric_expression*. Аргумент *length* должен быть выражением типа **tinyint**, **smallint** или **int**. Если аргумент *length* является положительным числом, значение *numeric_expression* округляется до числа десятичных разрядов, указанных в аргументе *length*. Если аргумент *length* является отрицательным числом, значение *numeric_expression* округляется слева от десятичной запятой, как указано в аргументе *length*.  
  
 *function*  
 Тип выполняемой операции. Аргумент *function* должен иметь тип **tinyint**, **smallint** или **int**. Если аргумент *function* не указан или имеет значение 0 (по умолчанию), значение *numeric_expression* округляется. Когда указывается значение, не равное 0, значение *numeric_expression* усекается.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Возвращает следующие типы данных.  
  
|Результат выражения|Возвращаемый тип|  
|-----------------------|-----------------|  
|**tinyint**|**int**|  
|**smallint**|**int**|  
|**int**|**int**|  
|**bigint**|**bigint**|  
|Категория **decimal** и **numeric** (p, s)|**decimal(p, s)**|  
|Категории **money** и **smallmoney**|**money**|  
|Категории **float** и **real**|**float**|  
  
## <a name="remarks"></a>Remarks  
 Функция ROUND всегда возвращает значение. Если аргумент *length* имеет отрицательное значение и больше числа знаков перед десятичной запятой, ROUND возвращает 0.  
  
|Пример|Результат|  
|-------------|------------|  
|ROUND(748,58, -4)|0|  
  
 Функция ROUND возвращает округленное значение выражения *numeric_expression* независимо от типа данных, когда *length* является отрицательным числом.  
  
|Примеры|Результат|  
|--------------|------------|  
|ROUND(748,58, -1)|750,00|  
|ROUND(748,58, -2)|700,00|  
|ROUND(748.58, -3)|В результате возникает арифметическое переполнение, так как для значения 748,58 по умолчанию используется тип decimal (5,2), который не позволяет вернуть значение 1000.|  
|Чтобы округлить результат до четырех цифр, измените тип данных на входе. Пример:<br /><br /> `SELECT ROUND(CAST (748.58 AS decimal (6,2)),-3);`|1000.00|  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-round-and-estimates"></a>A. Использование функции ROUND и приближений  
 Следующий пример показывает два выражения, которые демонстрируют, используя `ROUND`, что последний знак всегда является приближением.  
  
```sql  
SELECT ROUND(123.9994, 3), ROUND(123.9995, 3);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------- -----------  
123.9990    124.0000      
```  
  
### <a name="b-using-round-and-rounding-approximations"></a>Б. Использование функции ROUND и округляющих аппроксимаций  
 В следующем примере показаны округление и аппроксимация.  
  
```  
SELECT ROUND(123.4545, 2), ROUND(123.45, -2);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

 ```
----------  ----------
123.4500    100.00
```
  
### <a name="c-using-round-to-truncate"></a>В. Использование функции ROUND для усечения  
 В следующем примере используются две инструкции `SELECT` для демонстрации различия между округлением и усечением. Первая инструкция округляет результат. Вторая инструкция усекает результат.  
  
```sql  
SELECT ROUND(150.75, 0);  
GO  
SELECT ROUND(150.75, 0, 1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------  
151.00  
  
(1 row(s) affected)  
  
--------  
150.00  
  
(1 row(s) affected)  
```
  
## <a name="see-also"></a>См. также:  
 [CEILING (Transact-SQL)](../../t-sql/functions/ceiling-transact-sql.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [FLOOR (Transact-SQL)](../../t-sql/functions/floor-transact-sql.md)   
 [Математические функции (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)

---
description: ATAN (Transact-SQL)
title: ATAN (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ATAN_TSQL
- ATAN
dev_langs:
- TSQL
helpviewer_keywords:
- arctangent
- ATAN function
- tangent
ms.assetid: 6d3dd28e-4fa6-40ba-94cf-b33c0ff614ec
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fbbc2cb7d9774cc5da44af311cd436072fbbd679
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96119316"
---
# <a name="atan-transact-sql"></a>ATAN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Функция, возвращающая угол в радианах, тангенс которого задан выражением **float**. Эта функция арктангенсом.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
ATAN ( float_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*float_expression*  
[Выражение](../../t-sql/language-elements/expressions-transact-sql.md) типа **float** или типа, который может быть неявно преобразован в тип **float**.
  
## <a name="return-types"></a>Типы возвращаемых данных
**float**
  
## <a name="examples"></a>Примеры  
В этом примере принимается выражение **float** и возвращается ATAN указанного угла.
  
```sql
SELECT 'The ATAN of -45.01 is: ' + CONVERT(varchar, ATAN(-45.01))  
SELECT 'The ATAN of -181.01 is: ' + CONVERT(varchar, ATAN(-181.01))  
SELECT 'The ATAN of 0 is: ' + CONVERT(varchar, ATAN(0))  
SELECT 'The ATAN of 0.1472738 is: ' + CONVERT(varchar, ATAN(0.1472738))  
SELECT 'The ATAN of 197.1099392 is: ' + CONVERT(varchar, ATAN(197.1099392))  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
  
-------------------------------   
The ATAN of -45.01 is: -1.54858                         
  
(1 row(s) affected)  
  
--------------------------------   
The ATAN of -181.01 is: -1.56527                         
  
(1 row(s) affected)  
  
--------------------------------   
The ATAN of 0 is: 0                                
  
(1 row(s) affected)  
  
----------------------------------   
The ATAN of 0.1472738 is: 0.146223                         
  
(1 row(s) affected)  
  
-----------------------------------   
The ATAN of 197.1099392 is: 1.56572                          
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
В этом примере принимается выражение **float** и возвращается арктангенс указанного угла.
  
```sql
SELECT ATAN(45.87) AS atanCalc1,  
    ATAN(-181.01) AS atanCalc2,  
    ATAN(0) AS atanCalc3,  
    ATAN(0.1472738) AS atanCalc4,  
    ATAN(197.1099392) AS atanCalc5;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
atanCalc1  atanCalc2  atanCalc3  atanCalc4  atanCalc5
---------  ---------  ---------  ---------  ---------
1.55       -1.57       0.00       0.15       1.57
```
  
## <a name="see-also"></a>См. также
[CEILING (Transact-SQL)](../../t-sql/functions/ceiling-transact-sql.md)  
[Математические функции (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)
  
  


---
description: TIMEFROMPARTS (Transact-SQL)
title: TIMEFROMPARTS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- TIMEFROMPARTS_TSQL
- TIMEFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- TIMEFROMPARTS function
ms.assetid: 786c65a1-2b3f-4e4b-82b6-4940d62f3801
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 889cdc554a27da1ba5db64fa3ee67bc1b51f2dad
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748227"
---
# <a name="timefromparts-transact-sql"></a>TIMEFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает значение **time**, соответствующее указанному времени с заданной точностью.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
TIMEFROMPARTS ( hour, minute, seconds, fractions, precision )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *hour*  
 Целочисленное выражение, задающее часы.  
  
 *minute*  
 Целочисленное выражение, задающее минуты.  
  
 *секунд*  
 Целочисленное выражение, задающее секунды.  
  
 *fractions*  
 Целочисленное выражение, задающее доли секунд.  
  
 *precision*  
 Целочисленное литеральное значение, определяющее точность возвращаемого значения **time**.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **time(** *precision* **)**  
  
## <a name="remarks"></a>Комментарии  
 TIMEROMPARTS возвращает полностью инициализированное значение времени. Если аргументы недопустимы, то возникает ошибка. Если любой из параметров имеет значение NULL, возвращается NULL. Однако если аргумент *precision* равен NULL, то возникает ошибка.  
  
 Аргумент *fractions* зависит от аргумента *precision*. Например, если значение *precision* равно 7, то каждая дробная часть представляет 100 наносекунд; если значение *precision* равно 3, то каждая дробная часть представляет миллисекунду. Если значение *precision* равно нулю, то значение *fractions* также должно быть равно нулю, иначе возникает ошибка.  
  
 Данная функция может быть удаленной для серверов [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий. Она не может быть удаленной для серверов с версией ниже [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-simple-example-without-fractions-of-a-second"></a>A. Простой пример без долей секунд  
  
```sql
SELECT TIMEFROMPARTS ( 23, 59, 59, 0, 0 ) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
--------------------  
23:59:59.0000000  
  
(1 row(s) affected)  
```  
  
### <a name="b-example-with-fractions-of-a-second"></a>Б. Пример с долями секунд  
 В приведенном ниже примере показано использование параметров *fractions* и *precision*.  
  
1.  Если параметр *fractions* имеет значение 5, а параметр *precision* — значение 1, то значение параметра *fractions* представляет 5/10 секунды.  
  
2.  Если параметр *fractions* имеет значение 50, а параметр *precision* — значение 2, то значение параметра *fractions* представляет 50/100 секунды.  
  
3.  Если параметр *fractions* имеет значение 500, а параметр *precision* — значение 3, то значение параметра *fractions* представляет 500/1000 секунды.  
  
```sql  
SELECT TIMEFROMPARTS ( 14, 23, 44, 5, 1 );  
SELECT TIMEFROMPARTS ( 14, 23, 44, 50, 2 );  
SELECT TIMEFROMPARTS ( 14, 23, 44, 500, 3 );  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------------  
14:23:44.5  
  
(1 row(s) affected)  
  
----------------  
14:23:44.50  
  
(1 row(s) affected)  
  
----------------  
14:23:44.500  
  
(1 row(s) affected)  
```  
  


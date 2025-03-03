---
description: DATEFROMPARTS (Transact-SQL)
title: DATEFROMPARTS (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATEFROMPARTS_TSQL
- DATEFROMPARTS
dev_langs:
- TSQL
helpviewer_keywords:
- DATEFROMPARTS function
ms.assetid: 5b885376-87aa-41f1-9e18-04987aead250
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 345cdd42525fb4227989320f16d28a6ca79e1255
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735549"
---
# <a name="datefromparts-transact-sql"></a>DATEFROMPARTS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Эта функция возвращает значение типа **date**, соответствующее указанному числу, месяцу и году.
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DATEFROMPARTS ( year, month, day )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*year*  
Целочисленное выражение, задающее год.
  
*month*  
Целочисленное выражение, задающее месяц (от 1 до 12).
  
*day*  
Целочисленное выражение, задающее день.
  
## <a name="return-types"></a>Типы возвращаемых данных
**date**
  
## <a name="remarks"></a>Remarks  
Функция `DATEFROMPARTS` возвращает значение **date**, в котором для компонента даты заданы указанные число, месяц и год, а компонент времени установлен в значение по умолчанию. Если аргументы имеют недопустимые значения, функция `DATEFROMPARTS` вызывает ошибку. Функция `DATEFROMPARTS` возвращает NULL, если по крайней мере один обязательный аргумент имеет значение NULL.
  
Эта функция поддерживает удаленное взаимодействие с серверами [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий. Она не поддерживает удаленное взаимодействие с серверами версий ниже [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].
  
## <a name="examples"></a>Примеры  
В приведенном ниже примере показано, как работает функция `DATEFROMPARTS`.
  
```sql
SELECT DATEFROMPARTS ( 2010, 12, 31 ) AS Result;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Result  
----------------------------------  
2010-12-31  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также раздел
[date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md)
  
  


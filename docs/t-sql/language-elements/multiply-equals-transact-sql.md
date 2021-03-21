---
description: '*= (присваивание умножения) (Transact-SQL)'
title: '*= (присваивание умножения) (Transact-SQL) | Документы Майкрософт'
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '*=_TSQL'
- '*='
dev_langs:
- TSQL
helpviewer_keywords:
- compound operators, *=
- assignment operators, *=
- augmented operators, *=
- '*= (multiply equals)'
- '*= (multiplication assignment)'
ms.assetid: 816ff5dc-9a40-4c07-8351-39c194dbc079
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 86bf2968ac374ab5143faefb2704cb0d136586f7
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748854"
---
# <a name="-multiplication-assignment-transact-sql"></a>*= (присваивание умножения) (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Умножает два числа и присваивает значению результат этой операции. Например, если переменная @x равна 35, то @x *= 2 принимает исходное значение @x, умножает его на 2 и присваивает переменной @x новое значение (70).  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
expression *= expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
_expression_  
Любой допустимый тип данных [expression](../../t-sql/language-elements/expressions-transact-sql.md), кроме типа **bit**, в числовой категории.  
  
## <a name="result-types"></a>Типы результата  
Возвращает результат типа данных аргумента с более высоким приоритетом. Дополнительные сведения см. в разделе [Приоритет типов данных (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
## <a name="remarks"></a>Комментарии  
Дополнительные сведения см. в статье [&#42; (умножение) (Transact-SQL)](../../t-sql/language-elements/multiply-transact-sql.md).  
  
## <a name="see-also"></a>См. также  
[Составные операторы (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)   
[Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
[Операторы (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)  
  
  

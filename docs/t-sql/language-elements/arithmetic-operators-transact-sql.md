---
description: Арифметические операторы (Transact-SQL)
title: Арифметические операторы (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], arithmetic
- arithmetic operators
- math operations [Transact-SQL]
ms.assetid: a41b92a5-1061-4e4d-bb3b-a180b73c88fa
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0bde4e3f5c12dcce82df668aeb01ba0653cebcc4
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751734"
---
# <a name="arithmetic-operators-transact-sql"></a>Арифметические операторы (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Арифметические операторы выполняют математические операции над двумя выражениями одного или нескольких типов данных. Они запускаются из категории числовых типов данных. Дополнительные сведения о категориях типов данных см. в статье [Соглашения о синтаксисе в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
|Оператор|Значение|  
|--------------|-------------|  
|[+ (сложение)](../../t-sql/language-elements/add-transact-sql.md)|Сложение|  
|[- (вычитание)](../../t-sql/language-elements/subtract-transact-sql.md)|Вычитание|  
|[* (умножение)](../../t-sql/language-elements/multiply-transact-sql.md)|Умножение|  
|[/ (Деление)](../../t-sql/language-elements/divide-transact-sql.md)|Отдел|  
|[% (остаток от деления)](../../t-sql/language-elements/modulo-transact-sql.md)|Возвращает целочисленный остаток при делении. Например, 12 % 5 = 2, поскольку остаток от деления 12 на 5 равен 2.|  
  
Операторы (+) и (–) можно также использовать для выполнения арифметических операций со значениями типов **datetime** и **smalldatetime**.  
  
Дополнительные сведения о точности и масштабе результатов арифметических операций см. в статье [Точность, масштаб и длина (Transact-SQL)](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
## <a name="see-also"></a>См. также  
[Математические функции (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)   
[Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
[Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  

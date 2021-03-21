---
description: STR (Transact-SQL)
title: STR (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STR
- STR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- converting numbers to characters
- characters [SQL Server], converting
- character data [SQL Server]
- STR function
ms.assetid: de03531b-d9e7-4c3c-9604-14e582ac20c6
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1151dd72f4f1ccf12ed38d39143a79d4180fc70a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753424"
---
# <a name="str-transact-sql"></a>STR (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает символьные данные, преобразованные из числовых данных. Символьные данные выровнены по правому краю с заданной длиной и десятичной точностью. 
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
STR ( float_expression [ , length [ , decimal ] ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *float_expression*  
 Выражение приблизительного числового (**float**) типа данных с десятичной запятой.  
  
 *length*  
 Общая длина. Она включает десятичную запятую, знак, цифры и пробелы. Значение по умолчанию равно 10.  
  
 *decimal*  
 Количество знаков справа от десятичной запятой. Значение *decimal* должно быть меньше или равно 16. Если значение *decimal* больше 16, то результат усекается до шестнадцати знаков после запятой.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **varchar**  
  
## <a name="remarks"></a>Remarks  
 Если указано, то значения параметров *length* и *decimal* в функции STR должны быть положительными. Число округляется до целого или по умолчанию, или если десятичный параметр равен 0. Указанная длина должна быть больше или равна части числа до запятой плюс знак числа (если имеется). Короткое выражение *float_expression* выравнивается справа до указанной длины, а длинное *float_expression* усекается до заданного количества цифр после запятой. Например, значением выражения STR(12,10) будет 12. Выравнивается в результирующем наборе вправо. Однако STR(1223, 2) усекает результирующий набор до \*\*. Строковые функции могут быть вложенными.  
  
> [!NOTE]  
>  Чтобы преобразовать данные в кодировку Юникод, используйте функцию STR внутри функций преобразования CONVERT или [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
 В следующем примере выражение, состоящее из пяти знаков и десятичной запятой, преобразуется в шестизначную символьную строку. Дробная часть числа округляется до одного десятичного знака.  
  
```sql
SELECT STR(123.45, 6, 1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------  
 123.5  
  
(1 row(s) affected)  
```  
  
 Когда выражение превышает заданную длину, строка возвращает `**` для указанной длины.  
  
```sql
SELECT STR(123.45, 2, 2);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--  
**  
  
(1 row(s) affected)  
```  
  
 Даже когда в `STR` вложены числовые данные, результатом являются символьные данные с указанным форматом.  
  
```sql
SELECT STR (FLOOR (123.45), 8, 3);
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------  
 123.000  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также:  
 [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
 [FORMAT &#40;Transact-SQL&#41;](../../t-sql/functions/format-transact-sql.md)  
 [Строковые функции (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
  
  


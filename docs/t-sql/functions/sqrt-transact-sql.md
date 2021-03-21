---
description: SQRT (Transact-SQL)
title: SQRT (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQRT
- SQRT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SQRT function
- square root values
ms.assetid: 26e244e8-e82d-4664-a445-1226230ee1c5
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4f7792a971cc732015d83e6c04fc2cd85826e347
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748624"
---
# <a name="sqrt-transact-sql"></a>SQRT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает квадратный корень данного числа с плавающей точкой.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
SQRT ( float_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *float_expression*  
 [Выражение](../../t-sql/language-elements/expressions-transact-sql.md) типа **float** или типа, который может быть неявно преобразован в тип float.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **float**  
  
## <a name="examples"></a>Примеры  
 Следующий пример возвращает квадратный корень чисел от `1.00` до `10.00`.  
  
```sql  
DECLARE @myvalue FLOAT;  
SET @myvalue = 1.00;  
WHILE @myvalue < 10.00  
   BEGIN  
      SELECT SQRT(@myvalue);  
      SET @myvalue = @myvalue + 1  
   END;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------------------------   
1.0                        
------------------------   
1.4142135623731            
------------------------   
1.73205080756888           
------------------------   
2.0                        
------------------------   
2.23606797749979           
------------------------   
2.44948974278318           
------------------------   
2.64575131106459           
------------------------   
2.82842712474619           
------------------------   
3.0  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 В приведенном ниже примере возвращается квадратный корень чисел `1.00` и `10.00`.  
  
```sql  
SELECT SQRT(1.00), SQRT(10.00);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
----------  ------------  
1.00        3.16
```  
  
## <a name="see-also"></a>См. также:  
 [Математические функции (Transact-SQL)](../../t-sql/functions/mathematical-functions-transact-sql.md)  
  
  


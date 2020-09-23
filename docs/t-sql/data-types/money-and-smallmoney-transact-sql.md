---
description: Типы money и smallmoney (Transact-SQL)
title: Типы money и smallmoney (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- money_TSQL
- money
- smallmoney
- smallmoney_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- money data type, about money data type
- money data type
- smallmoney data type
- values [SQL Server], monetary
- currency [SQL Server]
ms.assetid: 57861137-89ea-4b89-b361-390597d7bccc
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8218bf8b98cd072dd60f8458c75819761153b634
ms.sourcegitcommit: cc23d8646041336d119b74bf239a6ac305ff3d31
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2020
ms.locfileid: "91111285"
---
# <a name="money-and-smallmoney-transact-sql"></a>Типы money и smallmoney (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Типы данных, представляющие денежные (валютные) значения.
  
## <a name="remarks"></a>Remarks  
  
|Тип данных|Диапазон|Память|  
|---|---|---|
|**money**|От –922,337,203,685,477.5808 до 922,337,203,685,477.5807 (от –922,337,203,685,477.58<br />до 922,337,203,685,477.58 в Informatica.  В Informatica поддерживается только два десятичных знака, а не четыре)|8 байт|  
|**smallmoney**|От -214 748,3648 до 214 748,3647|4 байта|  
  
Типы данных **money** и **smallmoney** имеют точность до одной десятитысячной денежной единицы, которую они представляют. В Informatica типы данных **money** и **smallmoney** имеют точность до одной сотой денежной единицы, которую они представляют.
  
Чтобы отделить дробные денежные единицы, например центы, от целых денежных единиц, используйте запятую. Например, 2,15 соответствует 2 долларам и 15 центам.
  
Для этих типов данных может использоваться любой из следующих символов валют.
  
![Таблица символов валют, шестнадцатеричные значения](../../t-sql/data-types/media/money01.gif "Таблица символов валют, шестнадцатеричные значения")
  
Валютные или денежные данные не требуется заключать в одинарные кавычки ( ' ). Важно помнить, что, несмотря на возможность указания денежных значений, которым предшествует символ валюты, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не сохраняет какие-либо сведения о валюте, связанные с символом, а хранит только числовое значение.
  
## <a name="converting-money-data"></a>Преобразование данных типа money
При преобразовании типа данных integer в тип **money** используются денежные единицы. Например, целочисленное значение 4 преобразуется в значение типа данных **money** величиной 4 денежные единицы.
  
В следующем примере выполняется преобразование значений типов **smallmoney** и **money** в значения типов **varchar** и **decimal** соответственно.
  
```sql
DECLARE @mymoney_sm SMALLMONEY = 3148.29,  
        @mymoney    MONEY = 3148.29;  
SELECT  CAST(@mymoney_sm AS VARCHAR) AS 'SM_MONEY varchar',  
        CAST(@mymoney AS DECIMAL)    AS 'MONEY DECIMAL';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
SM_MONEY VARCHAR               MONEY DECIMAL  
------------------------------ ----------------------  
3148.29                        3148    
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также раздел
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)
[CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
[CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)
[Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)
[DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
[SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)
[sys.types (Transact-SQL)](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
  

---
description: binary и varbinary (Transact-SQL)
title: binary и varbinary (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- binary_TSQL
- varbinary_TSQL
- binary
- varbinary
dev_langs:
- TSQL
helpviewer_keywords:
- varbinary data type
- binary [SQL Server], about binary data type
ms.assetid: bcce65f9-10db-4b3e-bfaf-dfc06c6f820f
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 49c20ae59ec25668725b3fa763eb4f81304d0962
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104737114"
---
# <a name="binary-and-varbinary-transact-sql"></a>binary и varbinary (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Типы двоичных данных фиксированной или переменной длины.
  
## <a name="arguments"></a>Аргументы

**binary** [ ( _n_ ) ] Двоичные данные фиксированной длины размером в _n_ байт, где _n_ — значение от 1 до 8000. Размер при хранении составляет _n_ байт.
  
**varbinary** [ ( _n_ | **max**) ] Двоичные данные с переменной длиной. _n_ может иметь значение от 1 до 8000. Значение **max** указывает, что максимальный размер при хранении составляет 2^31-1 байт. Размер хранения — это фактическая длина введенных данных плюс 2 байта. Введенные данные могут иметь размер 0 символов. В ANSI SQL синонимом для **varbinary** является **binary varying**.
  
## <a name="remarks"></a>Примечания  
Если значение _n_ в определении данных или инструкции объявления переменной не указано, длина по умолчанию равна 1. Когда _n_ не указано функцией CAST, длина по умолчанию равна 30.

| Тип данных | Применение |
| --- | --- |
| **binary** | Размер данных в столбце одинаков.|
| **varbinary** | Данные в столбце значительно различаются по размеру.|
| **varbinary(max)** | Длина элементов данных в столбце превышает 8000 байт.|


## <a name="converting-binary-and-varbinary-data"></a>Преобразование типов данных binary и varbinary
При преобразовании данных из строкового типа данных в тип данных **binary** или **varbinary** разной длины, тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] дополняет или усекает данные справа. Строковые типы данных:

* **char** 
* **varchar**
* **nchar**
* **nvarchar**
* **binary**
* **varbinary**
* **text**
* **ntext**
* **image**

При преобразовании других типов данных в тип **binary** или **varbinary** данные дополняются или усекаются слева. Дополнение осуществляется шестнадцатеричными нулями.
  
Если для обмена данными лучше всего подходит тип **binary**, то другие типы данных удобнее всего будет преобразовать в **binary** и **varbinary**. В определенный момент вы можете преобразовать тип значения в двоичное значение достаточно большого размера, а затем преобразовать его обратно. Это преобразование всегда возвращает одно и то же значение, если оба преобразования выполняются с использованием одной и той же версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Двоичное представление значения может меняться в зависимости от версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
Вы можете преобразовать тип данных **int**, **smallint** и **tinyint** в **binary** или **varbinary**. При преобразовании значения типа **binary** обратно в целочисленное это значение будет отличаться от исходного в случае усечения. Например, в следующей инструкции SELECT показано, что целочисленное значение `123456` хранится как двоичное `0x0001e240`:
  
```sql
SELECT CAST( 123456 AS BINARY(4) );  
```  
  
Однако в следующей инструкции `SELECT` показано, что если целевой тип **binary** слишком мал для хранения всего значения, то начальные цифры неявно усекаются и то же самое число хранится как `0xe240`:
  
```sql
SELECT CAST( 123456 AS BINARY(2) );  
```  
  
В следующем пакете показано, что это необъявленное усечение может повлиять на арифметические операции без возникновения ошибки.
  
```sql
DECLARE @BinaryVariable2 BINARY(2);  
  
SET @BinaryVariable2 = 123456;  
SET @BinaryVariable2 = @BinaryVariable2 + 1;  
  
SELECT CAST( @BinaryVariable2 AS INT);  
GO  
```  
  
Окончательный результат — `57921`, но не `123457`.
  
> [!NOTE]  
>  Преобразование любого типа данных в **binary** может различаться в зависимости от версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>См. также раздел
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Преобразование типов данных (ядро СУБД)](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)
  
  

---
description: SQL_VARIANT_PROPERTY (Transact-SQL)
title: SQL_VARIANT_PROPERTY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 02/25/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SQL_VARIANT_PROPERTY_TSQL
- SQL_VARIANT_PROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- SQL_VARIANT_PROPERTY function
- sql_variant data type
ms.assetid: 50e5c1d9-4e95-4ed0-9c92-435c872a399e
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 24134065588787a367b7a975c36b3be5631483cd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750514"
---
# <a name="sql_variant_property-transact-sql"></a>SQL_VARIANT_PROPERTY (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает базовый тип данных и другие сведения о значении **sql_variant**.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
SQL_VARIANT_PROPERTY ( expression , property )  
```  
  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *expression*  
 Выражение типа **sql_variant**.  
  
 *property*  
 Содержит имя свойства **sql_variant**, для которого будут предоставлены сведения. Аргумент *property* имеет тип **varchar(** 128 **)** и может принимать одно из перечисленных ниже значений.  
  
|Значение|Описание|Возвращаемый базовый тип sql_variant|  
|-----------|-----------------|----------------------------------------|  
|**BaseType**|Тип данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], например:<br /><br /> **bigint**<br /><br /> **binary**<br /><br /> **bit**<br /><br /> **char**<br /><br /> **date**<br /><br /> **datetime**<br /><br /> **datetime2**<br /><br /> **datetimeoffset**<br /><br /> **decimal**<br /><br /> **float**<br /><br /> **int**<br /><br /> **money**<br /><br /> **nchar**<br /><br /> **numeric**<br /><br /> **nvarchar**<br /><br /> **real**<br /><br /> **smalldatetime**<br /><br /> **smallint**<br /><br /> **smallmoney**<br /><br /> **time**<br /><br /> **tinyint**<br /><br /> **uniqueidentifier**<br /><br /> **varbinary**<br /><br /> **varchar**|**sysname**<br /><br /> NULL = Введенные значения недопустимы.|  
|**Точность**|Количество знаков числового базового типа данных:<br /><br /> **date** = 10<br /><br /> **datetime** = 23<br /><br /> **datetime2** = 27<br /><br /> **datetime2** (s) = 19 when s = 0, else s + 20<br /><br /> **datetimeoffset** = 34<br /><br /> **datetimeoffset** (s) = 26 when s = 0, else s + 27<br /><br /> **smalldatetime** = 16<br /><br /> **time** = 16<br /><br /> **time** (s) = 8 when s = 0, else s + 9<br /><br /> **float** = 53<br /><br /> **real** = 24<br /><br /> **decimal** и **numeric** = 18<br /><br /> **decimal** (p,s) и **numeric** (p,s) = p<br /><br /> **money** = 19<br /><br /> **smallmoney** = 10<br /><br /> **bigint** = 19<br /><br /> **int** = 10<br /><br /> **smallint** = 5<br /><br /> **tinyint** = 3<br /><br /> **bit** = 1<br /><br /> Все остальные типы = 0|**int**<br /><br /> NULL = Введенные значения недопустимы.|  
|**Масштабирование**|Количество знаков справа от десятичного разделителя числового базового типа данных:<br /><br /> **decimal** и **numeric** = 0<br /><br /> **decimal** (p,s) и **numeric** (p,s) = s<br /><br /> **money** и **smallmoney** = 4<br /><br /> **datetime** = 3<br /><br /> **datetime2** = 7<br /><br /> **datetime2** (s) = s (0 - 7)<br /><br /> **datetimeoffset** = 7<br /><br /> **datetimeoffset** (s) = s (0 - 7)<br /><br /> **time** = 7<br /><br /> **time** (s) = s (0 - 7)<br /><br /> все остальные типы = 0|**int**<br /><br /> NULL = Введенные значения недопустимы.|  
|**TotalBytes**|Число байтов, необходимое для хранения данных и метаданных значения. Эта информация может быть полезной при проверке максимального размера данных в столбце **sql_variant**. Если значение превышает 900, создание индекса завершается сбоем.|**int**<br /><br /> NULL = Введенные значения недопустимы.|  
|**Параметры сортировки**|Представляет параметры сортировки конкретного значения **sql_variant**.|**sysname**<br /><br /> NULL = Введенные значения недопустимы.|  
|**MaxLength**|Максимальная длина типа данных, в байтах. Например, **MaxLength** **nvarchar(** 50 **)** равно 100, а **MaxLength** **int** равно 4.|**int**<br /><br /> NULL = Введенные значения недопустимы.|  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **sql_variant**  
  
## <a name="examples"></a>Примеры  
### <a name="a-using-a-sql_variant-in-a-table"></a>A. Использование sql_variant в таблице  
 В приведенном ниже примере извлекаются сведения `SQL_VARIANT_PROPERTY` о значении `colA``46279.1`, где `colB` =`1689` при условии, что `tableA` имеет `colA` типа `sql_variant` и `colB`.  
  
```sql    
CREATE   TABLE tableA(colA sql_variant, colB int)  
INSERT INTO tableA values ( cast (46279.1 as decimal(8,2)), 1689)  
SELECT   SQL_VARIANT_PROPERTY(colA,'BaseType') AS 'Base Type',  
         SQL_VARIANT_PROPERTY(colA,'Precision') AS 'Precision',  
         SQL_VARIANT_PROPERTY(colA,'Scale') AS 'Scale'  
FROM      tableA  
WHERE      colB = 1689  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Обратите внимание на то, что каждое из этих трех значений имеет тип **sql_variant**.  
  
```  
Base Type    Precision    Scale  
---------    ---------    -----  
decimal      8           2  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-a-sql_variant-as-a-variable"></a>Б. Использование типа sql_variant в качестве переменной   
 В приведенном ниже примере происходит получение сведений `SQL_VARIANT_PROPERTY` о переменной с именем @v1.  
  
```sql    
DECLARE @v1 sql_variant;  
SET @v1 = 'ABC';  
SELECT @v1;  
SELECT SQL_VARIANT_PROPERTY(@v1, 'BaseType');  
SELECT SQL_VARIANT_PROPERTY(@v1, 'MaxLength');  
```  
  
## <a name="see-also"></a>См. также:  
 [sql_variant (Transact-SQL)](../../t-sql/data-types/sql-variant-transact-sql.md)  
  
  


---
description: NEWID (Transact-SQL)
title: NEWID (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: synapse-analytics, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NEWID
- NEWID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- uniqueidentifier data type
- NEWID function
ms.assetid: f7014e60-96d5-457e-afc3-72b60ba20c0f
author: julieMSFT
ms.author: jrasnick
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a79dada161166bbc7975176fe3a727f17455a2b0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104734950"
---
# <a name="newid-transact-sql"></a>NEWID (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

Создает уникальное значение типа **uniqueidentifier**.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql 
NEWID ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Типы возвращаемых данных
 **uniqueidentifier**  
  
## <a name="remarks"></a>Комментарии  
 `NEWID()` соответствует стандарту RFC4122.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-the-newid-function-with-a-variable"></a>A. Использование функции NEWID с переменной  
 В приведенном ниже примере функция `NEWID()` используется для присвоения значения переменной с типом данных **uniqueidentifier**. Значение переменной типа данных **uniqueidentifier** выводится перед его проверкой.  
  
```sql
-- Creating a local variable with DECLARE/SET syntax.  
DECLARE @myid uniqueidentifier  
SET @myid = NEWID()  
PRINT 'Value of @myid is: '+ CONVERT(varchar(255), @myid)  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Value of @myid is: 6F9619FF-8B86-D011-B42D-00C04FC964FF  
```  
  
> [!NOTE]  
>  Значение, полученное с помощью функции NEWID, на разных компьютерах различается. Это число приведено только для иллюстрации.  
  
### <a name="b-using-newid-in-a-create-table-statement"></a>Б. Использование функции NEWID в инструкции CREATE TABLE  
  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
  
 В приведенном ниже примере создается таблица `cust` с типом данных **uniqueidentifier** и заполняется значением по умолчанию с помощью функции NEWID. При присвоении значения функции `NEWID()` по умолчанию каждая новая и уже существующая строка будет иметь уникальное значение для столбца `CustomerID`.  
  
```sql
-- Creating a table using NEWID for uniqueidentifier data type.  
CREATE TABLE cust  
(  
 CustomerID uniqueidentifier NOT NULL  
   DEFAULT newid(),  
 Company VARCHAR(30) NOT NULL,  
 ContactName VARCHAR(60) NOT NULL,   
 Address VARCHAR(30) NOT NULL,   
 City VARCHAR(30) NOT NULL,  
 StateProvince VARCHAR(10) NULL,  
 PostalCode VARCHAR(10) NOT NULL,   
 CountryRegion VARCHAR(20) NOT NULL,   
 Telephone VARCHAR(15) NOT NULL,  
 Fax VARCHAR(15) NULL  
);  
GO  
-- Inserting 5 rows into cust table.  
INSERT cust  
(Company, ContactName, Address, City, StateProvince,   
 PostalCode, CountryRegion, Telephone, Fax)  
VALUES  
 ('Wartian Herkku', 'Pirkko Koskitalo', 'Torikatu 38', 'Oulu', NULL,  
 '90110', 'Finland', '981-443655', '981-443655')  
,('Wellington Importadora', 'Paula Parente', 'Rua do Mercado, 12', 'Resende', 'SP',  
 '08737-363', 'Brasil', '(14) 555-8122', '')  
,('Cactus Comidas para Ilevar', 'Patricio Simpson', 'Cerrito 333', 'Buenos Aires', NULL,   
 '1010', 'Argentina', '(1) 135-5555', '(1) 135-4892')  
,('Ernst Handel', 'Roland Mendel', 'Kirchgasse 6', 'Graz', NULL,  
 '8010', 'Austria', '7675-3425', '7675-3426')  
,('Maison Dewey', 'Catherine Dewey', 'Rue Joseph-Bens 532', 'Bruxelles', NULL,  
 'B-1180', 'Belgium', '(02) 201 24 67', '(02) 201 24 68');  
GO
```  
  
### <a name="c-using-uniqueidentifier-and-variable-assignment"></a>В. Использование типа uniqueidentifier и присвоение значения переменной  
 В приведенном ниже примере объявляется локальная переменная типа данных **uniqueidentifier** с именем `@myid`. Затем переменной присваивается значение с помощью оператора `SET`.  
  
```sql  
DECLARE @myid uniqueidentifier ;  
SET @myid = 'A972C577-DFB0-064E-1189-0154C99310DAAC12';  
SELECT @myid;  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [NEWSEQUENTIALID (Transact-SQL)](../../t-sql/functions/newsequentialid-transact-sql.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [Системные функции (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [uniqueidentifier (Transact-SQL)](../../t-sql/data-types/uniqueidentifier-transact-sql.md)   
 [Порядковые номера](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  

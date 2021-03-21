---
description: sp_columns (Transact-SQL)
title: sp_columns (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 10/17/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_columns_TSQL
- sp_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sp_columns
ms.assetid: 2dec79cf-2baf-4c0f-8cbb-afb1a8654e1e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7ef2d20ec911fc361ebbb7162853ee2e2cfe748c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750244"
---
# <a name="sp_columns-transact-sql"></a>sp_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает сведения о столбце для указанных объектов, которые могут быть запрошены в текущей среде.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_columns [ @table_name = ] object  
     [ , [ @table_owner = ] owner ]   
     [ , [ @table_qualifier = ] qualifier ]   
     [ , [ @column_name = ] column ]   
     [ , [ @ODBCVer = ] ODBCVer ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ \@table_name = ] object` Имя объекта, который используется для возврата сведений о каталоге. *объект* может быть таблицей, представлением или другим объектом, имеющим такие столбцы, как функции с табличным значением. *объект* имеет тип **nvarchar (384)** и не имеет значения по умолчанию. Поиск совпадений по шаблону поддерживается.  
  
`[ \@table_owner = ] owner` Владелец объекта, который используется для возврата сведений о каталоге. *owner* имеет тип **nvarchar (384)** и значение по умолчанию NULL. Поиск совпадений по шаблону поддерживается. Если параметр *owner* не указан, применяются правила видимости объектов по умолчанию базовой СУБД.  
  
 Если текущий пользователь является владельцем объекта с указанным именем, то возвращаются столбцы этого объекта. Если *владелец* не указан и текущий пользователь не владеет объектом с указанным *объектом*, **sp_columns** ищет объект с указанным *объектом* , принадлежащим владельцу базы данных. Если таковой существует, возвращаются столбцы этого объекта.  
  
`[ \@table_qualifier = ] qualifier` Имя квалификатора объекта. *квалификатор* имеет тип **sysname** и значение по умолчанию NULL. Различные продукты СУБД поддерживают имена объектов (_Квалификаторы_**,** состоящие из трех частей). _владелец_**.** _имя_). В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] этот столбец представляет имя базы данных. В некоторых продуктах он представляет имя сервера в среде базы данных объекта.  
  
`[ \@column_name = ] column` Является одним столбцом и используется, когда требуется только один столбец сведений о каталоге. *столбец* имеет тип **nvarchar (384)** и значение по умолчанию NULL. Если *столбец* не указан, возвращаются все столбцы. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] *столбце столбец* представляет имя столбца, указанное в таблице **syscolumns** . Поиск совпадений по шаблону поддерживается. Для максимальной совместимости клиент шлюза должен использовать только стандартное согласование SQL-92 (символы-шаблоны % и _).  
  
`[ \@ODBCVer = ] ODBCVer` Используемая версия ODBC. *Одбквер* имеет **тип int** и значение по умолчанию 2. Это значение соответствует ODBC версии 2. Допустимы значения 2 или 3. Различия в поведении версий 2 и 3 см. в спецификации ODBC **SQLColumns** .  
  
## <a name="return-code-values"></a>Значения кода возврата  
 Нет  
  
## <a name="result-sets"></a>Результирующие наборы  
 Хранимая процедура каталога **sp_columns** эквивалентна **SQLColumns** в ODBC. Возвращаемые результаты упорядочиваются по **TABLE_QUALIFIER**, **table_owner** и **table_name**.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|Имя квалификатора объекта. Это поле может иметь значение NULL.|  
|**TABLE_OWNER**|**sysname**|Имя владельца объекта. Это поле всегда возвращает значение.|  
|**TABLE_NAME**|**sysname**|Имя объекта. Это поле всегда возвращает значение.|  
|**COLUMN_NAME**|**sysname**|Имя столбца для каждого столбца возвращаемого **table_name** . Это поле всегда возвращает значение.|  
|**DATA_TYPE**|**smallint**|Целочисленный код типа данных ODBC. Если этот тип данных не может быть сопоставлен с типом данных ODBC, возвращается значение NULL. Имя собственного типа данных возвращается в столбец **TYPE_NAME** .|  
|**TYPE_NAME**|**sysname**|Тип данных в символьном представлении. Название типа предоставляется базовой СУБД.|  
|**PRECISION**|**int**|Количество значащих цифр. Возвращаемое значение для столбца **точности** находится в базовом 10.|  
|**LENGTH**|**int**|Размер передаваемых данных. <sup>1</sup>|  
|**Измените**|**smallint**|Число цифр справа от десятичной запятой.|  
|**RADIX**|**smallint**|Основание системы счисления числовых типов данных.|  
|**ОБНУЛЯЕМОГО**|**smallint**|Указывает возможность содержать значение NULL.<br /><br /> 1 = значение NULL допустимо.<br /><br /> 0 = значение NULL недопустимо.|  
|**ЗАМЕЧАНИЯ**|**varchar (254)**|Это поле всегда возвращает значение NULL.|  
|**COLUMN_DEF**|**nvarchar(4000)**|Значение столбца по умолчанию.|  
|**SQL_DATA_TYPE**|**smallint**|Значение типа данных SQL в том же виде, что и в поле TYPE дескриптора. Этот столбец аналогичен столбцу **data_type** , за исключением типов данных **DateTime** и SQL-92 **Interval** . Этот столбец всегда возвращает значение.|  
|**SQL_DATETIME_SUB**|**smallint**|Код подтипа для типов данных **DateTime** и SQL-92 **Interval** . Для других типов данных этот столбец возвращает значение NULL.|  
|**CHAR_OCTET_LENGTH**|**int**|Максимальная длина столбца символьного или целочисленного типа в байтах. Для всех других типов данных этот столбец возвращает значение NULL.|  
|**ORDINAL_POSITION**|**int**|Порядковый номер столбца в объекте. Первый столбец в объекте имеет порядковый номер 1. Этот столбец всегда возвращает значение.|  
|**IS_NULLABLE**|**varchar (254)**|Допустимость значений NULL для столбца объекта. Допустимость значений NULL определяется в соответствии с правилами ISO. СУБД, совместимая с ISO SQL, не может вернуть пустую строку.<br /><br /> YES = Столбец может содержать значение NULL.<br /><br /> NO = Столбец не может содержать значения NULL.<br /><br /> Если допустимость значения NULL неизвестна, то этот столбец возвращает строку нулевой длины.<br /><br /> Значение, возвращаемое для этого столбца, отличается от значения, возвращаемого для столбца, **допускающего значение NULL** .|  
|**SS_DATA_TYPE**|**tinyint**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] тип данных используется для расширенных хранимых процедур. Дополнительные сведения см. в разделе [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md).|  
  
 <sup>1</sup> дополнительные сведения см. в документации по Microsoft ODBC.  
  
## <a name="permissions"></a>Разрешения  
 Требуются разрешения SELECT и VIEW DEFINITION на схему.  
  
## <a name="remarks"></a>Remarks  
 **sp_columns** соответствует требованиям для идентификаторов с разделителями. Дополнительные сведения см. в разделе [Идентификаторы баз данных](../../relational-databases/databases/database-identifiers.md).  
  
## <a name="examples"></a>Примеры  
 Следующий пример возвращает данные столбца для указанной таблицы.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_columns @table_name = N'Department',  
   @table_owner = N'HumanResources';  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Следующий пример возвращает данные столбца для указанной таблицы.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_columns @table_name = N'DimEmployee',  
   @table_owner = N'dbo';  
```  
  
## <a name="see-also"></a>См. также:  
 [sp_tables &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-tables-transact-sql.md)   
 [Хранимые процедуры каталога &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  



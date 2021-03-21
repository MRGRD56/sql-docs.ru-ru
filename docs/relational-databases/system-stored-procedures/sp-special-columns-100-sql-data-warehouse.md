---
description: sp_special_columns_100 (Azure синапсе Analytics)
title: sp_special_columns_100 (Azure синапсе Analytics)
ms.custom: ''
ms.date: 03/14/2017
ms.prod_service: synapse-analytics, pdw
ms.service: sql-data-warehouse
ms.subservice: design
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5774fadc-77cc-46f8-8f9f-a0f9efe95e21
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 45385004023168f40ea47ce9dfe88d14ba4e9fb3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745004"
---
# <a name="sp_special_columns_100-azure-synapse-analytics"></a>sp_special_columns_100 (Azure синапсе Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Возвращает оптимальный набор столбцов, уникально идентифицирующих строку таблицы. Также возвращает столбцы, автоматически обновляемые, когда любое значение в строке обновляется транзакцией.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_special_columns_100 [ @table_name = ] 'table_name'     
     [ , [ @table_owner = ] 'table_owner' ]   
     [ , [ @qualifier = ] 'qualifier' ]   
     [ , [ @col_type = ] 'col_type' ]   
     [ , [ @scope = ] 'scope' ]  
     [ , [ @nullable = ] 'nullable' ]   
     [ , [ @ODBCVer = ] 'ODBCVer' ]   
[ ; ]  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Аргументы  
 [ @table_name =] "*table_name*"  
 Название таблицы, используемой для возврата сведений о каталоге. Аргумент *Name* имеет тип **sysname** и не имеет значения по умолчанию. Сопоставление по шаблону не поддерживается.  
  
 [ @table_owner =] "*table_owner*"  
 Владелец таблицы, используемой для возврата сведений о каталоге. Аргумент *owner* имеет тип **sysname** и значение по умолчанию NULL. Сопоставление по шаблону не поддерживается. Если параметр *owner* не указан, применяются правила видимости таблиц по умолчанию базовой СУБД.  
  
 В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если текущий пользователь является владельцем таблицы с указанным именем, возвращаются ее столбцы. Если *владелец* не указан и текущий пользователь не владеет таблицей с указанным *именем*, эта процедура ищет таблицу с указанным *именем* , принадлежащую владельцу базы данных. Если таблица существует, возвращаются ее столбцы.  
  
 [ @qualifier =] "*квалификатор*"  
 Имя квалификатора таблицы. *квалификатор* имеет тип **sysname** и значение по умолчанию NULL. Различные продукты СУБД поддерживают имена таблиц, состоящие из трех частей (*Qualifier.Owner.Name*). В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] этот столбец представляет имя базы данных. В некоторых СУБД он представляет имя сервера в среде базы данных, в которой находится таблица.  
  
 [ @col_type =] "*col_type*"  
 Тип — Column. *col_type* имеет **тип char (** 1 **)** и значение по умолчанию r. Type r Возвращает оптимальный столбец или набор столбцов, которые, получая значения из столбца или столбцов, позволяют однозначно идентифицировать любую строку в указанной таблице. Столбец может быть либо псевдостолбцом, специально созданным для этой цели, либо столбцом или столбцами любого уникального индекса таблицы. Тип V возвращает столбец или столбцы в указанной таблице, которые источник данных обновляет автоматически, когда любое значение в строке обновляется транзакцией.  
  
 [ @scope =] "*область*"  
 Минимально требуемая область ROWID. *Scope* имеет **тип char (** 1 **)** и значение по умолчанию T. Scope C указывает, что ROWID действителен только при размещении в этой строке. Область T указывает, что ROWID действителен для транзакции.  
  
 [ @nullable =] "*Nullable*"  
 Определяет, допускают ли специальные столбцы значение NULL. *Nullable* имеет **тип char (** 1 **)** и значение по умолчанию U. O указывает специальные столбцы, которые не допускают значения NULL. U указывает столбцы, частично допускающие значения NULL.  
  
 [ @ODBCVer =] '*Одбквер*'  
 Используемая версия ODBC. *Одбквер* имеет **тип int (** 4 **)** и значение по умолчанию 2. Это значение соответствует версии ODBC 2.0. Дополнительные сведения о различиях между ODBC 2.0 и ODBC 3.0 см. в спецификации ODBC SQLSpecialColumns для ODBC 3.0.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 Нет  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|SCOPE|**smallint**|Фактическая область идентификатора строки. Возможны следующие варианты: 0, 1 или 2. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] всегда возвращает значение 0. Это поле всегда возвращает значение.<br /><br /> 0 = SQL_SCOPE_CURROW. Идентификатор строки гарантированно действителен до тех пор, пока он расположен на этой строке. Проведенная позднее повторная выборка с использованием идентификатора строки может не вернуть строку, если строка была обновлена или удалена другой транзакцией.<br /><br /> 1 = SQL_SCOPE_TRANSACTION. Идентификатор строки гарантированно действителен в течение текущей транзакции.<br /><br /> 2 = SQL_SCOPE_SESSION. Идентификатор строки гарантированно действителен на протяжении сеанса (несмотря на границы транзакций).|  
|COLUMN_NAME|**sysname**|Имя столбца для каждого столбца возвращаемой *таблицы* . Это поле всегда возвращает значение.|  
|DATA_TYPE|**smallint**|Тип данных ODBC SQL.|  
|TYPE_NAME|**sysname**|Имя типа данных, зависящее от источника данных; Например, **char**, **varchar**, **money** или **Text**.|  
|PRECISION|**Int**|Точность столбца на источнике данных. Это поле всегда возвращает значение.|  
|LENGTH|**Int**|Длина (в байтах), необходимая для типа данных в его двоичной форме в источнике данных, например 10 для **char (** 10 **)**, 4 для **целого числа** и 2 для **smallint**.|  
|SCALE|**smallint**|Масштаб столбца в источнике данных. Для тех типов данных, для которых масштаб не применим, возвращается значение NULL.|  
|PSEUDO_COLUMN|**smallint**|Указывает, является ли столбец псевдостолбцом. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] всегда возвращает 1:<br /><br /> 0 = SQL_PC_UNKNOWN<br /><br /> 1 = SQL_PC_NOT_PSEUDO<br /><br /> 2 = SQL_PC_PSEUDO|  
  
## <a name="remarks"></a>Remarks  
 В ODBC sp_special_columns эквивалентно SQLSpecialColumns. Возвращенные результаты сортируются по столбцу SCOPE.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение SELECT для схемы.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 Следующий пример возвращает данные о столбце, которые уникально идентифицируют строки в таблице `FactFinance`.  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_special_columns_100 @table_name = 'FactFinance';  
```  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры Azure синапсе Analytics](../../relational-databases/system-stored-procedures/sql-data-warehouse-stored-procedures.md)  
  
  


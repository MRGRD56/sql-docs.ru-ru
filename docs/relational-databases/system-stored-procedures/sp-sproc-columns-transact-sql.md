---
description: sp_sproc_columns (Transact-SQL)
title: sp_sproc_columns (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_sproc_columns
- sp_sproc_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_sproc_columns
ms.assetid: 62c18c21-35c5-4772-be0d-ffdcc19c97ab
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 66af1e613e5b02eee96357da383a0b535424ebf5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104745034"
---
# <a name="sp_sproc_columns-transact-sql"></a>sp_sproc_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает сведения о столбце для одной хранимой процедуры или определяемой пользователем функции в текущем окружении.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_sproc_columns [[@procedure_name = ] 'name']   
    [ , [@procedure_owner = ] 'owner']   
    [ , [@procedure_qualifier = ] 'qualifier']   
    [ , [@column_name = ] 'column_name']  
    [ , [@ODBCVer = ] 'ODBCVer']  
    [ , [@fUsePattern = ] 'fUsePattern']  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @procedure_name = ] 'name'` Имя процедуры, используемой для возврата сведений о каталоге. *Name* имеет тип **nvarchar (** 390 **)** и значение по умолчанию%, что означает все таблицы в текущей базе данных. Поиск совпадений по шаблону поддерживается.  
  
`[ @procedure_owner = ] 'owner'` Имя владельца процедуры. *owner* имеет тип **nvarchar (** 384 **)** и значение по умолчанию NULL. Поиск совпадений по шаблону поддерживается. Если параметр *owner* не указан, применяются правила видимости процедур по умолчанию базовой СУБД.  
  
 Если текущий пользователь является владельцем процедуры с указанным именем, возвращаются сведения о процедуре. Если параметр *owner* не указан и текущий пользователь не владеет процедурой с указанным именем, **sp_sproc_columns** ищет процедуру с указанным именем, принадлежащую владельцу базы данных. Если процедура существует, возвращаются сведения о ее столбцах.  
  
`[ @procedure_qualifier = ] 'qualifier'` Имя квалификатора процедуры. *квалификатор* имеет тип **sysname** и значение по умолчанию NULL. Различные продукты СУБД поддерживают имена таблиц, состоящие из трех частей (*Qualifier.Owner.Name*). В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] этот аргумент представляет имя базы данных. В некоторых СУБД он представляет имя сервера в среде базы данных, где находится таблица.  
  
`[ @column_name = ] 'column_name'` Является одним столбцом и используется, когда требуется только один столбец сведений о каталоге. *column_name* имеет тип **nvarchar (** 384 **)** и значение по умолчанию NULL. Если аргумент *column_name* опущен, возвращаются все столбцы. Поиск совпадений по шаблону поддерживается. Для максимальной совместимости клиент шлюза должен использовать только согласование установленного образца ISO (символы-шаблоны % и _).  
  
`[ @ODBCVer = ] 'ODBCVer'` Используемая версия ODBC. *Одбквер* имеет **тип int** и значение по умолчанию 2, которое указывает на ODBC версии 2,0. Дополнительные сведения о различиях между ODBC версии 2,0 и ODBC версии 3,0 см. в статье Спецификация ODBC **SQLProcedureColumns** для odbc версии 3,0.  
  
`[ @fUsePattern = ] 'fUsePattern'` Определяет, обрабатываются ли символы подчеркивания (_), процента (%) и квадратных скобок ([]) как подстановочные знаки. Допустимые значения: 0 (сопоставление с шаблоном отключено) и 1 (сопоставление с шаблоном включено). *фусепаттерн* имеет **бит** и значение по умолчанию 1.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 Нет  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**PROCEDURE_QUALIFIER**|**sysname**|Имя квалификатора процедуры. Этот столбец может принимать значение NULL.|  
|**PROCEDURE_OWNER**|**sysname**|Имя владельца процедуры. Этот столбец всегда возвращает значение.|  
|**PROCEDURE_NAME**|**nvarchar (** 134 **)**|Имя процедуры. Этот столбец всегда возвращает значение.|  
|**COLUMN_NAME**|**sysname**|Имя столбца для каждого столбца возвращаемого **table_name** . Этот столбец всегда возвращает значение.|  
|**COLUMN_TYPE**|**smallint**|Это поле всегда возвращает значение:<br /><br /> 0 = SQL_PARAM_TYPE_UNKNOWN<br /><br /> 1 = SQL_PARAM_TYPE_INPUT<br /><br /> 2 = SQL_PARAM_TYPE_OUTPUT<br /><br /> 3 = SQL_RESULT_COL<br /><br /> 4 = SQL_PARAM_OUTPUT<br /><br /> 5 = SQL_RETURN_VALUE|  
|**DATA_TYPE**|**smallint**|Целочисленный код для типа данных ODBC. Если этот тип данных нельзя сопоставить с типом ISO, то значением будет NULL. Имя собственного типа данных возвращается в столбец **TYPE_NAME** .|  
|**TYPE_NAME**|**sysname**|Строковое представление типа данных. Это имя типа данных, как представлено соответствующей СУБД.|  
|**PRECISION**|**int**|Количество значащих цифр. Возвращаемое значение для столбца **точности** находится в базовом 10.|  
|**LENGTH**|**int**|Размер передаваемых данных.|  
|**Измените**|**smallint**|Число цифр справа от десятичной запятой.|  
|**RADIX**|**smallint**|Основание системы счисления для числовых типов.|  
|**ОБНУЛЯЕМОГО**|**smallint**|Определяет допустимость значений NULL:<br /><br /> 1 = Может быть создан тип данных, допускающий значения NULL.<br /><br /> 0 = значения NULL недопустимы.|  
|**ЗАМЕЧАНИЯ**|**varchar (** 254 **)**|Описание столбца процедуры. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не возвращает значение для этого столбца.|  
|**COLUMN_DEF**|**nvarchar (** 4000 **)**|Значение столбца по умолчанию.|  
|**SQL_DATA_TYPE**|**smallint**|Значение типа данных SQL в том виде, в каком оно отображается в поле **Type** дескриптора. Этот столбец содержит то же значение, что и столбец **DATA_TYPE**, за исключением типов данных **datetime** и ISO **interval**. Этот столбец всегда возвращает значение.|  
|**SQL_DATETIME_SUB**|**smallint**|Дополнительный код **datetime** ISO **interval**, если значение **SQL_DATA_TYPE** равно **SQL_DATETIME** или **SQL_INTERVAL**. Для типов данных, отличных от **DateTime** и **интервалов** ISO, это поле имеет значение null.|  
|**CHAR_OCTET_LENGTH**|**int**|Максимальная длина в байтах для столбца **символьного** или **двоичного** типа данных. Для всех других типов данных этот столбец возвращает значение NULL.|  
|**ORDINAL_POSITION**|**int**|Порядковый номер столбца в таблице. Первый столбец в таблице имеет порядковый номер 1. Этот столбец всегда возвращает значение.|  
|**IS_NULLABLE**|**varchar (254)**|Способность столбца таблицы содержать значение NULL. Допустимость значений NULL определяется в соответствии с правилами ISO. СУБД, совместимая с ISO, не может возвращать пустую строку.<br /><br /> Отображает YES, если столбец может включать значения NULL, и NO, если столбец не может содержать значения NULL.<br /><br /> Если допустимость значения NULL неизвестна, то этот столбец возвращает строку нулевой длины.<br /><br /> Значение, возвращаемое в данном столбце, отличается от значения, возвращаемого в столбце NULLABLE.|  
|**SS_DATA_TYPE**|**tinyint**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] тип данных используется для расширенных хранимых процедур. Дополнительные сведения см. в разделе [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md).|  
  
## <a name="remarks"></a>Remarks  
 **sp_sproc_columns** эквивалентен **SQLProcedureColumns** в ODBC. Возвращаемые результаты упорядочиваются по **PROCEDURE_QUALIFIER**, **PROCEDURE_OWNER**, **procedure_name** и порядку, в котором эти параметры отображаются в определении процедуры.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение SELECT для схемы.  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры каталога &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

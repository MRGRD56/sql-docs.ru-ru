---
description: ROUTINES (Transact-SQL)
title: Подпрограммы (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- ROUTINES_TSQL
- ROUTINES
dev_langs:
- TSQL
helpviewer_keywords:
- ROUTINES view
- INFORMATION_SCHEMA.ROUTINES view
ms.assetid: c75561b2-c9a1-48a1-9afa-a5896b6454cf
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 231973e8fa89e1e507502bf2f2a62d730fb9f517
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104736094"
---
# <a name="routines-transact-sql"></a>ROUTINES (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает одну строку для каждой хранимой процедуры и функции, к которым текущий пользователь может получить доступ в текущей базе данных. Столбцы, описывающие возвращаемое значение, относятся только к функциям. Для хранимых процедур эти столбцы будут иметь значение NULL.  
  
 Чтобы получить сведения из этих представлений, укажите полное имя INFORMATION_SCHEMA. *view_name*.  
  
> [!NOTE]  
>  Столбец ROUTINE_DEFINITION включает исходные инструкции, создавшие эту функцию или хранимую процедуру. Эти исходные инструкции, вероятно, содержат встроенные символы возврата каретки. Если этот столбец возвращается приложению, которое выводит результаты в текстовом формате, эти встроенные символы в результатах ROUTINE_DEFINITION могут нарушить форматирование общего результирующего набора. Если выбирается столбец ROUTINE_DEFINITION, необходимо учитывать встроенные символы возврата каретки, например возвращая результирующий набор в сетку или выводя результаты ROUTINE_DEFINITION в собственное текстовое поле.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|SPECIFIC_CATALOG|**nvarchar (** 128 **)**|Определенное имя каталога. Это имя совпадает с ROUTINE_CATALOG.|  
|SPECIFIC_SCHEMA|**nvarchar (** 128 **)**|Определенное имя схемы.<br /><br /> Важно. не используйте INFORMATION_SCHEMA представления для определения схемы объекта. **\* \* \* \*** INFORMATION_SCHEMA представления представляют только подмножество метаданных объекта. Единственный надежный способ найти схему объекта — направить запрос к представлению каталога sys.objects.|  
|SPECIFIC_NAME|**nvarchar (** 128 **)**|Определенное имя каталога. Это имя совпадает с ROUTINE_NAME.|  
|ROUTINE_CATALOG|**nvarchar (** 128 **)**|Имя каталога функции.|  
|ROUTINE_SCHEMA|**nvarchar (** 128 **)**|Имя схемы, содержащей эту функцию.<br /><br /> Важно. не используйте INFORMATION_SCHEMA представления для определения схемы объекта. **\* \* \* \*** INFORMATION_SCHEMA представления представляют только подмножество метаданных объекта. Единственный надежный способ найти схему объекта — направить запрос к представлению каталога sys.objects.|  
|ROUTINE_NAME|**nvarchar (** 128 **)**|Имя функции.|  
|ROUTINE_TYPE|**nvarchar (** 20 **)**|Возвращает значение PROCEDURE для хранимой процедуры и FUNCTION для функций.|  
|MODULE_CATALOG|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|MODULE_SCHEMA|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|MODULE_NAME|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|UDT_CATALOG|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|UDT_SCHEMA|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|UDT_NAME|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|DATA_TYPE|**nvarchar (** 128 **)**|Тип данных возвращаемого значения функции. Возвращает **таблицу** , если возвращающая табличное значение функция.|  
|CHARACTER_MAXIMUM_LENGTH|**int**|Максимальная длина в символах, если возвращаемый тип символьный.<br /><br /> -1 для данных типа **XML** и больших значений.|  
|CHARACTER_OCTET_LENGTH|**int**|Максимальная длина в байтах, если возвращаемый тип символьный.<br /><br /> -1 для данных типа **XML** и больших значений.|  
|COLLATION_CATALOG|**nvarchar (** 128 **)**|Всегда возвращает значение NULL.|  
|COLLATION_SCHEMA|**nvarchar (** 128 **)**|Всегда возвращает значение NULL.|  
|COLLATION_NAME|**nvarchar (** 128 **)**|Имя параметров сортировки для возвращаемого значения. Для несимвольных типов возвращает значение NULL.|  
|CHARACTER_SET_CATALOG|**nvarchar (** 128 **)**|Всегда возвращает значение NULL.|  
|CHARACTER_SET_SCHEMA|**nvarchar (** 128 **)**|Всегда возвращает значение NULL.|  
|CHARACTER_SET_NAME|**nvarchar (** 128 **)**|Имя кодировки возвращаемого значения. Для несимвольных типов возвращает значение NULL.|  
|NUMERIC_PRECISION|**smallint**|Точность возвращаемого значения. Для нечисловых типов возвращает значение NULL.|  
|NUMERIC_PRECISION_RADIX|**smallint**|Основание системы счисления для точности возвращаемого значения. Для нецифровых типов возвращает значение NULL.|  
|NUMERIC_SCALE|**smallint**|Масштаб возвращаемого значения. Для нецифровых типов возвращает значение NULL.|  
|DATETIME_PRECISION|**smallint**|Точность в долях секунды, если возвращаемое значение имеет тип **DateTime**. В противном случае возвращается значение NULL.|  
|INTERVAL_TYPE|**nvarchar (** 30 **)**|NULL. Зарезервировано для будущего использования.|  
|INTERVAL_PRECISION|**smallint**|NULL. Зарезервировано для будущего использования.|  
|TYPE_UDT_CATALOG|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|TYPE_UDT_SCHEMA|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|TYPE_UDT_NAME|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|SCOPE_CATALOG|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|SCOPE_SCHEMA|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|SCOPE_NAME|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|MAXIMUM_CARDINALITY|**bigint**|NULL. Зарезервировано для будущего использования.|  
|DTD_IDENTIFIER|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|ROUTINE_BODY|**nvarchar (** 30 **)**|Возвращает значение SQL для функции [!INCLUDE[tsql](../../includes/tsql-md.md)] и EXTERNAL для функции, написанной внешними средствами.<br /><br /> Функции всегда SQL.|  
|ROUTINE_DEFINITION|**nvarchar (** 4000 **)**|Возвращает первые 4 000 символов текста определения функции или хранимой процедуры, если они не зашифрованы. В противном случае возвращается значение NULL.<br /><br /> Чтобы убедиться, что получено полное определение, выполните запрос к функции [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) или к столбцу определения в представлении каталога [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) .|  
|EXTERNAL_NAME|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|EXTERNAL_LANGUAGE|**nvarchar (** 30 **)**|NULL. Зарезервировано для будущего использования.|  
|PARAMETER_STYLE|**nvarchar (** 30 **)**|NULL. Зарезервировано для будущего использования.|  
|IS_DETERMINISTIC|**nvarchar (** 10 **)**|Возвращает «YES», если процедура детерминированная.<br /><br /> Возвращает «NO», если процедура недетерминированная.<br /><br /> Всегда возвращает «NO» для хранимых процедур.|  
|SQL_DATA_ACCESS|**nvarchar (** 30 **)**|Возвращает одно из следующих значений:<br /><br /> NONE = функция не содержит SQL;<br /><br /> CONTAINS = функция может содержать SQL;<br /><br /> READS = функция, возможно, считывает данные SQL;<br /><br /> MODIFIES = функция, возможно, изменяет данные SQL.<br /><br /> Возвращает READS для всех функций и MODIFIES для всех хранимых процедур.|  
|IS_NULL_CALL|**nvarchar (** 10 **)**|Указывает, будет ли вызываться процедура, если один из ее аргументов равен NULL.|  
|SQL_PATH|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|SCHEMA_LEVEL_ROUTINE|**nvarchar (** 10 **)**|Возвращает «YES», если это функция уровня схемы, или «NO» в противном случае.<br /><br /> Всегда возвращает «YES».|  
|MAX_DYNAMIC_RESULT_SETS|**smallint**|Максимальное число динамических результирующих наборов, возвращаемых процедурой.<br /><br /> Возвращает 0, если функция.|  
|IS_USER_DEFINED_CAST|**nvarchar (** 10 **)**|Возвращает «YES», если это пользовательская функция приведения, и «NO» в противном случае.<br /><br /> Всегда возвращает NO.|  
|IS_IMPLICITLY_INVOCABLE|**nvarchar (** 10 **)**|Возвращает «YES», если процедуру можно вызвать неявно, и «NO» в противном случае.<br /><br /> Всегда возвращает NO.|  
|CREATED|**datetime**|Время создания процедуры.|  
|LAST_ALTERED|**datetime**|Время последнего изменения функции.|  
  
## <a name="see-also"></a>См. также:  
 [Системные представления &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Представления информационной схемы &#40;&#41;Transact-SQL ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.procedures (Transact-SQL)](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)  
  

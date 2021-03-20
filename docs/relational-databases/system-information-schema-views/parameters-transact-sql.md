---
description: PARAMETERS (Transact-SQL)
title: Параметры (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- PARAMETERS_TSQL
- PARAMETERS
dev_langs:
- TSQL
helpviewer_keywords:
- PARAMETERS view
- INFORMATION_SCHEMA.PARAMETERS view
ms.assetid: 06ded0ca-7d21-4400-864a-b801e855b257
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e0b6bc62151f8628e1fb93ea08a619da9f291e8b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755714"
---
# <a name="parameters-transact-sql"></a>PARAMETERS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает одну строку для каждого параметра определяемой пользователем функции или хранимой процедуры, к которой может получить доступ текущий пользователь в текущей базе данных. Для функций данное представление также возвращает одну строку, содержащую сведения о возвращаемых значениях.  
  
 Чтобы получить сведения из этих представлений, укажите полное имя **INFORMATION_SCHEMA.** _view_name_.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**SPECIFIC_CATALOG**|**nvarchar (** 128 **)**|Имя каталога процедуры, для которой это является параметром.|  
|**SPECIFIC_SCHEMA**|**nvarchar (** 128 **)**|Имя схемы процедуры, для которой это является параметром.<br /><br /> Важно. не используйте INFORMATION_SCHEMA представления для определения схемы объекта. <strong> \* \* \* \* </strong> INFORMATION_SCHEMA представления представляют только подмножество метаданных объекта. Единственный надежный способ найти схему объекта — направить запрос к представлению каталога sys.objects.|  
|**SPECIFIC_NAME**|**nvarchar (** 128 **)**|Имя процедуры, для которой это является параметром.|  
|**ORDINAL_POSITION**|**int**|Порядковый номер параметра, начиная с 1. Для возвращаемого значения функции это 0.|  
|**PARAMETER_MODE**|**nvarchar (** 10 **)**|Возвращает значение IN для входного параметра, OUT для выходного параметра и INOUT для изменяемого входного параметра.|  
|**IS_RESULT**|**nvarchar (** 10 **)**|Возвращает значение YES, если результат подпрограммы является результатом выполнения функции. В противном случае возвращается значение NO.|  
|**AS_LOCATOR**|**nvarchar (** 10 **)**|Возвращает значение YES, если результат объявлен как указатель. В противном случае возвращается значение NO.|  
|**PARAMETER_NAME**|**nvarchar (** 128 **)**|Имя параметра. Если соответствует результату выполнения функции, то возвращается значение NULL.|  
|**DATA_TYPE**|**nvarchar (** 128 **)**|Тип данных, поддерживаемый системой.|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|Максимальная длина в символах для двоичных или символьных данных.<br /><br /> -1 для данных типа **XML** и больших значений. В противном случае возвращается значение NULL.|  
|**CHARACTER_OCTET_LENGTH**|**int**|Максимальная длина в байтах для двоичных или символьных данных.<br /><br /> -1 для данных типа **XML** и больших значений. В противном случае возвращается значение NULL.|  
|**COLLATION_CATALOG**|**nvarchar (** 128 **)**|Всегда возвращает значение NULL.|  
|**COLLATION_SCHEMA**|**nvarchar (** 128 **)**|Всегда возвращает значение NULL.|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|Имя параметров сортировки параметра. Если введенные данные не принадлежат ни к одному из символьных типов, возвращает значение NULL.|  
|**CHARACTER_SET_CATALOG**|**nvarchar (** 128 **)**|Имя каталога кодировки параметра. Если введенные данные не принадлежат ни к одному из символьных типов, возвращает значение NULL.|  
|**CHARACTER_SET_SCHEMA**|**nvarchar (** 128 **)**|Всегда возвращает значение NULL.|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|Имя кодировки параметра. Если введенные данные не принадлежат ни к одному из символьных типов, возвращает значение NULL.|  
|**NUMERIC_PRECISION**|**tinyint**|Точность приблизительных числовых данных, точных числовых данных, целочисленных данных или денежных данных. В противном случае возвращается значение NULL.|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|Основание системы счисления точности приблизительных числовых данных, точных числовых данных, целочисленных данных или денежных данных. В противном случае возвращается значение NULL.|  
|**NUMERIC_SCALE**|**tinyint**|Масштаб приблизительных числовых данных, точных числовых данных, целочисленных данных или денежных данных. В противном случае возвращается значение NULL.|  
|**DATETIME_PRECISION**|**smallint**|Точность в долях секунды, если параметр имеет тип **DateTime** или **smalldatetime**. В противном случае возвращается значение NULL.|  
|**INTERVAL_TYPE**|**nvarchar (** 30 **)**|NULL. Зарезервировано для будущего использования.|  
|**INTERVAL_PRECISION**|**smallint**|NULL. Зарезервировано для будущего использования.|  
|**USER_DEFINED_TYPE_CATALOG**|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|**USER_DEFINED_TYPE_SCHEMA**|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|**USER_DEFINED_TYPE_NAME**|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|**SCOPE_CATALOG**|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|**SCOPE_SCHEMA**|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
|**SCOPE_NAME**|**nvarchar (** 128 **)**|NULL. Зарезервировано для будущего использования.|  
  
## <a name="see-also"></a>См. также:  
 [Системные представления &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Представления информационной схемы &#40;&#41;Transact-SQL ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.parameters (Transact-SQL)](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)  
  

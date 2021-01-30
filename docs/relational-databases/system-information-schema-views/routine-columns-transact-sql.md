---
description: ROUTINE_COLUMNS (Transact-SQL)
title: ROUTINE_COLUMNS (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- ROUTINE_COLUMNS_TSQL
- ROUTINE_COLUMNS
dev_langs:
- TSQL
helpviewer_keywords:
- ROUTINE_COLUMNS view
- INFORMATION_SCHEMA.ROUTINE_COLUMNS view
ms.assetid: 91dbc61b-e4c0-4826-976c-b2fce88b7793
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f2a60f579dd5769d4c555e79de1b67cfd75660ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189602"
---
# <a name="routine_columns-transact-sql"></a>ROUTINE_COLUMNS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает по одной строке для каждого столбца, возвращенного функциями с табличным значением к которым может получить доступ текущий пользователь в текущей базе данных.  
  
 Чтобы получить сведения из этого представления, укажите полное имя **INFORMATION_SCHEMA.** _view_name_.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Имя каталога или базы данных функции с табличным значением.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Имя схемы, содержащей функцию с табличным значением.<br /><br /> Важно. не используйте INFORMATION_SCHEMA представления для определения схемы объекта. <strong> \* \* \* \* </strong> INFORMATION_SCHEMA представления представляют только подмножество метаданных объекта. Единственный надежный способ найти схему объекта — направить запрос к представлению каталога sys.objects.|  
|**TABLE_NAME**|**nvarchar (** 128 **)**|Имя функции с табличным значением.|  
|**COLUMN_NAME**|**nvarchar (** 128 **)**|Имя столбца.|  
|**ORDINAL_POSITION**|**int**|Идентификационный номер столбца.|  
|**COLUMN_DEFAULT**|**nvarchar (** 4000 **)**|Значение столбца по умолчанию.|  
|**IS_NULLABLE**|**varchar (** 3 **)**|Если для этого столбца допустимо значение NULL, возвращается значение YES. В противном случае возвращается значение NO.|  
|**DATA_TYPE**|**nvarchar (** 128 **)**|Тип данных, поддерживаемый системой.|  
|**CHARACTER_MAXIMUM_LENGTH**|**int**|Максимальная длина в символах для двоичных данных, символьных данных или текстовых данных и изображений.<br /><br /> -1 для данных типа **XML** и больших значений. В противном случае возвращается значение NULL. Дополнительные сведения см. в разделе [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md).|  
|**CHARACTER_OCTET_LENGTH**|**int**|Максимальная длина в байтах для двоичных данных, символьных данных или текстовых данных и изображений.<br /><br /> -1 для данных типа **XML** и больших значений. В противном случае возвращается значение NULL.|  
|**NUMERIC_PRECISION**|**tinyint**|Точность приблизительных числовых данных, точных числовых данных, целочисленных данных или денежных данных. В противном случае возвращается значение NULL.|  
|**NUMERIC_PRECISION_RADIX**|**smallint**|Основание системы счисления точности приблизительных числовых данных, точных числовых данных, целочисленных данных или денежных данных. В противном случае возвращается значение NULL.|  
|**NUMERIC_SCALE**|**tinyint**|Масштаб приблизительных числовых данных, точных числовых данных, целочисленных данных или денежных данных. В противном случае возвращается значение NULL.|  
|**DATETIME_PRECISION**|**smallint**|Код подтипа для типов данных **DateTime** и **Integer** в формате ISO. Для других типов данных возвращается значение NULL.|  
|**CHARACTER_SET_CATALOG**|**varchar (** 6 **)**|Возвращает **образец**. Указывает базу данных, в которой находится набор символов, если столбец содержит символьные данные или **текстовый** тип данных. В противном случае возвращается значение NULL.|  
|**CHARACTER_SET_SCHEMA**|**varchar (** 3 **)**|Всегда возвращает значение NULL.|  
|**CHARACTER_SET_NAME**|**nvarchar (** 128 **)**|Возвращает уникальное имя для набора символов, если этот столбец имеет символьные данные или тип данных **Text** . В противном случае возвращается значение NULL.|  
|**COLLATION_CATALOG**|**varchar (** 6 **)**|Всегда возвращает значение NULL.|  
|**COLLATION_SCHEMA**|**varchar (** 3 **)**|Всегда возвращает значение NULL.|  
|**COLLATION_NAME**|**nvarchar (** 128 **)**|Возвращает уникальное имя для порядка сортировки, если столбец имеет символьные данные или **текстовый** тип данных. В противном случае возвращается значение NULL.|  
|**DOMAIN_CATALOG**|**nvarchar (** 128 **)**|Если столбец имеет тип данных псевдонима, то этот столбец содержит имя базы данных, в которой был создан определяемый пользователем тип данных. В противном случае возвращается значение NULL.|  
|**DOMAIN_SCHEMA**|**nvarchar (** 128 **)**|Если столбец имеет пользовательский тип данных, этот столбец является именем схемы, содержащей пользовательский тип данных. В противном случае возвращается значение NULL.<br /><br /> Важно. не используйте INFORMATION_SCHEMA представления для определения схемы объекта. <strong> \* \* \* \* </strong> INFORMATION_SCHEMA представления представляют только подмножество метаданных объекта. Единственный надежный способ найти схему объекта — направить запрос к представлению каталога sys.objects.|  
|**DOMAIN_NAME**|**nvarchar (** 128 **)**|Если столбец имеет определяемый пользователем тип данных, то столбец является именем этого типа данных. В противном случае возвращается значение NULL.|  
  
## <a name="see-also"></a>См. также:  
 [Системные представления &#40;&#41;Transact-SQL ](../../t-sql/language-reference.md)   
 [Представления информационной схемы &#40;&#41;Transact-SQL ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)  
  

---
description: sys.parameters (Transact-SQL)
title: sys. parameters (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.parameters_TSQL
- sys.parameters
- parameters
- parameters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.parameters catalog view
- table-valued parameters,sys.parameters
ms.assetid: 24e2764b-c8e5-4322-97a4-7407d8b8a92b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 26f50db59949b89f4abe5a8bfd284be46b3b3c3a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754934"
---
# <a name="sysparameters-transact-sql"></a>sys.parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Содержит строку для каждого параметра объекта, который принимает параметры. Если объект является скалярной функцией, также имеется одна строка, описывающая возвращаемое значение. Эта строка будет иметь значение **parameter_id** , равное 0.  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Идентификатор объекта, которому принадлежит этот параметр.|  
|**name**|**sysname**|Имя параметра. Уникален в пределах объекта.<br /><br /> Если объект является скалярной функцией, именем параметра будет пустая строка в строке, представляющей возвращаемое значение.|  
|**parameter_id**|**int**|Идентификатор параметра. Уникален в пределах объекта.<br /><br /> Если объект является скалярной функцией, **parameter_id** = 0 представляет возвращаемое значение.|  
|**system_type_id**|**tinyint**|Идентификатор системного типа параметра.|  
|**user_type_id**|**int**|Определенный пользователем идентификатор типа параметра.<br /><br /> Чтобы вернуть имя типа, присоединитесь к представлению каталога [sys. types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) в этом столбце.|  
|**max_length**|**smallint**|Максимальная длина параметра в байтах.<br /><br /> Значение =-1, если тип данных столбца — **varchar (max)**, **nvarchar (max)**, **varbinary (max)** или **XML**.|  
|**precision**|**tinyint**|Для числового аргумента — точность; иначе 0.|  
|**масштаб**|**tinyint**|Масштаб числового аргумента; иначе 0.|  
|**is_output**|**bit**|1 = выходной или возвращаемый параметр; иначе 0.|  
|**is_cursor_ref**|**bit**|1 = аргумент представляет собой ссылку на курсор.|  
|**has_default_value**|**bit**|1 = параметр имеет значение по умолчанию.<br /><br /> В данном представлении каталога [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] всего лишь поддерживает значения по умолчанию для объектов среды CLR; поэтому этот столбец содержит значение 0 для объектов [!INCLUDE[tsql](../../includes/tsql-md.md)]. Чтобы просмотреть значение параметра по умолчанию в [!INCLUDE[tsql](../../includes/tsql-md.md)] объекте, выполните запрос к столбцу **определения** [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) представления каталога или используйте системную функцию [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) .|  
|**is_xml_document**|**bit**|1 = содержимое является готовым XML-документом.<br /><br /> 0 = содержимое является фрагментом документа, или тип данных столбца не является **XML**.|  
|**default_value**|**sql_variant**|Если **has_default_value** равен 1, значение этого столбца равно значению по умолчанию для параметра; в противном случае значение NULL.|  
|**xml_collection_id**|**int**|Ненулевое значение, если тип данных параметра — **XML** , а XML-код типизирован. Значением будет идентификатор коллекции, содержащей проверочное пространство имен схемы XML параметра.<br /><br /> 0 = нет коллекции схем XML.|  
|**is_readonly**|**bit**|1 = неизменяемый параметр; иначе 0.|  
|**is_nullable**|**bit**|1 = параметр допускает значение NULL. (по умолчанию).<br /><br /> 0 = параметр не допускает значения NULL для более эффективного выполнения компилируемых в собственном коде хранимых процедур.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Запросы к системному каталогу SQL Server вопросы и ответы](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [sys.all_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-all-parameters-transact-sql.md)   
 [sys.system_parameters &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-parameters-transact-sql.md)  
  
  

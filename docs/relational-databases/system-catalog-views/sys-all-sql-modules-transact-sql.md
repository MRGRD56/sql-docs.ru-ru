---
description: sys.all_sql_modules (Transact-SQL)
title: sys.all_sql_modules (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- all_sql_modules_TSQL
- sys.all_sql_modules
- all_sql_modules
- sys.all_sql_modules_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.all_sql_modules catalog view
ms.assetid: 7477a3fe-afb3-44c8-bb2c-c6e1d9bdee6f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6ba97156a5c33be1009965b4d969c6f7eee71ac8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753804"
---
# <a name="sysall_sql_modules-transact-sql"></a>sys.all_sql_modules (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Возвращает объединение **sys.sql_modules** и **sys.system_sql_modules**.  
  
 Представление возвращает строку для каждой скомпилированной в собственном код скалярной определяемой пользователем функции. Дополнительные сведения см. в разделе [Скалярные определяемые пользователем функции для выполняющейся в памяти OLTP](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Идентификатор объекта, содержащего данный объект. Уникален в базе данных.|  
|**definition**|**nvarchar(max)**|Текст на языке SQL, определяющий этот модуль.<br /><br /> NULL = зашифрован|  
|**uses_ansi_nulls**|**bit**|Модуль был создан с параметром SET ANSI_NULLS ON.|  
|**uses_quoted_identifier**|**bit**|Модуль был создан с параметром SET QUOTED_IDENTIFIER ON.|  
|**is_schema_bound**|**bit**|Модуль был создан с параметром SCHEMABINDING.|  
|**uses_database_collation**|**bit**|1 = определение модуля, ограниченное схемой, зависит от принятых по умолчанию параметров сортировки базы данных для правильной оценки; в противном случае — 0. Такая зависимость предотвращает изменение параметров сортировки по умолчанию для базы данных.|  
|**is_recompiled**|**bit**|Процедура была создана с параметром WITH RECOMPILE.|  
|**null_on_null_input**|**bit**|Модуль был объявлен, чтобы обеспечить выходные значения NULL для любых входных значений NULL.|  
|**execute_as_principal_id**|**int**|ID-идентификатор участника базы данных, указанного в инструкции EXECUTE AS.<br /><br /> По умолчанию и в случае EXECUTE AS CALLER имеет значение NULL.<br /><br /> ИДЕНТИФИКАТОР указанного участника, если он выполняется как SELF или EXECUTE AS \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
|**uses_native_compilation**|bit|**Область применения**: [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более поздних версий.<br /><br /> 0 = не скомпилированы в собственном коде<br /><br /> 1 = скомпилированы в собственном коде<br /><br /> Значение по умолчанию — 0.|  
  
## <a name="permissions"></a>Разрешения  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>См. также:  
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления каталога объектов (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.system_sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-sql-modules-transact-sql.md)   
 [Выполняющаяся в памяти OLTP (оптимизация в памяти)](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  

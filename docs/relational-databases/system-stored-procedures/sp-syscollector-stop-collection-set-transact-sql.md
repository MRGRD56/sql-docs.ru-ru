---
description: sp_syscollector_stop_collection_set (Transact-SQL)
title: sp_syscollector_stop_collection_set (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_stop_collection_set_TSQL
- sp_syscollector_stop_collection_set
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_stop_collection_set
- data collector [SQL Server], stored procedures
ms.assetid: 4668cfb7-462f-40d0-948c-8f740a792a4d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 335e4966896ef6e08cdec78612caa2a53b3641b7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159484"
---
# <a name="sp_syscollector_stop_collection_set-transact-sql"></a>sp_syscollector_stop_collection_set (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Останавливает набор элементов сбора.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_syscollector_stop_collection_set   
    [ [ @collection_set_id = ] collection_set_id ]  
    , [ [ @name = ] 'name' ]  
    , [ [ @stop_collection_job = ] stop_collection_job ]  
```  
  
## <a name="arguments"></a>Аргументы  
 [ @collection_set_id =] *collection_set_id*  
 Уникальный локальный идентификатор набора элементов сбора. *collection_set_id* имеет **тип int** и значение по умолчанию NULL. *collection_set_id* должны иметь значение, если *Name* имеет значение null.  
  
 [ @name =] "*имя*"  
 Имя набора элементов сбора. Аргумент *Name* имеет тип **sysname** и значение по умолчанию NULL. *имя* должно иметь значение, если *collection_set_id* имеет значение null.  
  
 [ @stop_collection_job =] *stop_collection_job*  
 Указывает, что задание сбора для набора элементов сбора должно быть остановлено, если оно выполняется. *stop_collection_job* имеет **бит** со значением по умолчанию 1.  
  
 *stop_collection_job* применяется только к наборам сбора, для которых установлен режим сбора кэшированный. Дополнительные сведения см. в разделе [sp_syscollector_create_collection_set &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md).  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 Функция sp_syscollector_create_collection_set должна выполняться в контексте системной базы данных msdb.  
  
## <a name="permissions"></a>Разрешения  
 Для выполнения этой процедуры требуется членство в предопределенной роли базы данных dc_operator (с разрешением EXECUTE).  
  
## <a name="examples"></a>Примеры  
 В следующем примере набор сбора останавливается с помощью его идентификатора.  
  
```  
USE msdb;  
GO  
EXEC sp_syscollector_stop_collection_set @collection_set_id = 1;  
```  
  
## <a name="see-also"></a>См. также:  
 [Сбор данных](../../relational-databases/data-collection/data-collection.md)   
 [Хранимые процедуры сборщика данных (Transact-SQL)](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)  
  
  

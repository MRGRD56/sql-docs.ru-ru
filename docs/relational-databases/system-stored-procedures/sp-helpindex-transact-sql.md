---
description: Хранимая процедура sp_helpindex (Transact-SQL)
title: sp_helpindex (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_helpindex_TSQL
- sp_helpindex
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpindex
ms.assetid: c7f73ba0-ec35-4b10-aa5f-f1487e51fbf7
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a011e8f19650dad1c52b4a88e81edf28eb0bfc47
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462695"
---
# <a name="sp_helpindex-transact-sql"></a>Хранимая процедура sp_helpindex (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Сообщает данные об индексах в таблице или представлении.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_helpindex [ @objname = ] 'name'  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @objname = ] 'name'` Полное или неполное имя определяемой пользователем таблицы или представления. Кавычки требуются, только если задано уточненное имя таблицы или представления. Если предоставлено полное имя таблицы, включая имя базы данных, в качестве последнего должно использоваться имя текущей базы данных. *Name* имеет тип **nvarchar (776)** и не имеет значения по умолчанию.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**index_name**|**sysname**|Имя индекса.|  
|**index_description**|**varchar (210)**|Описание индекса, включая файловую группу, в которой он находится.|  
|**index_keys**|**nvarchar (2078)**|Таблица или представление, на которых построен индекс.|  
  
 Столбец, индексированный по убыванию, приводится в результирующем наборе со знаком «минус» (-), за которым следует его имя. Для столбца, индексированного по возрастанию (по умолчанию), приводится только его имя.  
  
## <a name="remarks"></a>Комментарии  
 Если индексы были заданы с помощью параметра NORECOMPUTE инструкции UPDATE STATISTICS, эти сведения включаются в столбец **index_description** .  
  
 **sp_helpindex** предоставляет только столбцы индексов, упорядоченные по столбцам; Поэтому он не предоставляет сведения о XML-индексах и пространственных индексах.  
  
## <a name="permissions"></a>Разрешения  
 Необходимо быть членом роли **public**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере сообщаются данные о типах индексов в таблице `Customer`.  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_helpindex N'Sales.Customer';  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Ядро СУБД хранимых процедур &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [UPDATE STATISTICS (Transact-SQL)](../../t-sql/statements/update-statistics-transact-sql.md)  
  
  

---
description: DROP SCHEMA (Transact-SQL)
title: DROP SCHEMA (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 05/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP SCHEMA
- DROP_SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting schemas
- schemas [SQL Server], removing
- DROP SCHEMA statement
- dropping schemas
- removing schemas
ms.assetid: 874aa29e-c8ad-41e4-a672-900fdc58f1f6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 42f12f76ca4652c131ce0353f53f6d48c887e8cb
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752914"
---
# <a name="drop-schema-transact-sql"></a>DROP SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Удаляет схему из базы данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database  
  
DROP SCHEMA  [ IF EXISTS ] schema_name  
```  
  

```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DROP SCHEMA schema_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *IF EXISTS*  
 **Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] до [текущей версии](/troubleshoot/sql/general/determine-version-edition-update-level)).  
  
 Условное удаление схемы только в том случае, если она уже существует.  
  
 *schema_name*  
 Имя, под которым схема известна в пределах базы данных.  
  
## <a name="remarks"></a>Комментарии  
 Удаляемая схема не должна содержать никаких объектов. Если схема содержит объекты, выполнение инструкции DROP заканчивается сбоем.  
  
 Сведения о схемах можно увидеть в представлении каталога [sys.schemas](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md).  
  
 **Внимание!** [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>Разрешения  
 Необходимо разрешение CONTROL на схему или разрешение ALTER ANY SCHEMA в базе данных.  
  
## <a name="examples"></a>Примеры  
 Следующий пример начинается с единственной инструкции `CREATE SCHEMA`. Эта инструкция создает схему `Sprockets`, владельцем которой является `Krishna`, и таблицу `Sprockets.NineProngs`, затем предоставляет разрешение `SELECT` на `Anibal` и отзывает разрешение `SELECT` на `Hung-Fu`.  
  
```sql  
CREATE SCHEMA Sprockets AUTHORIZATION Krishna   
    CREATE TABLE NineProngs (source INT, cost INT, partnumber INT)  
    GRANT SELECT TO Anibal   
    DENY SELECT TO [Hung-Fu];  
GO  
```  
  
 При помощи следующих инструкций схема удаляется. Обратите внимание, что вначале необходимо удалить таблицу, содержащуюся в схеме.  
  
```sql  
DROP TABLE Sprockets.NineProngs;  
DROP SCHEMA Sprockets;  
GO  
```  
  
  
## <a name="see-also"></a>См. также:  
 [CREATE SCHEMA (Transact-SQL)](../../t-sql/statements/create-schema-transact-sql.md)   
 [ALTER SCHEMA (Transact-SQL)](../../t-sql/statements/alter-schema-transact-sql.md)   
 [DROP SCHEMA (Transact-SQL)](../../t-sql/statements/drop-schema-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)
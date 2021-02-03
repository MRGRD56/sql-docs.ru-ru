---
description: DROP XML SCHEMA COLLECTION (Transact-SQL)
title: DROP XML SCHEMA COLLECTION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 11/25/2015
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP XML SCHEMA COLLECTION
- DROP_XML_SCHEMA_COLLECTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting XML schema collections
- XML schema collections [SQL Server], removing
- schema collections [SQL Server], removing
- removing XML schema collections
- dropping XML schema collections
- DROP XML SCHEMA COLLECTION statement
ms.assetid: d686f2f5-e03a-4ffe-a566-6036628f46f1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 158821f7c317e606ccf2643a3db82aac99b9ec78
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188712"
---
# <a name="drop-xml-schema-collection-transact-sql"></a>DROP XML SCHEMA COLLECTION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Удаляет всю коллекцию XML-схем и все ее компоненты.  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
DROP XML SCHEMA COLLECTION [ relational_schema. ]sql_identifier  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*relational_schema*  
Определяет имя реляционной схемы. Если не задано, предполагается использование реляционной схемы по умолчанию.  
  
*sql_identifier*  
Имя удаляемой коллекции схем XML.  
  
## <a name="remarks"></a>Примечания  
Удаление коллекции XML-схем является транзакционной операцией. Если удалить коллекцию схем XML внутри транзакции, а потом сделать откат, коллекция XML-схем не будет удалена.  
  
Коллекцию XML-схем нельзя удалить, если она используется. Таким образом, удаляемая коллекция не может быть в любом из следующих состояний:  
  
-   ассоциированной с любым параметром типа **xml** или столбцом;  
  
-   указанной в ограничениях любой из таблиц;  
  
-   той, на которую ссылается привязанная к схеме функция или хранимая процедура. Например, следующая функция блокирует коллекцию XML-схем `MyCollection`, так как функция включает `WITH SCHEMABINDING`. Если удалить ее, блокировки на XML SCHEMA COLLECTION не будет.  
  
    ```sql  
    CREATE FUNCTION dbo.MyFunction()  
    RETURNS int  
    WITH SCHEMABINDING  
    AS  
    BEGIN  
       ...  
       DECLARE @x XML(MyCollection)  
       ...  
    END;  
    ```  
  
## <a name="permissions"></a>Разрешения  
Для удаления XML SCHEMA COLLECTION необходимо обладать разрешением DROP для коллекции.  
  
## <a name="examples"></a>Примеры  
Следующий пример демонстрирует удаление коллекции XML-схем.  
  
```sql  
DROP XML SCHEMA COLLECTION ManuInstructionsSchemaCollection;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE XML SCHEMA COLLECTION (Transact-SQL)](../../t-sql/statements/create-xml-schema-collection-transact-sql.md)   
 [ALTER XML SCHEMA COLLECTION (Transact-SQL)](../../t-sql/statements/alter-xml-schema-collection-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [Сравнение типизированного и нетипизированного XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Требования и ограничения для коллекций схем XML на сервере](../../relational-databases/xml/requirements-and-limitations-for-xml-schema-collections-on-the-server.md)  
  
  

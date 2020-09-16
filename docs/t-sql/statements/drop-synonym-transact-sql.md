---
description: DROP SYNONYM (Transact-SQL)
title: DROP SYNONYM (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP SYNONYM
- DROP_SYNONYM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting synonyms
- synonyms [SQL Server], removing
- removing synonyms
- DROP SYNONYM statement
- dropping synonyms
ms.assetid: 23578932-e4de-4c39-a5a0-ce45139c4269
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fb5f6e5153628ce7d452bb06c31625e6515503e4
ms.sourcegitcommit: dd36d1cbe32cd5a65c6638e8f252b0bd8145e165
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/08/2020
ms.locfileid: "89549281"
---
# <a name="drop-synonym-transact-sql"></a>DROP SYNONYM (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Удаляет синонимы из указанной схемы.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
DROP SYNONYM [ IF EXISTS ] [ schema. ] synonym_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *IF EXISTS*  
**Применимо к**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] до [текущей версии](https://go.microsoft.com/fwlink/p/?LinkId=299658))
  
 Условное удаление синонима только в том случае, если он уже существует.  
  
 *schema*  
 Указывает схему, в которой существует этот синоним. Если схема не указана, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует применяемую по умолчанию схему текущего пользователя.  
  
 *synonym_name*  
 Имя синонима, который нужно удалить.  
  
## <a name="remarks"></a>Remarks  
 Ссылки на синонимы не привязаны к схемам, поэтому удаление синонима возможно в любое время. Ссылки на удаленные синонимы можно обнаружить только во время выполнения.  
  
 Синонимы можно создавать, удалять и ссылаться на них в динамическом SQL.  
  
## <a name="permissions"></a>Разрешения  
 Чтобы удалить синоним, пользователь должен выполнить, по крайней мере, одно из следующих условий. Пользователь должен являться:  
  
-   текущим владельцем синонима;  
  
-   участником, которому предоставлено разрешение CONTROL на синоним;  
  
-   участником, которому предоставлено разрешение ALTER SCHEMA на содержащую синоним схему.  
  
## <a name="examples"></a>Примеры  
 В следующем примере сначала создается синоним `MyProduct`, а затем этот синоним удаляется.  
  
```  
USE tempdb;  
GO  
-- Create a synonym for the Product table in AdventureWorks2012.  
CREATE SYNONYM MyProduct  
FOR AdventureWorks2012.Production.Product;  
GO  
-- Drop synonym MyProduct.  
USE tempdb;  
GO  
DROP SYNONYM MyProduct;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [CREATE SYNONYM (Transact-SQL)](../../t-sql/statements/create-synonym-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

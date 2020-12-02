---
description: USER_ID (Transact-SQL)
title: USER_ID (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- USER_ID
- USER_ID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- USER_ID function
- identification numbers [SQL Server]
- IDs [SQL Server], databases
- users [SQL Server], database ID numbers
- database IDs [SQL Server]
- identification numbers [SQL Server], databases
ms.assetid: 67fd29bc-eda9-4d4d-b148-5d3659181a43
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 50ffbb7ff9afa07b022f1c119afe07f43ab8ff6d
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "91379429"
---
# <a name="user_id-transact-sql"></a>USER_ID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Возвращает идентификационный номер пользователя базы данных.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Используйте вместо этого [DATABASE_PRINCIPAL_ID](../../t-sql/functions/database-principal-id-transact-sql.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
USER_ID ( [ 'user' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *user*  
 Используемое имя пользователя. Аргумент *user* имеет тип **nchar**. Если указано значение типа **char**, оно неявно преобразуется в тип **nchar**. Необходимо поставить скобки.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 **int**  
  
## <a name="remarks"></a>Remarks  
 Если аргумент *user* опущен, подразумевается текущий пользователь. Если параметр содержит слово NULL, то возвращается NULL. При вызове USER_ID после EXECUTE AS, USER_ID возвращает идентификатор олицетворенного контекста.  
  
 Если участник Windows, не сопоставленный с указанным пользователем базы данных, получает доступ к базе данных через участие в группе, функция USER_ID возвращает значение 0 (идентификатор пользователя public). Если такой участник создает объект без указания схемы, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создаст неявного пользователя и схему, сопоставленные с участником Windows. Пользователь, созданный таким образом, не может быть использован для подключения к базе данных. Вызовы функции USER_ID участником Windows, сопоставленным с неявным пользователем, возвратят идентификатор неявного пользователя.  
  
 Функцию USER_ID можно использовать в списке выбора, в предложении WHERE и в любом месте, где разрешается выражение. Дополнительные сведения см. в разделе [Выражения (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
 В следующем примере возвращается идентификатор пользователя `AdventureWorks2012` базы данных `Harold`.  
  
```sql
USE AdventureWorks2012;  
SELECT USER_ID('Harold');  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [USER_NAME (Transact-SQL)](../../t-sql/functions/user-name-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [DATABASE_PRINCIPAL_ID (Transact-SQL)](../../t-sql/functions/database-principal-id-transact-sql.md)   
 [Функции безопасности &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  

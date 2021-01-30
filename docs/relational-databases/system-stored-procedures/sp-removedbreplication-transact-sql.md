---
title: sp_removedbreplication (T-SQL)
description: Описывает sp_removedbreplication хранимую процедуру, используемую для удаления всех объектов репликации в базе данных публикации для репликации SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_removedbreplication
- sp_removedbreplication_TSQL
helpviewer_keywords:
- sp_removedbreplication
ms.assetid: cb98d571-d1eb-467b-91f7-a6e091009672
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39a95fbdfa80cb76e4b73f912ea38de1bc567906
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193096"
---
# <a name="sp_removedbreplication-transact-sql"></a>sp_removedbreplication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Эта хранимая процедура удаляет все объекты репликации в базе данных публикации в экземпляре издателя SQL Server или в базе данных подписки в экземпляре подписчика SQL Server. Выполняйте в соответствующей базе данных или укажите базу данных, где необходимо удалить объекты репликации, при выполнении в контексте другой базы данных в том же экземпляре. Эта процедура не удаляет объекты из других баз данных, например базы данных распространителя.  
  
> [!NOTE]  
>  Эта процедура должна использоваться только в случае, если другие методы удаления объектов репликации потерпели неудачу.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_removedbreplication [ [ @dbname = ] 'dbname' ]  
    [ , [ @type = ] type ]   
```  
  
## <a name="arguments"></a>Аргументы  
`[ @dbname = ] 'dbname'` Имя базы данных. Аргумент *dbname* имеет тип **sysname** и значение по умолчанию NULL. Если значение NULL, используется текущая база данных.  
  
`[ @type = ] type` Тип репликации, для которого удаляются объекты базы данных. *Type имеет тип* **nvarchar (5)** и может принимать одно из следующих значений.  
  
|||  
|-|-|  
|**Tran**|Удаляет публикуемые объекты репликации транзакций.|  
|**AutoMerge**|Удаляет публикуемые объекты репликации слиянием.|  
|**both** (по умолчанию)|Удаляет все публикуемые объекты репликации.|  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Замечания  
 **sp_removedbreplication** используется во всех типах репликации.  
  
 **sp_removedbreplication** удобно использовать при восстановлении реплицированной базы данных, не требующей восстановления объектов репликации.  
  
 **sp_removedbreplication** нельзя использовать для базы данных, которая помечена как доступная только для чтения.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** могут выполнять **sp_removedbreplication**.  
  
## <a name="examples"></a>Примеры

### <a name="a-remove-replication-objects-adventureworks2012replica-subscription-database"></a>A. Удаление объектов репликации, база данных подписки AdventureWorks2012Replica
 [!code-sql[HowTo#sp_removedbreplication](../../relational-databases/replication/codesnippet/tsql/sp-removedbreplication-t_1.sql)]  
  
### <a name="b-remove-replication-objects-adventureworksreplica-subscription-database"></a>Б. Удаление объектов репликации, база данных подписки Адвентуреворксреплика
  
```sql
-- Remove replication objects from the subscription database on MYSUB.  
DECLARE @subscriptionDB AS sysname  
SET @subscriptionDB = N'AdventureWorksReplica'  
  
-- Remove replication objects from a subscription database (if necessary).  
USE master  
EXEC sp_removedbreplication @subscriptionDB  
GO  
  
```  
  
## <a name="see-also"></a>См. также:  
 [Disable Publishing and Distribution](../../relational-databases/replication/disable-publishing-and-distribution.md)  (Отключение публикации и распространения)  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

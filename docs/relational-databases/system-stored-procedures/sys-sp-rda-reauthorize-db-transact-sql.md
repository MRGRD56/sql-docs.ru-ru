---
title: sys.sp_rda_reauthorize_db (Transact-SQL) | Документация Майкрософт
description: Узнайте, как использовать sys.sp_rda_reauthorize_db для восстановления прошедшего проверку подлинности подключения между локальной базой данных с поддержкой Stretch и удаленной базой данных.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sp_rda_reauthorize_db
- sp_rda_reauthorize_db_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_reauthorize_db stored procedure
ms.assetid: f6f3e4b2-8c72-4d23-a5de-fe671ca5c5cd
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f8585fb11e753f2ae1d2a28aed3567aa13d3456a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197587"
---
# <a name="syssp_rda_reauthorize_db-transact-sql"></a>sys.sp_rda_reauthorize_db (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Восстанавливает подключение с проверкой подлинности между локальной базой данных, для которой включено растяжение, и удаленной базой данных.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_rda_reauthorize_db @credential = @credential, @with_copy = @with_copy [ , @azure_servername = @azure_servername, @azure_databasename = @azure_databasename ]  
```  
  
## <a name="arguments"></a>Аргументы  
 @credential= *\@ учетные данные*  
 Учетные данные области базы данных, связанные с локальной базой данных с поддержкой Stretch.  
  
 @with_copy= *\@ with_copy*  
 Указывает, следует ли создавать копию удаленных данных и подключаться к копии (рекомендуется). *\@ with_copy* имеет бит.  
  
 @azure_servername= *\@ azure_servername*  
 Указывает имя сервера Azure, который содержит удаленные данные. *\@ azure_servername* имеет тип sysname.  
  
 @azure_databasename= *\@ azure_databasename*  
 Указывает имя базы данных Azure, которая содержит удаленные данные. *\@ azure_databasename* имеет тип sysname.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или >0 (сбой)  
  
## <a name="permissions"></a>Разрешения  
 Требуются db_owner разрешения.  
  
## <a name="remarks"></a>Замечания  
 При запуске [sys.sp_rda_reauthorize_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-rda-reauthorize-db-transact-sql.md) для повторного подключения к удаленной базе данных Azure эта операция автоматически сбрасывает режим запроса на LOCAL_AND_REMOTE, что является поведением по умолчанию для Stretch Database. То есть запросы возвращают результаты как из локальных, так и удаленных данных.  
  
## <a name="example"></a>Пример  
 В следующем примере восстанавливается подключение с проверкой подлинности между локальной базой данных, для которой включено растяжение, и удаленной базой данных. Он создает копию удаленных данных (рекомендуется) и подключается к новой копии.  
  
```sql  
DECLARE @credentialName nvarchar(128);   
SET @credentialName = N'<existing_database_scoped_credential_name>';   
EXEC sp_rda_reauthorize_db @credential = @credentialName, @with_copy = 1;  
  
```  
  
## <a name="see-also"></a>См. также:  
 [sys.sp_rda_deauthorize_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-rda-deauthorize-db-transact-sql.md)   
 [База данных Stretch](../../sql-server/stretch-database/stretch-database.md)  
  
  

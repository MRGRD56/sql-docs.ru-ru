---
description: GRANT, предоставление разрешений на учетные данные для базы данных (Transact-SQL)
title: GRANT, предоставление разрешений на учетные данные для базы данных (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- GRANT DATABASE SCOPED CREDENTIAL
- GRANT_DATABASE_SCOPED_CREDENTIAL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], database scoped credential
- permissions [SQL Server], database scoped credential
- database scoped credential [SQL Server], permissions
- GRANT statement, database scoped credentials
ms.assetid: 501f2c8a-6aeb-41af-bf0b-974d17af33c0
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 68d07c588109efb74554e30668ead9b6e73ef847
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465855"
---
# <a name="grant-database-scoped-credential-permissions-transact-sql"></a>GRANT, предоставление разрешений на учетные данные для базы данных (Transact-SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Предоставляет разрешения на учетные данные для базы данных. 
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
GRANT permission  [ ,...n ]    
    ON DATABASE SCOPED CREDENTIAL :: credential_name   
    TO principal [ ,...n ] [ WITH GRANT OPTION ]   
    [ AS granting_principal ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *permission*  
 Указывает разрешение, которое можно предоставить для учетных данных для базы данных. Перечислены ниже.  
  
 ON DATABASE SCOPED CREDENTIAL **::** _credential_name_  
 Указывает учетные данные для базы данных, для которых предоставляется разрешение. Квалификатор области "::" является обязательным.  
  
 *database_principal*  
 Участник, которому предоставляется разрешение. Это может быть:  
  
-   пользователь базы данных;  
-   роль базы данных;  
-   роль приложения;  
-   пользователь базы данных, сопоставленный с именем входа Windows;  
-   пользователь базы данных, сопоставленный с группой Windows;  
-   пользователь базы данных, сопоставленный с сертификатом;  
-   пользователь базы данных, сопоставленный с асимметричным ключом;  
-   пользователь базы данных, не сопоставленный с участником на уровне сервера.  
  
GRANT OPTION  
 Показывает, что участнику будет дана возможность предоставлять указанное разрешение другим участникам.  
  
AS *granting_principal*  
 Указывает участника, от которого участник, выполняющий данный запрос, наследует право на предоставление разрешения. Это может быть:  
  
-   пользователь базы данных;  
-   роль базы данных;  
-   роль приложения;  
-   пользователь базы данных, сопоставленный с именем входа Windows;  
-   пользователь базы данных, сопоставленный с группой Windows;  
-   пользователь базы данных, сопоставленный с сертификатом;  
-   пользователь базы данных, сопоставленный с асимметричным ключом;  
-   пользователь базы данных, не сопоставленный с участником на уровне сервера.  
  
## <a name="remarks"></a>Remarks  
 Учетные данные для базы данных — это защищаемый объект уровня базы данных, содержащийся в базе данных, являющейся родительским элементом в иерархии разрешений. Ниже перечислены наиболее специфичные и ограниченные разрешения (вместе с наиболее общими разрешениями, куда они входят неявно), которые могут быть предоставлены для учетных данных для базы данных.  
  
|Разрешения на учетные данные для базы данных|Содержится в разрешении на учетные данные для базы данных|Содержится в разрешении базы данных|  
|----------------------------|---------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Разрешения  
 Объект, предоставляющий разрешение (или участник, указанный параметром AS), должен иметь либо само разрешение, выданное с помощью параметра GRANT OPTION, либо разрешение более высокого уровня, которое неявно включает предоставляемое.  
  
 Если используется параметр AS, налагаются следующие дополнительные требования.  
  
|AS *granting_principal*|Необходимо дополнительное разрешение|  
|------------------------------|------------------------------------|  
|пользователь базы данных;|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных **db_securityadmin**, членство в предопределенной роли базы данных **db_owner** или членство в предопределенной роли сервера **sysadmin**.|  
|пользователь базы данных, сопоставленный с именем входа Windows;|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных **db_securityadmin**, членство в предопределенной роли базы данных **db_owner** или членство в предопределенной роли сервера **sysadmin**.|  
|пользователь базы данных, сопоставленный с группой Windows;|Членство в группе Windows, членство в предопределенной роли базы данных **db_securityadmin**, членство в предопределенной роли базы данных **db_owner** или членство в предопределенной роли сервера **sysadmin**.|  
|пользователь базы данных, сопоставленный с сертификатом;|Членство в предопределенной роли базы данных **db_securityadmin**, членство в предопределенной роли базы данных **db_owner** или членство в предопределенной роли сервера **sysadmin**.|  
|пользователь базы данных, сопоставленный с асимметричным ключом;|Членство в предопределенной роли базы данных **db_securityadmin**, членство в предопределенной роли базы данных **db_owner** или членство в предопределенной роли сервера **sysadmin**.|  
|Пользователь базы данных, не сопоставленный ни с одним участником на уровне сервера|Разрешение IMPERSONATE для пользователя, членство в предопределенной роли базы данных **db_securityadmin**, членство в предопределенной роли базы данных **db_owner** или членство в предопределенной роли сервера **sysadmin**.|  
|роль базы данных;|Разрешение ALTER на роль, членство в предопределенной роли базы данных **db_securityadmin**, предопределенной роли базы данных **db_owner** или предопределенной роли сервера **sysadmin**.|  
|Роль приложения|Разрешение ALTER на роль, членство в предопределенной роли базы данных **db_securityadmin**, предопределенной роли базы данных **db_owner** или предопределенной роли сервера **sysadmin**.|  
  
 Владельцы объектов могут предоставлять разрешения на объекты, которыми они владеют. Участники, имеющие разрешение CONTROL на защищаемый объект, могут предоставлять разрешение на этот защищаемый объект.  
  
 Участники, которым предоставлено разрешение CONTROL SERVER, такие как члены предопределенной роли сервера **sysadmin**, могут предоставлять любое разрешение для любого защищаемого объекта сервера. Участники, получившие разрешение CONTROL для базы данных, такие как члены предопределенной роли базы данных **db_owner**, могут предоставлять любое разрешение для любого защищаемого объекта в базе данных. Владельцы разрешения CONTROL, связанного со схемой, могут предоставлять любые разрешения на работу с любыми объектами, содержащимися в данной схеме.  
  
## <a name="see-also"></a>См. также:  
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE, отмена учетных данных для базы данных (Transact-SQL)](../../t-sql/statements/revoke-database-scoped-credential-transact-sql.md)   
 [DENY, запрет разрешений на учетные данные для базы данных (Transact-SQL)](../../t-sql/statements/deny-database-scoped-credential-transact-sql.md)   
 [Разрешения (ядро СУБД)](../../relational-databases/security/permissions-database-engine.md)   
 [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

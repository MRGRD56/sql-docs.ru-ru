---
description: DENY, запрет разрешений на сертификат (Transact-SQL)
title: DENY, запрет разрешений на сертификат (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- certificates [SQL Server], permissions
- permissions [SQL Server], certificates
- DENY statement, certificates
- denying permissions [SQL Server], certificates
ms.assetid: 5971ff9e-d6a4-414b-ae1f-819bc2e348f5
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: feec699a67378db5a7cdd1c169e507c72859b639
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123994"
---
# <a name="deny-certificate-permissions-transact-sql"></a>DENY, запрет разрешений на сертификат (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Запрещает разрешения на сертификат.  

  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
DENY permission  [ ,...n ]   
    ON CERTIFICATE :: certificate_name   
    TO principal [ ,...n ]  
    [ CASCADE ]  
    [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *permission*  
 Указывает разрешение, которое может быть запрещено для сертификата. Перечислены ниже.  
  
 ON CERTIFICATE **::** _certificate_name_  
 Указывает сертификат, для которого выполняется запрет разрешения. Квалификатор области "::" является обязательным.  
  
 *database_principal*  
 Задает участника, для которого запрещается разрешение. Это может быть:  
  
-   пользователь базы данных;  
  
-   роль базы данных;  
  
-   роль приложения;  
  
-   пользователь базы данных, сопоставленный с именем входа Windows;  
  
-   пользователь базы данных, сопоставленный с группой Windows;  
  
-   пользователь базы данных, сопоставленный с сертификатом;  
  
-   пользователь базы данных, сопоставленный с асимметричным ключом;  
  
-   пользователь базы данных, не сопоставленный с участником на уровне сервера.  
  
 CASCADE  
 Указывает, что запрещаемое разрешение также запрещается для других участников, которым оно было предоставлено данным участником.  
  
 *denying_principal*  
 Задает участника, от которого участник, выполняющий данный запрос, получает право на запрет разрешения. Это может быть:  
  
-   пользователь базы данных;  
  
-   роль базы данных;  
  
-   роль приложения;  
  
-   пользователь базы данных, сопоставленный с именем входа Windows;  
  
-   пользователь базы данных, сопоставленный с группой Windows;  
  
-   пользователь базы данных, сопоставленный с сертификатом;  
  
-   пользователь базы данных, сопоставленный с асимметричным ключом;  
  
-   пользователь базы данных, не сопоставленный с участником на уровне сервера.  
  
## <a name="remarks"></a>Remarks  
 Сертификат является защищаемым объектом уровня базы данных, содержащимся в базе данных, являющейся его родительским элементом в иерархии разрешений. Ниже перечислены наиболее специфичные и ограниченные разрешения (вместе с наиболее общими разрешениями, куда они входят неявно), которые могут быть запрещены для сертификата.  
  
|Разрешение сертификата|Содержится в разрешении сертификата|Содержится в разрешении базы данных|  
|----------------------------|---------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY CERTIFICATE|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения CONTROL для сертификата. При использовании предложения AS указанный участник должен являться владельцем сертификата.  
  
## <a name="see-also"></a>См. также:  
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [Разрешения (ядро СУБД)](../../relational-databases/security/permissions-database-engine.md)   
 [Участники (ядро СУБД)](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/create-application-role-transact-sql.md)   
 [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

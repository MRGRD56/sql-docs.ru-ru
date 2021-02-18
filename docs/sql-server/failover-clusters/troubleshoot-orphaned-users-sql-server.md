---
title: Диагностика потерянных пользователей
description: Потерянным считается пользователь, для которого в базе данных master перестали существовать сведения о входе в базу данных. В этой статье описаны процессы обнаружения потерянных пользователей и устранения этих проблем.
ms.custom: seo-lt-2019
ms.date: 07/14/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: high-availability
ms.topic: troubleshooting
helpviewer_keywords:
- orphaned users [SQL Server]
- logins [SQL Server], orphaned users
- troubleshooting [SQL Server], user accounts
- user accounts [SQL Server], orphaned users
- failover [SQL Server], managing metadata
- database mirroring [SQL Server], metadata
- users [SQL Server], orphaned
ms.assetid: 11eefa97-a31f-4359-ba5b-e92328224133
author: cawrites
ms.author: chadam
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 2583e238295c87b555c43f25b7fd68b03f3647c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354030"
---
# <a name="troubleshoot-orphaned-users-sql-server"></a>Диагностика потерянных пользователей (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Пользователь, утративший связь с учетной записью, на сервере [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] — это пользователь базы данных, созданный с использованием учетных данных для входа в базу данных **master** , которые в этой базе данных **master** уже не существуют. Это может произойти, если учетные данные для входа удалены или если база данных перемещена на другой сервер, где такие учетные данные для входа не существуют. В этом разделе описывается, как выполнять поиск пользователей, утративших связь с учетной записью, и повторно сопоставлять их с учетными данными для входа.  
  
> [!NOTE]  
>  Чтобы снизить для пользователей риск утраты связи с учетной записью, доступ к базам данных, которые могут быть перемещены, следует осуществлять в качестве пользователей автономной базы данных. Дополнительные сведения см. в разделе [Пользователи автономной базы данных — создание переносимой базы данных](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
## <a name="background"></a>Историческая справка  
 Чтобы субъект безопасности (удостоверение пользователя базы данных) на основе имени входа мог подключаться к базе данных, расположенной в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , такой субъект должен иметь допустимые учетные данные для входа в базу данных **master** . Эти учетные данные для входа используются при проверке подлинности, в ходе которой проверяется идентификатор субъекта и определяются разрешения на его подключение к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Имена входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в экземпляр сервера можно просмотреть в представлении каталога **sys.server_principals** и представлении совместимости **sys.sql_logins** .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используются для доступа к отдельным базам данных в качестве пользователей базы данных, сопоставленных с учетными данными [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Есть три исключения из этого правила:  
  
-   Пользователи автономной базы данных.  
  
     Пользователи автономной базы данных проходят проверку подлинности на уровне базы данных пользователей; они не связаны с учетными данными для входа. Мы рекомендуем этот вариант, так как базы данных становятся более переносимыми, а пользователи автономной базы данных не могут утратить связь с учетной записью. Но таких пользователей приходится создавать заново для каждой базы данных. Это решение может быть неэффективным в среде со множеством баз данных.  
  
-   Учетная запись **гостя** .  
  
     Если такая учетная запись включена в базе данных, она позволяет входить в базу данных в качестве [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] гостя **с использованием учетных данных** , которые не сопоставлены с пользователями базы данных. По умолчанию учетная запись **гостя** отключена.  
  
-   Членство в группе Microsoft Windows.  
  
     Имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , созданное из учетной записи пользователя Windows, может войти в базу данных, если пользователь Windows входит в группу Windows, являющуюся также пользователем базы данных.  
  
 Сведения о сопоставлении имени входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с пользователем базы данных хранятся в базе данных. Эти сведения включают в себя имя пользователя базы данных и идентификатор безопасности соответствующего имени входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Для авторизации в базе данных используются разрешения, установленные для этого пользователя базы данных.  
  
 Пользователь базы данных (на основе учетных данных), для которого в экземпляре сервера не определены или неправильно определены соответствующие учетные данные [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , не сможет подключиться к этому экземпляру. Такой пользователь называется *утратившим связь с учетной записью* базы данных на этом экземпляре сервера. Утрата связи с учетной записью происходит, если пользователь базы данных сопоставлен с идентификатором безопасности учетных данных, который отсутствует в экземпляре `master` . Кроме того, пользователь базы данных может утратить связь с учетной записью, если база данных была восстановлена или прикреплена к другому экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , для которого такие учетные данные не были созданы. Также пользователь базы данных может утратить связь с учетной записью после удаления соответствующих учетных данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Даже если учетные данные будут созданы заново, им будет присвоен другой идентификатор безопасности, поэтому связь пользователя с учетной записью будет утеряна.  
  
## <a name="detect-orphaned-users"></a>Обнаружение потерянных пользователей  

**Для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и PDW**

Чтобы обнаружить утративших связь с учетной записью пользователей в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на основе отсутствующих имен для проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , выполните в пользовательской базе данных следующую инструкцию:  
  
```  
SELECT dp.type_desc, dp.SID, dp.name AS user_name  
FROM sys.database_principals AS dp  
LEFT JOIN sys.server_principals AS sp  
    ON dp.SID = sp.SID  
WHERE sp.SID IS NULL  
    AND authentication_type_desc = 'INSTANCE';  
```  
  
 Выходные данные содержат список пользователей проверки подлинности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и соответствующие идентификаторы безопасности в текущей базе данных, которые не связаны ни с одним именем входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

**Для Базы данных SQL Azure и Azure Synapse Analytics**

Таблица `sys.server_principals` недоступна в базе данных SQL или Azure Synapse Analytics. Чтобы определить пользователей, утративших связь с учетной записью, в этих средах, выполните указанные ниже действия.

1. Подключитесь к базе данных `master` и выберите идентификаторы безопасности для имен входа с помощью следующего запроса:
    ```
    SELECT sid 
    FROM sys.sql_logins 
    WHERE type = 'S'; 
    ```

2. Подключитесь к пользовательской базе данных и просмотрите идентификаторы безопасности пользователей в таблице `sys.database_principals` с помощью следующего запроса:

    ```
    SELECT name, sid, principal_id
    FROM sys.database_principals 
    WHERE type = 'S' 
      AND name NOT IN ('guest', 'INFORMATION_SCHEMA', 'sys')
      AND authentication_type_desc = 'INSTANCE';
    ```

3. Сравните эти два списка, чтобы определить, есть ли в таблице `sys.database_principals` пользовательской базы данных идентификаторы безопасности, для которых нет соответствующих идентификаторов безопасности в таблице `sql_logins` базы данных master. 
  
## <a name="resolve-an-orphaned-user"></a>Устранение проблем с потерянным пользователем  
Чтобы восстановить отсутствующие учетные данные, используйте инструкцию [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) с параметром SID в базе данных master. При этом укажите `SID` пользователя базы данных, полученный в предыдущем разделе:  
  
```  
CREATE LOGIN <login_name>   
WITH PASSWORD = '<use_a_strong_password_here>',  
SID = <SID>;  
```  
  
 Чтобы сопоставить пользователя, утратившего связь с учетной записью, с уже существующими учетными данными в базе данных **master**, выполните инструкцию [ALTER USER](../../t-sql/statements/alter-user-transact-sql.md) в пользовательской базе данных, указав имя входа.  
  
```  
ALTER USER <user_name> WITH Login = <login_name>;  
```  
  
 После повторного создания отсутствующих учетных данных пользователь сможет получать доступ к базе данных с использованием указанного пароля. Пользователь сможет затем изменить пароль для входа с помощью инструкции ALTER LOGIN.  
  
```  
ALTER LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';  
```  
  
> [!IMPORTANT]  
>  Любой пользователь может изменить свой пароль. Изменять пароли для других пользователей могут только пользователи с учетными данными, для которых задано разрешение `ALTER ANY LOGIN` . Однако только члены роли **sysadmin** могут изменять пароли членов роли **sysadmin** .  
  
## <a name="see-also"></a>См. также:  
 [CREATE LOGIN (Transact-SQL)](../../t-sql/statements/create-login-transact-sql.md)   
 [ALTER USER (Transact-SQL)](../../t-sql/statements/alter-user-transact-sql.md)   
 [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_change_users_login (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-change-users-login-transact-sql.md)   
 [sp_addlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [Хранимая процедура sp_grantlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_password (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-password-transact-sql.md)   
 [sys.sysusers (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-sysusers-transact-sql.md)   
 [sys.sql_logins](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md) [sys.syslogins (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-syslogins-transact-sql.md)  
  
  

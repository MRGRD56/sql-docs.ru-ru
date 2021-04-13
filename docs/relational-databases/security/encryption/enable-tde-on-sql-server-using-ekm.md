---
title: Включение прозрачного шифрования данных в SQL Server с помощью расширенного управления ключами | Документация Майкрософт
description: Включение прозрачного шифрования данных в SQL Server для защиты ключа базы данных с использованием асимметричного ключа, хранящегося в модуле расширенного управления ключами, при помощи Transact-SQL.
ms.custom: ''
ms.date: 07/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], TDE using an EKM
- TDE, EKM how to
- EKM, TDE how to
- Transparent Data Encryption, using EKM
ms.assetid: b892e7a7-95bd-4903-bf54-55ce08e225af
author: shohamMSFT
ms.author: shohamd
ms.openlocfilehash: ab339cd8fed2456d8794926b59f2bf730ec30b0c
ms.sourcegitcommit: cfffd03fe39b04034fa8551165476e53c4bd3c3b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2021
ms.locfileid: "107298897"
---
# <a name="enable-tde-on-sql-server-using-ekm"></a>Enable TDE on SQL Server Using EKM (Включение прозрачного шифрования данных в SQL Server с помощью расширенного управления ключами)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В этой статье описано, как включить прозрачное шифрование данных (TDE) в [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)], чтобы защитить ключ шифрования базы данных с помощью асимметричного ключа, хранящегося в модуле расширенного управления ключами (EKM) при помощи [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 При использовании TDE хранилище всей базы данных шифруется с помощью симметричного ключа, который называется ключом шифрования базы данных. Ключ шифрования базы данных можно также защитить с помощью сертификата, защищенного главным ключом базы данных master. Дополнительные сведения о защите ключа шифрования базы данных с помощью главного ключа базы данных см. в разделе [Прозрачное шифрование данных (TDE)](../../../relational-databases/security/encryption/transparent-data-encryption.md). Дополнительные сведения о настройке прозрачного шифрования данных при выполнении [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в виртуальной машине Azure см. в разделе [Расширенное управление ключами с помощью хранилища ключей Azure (SQL Server)](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md). Сведения о настройке прозрачного шифрования данных с помощью ключа в хранилище ключей Azure см. в разделе [Использование соединителя SQL Server с компонентами шифрования SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md). 

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Для создания ключа шифрования базы данных и шифрования в базе данных необходимо быть привилегированным пользователем (например, системным администратором). Этот пользователь должен также проходить проверку подлинности в модуле расширенного управления ключами.  
  
-   При запуске компонент [!INCLUDE[ssDE](../../../includes/ssde-md.md)] должен открыть базу данных. Чтобы это сделать, необходимо создать учетные данные, которые будут проверяться по расширенному управлению ключами, и добавить их к имени входа, основанном на асимметричном ключе. Пользователи не смогут войти в систему с этим именем входа, но компонент [!INCLUDE[ssDE](../../../includes/ssde-md.md)] сможет пройти проверку подлинности на устройстве расширенного управления ключами.  
  
-   Если асимметричный ключ, хранящийся в поставщике расширенного управления ключами, утерян, то [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]не сможет открыть базу данных. Если поставщик расширенного управления ключами позволяет создать резервную копию асимметричного ключа, создайте его резервную копию и сохраните ее в надежном месте.  
  
-   Параметры и настройки, которые требуются поставщиком расширенного управления ключами, могут отличаться от приведенного ниже примера кода. Дополнительные сведения см. в документации поставщика расширенного управления ключами.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 В этой статье используются следующие разрешения:  
  
-   Для изменения параметра конфигурации и выполнения инструкции RECONFIGURE должно быть предоставлено разрешение ALTER SETTINGS на уровне сервера. Разрешение ALTER SETTINGS неявным образом предоставлено предопределенным ролям сервера **sysadmin** и **serveradmin** .  
  
-   Требуется разрешение ALTER ANY CREDENTIAL.  
  
-   Необходимо разрешение ALTER ANY LOGIN.  
  
-   Требуется разрешение CREATE ASYMMETRIC KEY.  
  
-   Требуется разрешение CONTROL в базе данных для ее шифрования.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-enable-tde-using-ekm"></a>Включение TDE с расширенным управлением ключами  
  
1.  Скопируйте файлы поставщика расширенного управления ключами в соответствующую папку на компьютере [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . В этом примере используется папка **C:\EKM** .  
  
2.  Установите на компьютер сертификаты в соответствии с требованиями поставщика расширенного управления ключами.  
  
    > [!NOTE]  
    >  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не поддерживает поставщик расширенного управления ключами. Каждый поставщик расширенного управления ключами может иметь разные процедуры установки, настройки и авторизации пользователей.  Проконсультируйтесь с документацией поставщика расширенного управления ключами, чтобы завершить этот шаг.  
  
3.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
4.  На стандартной панели выберите пункт **Создать запрос**.  
  
5.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    -- Enable advanced options.  
    sp_configure 'show advanced options', 1 ;  
    GO  
    RECONFIGURE ;  
    GO  
    -- Enable EKM provider  
    sp_configure 'EKM provider enabled', 1 ;  
    GO  
    RECONFIGURE ;  
    GO  
    -- Create a cryptographic provider, which we have chosen to call "EKM_Prov," based on an EKM provider  
  
    CREATE CRYPTOGRAPHIC PROVIDER EKM_Prov   
    FROM FILE = 'C:\EKM_Files\KeyProvFile.dll' ;  
    GO  
  
    -- Create a credential that will be used by system administrators.  
    CREATE CREDENTIAL sa_ekm_tde_cred   
    WITH IDENTITY = 'Identity1',   
    SECRET = 'q*gtev$0u#D1v'   
    FOR CRYPTOGRAPHIC PROVIDER EKM_Prov ;  
    GO  
  
    -- Add the credential to a high privileged user such as your   
    -- own domain login in the format [DOMAIN\login].  
    ALTER LOGIN Contoso\Mary  
    ADD CREDENTIAL sa_ekm_tde_cred ;  
    GO  
    -- create an asymmetric key stored inside the EKM provider  
    USE master ;  
    GO  
    CREATE ASYMMETRIC KEY ekm_login_key   
    FROM PROVIDER [EKM_Prov]  
    WITH ALGORITHM = RSA_512,  
    PROVIDER_KEY_NAME = 'SQL_Server_Key' ;  
    GO  
  
    -- Create a credential that will be used by the Database Engine.  
    CREATE CREDENTIAL ekm_tde_cred   
    WITH IDENTITY = 'Identity2'   
    , SECRET = 'jeksi84&sLksi01@s'   
    FOR CRYPTOGRAPHIC PROVIDER EKM_Prov ;  
  
    -- Add a login used by TDE, and add the new credential to the login.  
    CREATE LOGIN EKM_Login   
    FROM ASYMMETRIC KEY ekm_login_key ;  
    GO  
    ALTER LOGIN EKM_Login   
    ADD CREDENTIAL ekm_tde_cred ;  
    GO  
  
    -- Create the database encryption key that will be used for TDE.  
    USE AdventureWorks2012 ;  
    GO  
    CREATE DATABASE ENCRYPTION KEY  
    WITH ALGORITHM  = AES_128  
    ENCRYPTION BY SERVER ASYMMETRIC KEY ekm_login_key ;  
    GO  
  
    -- Alter the database to enable transparent data encryption.  
    ALTER DATABASE AdventureWorks2012   
    SET ENCRYPTION ON ;  
    GO  
    ```  
  
 Дополнительные сведения см. в следующих разделах:  
  
-   [sp_configure (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
-   [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
-   [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)  
  
-   [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
-   [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
-   [CREATE DATABASE ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)  
  
-   [ALTER LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-login-transact-sql.md)  
  
-   [ALTER DATABASE (Transact-SQL)](../../../t-sql/statements/alter-database-transact-sql.md)  
  
## <a name="see-also"></a>См. также:  
 [Прозрачное шифрование данных в Базе данных SQL Azure](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
  

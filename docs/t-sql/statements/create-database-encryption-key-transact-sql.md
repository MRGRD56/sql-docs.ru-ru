---
description: CREATE DATABASE ENCRYPTION KEY (Transact-SQL)
title: CREATE DATABASE ENCRYPTION KEY (Transact-SQL)
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE_ENCRYPTION_KEY_TSQL
- ENCRYPTION_KEY_TSQL
- sql13.swb.dbencryptionkeyo.f1
- ENCRYPTION KEY
- DATABASE ENCRYPTION KEY
- CREATE_DATABASE_ENCRYPTION_KEY_TSQL
- CREATE DATABASE ENCRYPTION KEY
- sql13.swb.dbencryptionkeyg.f1
- CREATE DATABASE ENCRYPTION
- CREATE_DATABASE_ENCRYPTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- database encryption key
- CREATE DATABASE ENCRYPTION KEY statement
- database encryption key, create
ms.assetid: 2ee95a32-5140-41bd-9ab3-a947b9990688
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f0bf20d9ac242727ec98a3d617c07ea5588b8fb9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192715"
---
# <a name="create-database-encryption-key-transact-sql"></a>CREATE DATABASE ENCRYPTION KEY (Transact-SQL)

[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

 Создает ключ шифрования, используемый для прозрачного шифрования базы данных. Дополнительные сведения о прозрачном шифровании баз данных см. в статье [Прозрачное шифрование данных (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md).  
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server  

CREATE DATABASE ENCRYPTION KEY  
       WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   ENCRYPTION BY SERVER   
    {  
        CERTIFICATE Encryptor_Name |  
        ASYMMETRIC KEY Encryptor_Name  
    }  
[ ; ]  
```  
  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  

CREATE DATABASE ENCRYPTION KEY  
       WITH ALGORITHM = { AES_128 | AES_192 | AES_256 | TRIPLE_DES_3KEY }  
   ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name   
[ ; ]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

WITH ALGORITHM = { AES_128 \| AES_192 \| AES_256 \| TRIPLE_DES_3KEY  }  
Определяет алгоритм шифрования, который используется для ключа шифрования.

> [!WARNING]
> Начиная с версии SQL Server 2016 все алгоритмы, отличные от AES_128, AES_192 и AES_256, являются нерекомендуемыми. Чтобы использовать старые алгоритмы (что не рекомендуется), необходимо установить уровень совместимости базы данных 120 или ниже.  
  
ENCRYPTION BY SERVER CERTIFICATE Encryptor_Name  
Указывает имя шифратора, используемого для шифрования ключа шифрования базы данных.  
  
ENCRYPTION BY SERVER ASYMMETRIC KEY Encryptor_Name  
Указывает имя асимметричного ключа, используемого для шифрования ключа шифрования базы данных. Чтобы зашифровать ключ шифрования базы данных с помощью асимметричного ключа, используемый асимметричный ключ должен находиться в поставщике расширенного управления ключами.  
  
## <a name="remarks"></a>Remarks  
Прежде чем зашифровать базу данных с помощью *прозрачного шифрования базы данных* (TDE), необходимо создать ключ шифрования базы данных. При выполнении прозрачного шифрования базы данных вся база данных шифруется на уровне файлов без каких-либо специальных изменений кода. Сертификат или асимметричный ключ, который используется для шифрования ключа шифрования базы данных, должен находиться в системной базе данных master.  
  
Инструкции шифрования базы данных разрешены только для пользовательских баз данных.  
  
Ключ шифрования базы данных нельзя экспортировать из базы данных. Он доступен только для системы, пользователей, имеющих разрешение на отладку на сервере, и пользователей, имеющих доступ к сертификатам, с использованием которых выполняется шифрование и расшифровка ключа шифрования базы данных.  
  
Нет необходимости повторно создавать ключ шифрования базы данных, когда меняется владелец базы данных (dbo).  
  
Ключ шифрования базы данных автоматически создается для базы данных [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Не нужно создавать ключ с помощью инструкции CREATE DATABASE ENCRYPTION KEY.  
  
## <a name="permissions"></a>Разрешения  
Требуется разрешение CONTROL на базу данных и разрешение VIEW DEFINITION на сертификат или асимметричный ключ, используемый при шифровании ключа шифрования базы данных.  
  
## <a name="examples"></a>Примеры  
Дополнительные примеры использования прозрачного шифрования данных см. в разделах [Прозрачное шифрование данных (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md), [Включение прозрачного шифрования данных в SQL Server с помощью расширенного управления ключами](../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md) и [Расширенное управление ключами с помощью хранилища ключей Azure (SQL Server)](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  
  
В следующем примере создается ключ шифрования базы данных с помощью алгоритма `AES_256` и защищается закрытый ключ с помощью сертификата с именем `MyServerCert`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE DATABASE ENCRYPTION KEY  
WITH ALGORITHM = AES_256  
ENCRYPTION BY SERVER CERTIFICATE MyServerCert;  
GO  
```  
  
## <a name="see-also"></a>См. также:  
[Прозрачное шифрование данных (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)   
[Шифрование SQL Server](../../relational-databases/security/encryption/sql-server-encryption.md)   
[Ключи шифрования базы данных и SQL Server (ядро СУБД)](../../relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine.md)   
[Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)   
[Параметры ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
[ALTER DATABASE ENCRYPTION KEY (Transact-SQL)](../../t-sql/statements/alter-database-encryption-key-transact-sql.md)   
[DROP DATABASE ENCRYPTION KEY (Transact-SQL)](../../t-sql/statements/drop-database-encryption-key-transact-sql.md)   
[sys.dm_database_encryption_keys (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md)  
    

---
description: DECRYPTBYKEY (Transact-SQL)
title: DECRYPTBYKEY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DecryptByKey_TSQL
- DECRYPTBYKEY
dev_langs:
- TSQL
helpviewer_keywords:
- symmetric keys [SQL Server], DecryptByKey function
- decryption [SQL Server], keys
- decryption [SQL Server], symmetric keys
- DECRYPTBYKEY function
ms.assetid: 6edf121f-ac62-4dae-90e6-6938f32603c9
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: e9c7ece32a16f1b37b19ad4164e9755f200b776d
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575294"
---
# <a name="decryptbykey-transact-sql"></a>DECRYPTBYKEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

Эта функция расшифровывает данные с помощью симметричного ключа.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]   
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
DecryptByKey ( { 'ciphertext' | @ciphertext }   
    [ , add_authenticator, { authenticator | @authenticator } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
*ciphertext*  
Переменная типа **varbinary**, содержащая данные, зашифрованные с помощью ключа.  
  
**\@ciphertext**  
Переменная типа **varbinary**, содержащая данные, зашифрованные с помощью ключа.  
  
 *add_authenticator*  
Указывает, включен ли исходный процесс шифрования и зашифрована ли структура проверки подлинности вместе с данными в виде открытого текста. Значение должно соответствовать значению, переданному функции [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md) во время шифрования данных. *add_authenticator* имеет тип данных **int**.  
  
 *authenticator*  
Данные, используемые в качестве основы для создания структуры проверки подлинности. Значение должно соответствовать значению, заданному функции [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *authenticator* имеет тип данных **sysname**.  

**\@authenticator**  
Переменная, содержащая данные, из которых формируется структура проверки подлинности. Значение должно соответствовать значению, заданному функции [ENCRYPTBYKEY (Transact-SQL)](./encryptbykey-transact-sql.md). *\@authenticator* имеет тип данных **sysname**.  

## <a name="return-types"></a>Типы возвращаемых данных  
Переменная типа **varbinary** с максимальным размером 8000 байт. `DECRYPTBYKEY` возвращает NULL, если симметричный ключ, используемый для шифрования данных, не является открытым или если *ciphertext* имеет значение NULL.  
  
## <a name="remarks"></a>Комментарии  
`DECRYPTBYKEY` использует симметричный ключ. Этот ключ уже должен быть открыт в базе данных. `DECRYPTBYKEY` допускает несколько одновременно открытых ключей. Открывать ключ непосредственно перед расшифровкой зашифрованного текста необязательно.  
  
Симметричные шифрование и расшифровка обычно выполняются относительно быстро и хорошо подходят для операций, связанных с большими объемами данных.  

Вызов `DECRYPTBYKEY` должен быть выполнен в контексте базы данных, содержащей ключ шифрования. Это можно гарантировать путем вызова `DECRYPTBYKEY` из объекта (например, представления, хранимой процедуры или функции), который находится в этой базе данных. 
  
## <a name="permissions"></a>Разрешения  
Симметричный ключ уже должен быть открыт в текущем сеансе. Дополнительные сведения см. в статье [OPEN SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/open-symmetric-key-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-decrypting-by-using-a-symmetric-key"></a>A. Декодирование с помощью симметричного ключа  
В этом примере зашифрованный текст расшифровывается с помощью симметричного ключа.  
  
```sql  
-- First, open the symmetric key with which to decrypt the data.  
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037;  
GO  
  
-- Now list the original ID, the encrypted ID, and the   
-- decrypted ciphertext. If the decryption worked, the original  
-- and the decrypted ID will match.  
SELECT NationalIDNumber, EncryptedNationalID   
    AS 'Encrypted ID Number',  
    CONVERT(nvarchar, DecryptByKey(EncryptedNationalID))   
    AS 'Decrypted ID Number'  
    FROM HumanResources.Employee;  
GO  
```  
  
### <a name="b-decrypting-by-using-a-symmetric-key-and-an-authenticating-hash"></a>Б. Декодирование с помощью симметричного ключа и цифровой подписи  
В этом примере расшифровываются данные, изначально зашифрованные вместе со структурой проверки подлинности.  
  
```sql  
-- First, open the symmetric key with which to decrypt the data  
OPEN SYMMETRIC KEY CreditCards_Key11  
   DECRYPTION BY CERTIFICATE Sales09;  
GO  
  
-- Now list the original card number, the encrypted card number,  
-- and the decrypted ciphertext. If the decryption worked,   
-- the original number will match the decrypted number.  
SELECT CardNumber, CardNumber_Encrypted   
    AS 'Encrypted card number', CONVERT(nvarchar,  
    DecryptByKey(CardNumber_Encrypted, 1 ,   
    HashBytes('SHA1', CONVERT(varbinary, CreditCardID))))   
    AS 'Decrypted card number' FROM Sales.CreditCard;  
GO  
  
```  

### <a name="c-fail-to-decrypt-when-not-in-the-context-of-database-with-key"></a>В. Ошибка расшифровки не в контексте базы данных с помощью ключа
В следующем примере показано, что `DECRYPTBYKEY` должна выполняться в контексте базы данных, которая содержит ключ. Строку не удастся расшифровать, если `DECRYPTBYKEY` выполняется в базе данных Master; результатом является значение NULL. 

```sql
-- Create the database
CREATE DATABASE TestingDecryptByKey
GO

USE [TestingDecryptByKey]

-- Create the table and view
CREATE TABLE TestingDecryptByKey.dbo.Test(val VARBINARY(8000) NOT NULL);
GO
CREATE VIEW dbo.TestView AS SELECT CAST(DecryptByKey(val) AS VARCHAR(30)) AS DecryptedVal FROM TestingDecryptByKey.dbo.Test;
GO

-- Create the key, and certificate
USE TestingDecryptByKey;
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'ItIsreallyLong1AndSecured!Passsword#';
CREATE CERTIFICATE TestEncryptionCertificate WITH SUBJECT = 'TestEncryption';
CREATE SYMMETRIC KEY TestEncryptSymmmetricKey WITH ALGORITHM = AES_256, IDENTITY_VALUE = 'It is place for test',
KEY_SOURCE = 'It is source for test' ENCRYPTION BY CERTIFICATE TestEncryptionCertificate;

-- Insert rows into the table
DECLARE @var VARBINARY(8000), @Val VARCHAR(30);
SELECT @Val = '000-123-4567';
OPEN SYMMETRIC KEY TestEncryptSymmmetricKey DECRYPTION BY CERTIFICATE TestEncryptionCertificate;
SELECT @var = EncryptByKey(Key_GUID('TestEncryptSymmmetricKey'), @Val);
SELECT CAST(DecryptByKey(@var) AS VARCHAR(30)), @Val;
INSERT INTO dbo.Test VALUES(@var);
GO

-- Switch to master
USE [Master];
GO

-- Results show the date inserted
SELECT DecryptedVal FROM TestingDecryptByKey.dbo.TestView;

-- Results are NULL because we are not in the context of the TestingDecryptByKey Database
SELECT CAST(DecryptByKey(val) AS VARCHAR(30)) AS DecryptedVal FROM TestingDecryptByKey.dbo.Test;
GO

-- Clean up resources
USE TestingDecryptByKey;

DROP SYMMETRIC KEY TestEncryptSymmmetricKey REMOVE PROVIDER KEY;
DROP CERTIFICATE TestEncryptionCertificate;

Use [Master]
DROP DATABASE TestingDecryptByKey;
GO
```

  
## <a name="see-also"></a>См. также  
 [ENCRYPTBYKEY (Transact-SQL)](../../t-sql/functions/encryptbykey-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Выбор алгоритма шифрования](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
  
  

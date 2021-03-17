---
description: ENCRYPTBYKEY (Transact-SQL)
title: ENCRYPTBYKEY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ENCRYPTBYKEY_TSQL
- ENCRYPTBYKEY
dev_langs:
- TSQL
helpviewer_keywords:
- authenticators [SQL Server]
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], ENCRYPTBYKEY function
- ENCRYPTBYKEY function
ms.assetid: 0e11f8c5-f79d-46c1-ab11-b68ef05d6787
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: b3505930ab119a5fd43d642c9671e88c4d5ca839
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575307"
---
# <a name="encryptbykey-transact-sql"></a>ENCRYPTBYKEY (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  Производит шифрование данных при использовании симметричного ключа.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)] 
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
  
EncryptByKey ( key_GUID , { 'cleartext' | @cleartext }  
    [, { add_authenticator | @add_authenticator }  
     , { authenticator | @authenticator } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *key_GUID*  
 Идентификатор GUID ключа, используемого для шифрования *cleartext*. **uniqueidentifier**.  
  
 '*cleartext*'  
 Данные, которые шифруются ключом.  
  
 @cleartext  
 Переменная типа **nvarchar**, **char**, **varchar**, **binary**, **varbinary** или **nchar**, содержащая данные, которые будут зашифрованы с помощью ключа.  
  
 *add_authenticator*  
 Указывает, будет ли вместе с аргументом *cleartext* зашифрована структура проверки подлинности. При использовании структуры проверки подлинности аргумент должен иметь значение 1. **int**.  
  
 @add_authenticator  
 Указывает, будет ли вместе с аргументом *cleartext* зашифрована структура проверки подлинности. При использовании структуры проверки подлинности аргумент должен иметь значение 1. **int**.  
  
 *authenticator*  
 Данные, на основе которых формируется структура проверки подлинности. **sysname**.  
  
 @authenticator  
 Переменная, содержащая данные, на основе которых формируется структура проверки подлинности.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Переменная типа **varbinary** с максимальным размером 8000 байт.  
  
 Возвращает значение NULL, если ключ не открыт, не существует или является устаревшим ключом RC4, а база данных не находится на уровне совместимости 110 или выше.  
 
 Возвращает NULL, если *cleartext* имеет значение NULL.
  
## <a name="remarks"></a>Комментарии  
 Функция EncryptByKey использует симметричный ключ. Этот ключ должен быть открыт. Если симметричный ключ уже открыт в текущем сеансе, не нужно открывать его снова в контексте запроса.  
  
 Структура проверки подлинности позволяют исключить прямую подмену зашифрованных полей. Например, рассмотрим следующую таблицу с платежной ведомостью.  
  
|ИД_сотрудника|Стандартная_должность|Базовый_оклад|  
|------------------|---------------------|---------------|  
|345|Помощник копировщика|Fskj%7^edhn00|  
|697|Финансовый директор|M0x8900f56543|  
|694|Контролер ввода данных|Cvc97824%^34f|  
  
 Не взламывая шифр, злоумышленник может получить важные сведения из контекста, в котором хранятся зашифрованные данные. Но, поскольку оплата труда финансового директора заведомо выше, чем оплата труда помощника копировщика, из этого следует, что значение, зашифрованное как «M0x8900f56543», должно быть больше, чем значение, зашифрованное как «Fskj%7^edhn00». В таком случае любой пользователь с разрешением ALTER для данной таблицы может дать помощнику копировщика повышение, заменив его данные в поле Базовый_оклад простым копированием данных из поля Базовый_оклад финансового директора. Этот прием называется прямой подменой и обходит шифрование как таковое.  
  
 Атаку методом подмены значения целиком можно предотвратить, добавив к данным перед шифрованием сведения о контексте. Эти сведения будут использоваться для гарантии того, что открытый текст не был перенесен с другого места.  
  
 Если при шифровании данных указана структура проверки подлинности, то для дешифровки данных функцией DecryptByKey потребуются эти же данная структура проверки подлинности. При шифровании хэш данных проверки подлинности шифруется вместе с данными. При дешифровке эта же структура проверки подлинности должны быть переданы функции DecryptByKey. Если эти данные не совпадают, дешифровка завершится ошибкой. Это будет означать, что значение перемещено с другого места со времени шифрования. В качестве структуры проверки подлинности рекомендуется использовать столбец, содержащий уникальное и неизменное значение. Если значение структуры проверки подлинности изменится, можно утратить доступ к данным.  
  
 Симметричное кодирование и декодирование осуществляется относительно быстро и подходит для работы с большими объемами данных.  
  
> [!IMPORTANT]  
>  Использование функций шифрования [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] совместно с параметром ANSI_PADDING OFF может привести к потере данных из-за неявных преобразований. Дополнительные сведения об ANSI_PADDING см. в статье [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md).  
  
## <a name="examples"></a>Примеры  
 Возможности, продемонстрированные в следующих примерах, работают с ключами и сертификатами, созданными в разделе [Шифрование столбца данных](../../relational-databases/security/encryption/encrypt-a-column-of-data.md).  
  
### <a name="a-encrypting-a-string-with-a-symmetric-key"></a>A. Шифрование строки симметричным ключом  
 Следующий пример показывает, как добавить столбец к таблице `Employee`, а затем зашифровать значение номера социального страхования, который хранится в столбце `NationalIDNumber`.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
-- Create a column in which to store the encrypted data.  
ALTER TABLE HumanResources.Employee  
    ADD EncryptedNationalIDNumber varbinary(128);   
GO  
  
-- Open the symmetric key with which to encrypt the data.  
OPEN SYMMETRIC KEY SSN_Key_01  
   DECRYPTION BY CERTIFICATE HumanResources037;  
  
-- Encrypt the value in column NationalIDNumber with symmetric key  
-- SSN_Key_01. Save the result in column EncryptedNationalIDNumber.  
UPDATE HumanResources.Employee  
SET EncryptedNationalIDNumber  
    = EncryptByKey(Key_GUID('SSN_Key_01'), NationalIDNumber);  
GO  
```  
  
### <a name="b-encrypting-a-record-together-with-an-authentication-value"></a>Б. Шифрование записи вместе со значением проверки подлинности  
  
```sql 
USE AdventureWorks2012;  
  
-- Create a column in which to store the encrypted data.  
ALTER TABLE Sales.CreditCard   
    ADD CardNumber_Encrypted varbinary(128);   
GO  
  
-- Open the symmetric key with which to encrypt the data.  
OPEN SYMMETRIC KEY CreditCards_Key11  
    DECRYPTION BY CERTIFICATE Sales09;  
  
-- Encrypt the value in column CardNumber with symmetric   
-- key CreditCards_Key11.  
-- Save the result in column CardNumber_Encrypted.    
UPDATE Sales.CreditCard  
SET CardNumber_Encrypted = EncryptByKey(Key_GUID('CreditCards_Key11'),   
    CardNumber, 1, CONVERT( varbinary, CreditCardID) );  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [DECRYPTBYKEY (Transact-SQL)](../../t-sql/functions/decryptbykey-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [ALTER SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/alter-symmetric-key-transact-sql.md)   
 [DROP SYMMETRIC KEY (Transact-SQL)](../../t-sql/statements/drop-symmetric-key-transact-sql.md)   
 [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [HASHBYTES (Transact-SQL)](../../t-sql/functions/hashbytes-transact-sql.md)  
  
  

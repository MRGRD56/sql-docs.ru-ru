---
description: ENCRYPTBYPASSPHRASE (Transact-SQL)
title: ENCRYPTBYPASSPHRASE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ENCRYPTBYPASSPHRASE
- ENCRYPTBYPASSPHRASE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTBYPASSPHRASE function
- encryption [SQL Server], symmetric keys
- symmetric keys [SQL Server], ENCRYPTBYPASSPHRASE function
ms.assetid: f8dbb9e6-94d6-40d7-8b38-6833a409d597
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2e9af2f7806a7d4435bd5bd2d417a3a217afc3df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199556"
---
# <a name="encryptbypassphrase-transact-sql"></a>ENCRYPTBYPASSPHRASE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Шифрование данных с помощью парольной фразы с использованием алгоритма TRIPLE DES и 128-битного ключа.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
EncryptByPassPhrase ( { 'passphrase' | @passphrase }   
    , { 'cleartext' | @cleartext }  
  [ , { add_authenticator | @add_authenticator }  
    , { authenticator | @authenticator } ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *passphrase*  
 Парольная фраза, из которой создается симметричный ключ.  
  
 @passphrase  
 Переменная типа **nvarchar**, **char**, **varchar**, **binary**, **varbinary** или **nchar**, содержащая парольную фразу, по которой создается симметричный ключ.  
  
 *cleartext*  
 Открытый текст для шифрования.  
  
 @cleartext  
 Переменная типа **nvarchar**, **char**, **varchar**, **binary**, **varbinary** или **nchar**, содержащая открытый текст. Максимальный размер 8 000 байт.  
  
 *add_authenticator*  
 Указывает, будут ли вместе с аргументом cleartext зашифрована структура проверки подлинности. Значение 1, если структура проверки подлинности будет добавлено. **int**.  
  
 @add_authenticator  
 Показывает, будет ли зашифрован хэш вместе с открытым текстом.  
  
 *authenticator*  
 Данные, из которых формируется структура проверки подлинности. **sysname**.  
  
 @authenticator  
 Переменная, содержащая данные, из которых формируется структура проверки подлинности.  
  
## <a name="return-types"></a>Типы возвращаемых данных  
 Переменная типа **varbinary** с максимальным размером 8000 байт.  
  
## <a name="remarks"></a>Комментарии  
 Парольная фраза представляет собой пароль, содержащий пробелы. Преимущество использования парольной фразы состоит в том, что смысловая фраза или предложение легче для запоминания, чем длинная строка символов.  
  
 Данная функция не проверяет сложность пароля.  
  
## <a name="examples"></a>Примеры  
 Следующий пример обновляет запись в таблице `SalesCreditCard` и шифрует номер кредитной карты, хранящийся в столбце `CardNumber_EncryptedbyPassphrase`, с помощью первичного ключа в качестве структуры проверки подлинности.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create a column in which to store the encrypted data.  
ALTER TABLE Sales.CreditCard   
    ADD CardNumber_EncryptedbyPassphrase VARBINARY(256);   
GO  
-- First get the passphrase from the user.  
DECLARE @PassphraseEnteredByUser NVARCHAR(128);  
SET @PassphraseEnteredByUser   
    = 'A little learning is a dangerous thing!';  
  
-- Update the record for the user's credit card.  
-- In this case, the record is number 3681.  
UPDATE Sales.CreditCard  
SET CardNumber_EncryptedbyPassphrase = EncryptByPassPhrase(@PassphraseEnteredByUser  
    , CardNumber, 1, CONVERT(varbinary, CreditCardID))  
WHERE CreditCardID = '3681';  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [DECRYPTBYPASSPHRASE (Transact-SQL)](../../t-sql/functions/decryptbypassphrase-transact-sql.md)   
 [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

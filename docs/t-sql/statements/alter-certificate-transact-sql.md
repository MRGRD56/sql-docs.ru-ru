---
description: ALTER CERTIFICATE (Transact-SQL)
title: ALTER CERTIFICATE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 04/22/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_CERTIFICATE_TSQL
- ALTER CERTIFICATE
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTION BY PASSWORD option
- encryption [SQL Server], certificates
- modifying certificates
- private keys [SQL Server]
- ALTER CERTIFICATE statement
- certificates [SQL Server], modifying
ms.assetid: da4dc25e-72e0-4036-87ce-22de83160836
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2b8b8bd2539a15a6023ec77513679e4fe9e1ca15
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748834"
---
# <a name="alter-certificate-transact-sql"></a>ALTER CERTIFICATE (Transact-SQL)

[!INCLUDE [sql-asdb-pdw](../../includes/applies-to-version/sql-asdb-pdw.md)]

  Изменяет пароль, используемый для шифрования закрытого ключа сертификата, удаляет закрытый ключ или импортирует закрытый ключ, если он не существует. Изменяет доступность сертификата для компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER CERTIFICATE certificate_name   
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY ( <private_key_spec> )  
    | WITH ACTIVE FOR BEGIN_DIALOG = { ON | OFF }  
  
<private_key_spec> ::=   
      {   
        { FILE = 'path_to_private_key' | BINARY = private_key_bits }  
         [ , DECRYPTION BY PASSWORD = 'current_password' ]  
         [ , ENCRYPTION BY PASSWORD = 'new_password' ]  
      }  
    |  
      {  
         [ DECRYPTION BY PASSWORD = 'current_password' ]  
         [ [ , ] ENCRYPTION BY PASSWORD = 'new_password' ]  
      }  
``` 
 
 
```syntaxsql  
-- Syntax for Parallel Data Warehouse  
  
ALTER CERTIFICATE certificate_name   
{  
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY (   
        FILE = '<path_to_private_key>',  
        DECRYPTION BY PASSWORD = '<key password>' )
}  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *certificate_name*  
 Уникальное имя, под которым сертификат известен в базе данных.  
  
 REMOVE PRIVATE KEY  
 Указывает, что закрытый ключ больше не нужно хранить в базе данных.  
  
 WITH PRIVATE KEY указывает, что закрытый ключ сертификата следует загрузить в SQL Server.

 FILE ='*path_to_private_key*'  
 Указывает полный путь к закрытому ключу, включая имя файла. Этот аргумент может быть локальным путем или UNC-путем к точке в сети. Доступ к файлу осуществляется в контексте безопасности учетной записи службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Используя этот аргумент, необходимо убедиться в том, что учетная запись службы имеет доступ к указанному файлу.
 
 Если указано только имя файла, файл сохраняется в папке данных пользователя по умолчанию для экземпляра. Это может быть, а может и не быть папка данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. В SQL Server Express LocalDB папкой данных пользователя по умолчанию для экземпляра является путь, указанный в переменной среды `%USERPROFILE%` учетной записи, создавшей этот экземпляр.  
  
 BINARY ='*биты закрытого ключа*'  
 **Область применения**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и более поздних версий.  
  
 Биты закрытого ключа, указанного в качестве двоичной константы. Биты могут находиться в зашифрованном виде. Если они зашифрованы, пользователь должен предоставить пароль для расшифровки. Проверка политики паролей для данного пароля не выполняется. Биты закрытого ключа должны быть в формате файла PVK.  
  
 DECRYPTION BY PASSWORD ='*текущий пароль*'  
 Указывает пароль, необходимый для расшифровки закрытого ключа.  
  
 ENCRYPTION BY PASSWORD ='*новый пароль*'  
 Задает пароль, который используется для шифрования закрытого ключа сертификата в базе данных. *Новый пароль* должен соответствовать требованиям политики паролей Windows применительно к компьютеру, на котором запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в разделе [Политика паролей](../../relational-databases/security/password-policy.md).  
  
 ACTIVE FOR BEGIN_DIALOG **=** { ON | OFF }  
 Делает сертификат доступным для инициатора диалога с компонентом [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
## <a name="remarks"></a>Комментарии  
 Закрытый ключ должен соответствовать открытому ключу, заданному аргументом *certificate_name*.  
  
 Предложение DECRYPTION BY PASSWORD может быть опущено, если пароль в файле защищен паролем, равным NULL.  
  
 Если импортируется закрытый ключ сертификата, уже существующий в базе данных, закрытый ключ будет автоматически защищен главным ключом базы данных. Чтобы защитить закрытый ключ паролем, используйте предложение ENCRYPTION BY PASSWORD.  
  
 Параметр REMOVE PRIVATE KEY удалит закрытый ключ сертификата из базы данных. Это можно сделать, если сертификат будет использоваться для проверки подписей или в сценариях компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)], не требующих закрытого ключа. Не удаляйте закрытый ключ сертификата, который защищает симметричный ключ. Закрытый ключ необходимо восстановить для регистрации любых дополнительных модулей или строк, которые необходимо проверить с помощью сертификата, или для расшифровки значений, которые были зашифрованы с помощью сертификата.   
  
 Указывать пароль расшифровки не нужно, если закрытый ключ зашифрован с помощью главного ключа базы данных.  
 
 Чтобы изменить пароль, используемый для шифрования закрытого ключа, не указывайте предложения FILE или BINARY.
  
> [!IMPORTANT]  
>  Всегда создавайте архивную копию закрытого ключа, прежде чем удалять его из базы данных. Дополнительные сведения см. в разделе [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md) и [CERTPRIVATEKEY (Transact-SQL)](../../t-sql/functions/certprivatekey-transact-sql.md).  
  
 Параметр WITH PRIVATE KEY недоступен в автономной базе данных.  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения ALTER для сертификата.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-removing-the-private-key-of-a-certificate"></a>A. Удаление закрытого ключа сертификата  
  
```sql  
ALTER CERTIFICATE Shipping04   
    REMOVE PRIVATE KEY;  
GO  
```  
  
### <a name="b-changing-the-password-that-is-used-to-encrypt-the-private-key"></a>Б. Изменение пароля, используемого для шифрования закрытого ключа  
  
```sql  
ALTER CERTIFICATE Shipping11   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hkjdskghFDGGG4%',  
    ENCRYPTION BY PASSWORD = '34958tosdgfkh##38');  
GO  
```  
  
### <a name="c-importing-a-private-key-for-a-certificate-that-is-already-present-in-the-database"></a>В. Импорт закрытого ключа для сертификата, который уже присутствует в базе данных  
  
```sql  
ALTER CERTIFICATE Shipping13   
    WITH PRIVATE KEY (FILE = 'c:\importedkeys\Shipping13',  
    DECRYPTION BY PASSWORD = 'GDFLKl8^^GGG4000%');  
GO  
```  
  
### <a name="d-changing-the-protection-of-the-private-key-from-a-password-to-the-database-master-key"></a>Г. Изменение защиты закрытого ключа паролем на защиту главным ключом базы данных  
  
```sql  
ALTER CERTIFICATE Shipping15   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hk000eEnvjkjy#F%');  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)  
 [DROP CERTIFICATE (Transact-SQL)](../../t-sql/statements/drop-certificate-transact-sql.md)  
 [BACKUP CERTIFICATE (Transact-SQL)](../../t-sql/statements/backup-certificate-transact-sql.md)  
 [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)  
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
 [CERTENCODED (Transact-SQL)](../../t-sql/functions/certencoded-transact-sql.md)  
 [CERTPRIVATEKEY (Transact-SQL)](../../t-sql/functions/certprivatekey-transact-sql.md)  
 [CERT_ID (Transact-SQL)](../../t-sql/functions/cert-id-transact-sql.md)  
 [CERTPROPERTY (Transact-SQL)](../../t-sql/functions/certproperty-transact-sql.md)  
  
  


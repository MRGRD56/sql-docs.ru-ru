---
description: ALTER MASTER KEY (Transact-SQL)
title: ALTER MASTER KEY (Transact-SQL) | Документы Майкрософт
ms.custom: fasttrack-edit
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: synapse-analytics, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER MASTER KEY
- ALTER_MASTER_KEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- REGENERATE phrase
- ALTER MASTER KEY statement
- decryption [SQL Server], Database Master Key
- FORCE option
- encryption [SQL Server], Database Master Key
- database master key [SQL Server], modifying
- cryptography [SQL Server], Database Master Key
- modifying Database Master Key
- service master key [SQL Server], modifying
- DROP ENCRYPTION BY SERVICE MASTER KEY phrase
ms.assetid: 8ac501c3-4280-4d5b-b58a-1524fa715b50
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ff7acc724a16f6b5304d4be5fa412d1ae1345557
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742944"
---
# <a name="alter-master-key-transact-sql"></a>ALTER MASTER KEY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Изменяет свойства главного ключа базы данных.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for SQL Server

ALTER MASTER KEY <alter_option>

<alter_option> ::=
    <regenerate_option> | <encryption_option>

<regenerate_option> ::=
    [ FORCE ] REGENERATE WITH ENCRYPTION BY PASSWORD = 'password'

<encryption_option> ::=
    ADD ENCRYPTION BY { SERVICE MASTER KEY | PASSWORD = 'password' }
    |
    DROP ENCRYPTION BY { SERVICE MASTER KEY | PASSWORD = 'password' }
```

```syntaxsql
-- Syntax for Azure SQL Database
-- Note: DROP ENCRYPTION BY SERVICE MASTER KEY is not supported on Azure SQL Database.

ALTER MASTER KEY <alter_option>

<alter_option> ::=
    <regenerate_option> | <encryption_option>

<regenerate_option> ::=
    [ FORCE ] REGENERATE WITH ENCRYPTION BY PASSWORD = 'password'

<encryption_option> ::=
    ADD ENCRYPTION BY { SERVICE MASTER KEY | PASSWORD = 'password' }
    |
    DROP ENCRYPTION BY { PASSWORD = 'password' }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics and Analytics Platform System

ALTER MASTER KEY <alter_option>

<alter_option> ::=
    <regenerate_option> | <encryption_option>

<regenerate_option> ::=
    [ FORCE ] REGENERATE WITH ENCRYPTION BY PASSWORD ='password'

<encryption_option> ::=
    ADD ENCRYPTION BY SERVICE MASTER KEY
    |
    DROP ENCRYPTION BY SERVICE MASTER KEY
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

PASSWORD ='*password*' — указывает пароль, используемый для шифрования или расшифровки главного ключа базы данных. *password* должен соответствовать требованиям политики паролей Windows применительно к компьютеру, на котором запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="remarks"></a>Remarks

С аргументом REGENERATE главный ключ базы данных и все защищаемые им ключи создаются повторно. Ключи сначала расшифровываются с помощью старого главного ключа, а затем шифруются с помощью нового. Данная ресурсоемкая операция должна быть назначена на период низкой загрузки, за исключением случаев, когда главный ключ был скомпрометирован.

[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] использует для защиты главного ключа службы и главного ключа базы данных алгоритм шифрования AES. AES - это новый алгоритм шифрования, отличный от алгоритма 3DES, используемого в более ранних версиях. После обновления экземпляра компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] до [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] необходимо заново сформировать главный ключ службы и главный ключ базы данных, чтобы обновить главные ключи до алгоритма AES. Дополнительные сведения о повторном создании главного ключа базы данных см. в статье об [ALTER SERVICE MASTER KEY](../../t-sql/statements/alter-service-master-key-transact-sql.md).

Если используется аргумент FORCE, повторное создание ключей продолжается даже в случае, если главный ключ недоступен или сервер не может расшифровать все зашифрованные закрытые ключи. Если главный ключ открыть невозможно, используется инструкция [RESTORE MASTER KEY](../../t-sql/statements/restore-master-key-transact-sql.md) для восстановления главного ключа из резервной копии. Аргумент FORCE следует использовать только в случае, если главный ключ получить невозможно или при неудачной попытке расшифровки. Данные, зашифрованные только недоступным ключом, будут потеряны.

Фраза DROP ENCRYPTION BY SERVICE MASTER KEY позволяет отменить шифрование главного ключа базы данных с помощью главного ключа службы.

Фраза ADD ENCRYPTION BY SERVICE MASTER KEY приводит к шифрованию копии главного ключа с помощью главного ключа службы, которая затем сохраняется как в текущей базе данных, так и в базе данных master.

## <a name="permissions"></a>Разрешения

Требует разрешения CONTROL для базы данных. Если главный ключ базы данных зашифрован с паролем, требуется также знание этого пароля.

## <a name="examples"></a>Примеры

На этом примере показано, как создается новый главный ключ базы данных `AdventureWorks`, после чего повторно шифруются ключи, стоящие внизу в иерархии шифрования.

```sql
USE AdventureWorks2012;
ALTER MASTER KEY REGENERATE WITH ENCRYPTION BY PASSWORD = 'dsjdkflJ435907NnmM#sX003';
GO
```

## <a name="examples-sssdwfull-and-sspdw"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

На этом примере показано, как создается новый главный ключ базы данных `AdventureWorksPDW2012`, после чего повторно шифруются ключи, стоящие внизу в иерархии шифрования.

```sql
USE master;
ALTER MASTER KEY REGENERATE WITH ENCRYPTION BY PASSWORD = 'dsjdkflJ435907NnmM#sX003';
GO
```

## <a name="see-also"></a>См. также

- [CREATE MASTER KEY](../../t-sql/statements/create-master-key-transact-sql.md)
- [OPEN MASTER KEY](../../t-sql/statements/open-master-key-transact-sql.md)
- [CLOSE MASTER KEY](../../t-sql/statements/close-master-key-transact-sql.md)
- [BACKUP MASTER KEY](../../t-sql/statements/backup-master-key-transact-sql.md)
- [RESTORE MASTER KEY](../../t-sql/statements/restore-master-key-transact-sql.md)
- [DROP MASTER KEY](../../t-sql/statements/drop-master-key-transact-sql.md)
- [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [Присоединение и отсоединение базы данных](../../relational-databases/databases/database-detach-and-attach-sql-server.md)

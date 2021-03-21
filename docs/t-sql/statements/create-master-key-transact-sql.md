---
description: CREATE MASTER KEY (Transact-SQL)
title: CREATE MASTER KEY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_MASTER_KEY_TSQL
- MASTER_KEY_TSQL
- MASTER KEY
- CREATE MASTER KEY
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], Database Master Key
- database master key [SQL Server]
- CREATE MASTER KEY statement
- cryptography [SQL Server], Database Master Key
- database master key [SQL Server], creating
ms.assetid: 1710a305-1a4f-48ec-836c-11ffd0356d76
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b327dd053659378674400329661e9a1ee7410bcd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748208"
---
# <a name="create-master-key-transact-sql"></a>CREATE MASTER KEY (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Создает главный ключ базы данных в базе данных master.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
CREATE MASTER KEY [ ENCRYPTION BY PASSWORD ='password' ]
[ ; ]
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

PASSWORD ='*password*' — это пароль, используемый для шифрования главного ключа базы данных. *password* должен соответствовать требованиям политики паролей Windows применительно к компьютеру, на котором запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Аргумент *password* является необязательным в [!INCLUDE[ssSDS](../../includes/sssds-md.md)] и [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].

## <a name="remarks"></a>Remarks

Главный ключ базы данных — это симметричный ключ, который применяется для защиты закрытых ключей сертификатов и асимметричный ключей, которые есть в базе данных. При создании этот главный ключ зашифровывается с помощью алгоритма AES_256 и предоставленного пользователем пароля. В [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] используется алгоритм Triple DES. Чтобы обеспечить возможность автоматического расшифровывания главного ключа, его копия шифруется с помощью главного ключа службы и хранится в текущей базе данных и базе данных master. Как правило, копия, которая хранится в базе данных master, обновляется без взаимодействия с пользователем при каждом изменении главного ключа. Это действие, заданное по умолчанию, можно изменить с помощью параметра DROP ENCRYPTION BY SERVICE MASTER KEY инструкции [ALTER MASTER KEY](../../t-sql/statements/alter-master-key-transact-sql.md). Главный ключ, который не зашифрован с помощью главного ключа службы, следует открывать с помощью инструкции [OPEN MASTER KEY](../../t-sql/statements/open-master-key-transact-sql.md) и пароля.

Столбец is_master_key_encrypted_by_server в представлении каталога sys.databases базы данных master показывает, зашифрован ли главный ключ базы данных с помощью главного ключа службы.

Сведения о главном ключе базы данных доступны в представлении каталога sys.symmetric_keys.

Для SQL Server и Parallel Data Warehouse главный ключ обычно защищен с помощью главного ключа службы и по меньшей мере одного пароля. При физическом перемещении на другой сервер (доставка журналов, восстановление резервной копии и т. д.) база данных будет содержать копию главного ключа, зашифрованную с помощью главного ключа службы сервера-источника (если это шифрование не было явным образом удалено с помощью `ALTER MASTER KEY DDL`), и его копию, зашифрованную с помощью каждого пароля, указанного во время операции `CREATE MASTER KEY` или последующих операций `ALTER MASTER KEY DDL`. Чтобы восстановить главный ключ и все данные, зашифрованные с его помощью в качестве корневого в иерархии ключей, после перемещения базы данных, пользователю потребуется использовать инструкцию `OPEN MASTER KEY` с одним из паролей, использовавшихся для защиты главного ключа, восстановить резервную копию главного ключа или резервную копию исходного главного ключа службы на новом сервере.

Для [!INCLUDE[ssSDS](../../includes/sssds-md.md)] и [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] защита паролем не считается надежным механизмом безопасности для предотвращения потери данных в случае, когда база данных перемещается с одного сервера на другой, так как защитой главного ключа службы в главном ключе управляет платформа Microsoft Azure. Таким образом, пароль главного ключа является необязательным в [!INCLUDE[ssSDS](../../includes/sssds-md.md)] и [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].

> [!IMPORTANT]
> Следует создать резервную копию главного ключа с помощью инструкции [BACKUP MASTER KEY](../../t-sql/statements/backup-master-key-transact-sql.md) и хранить ее в безопасном месте вне данного веб-сайта.

Главный ключ службы и главный ключ базы данных защищаются алгоритмом шифрования AES-256.

## <a name="permissions"></a>Разрешения

Требует разрешения CONTROL для базы данных.

## <a name="examples"></a>Примеры

В следующем примере создается главный ключ для базы данных `master`. Ключ зашифрован с помощью пароля `23987hxJ#KL95234nl0zBe`.

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '23987hxJ#KL95234nl0zBe';
GO
```

## <a name="see-also"></a>См. также:

- [sys.symmetric_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)
- [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [OPEN MASTER KEY (Transact-SQL)](../../t-sql/statements/open-master-key-transact-sql.md)
- [ALTER MASTER KEY (Transact-SQL)](../../t-sql/statements/alter-master-key-transact-sql.md)
- [DROP MASTER KEY (Transact-SQL)](../../t-sql/statements/drop-master-key-transact-sql.md)
- [CLOSE MASTER KEY (Transact-SQL)](../../t-sql/statements/close-master-key-transact-sql.md)
- [Иерархия средств шифрования](../../relational-databases/security/encryption/encryption-hierarchy.md)

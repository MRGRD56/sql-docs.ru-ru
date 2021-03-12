---
description: Настройка шифрования столбцов с помощью Always Encrypted с PowerShell
title: Настройка шифрования столбцов с помощью Always Encrypted с PowerShell | Документация Майкрософт
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 074c012b-cf14-4230-bf0d-55e23d24f9c8
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 286a5590fea6ccd96351f8d9ad558cb92a7317af
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622787"
---
# <a name="configure-column-encryption-using-always-encrypted-with-powershell"></a>Настройка шифрования столбцов с помощью Always Encrypted с PowerShell
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

В этой статье описаны шаги по настройке целевой конфигурации постоянного шифрования для столбцов базы данных с помощью командлета [Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption) (в модуле *SqlServer* PowerShell). Командлет **Set-SqlColumnEncryption** изменяет схему целевой базы данных и данные, хранящиеся в выбранных столбцах. В зависимости от целевых параметров шифрования, указанных для столбцов и текущей конфигурации шифрования, хранимые в столбце данные могут быть зашифрованы, повторно зашифрованы или расшифрованы.

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Если вы используете [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] и для экземпляра SQL Server настроен безопасный анклав, можно выполнять криптографические операции на месте без перемещения данных из базы данных. См. статью [Настройка шифрования столбцов на месте с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-configure-encryption.md). Обратите внимание, что PowerShell не поддерживает шифрование на месте.

::: moniker-end
Дополнительные сведения о поддержке функции Always Encrypted в модуле SqlServer PowerShell см. в руководстве по [настройке постоянного шифрования с помощью PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md).

## <a name="prerequisites"></a>Предварительные требования

Чтобы задать целевую конфигурацию шифрования, необходимо выполнить следующие условия:
- в базе данных должен быть настроен ключ шифрования столбца (если выполняется шифрование или повторное шифрование столбца). Дополнительные сведения см. в разделе [Настройка ключей Always Encrypted с помощью PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-keys-using-powershell.md).
- главный ключ столбца для каждого столбца, который требуется зашифровать, повторно зашифровать или расшифровать, должен быть доступен с компьютера, на котором выполняются командлеты PowerShell. 

## <a name="performance-and-availability-considerations"></a>Вопросы, касающиеся производительности и доступности

Чтобы применить заданные целевые параметры шифрования базы данных, командлет **Set-SqlColumnEncryption** прозрачно скачивает все данные из столбцов, содержащих целевые таблицы, отправляет данные обратно в набор временной таблицы (с применением целевых параметров шифрования) и, наконец, заменяет исходные таблицы на новые версии таблиц. Базовый поставщик данных .NET Framework для SQL Server шифрует и (или) расшифровывает данные при загрузке или отправке в соответствии с текущей конфигурацией шифрования целевого столбца и целевых параметров шифрования, заданных для целевых столбцов. Операция перемещения данных может занять много времени в зависимости от размера данных в затронутых таблицах и пропускной способности сети.

Командлет **Set-SqlColumnEncryption** поддерживает два режима настройки целевой конфигурации шифрования: в сети и вне сети.

При использовании автономного режима целевые таблицы (и любые, относящиеся к ним таблицы, например таблицы, связанные с целевой таблицей по внешнему ключу) недоступны для записи транзакций на протяжении операции. Семантика ограничений внешнего ключа (**CHECK** или **NOCHECK**) всегда сохраняется при использовании режима "вне сети".

При использовании подключенного режима (требуется модуль SqlServer PowerShell 21.x или более поздней версии) операция копирования и шифрования, расшифровки или повторного шифрования данных выполняется постепенно. Приложения могут считывать данные в целевых таблицах и записывать их в эти таблицы во время выполнения операции перемещения, за исключением последней итерации, длительность которой ограничивается параметром **MaxDownTimeInSeconds** (его можно определить). Приложения могут обнаруживать и обрабатывать изменения во время копирования данных. [Отслеживание изменений](../../track-changes/enable-and-disable-change-tracking-sql-server.md) включается с помощью командлета в целевой базе данных. Поэтому при использовании режима "в сети" на стороне сервера потребляется больше ресурсов. Операции в этом режиме также могут занять гораздо больше времени, в частности при интенсивной записи в базу данных. Режим "в сети" можно использовать для шифрования одной таблицы за раз. При этом она должна содержать первичный ключ. По умолчанию ограничения внешнего ключа создаются повторно с помощью параметра **NOCHECK**, чтобы свести к минимуму воздействие на приложения. Вы можете принудительно задать сохранение семантики ограничений внешнего ключа, указав параметр **KeepCheckForeignKeyConstraints**. 

Ниже приведены рекомендации по выбору между режимом "в сети" и "вне сети".

Режим "вне сети" следует использовать:
- чтобы свести к минимуму время выполнения операции; 
- чтобы выполнять шифрование, расшифровку и повторное шифрование столбцов в нескольких таблицах одновременно;
- если целевая таблица не содержит первичный ключ.

Режим "в сети" следует использовать:
- чтобы свести к минимуму время простоя или недоступность базы данных для приложений.

## <a name="security-considerations"></a>Вопросы безопасности

Командлет **Set-SqlColumnEncryption** , используемый для настройки шифрования столбцов базы данных, обрабатывает ключи постоянного шифрования и данные, хранящиеся в столбцах базы данных. Поэтому командлет нужно запускать на защищенном компьютере. Если база данных размещена на сервере SQL Server, командлет нужно запускать на компьютере, отличном от компьютера с экземпляром SQL Server. Поскольку основной задачей функции постоянного шифрования является обеспечение целостности зашифрованных конфиденциальных данных даже в случае нарушения безопасности системы базы данных, выполнение скрипта PowerShell, обрабатывающего ключи или конфиденциальные данные на сервере SQL Server, может снизить или вообще отменить эффект действия функции.

Задача  |Статья  |Доступ к ключам с открытым текстом или хранилищу ключей  |Доступ к базе данных   
---|---|---|---
Шаг 1. Запуск среды PowerShell и импорт модуля SqlServer. | [Импорт модуля SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Нет | Нет
Шаг 2. Соединение с сервером и базой данных | [Подключение к базе данных](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Нет | Да
Шаг 3. Проверка подлинности в Azure, если главный ключ столбца (для смены, защищающий ключ шифрования) хранится в хранилище ключей Azure | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Да | Нет
Шаг 4. Создание массива объектов SqlColumnEncryptionSettings — по одному для каждого столбца базы данных, который требуется зашифровать, повторно зашифровать или расшифровать. SqlColumnMasterKeySettings — это объект, который существует в памяти (PowerShell). Он определяет целевую схему шифрования столбца. | [New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings) | нет | Нет
Шаг 5. Задание нужной конфигурации шифрования, указанной в массиве объектов SqlColumnMasterKeySettings, созданном на предыдущем шаге. В зависимости от заданных целевых параметров и текущей конфигурации шифрования столбец может быть зашифрован, повторно зашифрован или расшифрован.| [Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)<br><br>**Примечание.** Выполнение этого шага может занять длительное время. Приложения не будут иметь доступ к таблицам во время выполнения операции или ее части в зависимости от выбранного режима (в сети или вне сети). | Да | Да

## <a name="encrypt-columns-using-offline-approach---example"></a>Пример шифрования столбцов в режиме "вне сети"

В примере ниже показано, как задать целевую конфигурацию шифрования для нескольких столбцов. Если какой-либо столбец не зашифрован, он будет зашифрован. Если какой-либо столбец уже зашифрован с помощью другого ключа и (или) другого типа шифрования, он будет расшифрован и повторно зашифрован с помощью указанного целевого ключа или типа шифрования.


```PowerShell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Encrypt the selected columns (or re-encrypt, if they are already encrypted using keys/encrypt types, different than the specified keys/types.
$ces = @()
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.SSN" -EncryptionType "Deterministic" -EncryptionKey "CEK1"
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.BirthDate" -EncryptionType "Randomized" -EncryptionKey "CEK1"
Set-SqlColumnEncryption -InputObject $database -ColumnEncryptionSettings $ces -LogFileDirectory .
```
 
## <a name="encrypt-columns-using-online-approach---example"></a>Пример шифрования столбцов в режиме "в сети"

В примере ниже показано, как задать целевую конфигурацию шифрования для нескольких столбцов с помощью режима "в сети". Если какой-либо столбец не зашифрован, он будет зашифрован. Если какой-либо столбец уже зашифрован с помощью другого ключа и (или) другого типа шифрования, он будет расшифрован и повторно зашифрован с помощью указанного целевого ключа или типа шифрования.

```PowerShell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Encrypt the selected columns (or re-encrypt, if they are already encrypted using keys/encrypt types, different than the specified keys/types.
$ces = @()
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.SSN" -EncryptionType "Deterministic" -EncryptionKey "CEK1"
$ces += New-SqlColumnEncryptionSettings -ColumnName "dbo.Patients.BirthDate" -EncryptionType "Randomized" -EncryptionKey "CEK1"
Set-SqlColumnEncryption -InputObject $database -ColumnEncryptionSettings $ces -UseOnlineApproach -MaxDowntimeInSeconds 180 -LogFileDirectory .
```
## <a name="decrypt-columns---example"></a>Пример расшифровки столбцов

В примере ниже показано, как расшифровать все столбцы, которые на данный момент зашифрованы в базе данных.


```PowerShell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Find all encrypted columns, and create a SqlColumnEncryptionSetting object for each column.
$ces = @()
$tables = $database.Tables
for($i=0; $i -lt $tables.Count; $i++){
    $columns = $tables[$i].Columns
    for($j=0; $j -lt $columns.Count; $j++) {
        if($columns[$j].isEncrypted) {
            $threeColPartName = $tables[$i].Schema + "." + $tables[$i].Name + "." + $columns[$j].Name 
            $ces += New-SqlColumnEncryptionSettings -ColumnName $threeColPartName -EncryptionType "Plaintext" 
        }
    }
}

# Decrypt all columns.
Set-SqlColumnEncryption -ColumnEncryptionSettings $ces -InputObject $database -LogFileDirectory .
```
 
## <a name="next-steps"></a>Next Steps
- [Разработка приложений с помощью Always Encrypted](always-encrypted-client-development.md)

## <a name="see-also"></a>См. также:  
 - [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
 - [Общие сведения об управлении ключами для Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
 - [Настройка постоянного шифрования с помощью PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)
 - [Настройка шифрования столбцов с помощью мастера Always Encrypted](always-encrypted-wizard.md)
 - [Настройка шифрования столбцов с помощью Always Encrypted с пакетом приложения уровня данных](configure-always-encrypted-using-dacpac.md)
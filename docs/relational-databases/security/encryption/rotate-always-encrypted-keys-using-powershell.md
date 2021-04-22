---
description: Смена ключей Always Encrypted с помощью PowerShell
title: Смена ключей постоянного шифрования с помощью PowerShell | Документация Майкрософт
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 5117b4fd-c8d3-48d5-87c9-756800769f31
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 798df5a6953512c0c5a7aab62c516fecc3d7c3da
ms.sourcegitcommit: 233be9adaee3d19b946ce15cfcb2323e6e178170
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2021
ms.locfileid: "107561063"
---
# <a name="rotate-always-encrypted-keys-using-powershell"></a>Смена ключей Always Encrypted с помощью PowerShell
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

В этой статье описаны действия по смене ключей постоянного шифрования с помощью модуля SqlServer PowerShell. Сведения о начале работы с модулем SqlServer PowerShell для постоянного шифрования см. в разделе [Configure Always Encrypted using PowerShell](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md)(Настройка постоянного шифрования с помощью PowerShell).

Смена ключей постоянного шифрования — это процесс замены существующего ключа на новый. Смена ключа может потребоваться в случае его компрометации или для обеспечения соответствия политикам или нормам организации, предусматривающим регулярную смену ключей шифрования. 

Функция постоянного шифрования использует два типа ключей, поэтому существует два общих рабочих процесса по смене ключей — смена главных ключей столбцов и смена ключей шифрования столбцов.

* При **смене ключей шифрования столбцов** выполняется расшифровка данных, зашифрованных с помощью текущего ключа, и повторное шифрование данных с помощью нового ключа шифрования столбца. Поскольку для смены ключа шифрования столбца требуется доступ к ключам и базе данных, смена ключей шифрования столбцов осуществляется только без разделения ролей.
* Процедура **смены главного ключа столбца** включает в себя действия по расшифровке ключей шифрования столбцов, которые защищены с помощью текущего главного ключа столбца, их повторному шифрованию с помощью нового главного ключа столбца и обновлению метаданных для обоих типов ключей. Смену главного ключа столбца можно производить как с разделением, так и без разделения ролей (при использовании модуля SqlServer PowerShell).

## <a name="column-master-key-rotation-without-role-separation"></a>Смена главного ключа столбца без разделения ролей

Описанный здесь метод смены главного ключа столбца не поддерживает разделение ролей между администраторами безопасности и администраторами баз данных. В некоторых шагах операции с физическими ключами объединены с операциями с метаданными ключей. Таким образом, этот метод рекомендуется для организаций, использующих модель разработки и операций, или если база данных размещается в облаке и основная цель заключается в ограничении доступа администраторов облака (но не локальных администраторов баз данных) к конфиденциальным данным. Мы не рекомендуем использовать этот метод, если администраторы баз данных являются потенциальными злоумышленниками или если они не должны иметь доступ к конфиденциальным данным.  


| Задача | Статья | Доступ к ключам с открытым текстом или хранилищу ключей| Доступ к базе данных
|:---|:---|:---|:---
|Шаг 1. Создание нового главного ключа столбца в хранилище ключей.<br><br>**Примечание.** Этот шаг не поддерживается в модуле SqlServer PowerShell. Для выполнения этой задачи из командной строки используйте средства, поддерживаемые выбранным хранилищем ключей. | [Создание и хранение главных ключей столбцов для Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| Да | Нет
|Шаг 2. Запуск среды PowerShell и импорт модуля SqlServer. | [Импорт модуля SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Нет | Нет
|Шаг 3. Соединение с сервером и базой данных. | [Подключение к базе данных](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Нет | Да
|Шаг 4. Создание объекта SqlColumnMasterKeySettings, содержащего сведения о расположении нового главного ключа столбца. SqlColumnMasterKeySettings — это объект, который существует в памяти (PowerShell). Для его создания используйте командлет, поддерживаемый хранилищем ключей. |[New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)<br> | Нет | Нет
|Шаг 5. Создание метаданных о новом главном ключе столбца в базе данных. | [New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Примечание.** На самом деле для создания метаданных ключа командлет выполняет инструкцию [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md) . | Нет | Да
|Шаг 6. Проверка подлинности в Azure, если текущий или новый главный ключ столбца хранится в хранилище ключей или в управляемом модуле HSM в Azure Key Vault. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Да | Нет
|Шаг 7. Запуск процесса смены путем шифрования каждого ключа шифрования столбца, который в данный момент защищен с помощью старого главного ключа столбца, с использованием нового главного ключа столбца. После выполнения этого шага каждый затронутый ключ шифрования столбца (связанный со старым сменяемым главным ключом столбца) шифруется с помощью старого и нового главных ключей и имеет два зашифрованных значения в метаданных базы данных.| [Invoke-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/invoke-sqlcolumnmasterkeyrotation) | Да | Да
|Шаг 8. Работа с администраторами всех приложений, выполняющих запросы к зашифрованным столбцам в базе данных (и защищенных с помощью старого главного ключа столбца), по обеспечению доступа приложений к новому главному ключу столбца.| [Создание и хранение главных ключей столбцов (постоянное шифрование)](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md) | Да | Нет
|Шаг 9. Завершение процесса смены.<br><br>**Примечание.** Перед выполнением этого шага убедитесь, что все приложения, отправляющие запросы к зашифрованным столбцам, которые защищены с помощью старого главного ключа столбца, настроены для использования нового главного ключа столбца. В случае преждевременного выполнения этого шага некоторые приложения не смогут расшифровывать данные. Завершите процесс смены, удалив из базы данных зашифрованные значения, созданные с помощью старого главного ключа столбца. При этом будет удалена связь между старым главным ключом столбца и защищаемыми ими ключами шифрования столбцов. |[Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)| Нет | Да
|Шаг 10. Удаление метаданных из старого главного ключа столбца. |[Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)| Нет | Да

> [!NOTE]
> Настоятельно рекомендуется не удалять старый главный ключ столбца после смены без возможности восстановления. Старый главный ключ столбца следует сохранить в его текущем хранилище ключей или скопировать в другое надежное место. Старый ключ потребуется для доступа к данным при восстановлении базы данных из файла резервной копии на момент времени, *предшествующий* настройке нового главного ключа столбца.

### <a name="rotating-a-column-master-key-without-role-separation-windows-certificate-example"></a>Смена главного ключа столбца без разделения ролей (пример с сертификатом Windows)

Приведенный ниже скрипт является законченным примером замены существующего главного ключа столбца (CMK1) на новый главный ключ столбца (CMK2).

```powershell
# Create a new column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048

# Import the SqlServer module
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint

# Create metadata for your new column master key in the database.
$newCmkName = "CMK2"
New-SqlColumnMasterKey -Name $newCmkName -InputObject $database -ColumnMasterKeySettings $newCmkSettings

# Initiate the rotation from the current column master key to the new column master key.
$oldCmkName = "CMK1"
Invoke-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName -TargetColumnMasterKeyName $newCmkName -InputObject $database

# Complete the rotation of the old column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName  -InputObject $database

# Remove the old column master key metadata.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```

## <a name="column-master-key-rotation-with-role-separation"></a>Смена главного ключа столбца с разделением ролей

В описанном в этом разделе процессе смены главного ключа столбца существует разделение ролей между администраторами безопасности и администраторами баз данных.

> [!IMPORTANT]
> Перед выполнением действий, связанных с доступом к ключам с открытым текстом или хранилищу ключей (определены в столбце *Доступ к ключам с открытым текстом или хранилищу ключей*=**Да** в таблице ниже), убедитесь, что среда PowerShell запущена на защищенном компьютере, отличном от компьютера с базой данных. Дополнительные сведения см. в разделе [Security Considerations for Key Management](overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management)(Вопросы безопасности для управления ключами).

### <a name="part-1-dba"></a>Часть 1. Администратор баз данных

Администратор баз данных получает метаданные о подлежащем смене главном ключе столбца и о затрагиваемых ключах шифрования столбцов, которые связаны с текущим главным ключом столбца. Администратор баз данных предоставляет эти сведения администратору безопасности.


| Задача | Статья | Доступ к ключам с открытым текстом или хранилищу ключей| Доступ к базе данных
|:---|:---|:---|:---
|Шаг 1. Запуск среды PowerShell и импорт модуля SqlServer. | [Импорт модуля SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Нет | None
|Шаг 2. Соединение с сервером и базой данных. | [Соединение с базой данных](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Нет | Да
|Шаг 3. Получение метаданных о старом главном ключе столбца.| [Get-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnmasterkey) | Нет | Да
|Шаг 4. Получение метаданных о ключах шифрования столбцов, защищенных старым главным ключом столбца, включая их зашифрованные значения. | [Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey) | Нет | Да
|Шаг 5. Предоставление данных о расположении главного ключа столбца (имени поставщика и пути к главному ключу столбца) и зашифрованных значений соответствующих ключей шифрования столбцов, защищенных при помощи старого главного ключа столбца| Ознакомьтесь с указанными ниже примерами. | Нет | Нет

### <a name="part-2-security-administrator"></a>Часть 2. Администратор безопасности

Администратор безопасности создает новый главный ключ столбца, повторно шифрует затронутые ключи шифрования столбцов с помощью нового главного ключа столбца и предоставляет администратору баз данных сведения о новом главном ключе столбца, а также о наборе новых зашифрованных значений для затронутых ключей шифрования столбцов.

| Задача | Статья | Доступ к ключам с открытым текстом или хранилищу ключей| Доступ к базе данных
|:---|:---|:---|:---
|Шаг 1. Получение от администратора баз данных сведений о расположении старого главного ключа столбца и зашифрованных значений соответствующих ключей шифрования столбцов, защищенных с помощью старого главного ключа столбца.|Недоступно<br>Ознакомьтесь с указанными ниже примерами.|Нет| Нет
|Шаг 2. Создание нового главного ключа столбца в хранилище ключей.<br><br>**Примечание.** Этот шаг не поддерживается в модуле SqlServer. Для выполнения этой задачи из командной строки используйте средства, поддерживаемые выбранным хранилищем ключей.|[Создание и хранение главных ключей столбцов для Always Encrypted](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| Да | Нет
|Шаг 3. Запуск среды PowerShell и импорт модуля SqlServer. | [Импорт модуля SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Нет | Нет
|Шаг 4. Создание объекта SqlColumnMasterKeySettings, содержащего сведения о расположении **старого** главного ключа столбца. SqlColumnMasterKeySettings — это объект, который существует в памяти (PowerShell). |New-SqlColumnMasterKeySettings| Нет | Нет
|Шаг 5. Создание объекта SqlColumnMasterKeySettings, содержащего сведения о расположении **нового** главного ключа столбца. SqlColumnMasterKeySettings — это объект, который существует в памяти (PowerShell). Для его создания используйте командлет, поддерживаемый хранилищем ключей. | [New-SqlAzureKeyVaultColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlazurekeyvaultcolumnmasterkeysettings)<br><br>[New-SqlCertificateStoreColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcertificatestorecolumnmasterkeysettings)<br><br>[New-SqlCngColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcngcolumnmasterkeysettings)<br><br>[New-SqlCspColumnMasterKeySettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcspcolumnmasterkeysettings)| Нет | Нет
|Шаг 6. Проверка подлинности в Azure, если старый (текущий) или новый главный ключ столбца хранится в хранилище ключей или в управляемом модуле HSM в Azure Key Vault. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Да | Нет
|Шаг 7. Повторное шифрование каждого значения ключа шифрования столбца, который в данный момент защищен с помощью старого главного ключа столбца, с использованием нового главного ключа столбца. | [New-SqlColumnEncryptionKeyEncryptedValue](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkeyencryptedvalue)<br><br>**Примечание.** При вызове этого командлета передайте объекты SqlColumnMasterKeySettings для старого и нового главных ключей столбцов вместе с подлежащим повторному шифрованию значением ключа шифрования столбца.|Да|Нет
|Шаг 8. Предоставление администратору баз данных информации о расположении нового главного ключа столбца (имени поставщика и пути к главному ключу столбца) и набора новых зашифрованных значений ключей шифрования столбцов.| Ознакомьтесь с указанными ниже примерами. | Нет | Нет

> [!NOTE]
> Настоятельно рекомендуется не удалять старый главный ключ столбца после смены без возможности восстановления. Старый главный ключ столбца следует сохранить в его текущем хранилище ключей или скопировать в другое надежное место. Старый ключ потребуется для доступа к данным при восстановлении базы данных из файла резервной копии на момент времени, *предшествующий* настройке нового главного ключа столбца.

### <a name="part-3-dba"></a>Часть 3. Администратор баз данных

Администратор баз данных создает метаданные для нового главного ключа столбца и обновляет метаданные затронутых ключей шифрования столбцов для добавления нового набора зашифрованных значений. На этом этапе администратор баз данных работает вместе с администраторами приложений, выполняющих запросы к столбцам шифрования. Администраторы приложений обеспечивают доступ приложений к новому главному ключу столбца. После настройки всех приложений для использования нового главного ключа столбца администратор баз данных удаляет старый набор зашифрованных значений и метаданные старого главного ключа столбца.

| Задача | Статья | Доступ к ключам с открытым текстом или хранилищу ключей| Доступ к базе данных
|:---|:---|:---|:---
|Шаг 1. Получение от администратора безопасности сведений о расположении нового главного ключа столбца и нового набора зашифрованных значений соответствующих ключей шифрования столбцов, защищенных с помощью старого главного ключа столбца.| Ознакомьтесь с указанными ниже примерами. | Нет | Нет
|Шаг 2. Запуск среды PowerShell и импорт модуля SqlServer. | [Импорт модуля SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Нет | Нет
|Шаг 3. Соединение с сервером и базой данных. | [Подключение к базе данных](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Нет | Да
|Шаг 4. Создание объекта SqlColumnMasterKeySettings, содержащего сведения о расположении нового главного ключа столбца. SqlColumnMasterKeySettings — это объект, который существует в памяти (PowerShell). |New-SqlColumnMasterKeySettings| Нет| Нет
|Шаг 5. Создание метаданных о новом главном ключе столбца в базе данных.|[New-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnmasterkey)<br><br>**Примечание.** Фактически при создании метаданных ключа командлет выполняет инструкцию [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md). | Нет | Да
|Шаг 6. Получение метаданных о ключах шифрования столбцов, защищенных старым главным ключом столбца.| [Get-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/get-sqlcolumnencryptionkey)| Нет | Да
|Шаг 7. Добавление нового зашифрованного значения (созданного с помощью нового главного ключа столбца) в метаданные для каждого затронутого ключа шифрования столбца.|[Add-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/add-sqlcolumnencryptionkeyvalue)|Нет|Да
|Шаг 8. Работа с администраторами всех приложений, выполняющих запросы к зашифрованным столбцам в базе данных (и защищенных с помощью старого главного ключа столбца), по обеспечению доступа приложений к новому главному ключу столбца.|[Create and Store Column Master Keys (Always Encrypted) (Создание и хранение главных ключей столбцов (постоянное шифрование))](../../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)| нет|Нет
|Шаг 9. Завершение процесса смены путем удаления из базы данных зашифрованных значений, связанных со старым главным ключом столбца.<br><br>**Примечание.** Перед выполнением этого шага убедитесь, что все приложения, отправляющие запросы к зашифрованным столбцам, которые защищены с помощью старого главного ключа столбца, настроены для использования нового главного ключа столбца. В случае преждевременного выполнения этого шага некоторые приложения не смогут расшифровывать данные.<br><br>На этом шаге удаляется связь между старым главным ключом столбца и защищаемыми ими ключами шифрования столбцов. | [Complete-SqlColumnMasterKeyRotation](/powershell/sqlserver/sqlserver/vlatest/complete-sqlcolumnmasterkeyrotation)<br><br>Кроме того, можно использовать [Remove-SqlColumnEncryptionKeyValue](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkeyvalue). | Нет|Да
|Шаг 10. Удаление метаданных старого главного ключа столбца из базы данных.| [Remove-SqlColumnMasterKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnmasterkey)| Нет|Да

### <a name="rotating-a-column-master-key-with-role-separation-windows-certificate-example"></a>Смена главного ключа столбца с разделением ролей (пример с сертификатом Windows)

Приведенный ниже скрипт — законченный пример создания нового главного ключа столбца, который является сертификатом в хранилище сертификатов Windows, смены существующего (текущего) главного ключа столбца для его замены новым главным ключом столбца. В скрипте предполагается, что целевая база данных содержит главный ключ столбца с именем CMK1 (подлежащий смене), который шифрует некоторые ключи шифрования столбцов.

Часть 1. Администратор баз данных

```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Retrieve the data about the old column master key, which needs to be rotated.
$oldCmkName = "CMK1"
$oldCmk = Get-SqlColumnMasterKey -Name $oldCmkName -InputObject $database


# Share the location of the old column master key with a Security Administrator, via a CSV file on a share drive.
$oldCmkDataFile = "Z:\oldcmkdata.txt"
"KeyStoreProviderName, KeyPath" > $oldCmkDataFile
$oldCmk.KeyStoreProviderName +", " + $oldCmk.KeyPath >> $oldCmkDataFile


# Find column encryption keys associated with the old column master key and provide the encrypted values of column encryption keys to the Security Administrator, via a CSV file on a share drive.
$ceks = Get-SqlColumnEncryptionKey -InputObject $database
$oldCekValuesFile = "Z:\oldcekvalues.txt"
"CEKName, CEKEncryptedValue" > $oldCekValuesFile 
for($i=0; $i -lt $ceks.Length; $i++){
    if($ceks[$i].ColumnEncryptionKeyValues.Length -eq 2) {
        # This column encryption has 2 encrypted values - let's check, if it is associated with the old column master key.
        if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName -or $ceks[$i].ColumnEncryptionKeyValues[1].ColumnMasterKeyName -eq $oldCmkName) {
            Write-Host $ceks[$i].Name "already has 2 encrypted values and therefore" $oldCmkName + "cannot be rotated"
            exit 1
        }
    }
    if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName) {# This column encryption key has 1 encrypted value that was produced using the old column master key
        # Save the name and the encrypted value of the column encryption key in the file.
        $encryptedValue =  "0x" + -join ($ceks[$i].ColumnEncryptionKeyValues[0].EncryptedValue |  foreach {$_.ToString("X2") } )
        $ceks[$i].Name + "," + $encryptedValue >> $oldCekValuesFile
    }
} 
```


Часть 2. Администратор безопасности

```powershell
# Obtain the location of the old column master key and the encrypted values of the corresponding column encryption keys, from your DBA, via a CSV file on a share drive.
$oldCmkDataFile = "Z:\oldcmkdata.txt"
$oldCmkData = Import-Csv $oldCmkDataFile
$oldCekValuesFile = "Z:\oldcekvalues.txt"
$oldCekValues = @(Import-Csv $oldCekValuesFile)

# Create a new column master key in Windows Certificate Store.
$storeLocation = "CurrentUser"
$certPath = "Cert:\" + $storeLocation + "\My"
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation $certPath -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Create a SqlColumnMasterKeySettings object for your old column master key. 
$oldCmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $oldCmkData.KeyStoreProviderName -KeyPath $oldCmkData.KeyPath

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation $storeLocation -Thumbprint $cert.Thumbprint


# Prepare a CSV file, you will use to share the encrypted values of column encryption keys, produced using the new column master key.
$newCekValuesFile = "Z:\newcekvalues.txt"
"CEKName, CEKEncryptedValue" > $newCekValuesFile

# Re-encrypt each value with the new column master key and save the new encrypted value in the file.
for($i=0; $i -lt $oldCekValues.Count; $i++){
    # Re-encrypt each value with the new CMK
    $newValue = New-SqlColumnEncryptionKeyEncryptedValue -TargetColumnMasterKeySettings $newCmkSettings -ColumnMasterKeySettings $oldCmkSettings -EncryptedValue $oldCekValues[$i].CEKEncryptedValue
    $oldCekValues[$i].CEKName + ", " + $newValue >> $newCekValuesFile
}

# Share the new column master key data with your DBA, via a CSV file.
$newCmkDataFile = $home + "\newcmkdata.txt"
"KeyStoreProviderName, KeyPath" > $newCmkDataFile
$newCmkSettings.KeyStoreProviderName +", " + $newCmkSettings.KeyPath >> $newCmkDataFile
```


Часть 3. Администратор баз данных

```powershell
# Obtain the location of the new column master key and the new encrypted values of the corresponding column encryption keys, from your Security Administrator, via a CSV file on a share drive.
$newCmkDataFile = "Z:\newcmkdata.txt"
$newCmkData = Import-Csv $newCmkDataFile
$newCekValuesFile = "Z:\newcekvalues.txt"
$newCekValues = @(Import-Csv $newCekValuesFile)

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your new column master key. 
$newCmkSettings = New-SqlColumnMasterKeySettings -KeyStoreProviderName $newCmkData.KeyStoreProviderName -KeyPath $newCmkData.KeyPath
# Create metadata for the new column master key in the database.
$newCmkName = "CMK2"
New-SqlColumnMasterKey -Name $newCmkName -InputObject $database -ColumnMasterKeySettings $newCmkSettings


# Get all CEK objects
$oldCmkName = "CMK1"
$ceks = Get-SqlColumnEncryptionKey -InputObject $database
for($i=0; $i -lt $ceks.Length; $i++){
    if($ceks[$i].ColumnEncryptionKeyValues.Length -eq 2) {# This column encryption key has 2 encrypted values. Let's check, if it is associated with the old CMK.
        if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName -or $ceks[$i].ColumnEncryptionKeyValues[1].ColumnMasterKeyName -eq $oldCmkName) {
            Write-Host $ceks[$i].Name "already has 2 encrypted values and therefore" $oldCmkName + "cannot be rotated"
            exit 1
        }
    }
    if($ceks[$i].ColumnEncryptionKeyValues[0].ColumnMasterKeyName -eq $oldCmkName) {
        # Find the corresponding new encrypted value, received from the Security Administrator.
        $newValueRow = ($newCekValues| Where-Object {$_.CEKName -eq $ceks[$i].Name }[0])
        # Update the column encryption key metadata object by adding the new encrypted value
        Add-SqlColumnEncryptionKeyValue -ColumnMasterKeyName $newCmkName -Name $ceks[$i].Name -EncryptedValue $newValueRow.CEKEncryptedValue -InputObject $database 
    }
}

# Complete the rotation of the current column master key.
Complete-SqlColumnMasterKeyRotation -SourceColumnMasterKeyName $oldCmkName  -InputObject $database

# Remove the old column master key.
Remove-SqlColumnMasterKey -Name $oldCmkName -InputObject $database
```

## <a name="rotating-a-column-encryption-key"></a>Смена ключа шифрования столбца

Процедура смены ключа шифрования столбца включает в себя действия по расшифровке данных во всех столбцах, которые зашифрованы с помощью подлежащего смене ключа, и повторному шифрованию данных с помощью нового ключа шифрования столбца. Так как в этом процессе требуется доступ к ключам и базе данных, разделение ролей не поддерживается. Смена ключа шифрования столбца может занимать много времени, если размер таблиц содержащих столбцы, зашифрованные с помощью заменяемого ключа, слишком велик. Поэтому организации необходимо тщательно спланировать процесс смены ключа шифрования столбца.

Ключ шифрования столбца можно сменить в двух режимах — вне сети или в сети. Первый способ быстрее, но приложения не смогут записывать данные в затронутые таблицы. Второй способ, вероятно, займет больше времени, но вы сможете ограничить интервал времени, в течение которого затронутые таблицы будут недоступными для приложений. Дополнительные сведения см. в разделах [Настройка шифрования столбцов с помощью Always Encrypted с PowerShell](configure-column-encryption-using-powershell.md) и [Set-SqlColumnEncryption](/powershell/module/sqlserver/set-sqlcolumnencryption/).

| Задача | Статья | Доступ к ключам с открытым текстом или хранилищу ключей| Доступ к базе данных
|:---|:---|:---|:---
|Шаг 1. Запуск среды PowerShell и импорт модуля SqlServer. | [Импорт модуля SqlServer](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#importsqlservermodule) | Нет | Нет
|Шаг 2. Соединение с сервером и базой данных. | [Подключение к базе данных](../../../relational-databases/security/encryption/configure-always-encrypted-using-powershell.md#connectingtodatabase) | Нет | Да
|Шаг 3. Проверка подлинности в Azure, если главный ключ столбца (защищающий ключ шифрования столбца, подлежащий смене) хранится в хранилище ключей или в управляемом модуле HSM в Azure Key Vault. | [Add-SqlAzureAuthenticationContext](/powershell/sqlserver/sqlserver/vlatest/add-sqlazureauthenticationcontext) | Да | Нет
|Шаг 4. Создание ключа шифрования столбца, его шифрование с помощью главного ключа столбца и создание метаданных ключа шифрования столбца в базе данных.  | [New-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionkey)<br><br>**Примечание.** Используйте командлет, который внутренним образом создает и шифрует ключ шифрования столбца.<br>На самом деле для создания метаданных ключа командлет выполняет инструкцию [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md) . | Да | Да
|Шаг 5. Поиск всех столбцов, зашифрованных с помощью старого ключа шифрования столбца. | [Учебник по программированию управляющих объектов SQL Server (SMO)](../../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md) | Нет | Да
|Шаг 6. Создание объекта *SqlColumnEncryptionSettings* для каждого затронутого столбца.  SqlColumnEncryptionSettings — это объект, который существует в памяти (PowerShell). Он определяет целевую схему шифрования столбца. В этом случае объект должен указывать, что затронутый столбец следует зашифровать с помощью нового ключа шифрования столбца. | [New-SqlColumnEncryptionSettings](/powershell/sqlserver/sqlserver/vlatest/new-sqlcolumnencryptionsettings) | нет | Нет
|Шаг 7. Повторное шифрование столбцов, определенных на шаге 5, с помощью нового ключа шифрования столбца. | [Set-SqlColumnEncryption](/powershell/sqlserver/sqlserver/vlatest/set-sqlcolumnencryption)<br><br>**Примечание.** Выполнение этого шага может занять длительное время. Приложения не будут иметь доступ к таблицам во время выполнения операции или ее части в зависимости от выбранного режима (в сети или вне сети). | Да | Да
|Шаг 8. Удаление метаданных для старого ключа шифрования столбца. | [Remove-SqlColumnEncryptionKey](/powershell/sqlserver/sqlserver/vlatest/remove-sqlcolumnencryptionkey) | Нет | Да

### <a name="example---rotating-a-column-encryption-key"></a>Пример: смена ключа шифрования столбца

В приведенном ниже скрипте демонстрируется смена ключа шифрования столбца.  В скрипте предполагается, что целевая база данных содержит несколько столбцов, зашифрованных с помощью ключа шифрования CEK1 (подлежащего смене), который защищен главным ключом столбца CMK1 (главный ключ столбца не хранится в Azure Key Vault). 


```powershell
# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Generate a new column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cmkName = "CMK1"
$newCekName = "CEK2"
New-SqlColumnEncryptionKey -Name $newCekName -InputObject $database -ColumnMasterKey $cmkName 


# Find all columns encrypted with the old column encryption key, and create a SqlColumnEncryptionSetting object for each column.
$ces = @()
$oldCekName = "CEK1"
$tables = $database.Tables
for($i=0; $i -lt $tables.Count; $i++){
    $columns = $tables[$i].Columns
    for($j=0; $j -lt $columns.Count; $j++) {
        if($columns[$j].isEncrypted -and $columns[$j].ColumnEncryptionKeyName -eq $oldCekName) {
            $threeColPartName = $tables[$i].Schema + "." + $tables[$i].Name + "." + $columns[$j].Name 
            $ces += New-SqlColumnEncryptionSettings -ColumnName $threeColPartName -EncryptionType $columns[$j].EncryptionType -EncryptionKey $newCekName
        }
     }
}

# Re-encrypt all columns, currently encrypted with the old column encryption key, using the new column encryption key.
Set-SqlColumnEncryption -ColumnEncryptionSettings $ces -InputObject $database -UseOnlineApproach -MaxDowntimeInSeconds 120 -LogFileDirectory .

# Remove the old column encryption key metadata.
Remove-SqlColumnEncryptionKey -Name $oldCekName -InputObject $database
```

## <a name="next-steps"></a>Next Steps
- [Выполнение запросов к столбцам с помощью Always Encrypted с использованием SQL Server Management Studio](always-encrypted-query-columns-ssms.md)
- [Разработка приложений с помощью Always Encrypted](always-encrypted-client-development.md)
  
## <a name="see-also"></a>См. также:
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Общие сведения об управлении ключами для Always Encrypted](overview-of-key-management-for-always-encrypted.md) 
- [Настройка постоянного шифрования с помощью PowerShell](configure-always-encrypted-using-powershell.md)
- [Ротация ключей Always Encrypted с помощью SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
- [DROP COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/drop-column-master-key-transact-sql.md)
- [CREATE COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/create-column-encryption-key-transact-sql.md)
- [ALTER COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/alter-column-encryption-key-transact-sql.md)
- [DROP COLUMN ENCRYPTION KEY (Transact-SQL)](../../../t-sql/statements/drop-column-encryption-key-transact-sql.md) 
- [sys.column_master_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
- [sys.column_encryption_keys (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-column-encryption-keys-transact-sql.md)

---
title: Создание и хранение главных ключей столбцов для Always Encrypted
description: Узнайте, как выбрать хранилище ключей и создать главные ключи столбцов для SQL Server Always Encrypted.
ms.custom: seo-lt-2019
ms.date: 10/31/2019
ms.prod: sql
ms.prod_service: security, sql-database"
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
ms.assetid: 856e8061-c604-4ce4-b89f-a11876dd6c88
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c08fb0c0fc82d252e87847562957705e03e30512
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91867824"
---
# <a name="create-and-store-column-master-keys-for-always-encrypted"></a>Создание и хранение главных ключей столбцов для Always Encrypted
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]

*Главные ключи столбцов* являются защищающими ключами, которые используются в режиме постоянного шифрования для кодирования ключей шифрования столбцов. Главные ключи столбцов должны храниться в надежном хранилище ключей и быть доступны для приложений, которым необходимо зашифровать или расшифровать данные, а также для средств для настройки постоянного шифрования и управления ключами постоянного шифрования.

Эта статья содержит подробные сведения о выборе хранилища ключей и создании главных ключей столбцов для постоянного шифрования. Подробные сведения см. в разделе [Overview of Key Management for Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)(Общие сведения об управлении ключами для постоянного шифрования).

## <a name="selecting-a-key-store-for-your-column-master-key"></a>Выбор хранилища ключей для главного ключа столбца

Постоянное шифрование поддерживает несколько хранилищ ключей для хранения главных ключей столбцов постоянного шифрования. Поддерживаемые хранилища зависят от используемого драйвера и версии.

Следует принимать во внимание две высокоуровневые категории хранилищ ключей — *локальные хранилища ключей*и *централизованные хранилища ключей*.

###  <a name="local-or-centralized-key-store"></a>Какое хранилище использовать — локальное или централизованное?

* **Локальные хранилища ключей** могут использоваться только приложениями на компьютерах с локальным хранилищем ключей. Другими словами, нужно реплицировать хранилище ключей и ключ на каждый компьютер, где запущено приложение. Примером локального хранилища ключей является хранилище сертификатов Windows. При использовании локального хранилища ключей необходимо убедиться, что хранилище ключей существует на каждом компьютере, на котором размещается приложение, и что компьютер содержит главные ключи столбцов, необходимые приложению для доступа к данным, защищенным с помощью постоянного шифрования. При первой подготовке главного ключа столбца или при изменении (смене) ключа необходимо проверить, что ключ будет развернут на всех компьютерах, где размещены приложения.

* **Централизованные хранилища ключей** обслуживают приложения на нескольких компьютерах. Примером централизованного хранилища ключей является [хранилище ключей Azure](https://azure.microsoft.com/services/key-vault/). Централизованное хранилище ключей обычно упрощает управление ключами, так как вам не требуется поддерживать несколько копий главных ключей столбцов на нескольких компьютерах. Убедитесь, что приложения настроены для подключения к централизованному хранилищу ключей.

### <a name="which-key-stores-are-supported-in-always-encrypted-enabled-client-drivers"></a>Какие хранилища ключей поддерживаются клиентскими драйверами с включенным постоянным шифрованием?

Клиентские драйверы с включенным постоянным шифрованием — это драйверы клиента SQL Server со встроенной поддержкой для внедрения постоянного шифрования в клиентские приложения. Они включают в себя несколько встроенных поставщиков распространенных хранилищ ключей. Некоторые драйверы также позволяют реализовывать и регистрировать настраиваемый поставщик хранилища главных ключей столбцов, поэтому вы можете использовать любое хранилище ключей, даже если для него отсутствует встроенный поставщик. Делая выбор между встроенным поставщиком и настраиваемым поставщиком, необходимо учитывать, что использование встроенного поставщика обычно связано с незначительным изменением приложений (в некоторых случаях требуется только изменить строку подключения к базе данных).

Доступные встроенные поставщики зависят от выбранного драйвера, версии драйвера и операционной системы.  Обратитесь к документации по Always Encrypted для конкретного драйвера, чтобы определить, какие готовые хранилища ключей поддерживаются, и узнать, поддерживает ли ваш драйвер настраиваемые поставщики хранилищ ключей. См. раздел [Разработка приложений с помощью Always Encrypted](always-encrypted-client-development.md).

### <a name="which-key-stores-are-supported-in-sql-tools"></a>Какие хранилища ключей поддерживаются в инструментах SQL?
SQL Server Management Studio и модуль SqlServer PowerShell поддерживают только главные ключи столбцов, хранящиеся в Azure Key Vault, хранилище сертификатов Windows и хранилищах ключей, которые предоставляют API шифрования следующего поколения (CNG) или API шифрования (CAPI). 

## <a name="creating-column-master-keys-in-windows-certificate-store"></a>Создание главных ключей столбцов в хранилище сертификатов Windows    

Главный ключ столбца может быть сертификатом, который хранится в хранилище сертификатов Windows. Драйвер с поддержкой Always Encrypted не проверяет дату окончания срока действия или цепочку центра сертификации. Сертификат просто используется как пара ключей, состоящая из открытого и закрытого ключей.

Чтобы быть допустимым главным ключом столбца, сертификат должен отвечать следующим условиям:
* являться сертификатом X.509;
* храниться в одном из расположений хранилища сертификатов: *local machine* или *current user* (чтобы создать сертификат в хранилище сертификатов локального компьютера, необходимо иметь права администратора на целевом компьютере);
* содержать закрытый ключ (рекомендуемая длина ключей в сертификате — 2048 бит или больше);
* поддерживать обмен ключами.


Существует несколько способов создания сертификата, который является допустимым главным ключом столбца, но проще всего создать самозаверяющий сертификат.


### <a name="create-a-self-signed-certificate-using-powershell"></a>Создание самозаверяющего сертификата с помощью PowerShell

Для создания самозаверяющего сертификата используется командлет [New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) . В примере ниже показано создание сертификата, который можно использовать в качестве главного ключа столбца для постоянного шифрования.

```
# New-SelfSignedCertificate is a Windows PowerShell cmdlet that creates a self-signed certificate. The below examples show how to generate a certificate that can be used as a column master key for Always Encrypted.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048 

# To create a certificate in the local machine certificate store location you need to run the cmdlet as an administrator.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:LocalMachine\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage KeyEncipherment -KeySpec KeyExchange -KeyLength 2048
```

### <a name="create-a-self-signed-certificate-using-sql-server-management-studio-ssms"></a>Создание самозаверяющего сертификата с помощью среды SQL Server Management Studio (SSMS)

Дополнительные сведения см. в разделе [Подготовка к работе ключей Always Encrypted с помощью SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md).
Пошаговое руководство, где используется среда SSMS и ключи постоянного шифрования сохраняются в хранилище сертификатов Windows, см. в статье [Постоянное шифрование: защита конфиденциальных данных в базе данных SQL с помощью шифрования базы данных и хранение ключей шифрования в хранилище сертификатов Windows](/azure/azure-sql/database/always-encrypted-certificate-store-configure).


### <a name="making-certificates-available-to-applications-and-users"></a>Предоставление приложениям и пользователям доступа к сертификатам

Если главный ключ столбца является сертификатом, хранящимся в расположении хранилища сертификатов на *локальном компьютере* , необходимо экспортировать сертификат с закрытым ключом и импортировать его на все компьютеры, где размещены приложения, которые будут шифровать или расшифровывать данные, хранящиеся в зашифрованных столбцах, или средства для настройки постоянного шифрования и для управления ключами постоянного шифрования. Кроме того, каждому пользователю должно быть предоставлено разрешение на чтение сертификата, хранящегося в расположение хранилища сертификатов на локальном компьютера, чтобы он мог использовать сертификат в качестве главного ключа столбца.

Если главный ключ столбца является сертификатом, хранящимся в расположении хранилища сертификатов под учетной записью *текущего пользователя* , необходимо экспортировать сертификат с закрытым ключом и импортировать его в расположение хранилища сертификатов для всех учетных записей пользователей, где размещены приложения, которые будут шифровать или расшифровывать данные, хранящиеся в зашифрованных столбцах, или средства для настройки постоянного шифрования и для управления ключами постоянного шифрования (на всех компьютерах с этими приложениями или средствами). Настройка разрешений не требуется — после входа в систему пользователь может обращаться ко всем сертификатам в расположении хранилища сертификатов текущего пользователя.

#### <a name="using-powershell"></a>Использование PowerShell
Для импорта и экспорта сертификата используются командлеты [Import-PfxCertificate](/powershell/module/pkiclient/import-pfxcertificate) и [Export-PfxCertificate](/powershell/module/pkiclient/export-pfxcertificate) .

#### <a name="using-microsoft-management-console"></a>Использование консоли управления (MMC) 

Чтобы предоставить пользователю разрешение на *чтение* сертификата, хранящегося в расположение хранилища сертификатов на локальном компьютере, выполните приведенные далее действия.

1.  Откройте окно командной строки и введите **mmc**.
2.  В консоли MMC в меню **Файл** выберите **Добавить или удалить оснастку**.
3.  В диалоговом окне **Добавление или удаление оснастки** нажмите кнопку **Добавить**.
4.  В диалоговом окне **Добавление изолированной оснастки** выберите **Сертификаты**и нажмите кнопку **Добавить**.
5.  В диалоговом окне оснастки **Сертификаты** выберите **Учетная запись компьютера**и нажмите кнопку **Готово**.
6.  В диалоговом окне **Добавление изолированной оснастки** нажмите кнопку **Закрыть**.
7.  В диалоговом окне **Добавление или удаление оснастки** нажмите кнопку **ОК**.
8.  В оснастке **Сертификаты** найдите сертификат в папке **Сертификаты/Личное**, правой кнопкой мыши щелкните "Сертификат", укажите**Все задачи** и выберите **Управление закрытыми ключами**.
9.  В диалоговом окне **Безопасность** добавьте разрешение на чтение для учетной записи пользователя (если необходимо).

## <a name="creating-column-master-keys-in-azure-key-vault"></a>Создание главных ключей столбцов в хранилище ключей Azure

Хранилище ключей Azure обеспечивает защиту криптографических ключей и удобно для хранения главных ключей столбцов для постоянного шифрования, особенно в том случае, если приложения размещены в Azure. Для создания ключа в [хранилище ключей Azure](/azure/key-vault/general/overview)необходимы [подписка Azure](https://azure.microsoft.com/free/) и хранилище ключей Azure.

### <a name="using-powershell"></a>Использование PowerShell

В примере ниже показано создание хранилища ключей Azure и ключа и последующее предоставление разрешений нужному пользователю.

```
# Create a column master key in Azure Key Vault.
Connect-AzAccount
$SubscriptionId = "<Azure subscription ID>"
$resourceGroup = "<resource group name>"
$azureLocation = "<key vault location>"
$akvName = "<key vault name>"
$akvKeyName = "<column master key name>"
$azureCtx = Set-AzContext -SubscriptionId $SubscriptionId # Sets the context for the below cmdlets to the specified subscription.
New-AzResourceGroup -Name $resourceGroup -Location $azureLocation # Creates a new resource group - skip, if you desire group already exists.
New-AzKeyVault -VaultName $akvName -ResourceGroupName $resourceGroup -Location $azureLocation -SKU premium # Creates a new key vault - skip if your vault already exists.
Set-AzKeyVaultAccessPolicy -VaultName $akvName -ResourceGroupName $resourceGroup -PermissionsToKeys get, create, delete, list, update, import, backup, restore, wrapKey, unwrapKey, sign, verify -UserPrincipalName $azureCtx.Account
$akvKey = Add-AzKeyVaultKey -VaultName $akvName -Name $akvKeyName -Destination HSM
```

### <a name="using-sql-server-management-studio-ssms"></a>Использование среды SQL Server Management Studio (SSMS)

Дополнительные сведения о создании главного ключа столбца в Azure Key Vault с помощью среды SSMS см. в разделе [Подготовка ключей Always Encrypted с помощью SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md).
Пошаговое руководство, где используется среда SSMS и ключи постоянного шифрования сохраняются в хранилище ключей Azure, см. в статье [Постоянное шифрование: защита конфиденциальных данных в базе данных SQL с помощью шифрования данных и хранение ключей шифрования в хранилище ключей Azure](/azure/azure-sql/database/always-encrypted-azure-key-vault-configure).

### <a name="making-azure-key-vault-keys-available-to-applications-and-users"></a>Предоставление приложениям и пользователям доступа к ключам в хранилище ключей Azure

При использовании ключа хранилища ключей Azure в качестве главного ключа столбца приложение должно пройти проверку подлинности в Azure, а удостоверение приложения должно иметь следующие разрешения на доступ к хранилищу ключей: *get*, *unwrapKey* и *verify*. 

Для подготовки ключей шифрования столбцов, которые защищены с помощью главного ключа столбца, хранящегося в хранилище ключей Azure, необходимы разрешения *get*, *unwrapKey*, *wrapKey*, *sign*и *verify* . Кроме того, для создания ключа в хранилище ключей Azure необходимо разрешение *create* , для просмотра содержимого хранилища ключей требуется разрешение *list* .

#### <a name="using-powershell"></a>Использование PowerShell

Чтобы предоставить пользователям и приложениям доступ к фактическим ключам в Azure Key Vault, необходимо задать политику доступа к хранилищу ([Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy)):

```
$vaultName = "<vault name>"
$resourceGroupName = "<resource group name>"
$userPrincipalName = "<user to grant access to>"
$clientId = "<client Id>"

# grant users permissions to the keys:
Set-AzKeyVaultAccessPolicy -VaultName $vaultName -ResourceGroupName $resourceGroupName -PermissionsToKeys create,get,wrapKey,unwrapKey,sign,verify,list -UserPrincipalName $userPrincipalName
# grant applications permissions to the keys:
Set-AzKeyVaultAccessPolicy  -VaultName $vaultName  -ResourceGroupName $resourceGroupName -ServicePrincipalName $clientId -PermissionsToKeys get,wrapKey,unwrapKey,sign,verify,list
```

## <a name="creating-column-master-keys-in-hardware-security-modules-using-cng"></a>Создание главных ключей столбцов в аппаратных модулях безопасности с помощью CNG

Главный ключ столбца для постоянного шифрования может храниться в хранилище ключей, реализующем API криптографии следующего поколения (CNG). Как правило, этот тип хранилища является аппаратным модулем безопасности (HSM). HSM — это физическое устройство, которое защищает цифровые ключи и управляет ими, а также обеспечивает обработку шифрования. Аппаратные модули безопасности традиционно имеют вид подключаемой платы или внешнего устройства, напрямую подключаемого к компьютеру (локальный HSM) или сетевому серверу.

Чтобы модуль HSM был доступен для приложений на данном компьютере, на компьютере должен быть установлен и настроен поставщик хранилища ключей (KSP), реализующий CNG. Клиентский драйвер с постоянным шифрованием (поставщик хранилища главных ключей столбцов в драйвере) использует KSP для шифрования и расшифровки ключей шифрования столбцов, защищенных главным ключом столбца, который хранится в хранилище ключей.

В состав Windows входит ПО поставщика хранилища ключей Microsoft Software Key Storage Provider — программный поставщик KSP, который можно использовать для тестирования. См. раздел [CNG Key Storage Providers](/windows/desktop/SecCertEnroll/cng-key-storage-providers)(Поставщики хранилища ключей CNG).

### <a name="creating-column-master-keys-in-a-key-store-using-cngksp"></a>Создание главных ключей столбцов в хранилище ключей с помощью CNG или KSP

Главный ключ столбца должен быть асимметричным ключом (парой открытого и закрытого ключей), использующим алгоритм RSA. Рекомендуемая длина ключа — 2048 или больше.

#### <a name="using-hsm-specific-tools"></a>Использование средств с поддержкой HSM
Обратитесь к документации по имеющемуся аппаратному модулю безопасности.

#### <a name="using-powershell"></a>Использование PowerShell

Для создания ключа в хранилище ключей с помощью CNG в PowerShell можно использовать API-интерфейсы .NET.


```
$cngProviderName = "Microsoft Software Key Storage Provider" # If you have an HSM, you can use a KSP for your HSM instead of a Microsoft KSP
$cngAlgorithmName = "RSA"
$cngKeySize = 2048 # Recommended key size for Always Encrypted column master keys
$cngKeyName = "AlwaysEncryptedKey" # Name identifying your new key in the KSP
$cngProvider = New-Object System.Security.Cryptography.CngProvider($cngProviderName)
$cngKeyParams = New-Object System.Security.Cryptography.CngKeyCreationParameters
$cngKeyParams.provider = $cngProvider
$cngKeyParams.KeyCreationOptions = [System.Security.Cryptography.CngKeyCreationOptions]::OverwriteExistingKey
$keySizeProperty = New-Object System.Security.Cryptography.CngProperty("Length", [System.BitConverter]::GetBytes($cngKeySize), [System.Security.Cryptography.CngPropertyOptions]::None);
$cngKeyParams.Parameters.Add($keySizeProperty)
$cngAlgorithm = New-Object System.Security.Cryptography.CngAlgorithm($cngAlgorithmName)
$cngKey = [System.Security.Cryptography.CngKey]::Create($cngAlgorithm, $cngKeyName, $cngKeyParams)
```

#### <a name="using-sql-server-management-studio"></a>Использование среды SQL Server Management Studio

См. раздел [Подготовка к работе ключей Always Encrypted с помощью SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md).

### <a name="making-cng-keys-available-to-applications-and-users"></a>Предоставление приложениям и пользователям доступа к ключам CNG

Сведения о настройке KSP на компьютере и предоставлении приложениям и пользователям доступа к HSM см. в документации по HSM и KSP.

## <a name="creating-column-master-keys-in-hardware-security-modules-using-capi"></a>Создание главных ключей столбцов в аппаратных модулях безопасности с помощью CAPI

Главный ключ столбца для постоянного шифрования может храниться в хранилище ключей, реализующем API криптографии (CAPI). Как правило, такое хранилище представляет собой аппаратный модуль безопасности (HSM) — физическое устройство, которое защищает цифровые ключи и управляет ими, а также обеспечивает обработку шифрования. Аппаратные модули безопасности традиционно имеют вид подключаемой платы или внешнего устройства, напрямую подключаемого к компьютеру (локальный HSM) или сетевому серверу.

Чтобы модуль HSM был доступен для приложений на данном компьютере, на компьютере должен быть установлен и настроен поставщик служб шифрования (CSP), реализующий CAPI. Клиентский драйвер с постоянным шифрованием (поставщик хранилища главных ключей столбцов в драйвере) использует CSP для шифрования и расшифровки ключей шифрования столбцов, защищенных главным ключом столбца, который хранится в хранилище ключей. 

> [!NOTE]
> CAPI — это устаревший API, который не рекомендуется использовать. Если для вашего модуля HSM доступен поставщик KSP, следует использовать именно его, а не CSP или CAPI.

CSP должен поддерживать алгоритм RSA, используемый с постоянным шифрованием.

В состав Windows входят следующие программные (не поддерживаемые модулем HSM) поставщики CSP, которые поддерживают RSA и могут использоваться в целях тестирования: Microsoft Enhanced RSA и AES Cryptographic Provider.

### <a name="creating-column-master-keys-in-a-key-store-using-capicsp"></a>Создание главных ключей столбцов в хранилище ключей с помощью CAPI или CSP

Главный ключ столбца должен быть асимметричным ключом (парой открытого и закрытого ключей), использующим алгоритм RSA. Рекомендуемая длина ключа — 2048 или больше.

#### <a name="using-hsm-specific-tools"></a>Использование средств с поддержкой HSM
Обратитесь к документации по имеющемуся аппаратному модулю безопасности.

#### <a name="using-sql-server-management-studio-ssms"></a>Использование среды SQL Server Management Studio (SSMS)
См. раздел [Подготовка к работе ключей Always Encrypted с помощью SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md).

### <a name="making-cng-keys-available-to-applications-and-users"></a>Предоставление приложениям и пользователям доступа к ключам CNG
Сведения о настройке CSP на компьютере и предоставлении приложениям и пользователям доступа к HSM см. в документации по HSM и CSP.
 
## <a name="next-steps"></a>Next Steps  
- [Подготовка к работе ключей Always Encrypted с помощью SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md)
- [Подготовка ключей Always Encrypted с помощью PowerShell](configure-always-encrypted-keys-using-powershell.md)
  
## <a name="see-also"></a>См. также: 
- [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)
- [Общие сведения об управлении ключами для Always Encrypted](../../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
---
description: CREATE CRYPTOGRAPHIC PROVIDER (Transact-SQL)
title: CREATE CRYPTOGRAPHIC PROVIDER (Transact-SQL) | Документы Майкрософт
ms.date: 03/14/2017
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_CRYPTOGRAPHIC_TSQL
- CRYPTOGRAPHIC PROVIDER
- PROVIDER_TSQL
- CREATE CRYPTOGRAPHIC
- CREATE CRYPTOGRAPHIC PROVIDER
- CRYPTOGRAPHIC_PROVIDER_TSQL
- CREATE_CRYPTOGRAPHIC_PROVIDER_TSQL
- PROVIDER
dev_langs:
- TSQL
helpviewer_keywords:
- 33085 (Database Engine error)
- CREATE CRYPTOGRAPHIC PROVIDER statement
- 33032 (Database Engine error)
ms.assetid: 059a39a6-9d32-4d3f-965b-0a1ce75229c7
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 15793074e9aca337f2d1b1fe93ae3e603b518c01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192745"
---
# <a name="create-cryptographic-provider-transact-sql"></a>CREATE CRYPTOGRAPHIC PROVIDER (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Создает поставщика служб шифрования в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на основе поставщика расширенного управления ключами.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```syntaxsql  
CREATE CRYPTOGRAPHIC PROVIDER provider_name   
    FROM FILE = path_of_DLL  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы
 *provider_name*  
 Имя поставщика расширенного управления ключами.  
  
 *path_of_DLL*  
 Путь к DLL-библиотеке, реализующей интерфейс расширенного управления ключами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. При использовании **соединителя SQL Server для Microsoft Azure Key Vault** расположением по умолчанию является **C:\Program Files\Microsoft SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll**.  
  
## <a name="remarks"></a>Комментарии  
 Все ключи, созданные поставщиком, ссылаются на поставщик по его идентификатору GUID. Идентификатор GUID сохраняются во всех версиях DLL-библиотеки.  
  
 Библиотека, реализующая интерфейс SQLEKM, должна быть подписана цифровой подписью с использованием любого сертификата. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполнит проверку подписи. При этом проверяется цепочка сертификатов, корень которой должен быть установлен в расположение **Trusted Root Cert Authorities** системной папки Windows. Если проверка подписи не выполнена должным образом, выполнение инструкции CREATE CRYPTOGRAPHIC PROVIDER оканчивается неудачей. Дополнительные сведения о сертификатах и цепочках сертификатов см. в разделе [Сертификаты SQL Server и асимметричные ключи](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
 Когда поставщик EKM DLL-библиотек реализует не все необходимые методы, то инструкция CREATE CRYPTOGRAPHIC PROVIDER может вернуть ошибку 33085:  
  
 `One or more methods cannot be found in cryptographic provider library '%.*ls'.`  
  
 Если файл заголовка, используемый для создания поставщика EKM DLL-библиотек, устарел, то инструкция CREATE CRYPTOGRAPHIC PROVIDER может вернуть ошибку 33032:  
  
 `SQL Crypto API version '%02d.%02d' implemented by provider is not supported. Supported version is '%02d.%02d'.`  
  
## <a name="permissions"></a>Разрешения  
 Требует разрешения CONTROL SERVER или членства в предопределенной роли сервера **sysadmin**.  
  
## <a name="examples"></a>Примеры  
 В следующем примере из DLL-файла в `SecurityProvider` создается поставщик служб шифрования с именем [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Этот DLL-файл имеет имя `c:\SecurityProvider\SecurityProvider_v1.dll` и установлен на сервере. Сначала необходимо установить на сервере сертификат поставщика.  
  
```sql  
-- Install the provider  
CREATE CRYPTOGRAPHIC PROVIDER SecurityProvider  
    FROM FILE = 'C:\SecurityProvider\SecurityProvider_v1.dll';  
```  
  
## <a name="see-also"></a>См. также:  
 [Расширенное управление ключами (EKM)](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL)](../../t-sql/statements/alter-cryptographic-provider-transact-sql.md)   
 [DROP CRYPTOGRAPHIC PROVIDER (Transact-SQL)](../../t-sql/statements/drop-cryptographic-provider-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [Расширенное управление ключами с помощью хранилища ключей Azure (SQL Server)](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
 [Настройка расширенного управления ключами SQL Server TDE с помощью Azure Key Vault](../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)  
 [sys.cryptographic_providers](../../relational-databases/system-catalog-views/sys-cryptographic-providers-transact-sql.md)  
 [sys.dm_cryptographic_provider_properties](../../relational-databases/system-dynamic-management-views/sys-dm-cryptographic-provider-properties-transact-sql.md)
  
  

---
title: Использование Azure Active Directory
description: Узнайте о методах проверки подлинности Azure Active Directory, доступных в Microsoft OLE DB Driver for SQL Server, которые позволяют подключаться к базам данных SQL Azure.
ms.custom: ''
ms.date: 09/30/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
author: bazizi
ms.author: v-beaziz
ms.openlocfilehash: f75cfc74616a58975bedf2dc7b2de342c0c57066
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837403"
---
# <a name="using-azure-active-directory"></a>Использование Azure Active Directory
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="purpose"></a>Назначение


Начиная с версии [18.2.1](../release-notes-for-oledb-driver-for-sql-server.md#1821), Microsoft OLE DB Driver for SQL Server позволяет приложениям OLE DB подключаться к экземпляру Базы данных SQL Azure с помощью федеративного удостоверения. К новым методам проверки подлинности относятся:
- имя для входа Azure Active Directory и пароль;
- Маркер доступа Azure Active Directory
- Интегрированная проверка подлинности Azure Active Directory
- имя для входа SQL и пароль.


В версии [18.3.0](../release-notes-for-oledb-driver-for-sql-server.md#1830) добавлена поддержка следующих способов проверки подлинности:
- Интерактивная проверка подлинности Azure Active Directory
- Проверка подлинности Azure Active Directory с использованием управляемого удостоверения

В версии [18.5.0](../release-notes-for-oledb-driver-for-sql-server.md#1850) добавлена поддержка следующего способа проверки подлинности:
- Проверка подлинности субъекта-службы Azure Active Directory.

> [!NOTE]
> Использование следующих режимов проверки подлинности с параметром `DataTypeCompatibility` (или соответствующего свойства этого элемента) со значением `80` **не** поддерживается:
> - проверка подлинности Azure Active Directory с помощью имени для входа и пароля;
> - проверка подлинности Azure Active Directory с помощью маркера доступа;
> - Интегрированная проверка подлинности Azure Active Directory
> - Интерактивная проверка подлинности Azure Active Directory
> - проверка подлинности Azure Active Directory с использованием управляемых удостоверений;
> - проверка подлинности субъекта-службы Azure Active Directory.

## <a name="connection-string-keywords-and-properties"></a>Ключевые слова и свойства строки подключения
Для поддержки проверки подлинности Azure Active Directory были введены следующие ключевые слова строки подключения:

|Ключевое слово строки подключения|Свойства подключения|Описание|
|---               |---                |---        |
|Маркер доступа|SSPROP_AUTH_ACCESS_TOKEN|Указывает маркер доступа для проверки подлинности в Azure Active Directory. |
|Аутентификация|SSPROP_AUTH_MODE|Указывает используемый метод проверки подлинности.|

Дополнительные сведения о новых ключевых словах и свойствах см. на следующих страницах:
- [Использование ключевых слов строки подключения с драйвером OLE DB для SQL Server](../applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)
- [Свойства инициализации и авторизации](../ole-db-data-source-objects/initialization-and-authorization-properties.md)

## <a name="encryption-and-certificate-validation"></a>Шифрование и проверка сертификатов
В этом разделе обсуждаются изменения в поведении шифрования и проверки сертификатов. Эти изменения эффективны **только** при использовании новых ключевых слов строк подключения для маркера проверки подлинности или маркера доступа (или их соответствующих свойств).

### <a name="encryption"></a>Шифрование
Для повышения безопасности при использовании новых свойств или ключевых слов подключения драйвер переопределяет значение шифрования по умолчанию, задав вместо него значение `yes`. Переопределение происходит во время инициализации объекта источника данных. Если шифрование устанавливается до инициализации любым способом, значение учитывается и не переопределяется.

> [!NOTE]   
> В приложениях объектов ADO и приложениях, которые получают интерфейс `IDBInitialize` через `IDataInitialize::GetDataSource`, основной компонент, реализующий интерфейс, явно устанавливает для шифрования значение по умолчанию `no`. В результате новые свойства или ключевые слова проверки подлинности учитывают этот параметр и значение шифрования **не** переопределяется. Поэтому **рекомендуется**, чтобы эти приложения явно настраивали `Use Encryption for Data=true` для переопределения значения по умолчанию.

### <a name="certificate-validation"></a>Проверка сертификатов
Для повышения безопасности новые свойства или ключевые слова подключения учитывают параметр `TrustServerCertificate` (и его соответствующие ключевые слова и свойства строки подключения) **независимо от параметра шифрования клиента**. В результате сертификат сервера проверяется по умолчанию.

> [!NOTE]   
> Проверку сертификата можно также контролировать с помощью поля `Value` записи реестра `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SNI18.0\GeneralFlags\Flag2`. Допустимые значения: `0` или `1`. Драйвер OLE DB выбирает наиболее безопасный вариант между реестром и параметрами свойств или ключевых слов подключения. То есть драйвер будет проверять сертификат сервера, если по крайней мере один из параметров реестра или подключения включает проверку сертификата сервера.

## <a name="gui-additions"></a>Добавление графических пользовательских интерфейсов
Графический пользовательский интерфейс драйвера был усовершенствован, чтобы обеспечить проверку подлинности в Azure Active Directory. Дополнительные сведения см. в разделе:
- [Диалоговое окно входа SQL Server](../help-topics/sql-server-login-dialog.md)
- [Конфигурация универсального канала передачи данных (UDL)](../help-topics/data-link-pages.md)

## <a name="example-connection-strings"></a>Примеры строк подключения
В этом разделе приведены примеры новых и существующих ключевых слов строки подключения для использования со свойствами `IDataInitialize::GetDataSource` и `DBPROP_INIT_PROVIDERSTRING`.

### <a name="sql-authentication"></a>Проверка подлинности SQL
- Использование среды `IDataInitialize::GetDataSource`:
    - Добавления:
        > Provider=MSOLEDBSQL;Data Source=[сервер];Initial Catalog=[база_данных];**Authentication=SqlPassword**;User ID=[имя_пользователя];Password=[пароль];Use Encryption for Data=true.
    - Устарело:
        > Provider=MSOLEDBSQL;Data Source=[сервер];Initial Catalog=[база_данных];User ID=[имя_пользователя];Password=[пароль];Use Encryption for Data=true.
- Использование среды `DBPROP_INIT_PROVIDERSTRING`:
    - Добавления:
        > Server=[сервер];Database=[база_данных];**Authentication=SqlPassword**;UID=[имя_пользователя];PWD=[пароль];Encrypt=yes.
    - Устарело:
        > Server=[сервер];Database=[база_данных];UID=[имя_пользователя];PWD=[пароль];Encrypt=yes.

### <a name="integrated-windows-authentication-using-security-support-provider-interface-sspi"></a>Встроенная проверка подлинности в Windows с помощью интерфейса поставщика поддержки безопасности (SSPI)

- Использование среды `IDataInitialize::GetDataSource`:
    - Добавления:
        > Provider=MSOLEDBSQL;Data Source=[сервер];Initial Catalog=[база_данных];**Authentication=ActiveDirectoryIntegrated**;Use Encryption for Data=true.
    - Устарело:
        > Provider=MSOLEDBSQL;Data Source=[сервер];Initial Catalog=[база_данных];**Integrated Security=SSPI**;Use Encryption for Data=true.
- Использование среды `DBPROP_INIT_PROVIDERSTRING`:
    - Добавления:
        > Server=[сервер];Database=[база_данных];**Authentication=ActiveDirectoryIntegrated**;Encrypt=yes.
    - Устарело:
        > Server=[сервер];Database=[база_данных];**Trusted_Connection=yes**;Encrypt=yes.

### <a name="azure-active-directory-username-and-password-authentication"></a>Проверка подлинности с помощью имени пользователя и пароля Azure Active Directory

- Использование среды `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[сервер];Initial Catalog=[база_данных];**Authentication=ActiveDirectoryPassword**;User ID=[имя_пользователя];Password=[пароль];Use Encryption for Data=true.
- Использование среды `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[сервер];Database=[база_данных];**Authentication=ActiveDirectoryPassword**;UID=[имя_пользователя];PWD=[пароль];Encrypt=yes.

### <a name="azure-active-directory-integrated-authentication"></a>Интегрированная проверка подлинности Azure Active Directory

- Использование среды `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[сервер];Initial Catalog=[база_данных];**Authentication=ActiveDirectoryIntegrated**;Use Encryption for Data=true.
- Использование среды `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[сервер];Database=[база_данных];**Authentication=ActiveDirectoryIntegrated**;Encrypt=yes.

### <a name="azure-active-directory-authentication-using-an-access-token"></a>Проверка подлинности Azure Active Directory с помощью маркера доступа

- Использование среды `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[сервер];Initial Catalog=[база_данных];**Access Token=[токен доступа]** ;Use Encryption for Data=true.
- Использование среды `DBPROP_INIT_PROVIDERSTRING`:
    > Предоставление маркера доступа через `DBPROP_INIT_PROVIDERSTRING` не поддерживается.

### <a name="azure-active-directory-interactive-authentication"></a>Интерактивная проверка подлинности Azure Active Directory

- Использование среды `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[сервер];Initial Catalog=[база_данных];**Authentication=ActiveDirectoryInteractive**;User ID=[имя_пользователя];Use Encryption for Data=true.
- Использование среды `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[сервер];Database=[база_данных];**Authentication=ActiveDirectoryInteractive**;UID=[имя_пользователя];Encrypt=yes.

### <a name="azure-active-directory-managed-identity-authentication"></a>Проверка подлинности Azure Active Directory с использованием управляемого удостоверения

- Использование среды `IDataInitialize::GetDataSource`:
    - Управляемое удостоверение, назначаемое пользователем:
        > Provider=MSOLEDBSQL;Data Source=[сервер];Initial Catalog=[база_данных];**Authentication=ActiveDirectoryMSI**;User ID=[ИД объекта];Use Encryption for Data=true.
    - Управляемое удостоверение, назначаемое системой:
        > Provider=MSOLEDBSQL;Data Source=[сервер];Initial Catalog=[база_данных];**Authentication=ActiveDirectoryMSI**;Use Encryption for Data=true.
- Использование среды `DBPROP_INIT_PROVIDERSTRING`:
    - Управляемое удостоверение, назначаемое пользователем:
        > Server=[сервер];Database=[база_данных];**Authentication=ActiveDirectoryMSI**;UID=[ИД объекта];Encrypt=yes.
    - Управляемое удостоверение, назначаемое системой:
        > Server=[сервер];Database=[база_данных];**Authentication=ActiveDirectoryMSI**;Encrypt=yes.

### <a name="azure-active-directory-service-principal-authentication"></a>Проверка подлинности субъекта-службы Azure Active Directory.

- Использование среды `IDataInitialize::GetDataSource`:
    > Provider=MSOLEDBSQL;Data Source=[сервер];Initial Catalog=[база_данных];**Authentication=ActiveDirectoryServicePrincipal**;User ID=[ИД_приложения_(клиента)];Password=[секрет_приложения_(клиента)];Use Encryption for Data=true
- Использование среды `DBPROP_INIT_PROVIDERSTRING`:
    > Server=[сервер];Database=[база_данных];**Authentication=ActiveDirectoryServicePrincipal**;UID=[ИД_приложения_(клиента)];PWD=[секрет_приложения_(клиента)];Encrypt=yes

## <a name="code-samples"></a>Примеры кода

В следующих примерах показан код, необходимый для подключения к Azure Active Directory с помощью ключевых слов. 

### <a name="access-token"></a>Маркер доступа
```cpp
#include <string>
#include <iostream>
#include <msdasc.h>

int main()
{
    wchar_t azureServer[] = L"server";
    wchar_t azureDatabase[] = L"mydatabase";
    wchar_t accessToken[] = L"eyJ0eXAiOi...";
    IDBInitialize *pIDBInitialize = nullptr;
    IDataInitialize* pIDataInitialize = nullptr;
    HRESULT hr = S_OK;

    CoInitialize(nullptr);

    // Construct the connection string.
    std::wstring connString = L"Provider=MSOLEDBSQL;Data Source=" + std::wstring(azureServer) + L";Initial Catalog=" + 
                              std::wstring(azureDatabase) + L";Access Token=" + accessToken + L";Use Encryption for Data=true;";
    hr = CoCreateInstance(CLSID_MSDAINITIALIZE, nullptr, CLSCTX_INPROC_SERVER, 
                          IID_IDataInitialize, reinterpret_cast<LPVOID*>(&pIDataInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to create an IDataInitialize instance." << std::endl;
        goto Cleanup;
    }
    hr = pIDataInitialize->GetDataSource(nullptr, CLSCTX_INPROC_SERVER, connString.c_str(), 
                                         IID_IDBInitialize, reinterpret_cast<IUnknown**>(&pIDBInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to get data source object." << std::endl;
        goto Cleanup;
    }
    hr = pIDBInitialize->Initialize();
    if (FAILED(hr))
    {
        std::cout << "Failed to establish connection." << std::endl;
        goto Cleanup;
    }

Cleanup:
    if (pIDBInitialize)
    {
        pIDBInitialize->Uninitialize();
        pIDBInitialize->Release();
    }
    if (pIDataInitialize)
    {
        pIDataInitialize->Release();
    }

    CoUninitialize();
}
```
### <a name="active-directory-integrated"></a>Встроенная проверка подлинности Active Directory
```cpp
#include <string>
#include <iostream>
#include <msdasc.h>

int main()
{
    wchar_t azureServer[] = L"server";
    wchar_t azureDatabase[] = L"mydatabase";
    IDBInitialize *pIDBInitialize = nullptr;
    IDataInitialize* pIDataInitialize = nullptr;
    HRESULT hr = S_OK;

    CoInitialize(nullptr);

    // Construct the connection string.
    std::wstring connString = L"Provider=MSOLEDBSQL;Data Source=" + std::wstring(azureServer) + L";Initial Catalog=" + 
                              std::wstring(azureDatabase) + L";Authentication=ActiveDirectoryIntegrated;Use Encryption for Data=true;";

    hr = CoCreateInstance(CLSID_MSDAINITIALIZE, nullptr, CLSCTX_INPROC_SERVER, 
                          IID_IDataInitialize, reinterpret_cast<LPVOID*>(&pIDataInitialize));
    if (FAILED(hr)) 
    {
        std::cout << "Failed to create an IDataInitialize instance." << std::endl;
        goto Cleanup;
    }
    hr = pIDataInitialize->GetDataSource(nullptr, CLSCTX_INPROC_SERVER, connString.c_str(), 
                                         IID_IDBInitialize, reinterpret_cast<IUnknown**>(&pIDBInitialize));
    if (FAILED(hr))
    {
        std::cout << "Failed to get data source object." << std::endl;
        goto Cleanup;
    }
    hr = pIDBInitialize->Initialize();
    if (FAILED(hr))
    {
        std::cout << "Failed to establish connection." << std::endl;
        goto Cleanup;
    }

Cleanup:
    if (pIDBInitialize)
    {
        pIDBInitialize->Uninitialize();
        pIDBInitialize->Release();
    }
    if (pIDataInitialize)
    {
        pIDataInitialize->Release();
    }

    CoUninitialize();
}
```

## <a name="next-steps"></a>Дальнейшие действия
- [Выполните авторизацию доступа к веб-приложениям Azure Active Directory с помощью потока предоставления кода OAuth 2.0](/azure/active-directory/azuread-dev/v1-protocols-oauth-code).

- Ознакомьтесь со сведениями о [проверке подлинности Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview) для SQL Server.

- Настройте подключения к драйверам с помощью [ключевых слов строки подключения](../applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md), которые поддерживаются драйвером OLE DB.
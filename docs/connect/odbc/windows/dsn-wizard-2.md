---
description: Сведения о выборе метода проверки подлинности в мастере источников данных для создания подключения ODBC к SQL Server.
title: Мастер источников данных, экран 2 (драйвер ODBC для SQL Server)
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 76326eeb-1144-4b9f-85db-50524c655d30
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7db7d1d8e01929f9c43b78cd4d74e63dcc1b6e12
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673388"
---
# <a name="data-source-wizard-screen-2"></a>Экран 2 мастера источников данных

Укажите метод проверки подлинности, настройте записи расширенного клиента Microsoft SQL Server, а также имя входа и пароль, которые драйвер ODBC будет использовать для подключения к SQL Server при настройке источника данных.

## <a name="options"></a>Параметры

### <a name="with-integrated-windows-authentication"></a>Со встроенной проверкой подлинности Windows

Указывает, что драйвер запрашивает безопасное (или доверительное) соединение с сервером SQL Server. Если выбран данный режим, при установке соединений с использованием данного источника данных SQL Server будет использовать встроенную систему безопасности входа в систему, вне зависимости от того, какой режим безопасности установлен на сервере. Не учитываются любые предоставляемые идентификаторы входа и пароли. Системный администратор SQL Server должен связать идентификатор пользователя Windows с именем входа SQL Server (например, с помощью SQL Server Management Studio).

Дополнительно можно указать для сервера имя участника-службы.

### <a name="with-active-directory-integrated-authentication"></a>Со встроенной проверкой подлинности Active Directory

Указывает, что драйвер проходит проверку подлинности на сервере SQL Server с помощью Azure Active Directory. Если выбран данный режим, то при установке подключений с использованием данного источника данных SQL Server будет использовать встроенную систему безопасности входа в систему Azure Active Directory, вне зависимости от того, какой режим безопасности установлен на сервере.

### <a name="with-sql-server-authentication"></a>С проверкой подлинности SQL Server

Указывает, что драйвер проходит проверку подлинности на сервере SQL Server с помощью имени для входа и пароля.

### <a name="with-active-directory-password-authentication"></a>С проверкой подлинности с помощью пароля Active Directory

Указывает, что драйвер проходит проверку подлинности на сервере SQL Server с помощью имени для входа и пароля Azure Active Directory.

### <a name="with-active-directory-interactive-authentication"></a>С интерактивной проверкой подлинности Active Directory

Указывает, что драйвер проходит проверку подлинности на сервере SQL Server с помощью Интерактивного режима путем предоставления имени для входа в Azure Active Directory. Этот параметр вызовет диалоговое окно проверки подлинности Azure.

### <a name="with-managed-identity-authentication"></a>С проверкой подлинности управляемого удостоверения

Указывает, что драйвер проверяет подлинность для SQL Server с помощью управляемого удостоверения.

### <a name="with-active-directory-service-principal-authentication"></a>С проверкой подлинности субъекта-службы Active Directory

Указывает, что драйвер проходит проверку подлинности на сервере SQL Server с помощью субъекта-службы Azure Active Directory.

### <a name="login-id"></a>Идентификатор входа

Указание имени для входа, используемого драйвером при подключении к SQL Server, если выбрано **С помощью проверки подлинности SQL Server, с использованием имени для входа и паролем, введенным пользователем** или **С проверкой подлинности пароля Active Directory по имени пользователя и паролю, введенным пользователем** или **С интерактивной проверкой подлинности Active Directory, при которой пользователь вводит ИД для входа**. Если выбран параметр **С проверкой подлинности управляемого удостоверения**, укажите идентификатор объекта управляемого удостоверения или оставьте поле пустым, чтобы использовать идентификатор по умолчанию. Это поле относится только к соединениям, установленным для определения настроек по умолчанию сервера; не относится к последующим соединениям, установленным с использованием данного источника данных, после того как он был создан, за исключением использования проверки подлинности управляемого удостоверения.

### <a name="password"></a>Пароль

Указание пароля, используемого драйвером при подключении к SQL Server, если выбрано **С помощью проверки подлинности SQL Server, с использованием имени для входа и паролем, введенным пользователем** или **С проверкой подлинности пароля Active Directory по имени пользователя и паролю, введенным пользователем**. Это поле относится только к соединениям, установленным для определения настроек по умолчанию сервера; не относится к последующим соединениям, установленным с использованием нового источника данных.

Оба поля **Имя для входа** и **Пароль** отключаются, если выбрано **встроенная проверка подлинности Windows** или **Со встроенной проверкой подлинности Active Directory**.

### <a name="next"></a>Следующая

Переход к следующему экрану в мастере.

### <a name="back"></a>Назад

Возврат к предыдущему экрану в мастере.

## <a name="next-steps"></a>Дальнейшие шаги

[Экран 1 мастера источников данных](dsn-wizard-1.md)  
[Экран 3 мастера источников данных](dsn-wizard-3.md)  

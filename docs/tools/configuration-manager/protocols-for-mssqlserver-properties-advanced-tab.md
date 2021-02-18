---
title: Протоколы для свойств MSSQLSERVER (вкладка «Дополнительно»)
description: Сведения о преимуществах и требованиях расширенной защиты для проверки подлинности для ядра СУБД SQL Server. Сведения о том, как включить и настроить эту функцию.
ms.custom: seo-lt-2019
ms.date: 01/22/2021
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 34858dd9fa8e155c93ca486bd62499c36c302e31
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349709"
---
# <a name="protocols-for-mssqlserver-properties-advanced-tab"></a>Протоколы для свойств MSSQLSERVER (вкладка «Дополнительно»)

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Используйте вкладку **Дополнительно** в диалоговом окне **Протоколы для свойств MSSQLSERVER**, чтобы настроить функцию **Расширенная защита для проверки подлинности** для компонента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. **Расширенная защита** представляет собой функцию сетевых компонентов, реализованную операционной системой. **Расширенная защита** доступна в Windows 7 и Windows Server 2008 R2 и включена в пакеты обновления для операционных систем предыдущих версий. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при помощи **расширенную защиту**. Для включения определенных функций **Расширенной защиты** необходимо установить на вкладке **Флаги** флажок **Принудительное шифрование** .

> [!IMPORTANT]  
> В Windows **Расширенная защита** не включается по умолчанию. Сведения о включении **расширенной защиты** см. в статье
> - [Расширенная защита Windows \<extendedProtection\>](/iis/configuration/system.webserver/security/authentication/windowsauthentication/extendedprotection/)
> - [Общие сведения о расширенной защите для проверки подлинности](/dotnet/framework/wcf/feature-details/extended-protection-for-authentication-overview)

Дополнительные сведения о настройке других служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в разделе [Manage the Database Engine Services](../../database-engine/configure-windows/manage-the-database-engine-services.md). Полное описание расширенной защиты см. в разделе [Соединение с компонентом Database Engine с использованием расширенной защиты](../../database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection.md).

**Расширенная защита** полностью поддерживается [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, начиная с [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Поддержка **расширенной защиты** для других поставщиков клиентов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в настоящее время отсутствует.

## <a name="options"></a>Параметры

### <a name="extended-protection"></a>расширенную защиту

Возможны три значения.  

- **Off**: Это означает, что **Расширенная защита** отключена. Экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] принимает соединения от всех клиентов, независимо от того, является клиент защищенным или нет. При задании **Выкл.** обеспечивается совместимость с предыдущими версиями операционных систем и версиями без установленных обновлений, но этот режим отличается меньшей безопасностью. Этот режим можно использовать, если известно, что клиентские операционные системы не поддерживают расширенную защиту.

- **Разрешено**. Функция **Расширенная защита** требуется для установки подключений от операционных систем, поддерживающих функцию **Расширенная защита**. Соединения незащищенных клиентских приложений, выполняемых на защищенных клиентских операционных системах, отклоняются. **Расширенная защита** пропускается для соединений от незащищенных операционных систем. Этот параметр обеспечивает более высокий уровень защиты, чем **Выкл**, но, тем не менее, не обеспечивает самый высокий уровень защиты. Этот параметр следует использовать в смешанных средах, где одни операционные системы и приложения поддерживают **расширенную защиту** , а другие — нет.

- **Обязательный**: Означает, что подтверждаться будет только подключение из защищенного приложения на защищенной операционной системе. Этот вариант является самым безопасным из трех доступных. Но при его использовании станут невозможны подключения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из операционных систем, которые не поддерживают **расширенную защиту**.

### <a name="accepted-ntlm-spns"></a>Принятые имена участников-служб NTLM

Экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может иметь несколько имен участника-службы (SPN) для проверки подлинности Windows. Имена участников-служб перечисляются в виде набора строк, разделенных точкой с запятой. Например, значение **MSSQLSvc/HostName1.Contoso.com;MSSQLSvc/HostName2.Contoso.com** указывает, что разрешены клиенты, пытающиеся подключиться к SPN с именами **MSSQLSvc/HOST1.Contoso.com** или **MSSQLSvc/HOST2.Contoso.com**. Максимальная длина этой переменной 2048 символов.

## <a name="see-also"></a>См. также:

[Extended Protection for Authentication with Reporting Services](../../reporting-services/security/extended-protection-for-authentication-with-reporting-services.md)
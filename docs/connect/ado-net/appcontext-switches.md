---
title: Параметры AppContext в SqlClient
description: Узнайте о переключателях AppContext, доступных в SqlClient, и об их использовании для изменения некоторого поведения по умолчанию.
ms.date: 03/24/2021
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: 6ff171065f69ded313d96fd5156cc42daef37a35
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "107003770"
---
# <a name="appcontext-switches-in-sqlclient"></a>Параметры AppContext в SqlClient

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Класс AppContext позволяет SqlClient предоставлять новые функциональные возможности, продолжая поддерживать вызывающие объекты, которые зависят от предыдущего поведения. Пользователи могут отказаться от изменения в поведении, задав определенные параметры AppContext.

## <a name="enabling-decimal-truncation-behavior"></a>Включение усечения десятичных чисел

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

Начиная с Microsoft.Data.SqlClient 2.0, десятичные данные будут округляться по умолчанию, как это делается в SQL Server. Чтобы включить предыдущее поведение усечения, при запуске приложения можно задать для параметра AppContext **Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal** значение `true`.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal", true);
```

## <a name="enabling-managed-networking-on-windows"></a>Включение управляемых сетей в Windows

[!INCLUDE [appliesto-xxxx-netcore-netst-md](../../includes/appliesto-xxxx-netcore-netst-md.md)]

В Windows SqlClient по умолчанию использует собственную реализацию сетевого интерфейса SNI. Чтобы разрешить использование управляемой реализации SNI, при запуске приложения можно задать для параметра AppContext **Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows** значение `true`.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows", true);
```

Этот параметр позволяет переключить поведение драйвера на использование управляемой реализации сети в проектах .NET Core 2.1+ и .NET Standard 2.0+ в Windows, устраняя все зависимости от собственных библиотек для библиотеки Microsoft.Data.SqlClient. Он предназначен только для тестирования и отладки.

> [!NOTE]
> При сравнении с собственной реализацией существуют некоторые известные различия. Например, управляемая реализация не поддерживает проверку подлинности Windows без домена.

## <a name="disabling-transparent-network-ip-resolution"></a>Отключение разрешения IP-адресов прозрачной сети

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Разрешение IP-адресов прозрачной сети (TNIR) является исправленной версией существующей функции MultiSubnetFailover. TNIR влияет на последовательность подключений драйвера, когда первый разрешенный IP-адрес имени узла не отвечает и имеется несколько IP-адресов, связанных с именем этого узла. TNIR взаимодействует с MultiSubnetFailover и поддерживает следующие три варианта последовательности подключений.

* 0: Сначала один IP-адрес, а затем все IP-адреса в параллельном режиме.
* 1: Все IP-адреса в параллельном режиме.
* 2: Все IP-адреса последовательно.

|TransparentNetworkIPResolution|MultiSubnetFailover|Поведение|
|--------|--------|--------|
|True|True|1|
|Да|Неверно|0|
|False|True|1|
|False|False|2|

По умолчанию transparentNetworkIPResolution имеет значение true. Параметр MultiSubnetFailover отключен по умолчанию. Чтобы отключить TNIR, при запуске приложения можно задать для параметра AppContext **Switch.Microsoft.Data.SqlClient.DisableTNIRByDefaultInConnectionString** значение `true`.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.DisableTNIRByDefaultInConnectionString", true);
```

Дополнительные сведения об установке этих свойств см. в документации по свойству [SqlConnection.ConnectionString](/dotnet/api/microsoft.data.sqlclient.sqlconnection.connectionstring).

## <a name="enable-a-minimum-timeout-during-login"></a>Включение минимального времени ожидания при входе

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

Чтобы предотвратить неограниченное время ожидания входа, при запуске приложения можно задать для параметра AppContext **Switch.Microsoft.Data.SqlClient.UseOneSecFloorInTimeoutCalculationDuringLogin** значение `true`.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseOneSecFloorInTimeoutCalculationDuringLogin", false);
```

## <a name="disable-blocking-behavior-of-readasync"></a>Отключение блокирующего поведения ReadAsync

[!INCLUDE [appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

По умолчанию ReadAsync работает синхронно и блокирует вызывающий поток на .NET Framework. Чтобы отключить это поведение блокировки, при запуске приложения можно задать для параметра AppContext **Switch.Microsoft.Data.SqlClient.MakeReadAsyncBlocking** значение `false`.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.MakeReadAsyncBlocking", false);
```

## <a name="enable-configurable-retry-logic"></a>Включение настраиваемой логики повторных попыток

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

По умолчанию настраиваемая логика повторных попыток отключена. Чтобы включить эту возможность, задайте в AppContext для переключателя **Switch.Microsoft.Data.SqlClient.EnableRetryLogic** значение `true` при запуске приложения. Этот переключатель является обязательным, даже если для открытия подключения или выполнения команды назначен поставщик логики повторных попыток.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.EnableRetryLogic", false);
```

* Дополнительные сведения о том, как включить переключатель с помощью файла конфигурации, см. в разделе [Включение переключателя безопасности](configurable-retry-logic-config-file-sqlclient.md#enable-safety-switch).

## <a name="see-also"></a>См. также раздел

[Класс AppContext](/dotnet/api/system.appcontext?view=netcore-3.1&preserve-view=true)

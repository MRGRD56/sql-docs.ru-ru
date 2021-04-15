---
title: Настраиваемый файл конфигурации логики повторных попыток в SqlClient
description: Узнайте, как использовать файл конфигурации для определения поставщиков логики повторных попыток по умолчанию и настройки параметров логики повторных попыток в Microsoft.Data.SqlClient.
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-deshtehari
ms.openlocfilehash: da47a26e992b91729853d5a81a5dff3660c5df0c
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "107004020"
---
# <a name="configurable-retry-logic-configuration-file-with-sqlclient"></a>Настраиваемый файл конфигурации логики повторных попыток в SqlClient

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../includes/appliesto-netfx-netcore-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Метод повторных попыток по умолчанию, когда включен параметр-переключатель безопасности, — `Microsoft.Data.SqlClient.SqlConnection.SqlConfigurableRetryFactory.CreateNoneRetryProvider` для <xref:Microsoft.Data.SqlClient.SqlConnection> и <xref:Microsoft.Data.SqlClient.SqlCommand>. Вы можете определить другой метод повторных попыток с помощью файла конфигурации.

## <a name="configuration-sections"></a>Разделы конфигурации

Параметры логики повторных попыток по умолчанию для приложения можно изменить, добавив следующие разделы в раздел `configSections` файла конфигурации:

- `SqlConfigurableRetryLogicConnection` — определение логики повторных попыток по умолчанию для <xref:Microsoft.Data.SqlClient.SqlConnection>.

```csharp
<section name="SqlConfigurableRetryLogicConnection"
        type="Microsoft.Data.SqlClient.SqlConfigurableRetryConnectionSection, Microsoft.Data.SqlClient"/>
```

- `SqlConfigurableRetryLogicCommand` — определение логики повторных попыток по умолчанию для <xref:Microsoft.Data.SqlClient.SqlCommand>.

```csharp
<section name="SqlConfigurableRetryLogicCommand"
        type="Microsoft.Data.SqlClient.SqlConfigurableRetryCommandSection, Microsoft.Data.SqlClient"/>
```

- `AppContextSwitchOverrides` — .NET Framework поддерживает параметры-переключатели AppContext через раздел `AppContextSwitchOverrides`, который не нужно определять явным образом. Чтобы включить параметр-переключатель в .NET Core, необходимо определить этот раздел.

```csharp
<section name="AppContextSwitchOverrides"
        type="Microsoft.Data.SqlClient.AppContextSwitchOverridesSection, Microsoft.Data.SqlClient"/>
```

> [!NOTE]
> В разделе `configuration` должны быть указаны следующие конфигурации. Объявите эти новые разделы, чтобы настроить логику повторных попыток по умолчанию с помощью файла конфигурации приложения.

### <a name="enable-safety-switch"></a>Включение параметра-переключателя безопасности

Включить параметр-переключатель безопасности можно с помощью файла конфигурации. Дополнительные сведения о том, как включить его с помощью кода приложения, см. в разделе [Включение настраиваемой логики повторных попыток](appcontext-switches.md#enable-configurable-retry-logic).

- **.NET Framework**: дополнительные сведения см. в статье [Элемент <AppContextSwitchOverrides>](/dotnet/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element).

```csharp
<runtime>
    <AppContextSwitchOverrides value="Switch.Microsoft.Data.SqlClient.EnableRetryLogic=true"/>
</runtime>
```

- **.NET Core**: поддерживает использование нескольких параметров-переключателей, разделенных точкой с запятой (;), как и .NET Framework.

```csharp
<AppContextSwitchOverrides value="Switch.Microsoft.Data.SqlClient.EnableRetryLogic=true"/>
```

### <a name="connection-section"></a>Раздел подключения

Следующие атрибуты можно использовать для определения логики повторных попыток по умолчанию для всех экземпляров `Microsoft.Data.SqlClient.SqlConnection` в приложении:

- **numberOfTries**: задает количество попыток.

- **deltaTime**: задает интервал времени в виде объекта `System.TimeSpan`.

- **minTime**: задает допустимый минимальный интервал времени в виде объекта `System.TimeSpan`.

- **maxTime**: задает допустимый минимальный интервал времени в виде объекта `System.TimeSpan`.

- **transientErrors**: задает список номеров временных ошибок для повторных попыток.

- **retryMethod**: определяет создателя метода повтора, который получает конфигурацию повторных попыток через параметр `Microsoft.Data.SqlClient.SqlRetryLogicOption` и возвращает объект `Microsoft.Data.SqlClient.SqlRetryLogicBaseProvider`.

- **retryLogicType**: задает пользовательский поставщик логики повторных попыток с создателями метода повтора, которые предоставляют `retryMethod`. Эти методы должны соответствовать критериям для `retryMethod`. Следует использовать полное имя типа поставщика. Дополнительные сведения см. в статье [Определение полных имен типов](/dotnet/framework/reflection-and-codedom/specifying-fully-qualified-type-names).

> [!NOTE]
> При использовании встроенных поставщиков логики повторных попыток определять объект `retryLogicType` не нужно. Чтобы найти встроенные поставщики логики повторных попыток, см. статью [Внутренние поставщики логики повторных попыток в SqlClient](internal-retry-logic-providers-sqlclient.md).

### <a name="command-section"></a>Раздел команд

Следующий атрибут также можно задать для всех экземпляров <xref:Microsoft.Data.SqlClient.SqlCommand> в приложении:

- **authorizedSqlCondition**: задает для <xref:Microsoft.Data.SqlClient.SqlCommand.CommandText> регулярное выражение, выполняемое перед повторным выполнением, для фильтрации определенных инструкций SQL.

> [!NOTE]
> В регулярном выражении учитывается регистр.

### <a name="examples"></a>Примеры

- Попытка установить подключение до трех раз с задержкой около 1 секунды между попытками с помощью метода `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateFixedRetryProvider` и списка временных ошибок по умолчанию:

    ```csharp
    <SqlConfigurableRetryLogicConnection retryMethod ="CreateFixedRetryProvider" 
                                            numberOfTries ="3" deltaTime ="00:00:01"/>
    ```

- Попытка установить подключение до пяти раз с задержкой до 45 секунд между попытками с помощью метода `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateExponentialRetryProvider` и списка временных ошибок по умолчанию:

    ```csharp
    <SqlConfigurableRetryLogicConnection retryMethod ="CreateExponentialRetryProvider" 
                        numberOfTries ="5" deltaTime ="00:00:03" maxTime ="00:00:45"/>
    ```

- Попытка выполнить команду до четырех раз с задержкой в от 2 до 30 секунд с помощью метода `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateIncrementalRetryProvider` и списка временных ошибок по умолчанию:

    ```csharp
    <SqlConfigurableRetryLogicCommand retryMethod ="CreateIncrementalRetryProvider"
                        numberOfTries ="4" deltaTime ="00:00:02" maxTime ="00:00:30"/>
    ```

- Попытка выполнить команду до восьми раз с задержкой от 1 секунды до 1 минуты. Она ограничена командами с `CommandText` со словом `SELECT` и числом исключений 102 или 997. При этом используется встроенный метод `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateIncrementalRetryProvider`:

    ```csharp
    <SqlConfigurableRetryLogicCommand retryMethod ="CreateIncrementalRetryProvider" 
                            numberOfTries ="8" deltaTime ="00:00:01" maxTime ="00:01:00"
                            transientErrors="102, 997"
                            authorizedSqlCondition="\b(SELECT)\b"/>
    ```

> [!NOTE]
> В следующих двух примерах можно найти пользовательский исходный код логики повторных попыток из статьи [Базовые API настраиваемой логики повторных попыток в SqlClient](configurable-retry-logic-core-apis-sqlclient.md#example). Предполагается, что метод `CreateCustomProvider` определен в классе `CustomCRL_Doc.CustomRetry` в сборке `CustomCRL_Doc.dll`, которая находится в каталоге, в котором выполняется приложение.

- Попытка установить подключение до пяти раз с задержкой от 3 до 45 секунд и номерами ошибок 4060, 997 и 233 в списке с помощью указанного пользовательского поставщика логики повторных попыток:

    ```csharp
    <SqlConfigurableRetryLogicConnection retryLogicType ="CustomCRL_Doc.CustomRetry, CustomCRL_Doc, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"
                        retryMethod ="CreateCustomProvider" 
                        numberOfTries ="5" deltaTime ="00:00:03" maxTime ="00:00:45"
                        transientErrors ="4060, 997, 233"/>
    ```

- В этом примере выполняется та же операция, что и в предыдущем:

    ```csharp
    <SqlConfigurableRetryLogicConnection retryLogicType ="CustomCRL_Doc.CustomRetry, CustomCRL_Doc"
                        retryMethod ="CreateCustomProvider" 
                        numberOfTries ="5" deltaTime ="00:00:03" maxTime ="00:00:45"
                        transientErrors ="4060, 997, 233"/>
    ```

> [!NOTE]
> Поставщики логики повторных попыток будут кэшироваться при первом использовании для подключения или команды для последующего использования во время существования приложения.

> [!NOTE]
> Ошибки при чтении файла конфигурации приложения для параметров логики повторных попыток не вызывают ошибок в приложении. Будет использоваться `Microsoft.Data.SqlClient.SqlConnection.SqlConfigurableRetryFactory.CreateNoneRetryProvider` по умолчанию.
>
> Для проверки или устранения проблем с настройкой логики повторных попыток можно использовать трассировку источника событий. Дополнительные сведения см. в статье [Включение трассировки событий в SqlClient](enable-eventsource-tracing.md).

## <a name="see-also"></a>См. также раздел

- [Включение настраиваемой логики повторных попыток](appcontext-switches.md#enable-configurable-retry-logic)
- [Внутренние поставщики логики повторных попыток в SqlClient](internal-retry-logic-providers-sqlclient.md)
- [Включение трассировки событий в SqlClient](enable-eventsource-tracing.md)
- [Определение полных имен типов](/dotnet/framework/reflection-and-codedom/specifying-fully-qualified-type-names)
- [Настраиваемая логика повторных попыток в SqlClient](configurable-retry-logic.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)

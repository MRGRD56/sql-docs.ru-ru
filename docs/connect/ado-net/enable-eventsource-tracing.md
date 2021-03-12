---
title: Включение трассировки событий в SqlClient
description: Описывает включение трассировки событий в SqlClient путем реализации прослушивателя событий и доступа к данным события.
ms.date: 11/23/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: c13942dc5633deec4759ebd4c8a5661654c0952b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837091"
---
# <a name="enable-event-tracing-in-sqlclient"></a>Включение трассировки событий в SqlClient

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

[Средство трассировки событий для Windows (ETW)](/windows/win32/etw/event-tracing-portal) является эффективным средством трассировки на уровне ядра, которое позволяет записывать в журнал события, определяемые драйвером, в целях отладки и тестирования. SqlClient поддерживает запись событий ETW на разных информационных уровнях. Чтобы начать сбор трассировок событий, клиентские приложения должны прослушивать события от реализации EventSource для SqlClient.

```
Microsoft.Data.SqlClient.EventSource
```

Текущая реализация поддерживает следующие ключевые слова событий.

| Имя ключевого слова | Значение | Описание: |
| ------------ | ----- | ----------- |
| ExecutionTrace | 1 | Включает запись событий запуска и приостанавливается до и после выполнения команды. |
| Трассировка | 2 | Включает запись основных событий трассировки потока приложений. |
| Область | 4 | Включает запись событий входа и выхода. |
| NotificationTrace | 8 | Включает запись событий трассировки `SqlNotification`. |
| NotificationScope | 16 | Включает запись событий входа в область `SqlNotification` и выхода из нее. |
| PoolerTrace | 32 | Включает запись событий трассировки потока для пула соединений. |
| PoolerScope | 64 | Включает запись событий трассировки области пула соединений. |
| AdvancedTrace | 128 | Включает запись событий расширенной трассировки потока. |
| AdvancedTraceBin  | 256 | Включает запись событий расширенной трассировки потока с дополнительными сведениями. |
| CorrelationTrace | 512 | Включает запись событий трассировки потока корреляции. |
| StateDump | 1024 | Включает запись дампа полного состояния `SqlConnection`. |
| SNITrace | 2048 | Включает запись событий трассировки потока из реализации управляемой сети (применимо только в .NET Core). |
| SNIScope | 4096 | Включает запись событий области из реализации управляемой сети (применимо только в .NET Core). |
|||

## <a name="example"></a>Пример

В следующем примере включается трассировка событий для операции с данными в образце базы данных **AdventureWorks** и отображаются события в окне консоли.

[!code-csharp [SqlClientEventSource#1](~/../sqlclient/doc/samples/SqlClientEventSource.cs#1)]

## <a name="event-tracing-support-in-native-sni"></a>Поддержка трассировки событий в нативной библиотеке SNI

**Microsoft.Data.SqlClient** версии 2.1.0 расширяет поддержку трассировки событий, реализованную в **Microsoft.Data.SqlClient.SNI** и **Microsoft.Data.SqlClient.SNI.Runtime**. Отправляя EventCommand в `SqlClientEventSource`, можно собирать события из нативной библиотеки SNI.dll с помощью средств [Xperf](/windows-hardware/test/wpt/) и [PerfView](https://github.com/microsoft/perfview). Ниже перечислены допустимые значения EventCommand:

```cs
// Enables trace events:
EventSource.SendCommand(eventSource, (EventCommand)8192, null);

// Enables flow events:
EventSource.SendCommand(eventSource, (EventCommand)16384, null);

// Enables both trace and flow events:
EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
```

Следующий пример активирует трассировку событий в нативной библиотеке SNI.dll, если приложение предназначено для .NET Framework. 

```cs
// Native SNI tracing example
// .NET Framework application
using System;
using System.Diagnostics.Tracing;
using Microsoft.Data.SqlClient;

public class SqlClientListener : EventListener
{
    protected override void OnEventSourceCreated(EventSource eventSource)
    {
        if (eventSource.Name.Equals("Microsoft.Data.SqlClient.EventSource"))
        {
            // Enables both trace and flow events
            EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
        }
    }
}

class Program
{
    static string connectionString = @"Data Source = localhost; Initial Catalog = AdventureWorks;Integrated Security=true;";

    static void Main(string[] args)
    {
        using (SqlClientListener listener = new SqlClientListener())
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();
        }        
    }
}
```

### <a name="use-xperf-to-collect-trace-log"></a>Получение журнала трассировки с помощью XPerf

1. Запустите трассировку с помощью следующей командной строки.

   ```
   xperf -start trace -f myTrace.etl -on *Microsoft.Data.SqlClient.EventSource
   ```

2. Запустите собственный пример трассировки SNI и подключитесь к SQL Server.

3. Остановите трассировку с помощью следующей командной строки.

   ```
   xperf -stop trace
   ```

4. С помощью PerfView откройте файл myTrace.etl, указанный на шаге 1. Журнал трассировки SNI можно найти с помощью имен событий `Microsoft.Data.SqlClient.EventSource/SNIScope` и `Microsoft.Data.SqlClient.EventSource/SNITrace`.

   ![Использование PerfView для просмотра файла трассировки SNI](media/view-event-trace-native-sni.png)


### <a name="use-perfview-to-collect-trace-log"></a>Использование PerfView для сбора журнала трассировки

1. Запустите PerfView и откройте `Collect > Collect` из строки меню.

2. Настройте имя файла трассировки, путь вывода и имя поставщика.

   ![Настройка PerfView перед началом сбора журналов](media/collect-event-trace-native-sni.png)

3. Запустите коллекцию.

4. Запустите собственный пример трассировки SNI и подключитесь к SQL Server.

5. Остановите сбор в PerfView. Создание файла PerfViewData.etl с конфигурацией, настроенной на шаге 2, займет некоторое время.

6. Откройте файл `etl` в PerfView. Журнал трассировки SNI можно найти с помощью имен событий `Microsoft.Data.SqlClient.EventSource/SNIScope` и `Microsoft.Data.SqlClient.EventSource/SNITrace`.

## <a name="external-resources"></a>Внешние ресурсы  

Для получения дополнительных сведений см. следующие ресурсы.  
  
|Ресурс|Описание|  
|--------------|-----------------|  
|[EventSource](/dotnet/api/system.diagnostics.tracing.eventsource)|Используется для создания событий ETW.|
|[Класс EventListener](/dotnet/api/system.diagnostics.tracing.eventlistener)|Предоставляет методы для включения и отключения событий из источников событий.|

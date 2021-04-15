---
title: Внутренние поставщики логики повторных попыток в SqlClient
description: Узнайте, как использовать встроенные поставщики настраиваемой логики повторов в приложении для обработки временных ошибок в базе данных.
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-deshtehari
ms.openlocfilehash: 98a601b0d61f79bed6a802b86caecfabd16f75a6
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "107004007"
---
# <a name="internal-retry-logic-providers-in-sqlclient"></a>Внутренние поставщики логики повторных попыток в SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Встроенные внутренние поставщики логики повторных попыток реализуют наиболее распространенные шаблоны повторных попыток. Поставщики логики повторных попыток можно использовать с помощью следующих статических методов `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory`:

- `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateFixedRetryProvider`
- `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateIncrementalRetryProvider`
- `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateExponentialRetryProvider`
- `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory.CreateNoneRetryProvider`

> [!NOTE]
> Все внутренние поставщики логики повторных попыток немного изменяют длительность паузы перед каждой повторной попыткой. Это позволяет избежать обращения к базе данных в то же время, когда несколько клиентов пытаются подключиться или выполнить команду с такой же конфигурацией.

> [!WARNING]
> Внутренние поставщики логики повторных попыток не поддерживают повторные попытки выполнения команды в открытой транзакции. Такая операция будет выполнена без логики повторных попыток. Это поведение можно переопределить с помощью пользовательской логики повторных попыток. Дополнительные сведения см. в статье [Базовые API настраиваемой логики повторных попыток в SqlClient](configurable-retry-logic-core-apis-sqlclient.md).

<!-- These links won't be live until after the feature is released in a GA version.
## Example

You can find samples for `connection` and `command` retry logic at the following links:

- [Microsoft.Data.SqlClient.SqlConnection.RetryLogicProvider#example](/dotnet/api/microsoft.data.sqlclient.sqlconnection.RetryLogicProvider?view=sqlclient-dotnet-core-2.1&preserve-view=true#examples)
- [Microsoft.Data.SqlClient.SqlCommand.RetryLogicProvider#example](/dotnet/api/microsoft.data.sqlclient.sqlcommand.RetryLogicProvider?view=sqlclient-dotnet-core-2.1&preserve-view=true#examples)
-->

## <a name="see-also"></a>См. также раздел

- [Базовые API настраиваемой логики повторных попыток в SqlClient](configurable-retry-logic-core-apis-sqlclient.md)
- [Настраиваемая логика повторных попыток в SqlClient](configurable-retry-logic.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)

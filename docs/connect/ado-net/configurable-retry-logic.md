---
title: Настраиваемая логика повторных попыток в SqlClient
description: Узнайте, как использовать настраиваемую логику повторных попыток в Microsoft.Data.SqlClient при установки подключения или выполнении команды.
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-deshtehari
ms.openlocfilehash: aa0b6a5d984b2891f2fcf868b15a495b4984b8ba
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "107003988"
---
# <a name="configurable-retry-logic-in-sqlclient"></a>Настраиваемая логика повторных попыток в SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Приложение, которое взаимодействует с элементами, запущенными в облаке, должно быть чувствительно к временным сбоям, которые могут случаться в этой среде. Как правило, эти сбои устраняются без вмешательства пользователя. Если повторить действие через некоторый промежуток времени, оно с большой вероятностью будет выполнено успешно.

> [!NOTE]
> Эта возможность доступна, начиная с Microsoft.Data.SqlClient версии 3.0.0, предварительная версия 1.

## <a name="retry-pattern"></a>Шаблон повторов

Попытка выполнить операцию, несмотря на временные ошибки, вместо создания исключения и предоставления пользователю возможности принять решение о следующем действии, — это разумное решение, называемое шаблоном повторных попыток. Дополнительные сведения см. в статье [Шаблон повторных попыток](/azure/architecture/patterns/retry).

## <a name="transient-faults"></a>Временные сбои

Вы можете использовать надежную инфраструктуру и хорошо известные приложения, реализованные с помощью новейших технологий, чтобы сократить время простоя службы. Но невозможно предотвратить все сбои. Временные ошибки — это сбои, которые иногда возникают по известным причинам и исчезают через некоторое время. Например, если изменение балансировки нагрузки выполняется на стороне сервера, может произойти сбой или истечь время ожидания запрошенных служб. Дополнительные сведения см. в разделе [Временные ошибки (временные сбои)](/azure/azure-sql/database/troubleshoot-common-connectivity-issues#transient-errors-transient-faults).

## <a name="do-and-dont"></a>Рекомендации и предостережения

Хотя шаблон повторных попыток значительно повышает устойчивость приложения, его применение в несоответствующей ситуации может негативно повлиять на приложение. Прежде чем добавлять исключение в список временных сбоев, подумайте, не будет ли такой сбой устранен без вашего вмешательства в ближайшем времени. Не спешите. Изучите причины, если у вас нет ответа на этот вопрос. Дополнительные сведения см. в статье [Устранение неполадок с подключением и решение других проблем с Базой данных SQL Azure и Управляемым экземпляром SQL Azure](/azure/azure-sql/database/troubleshoot-common-errors-issues).

## <a name="in-this-section"></a>В этом разделе

[Настраиваемая логика повторных попыток в SqlClient — введение](configurable-retry-logic-sqlclient-introduction.md)  
Сведения о разных разделах настраиваемой логики повторных попыток.

[Внутренние поставщики логики повторных попыток в SqlClient](internal-retry-logic-providers-sqlclient.md)  
Сведения о том, как использовать предварительно определенные поставщики логики повторных попыток для применения логики повторных попыток к базе данных.

[Базовые API настраиваемой логики повторных попыток в SqlClient](configurable-retry-logic-core-apis-sqlclient.md)  
Сведения о том, как использовать базовые API для реализации пользовательской логики повторных попыток.

[Настраиваемый файл конфигурации логики повторных попыток в SqlClient](configurable-retry-logic-config-file-sqlclient.md)  
Сведения о том, как указать поставщиков логики повторных попыток по умолчанию с помощью файла конфигурации.

## <a name="see-also"></a>См. также раздел

- [Шаблон повторных попыток](/azure/architecture/patterns/retry)
- [Временные сбои](/azure/azure-sql/database/troubleshoot-common-connectivity-issues#transient-errors-transient-faults)
- [Устранение неполадок с подключением и решение других проблем с Базой данных SQL Azure и Управляемым экземпляром SQL Azure](/azure/azure-sql/database/troubleshoot-common-errors-issues)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)

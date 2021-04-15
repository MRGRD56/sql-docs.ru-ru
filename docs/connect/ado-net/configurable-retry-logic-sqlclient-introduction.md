---
title: Настраиваемая логика повторных попыток в SqlClient — введение
description: Узнайте о разных аспектах настраиваемой логики повторных попыток в Microsoft.Data.SqlClient и о том, как обеспечить устойчивость приложения к временным ошибкам.
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-deshtehari
ms.openlocfilehash: 6018dc69c0fa6733b1482a8bdaa75d7cfaa4cb71
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "107004012"
---
# <a name="configurable-retry-logic-in-sqlclient-introduction"></a>Настраиваемая логика повторных попыток в SqlClient — введение

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Настраиваемая логика повторных попыток позволяет разработчикам и администраторам управлять поведением приложения, когда происходят временные сбои. Эта возможность позволяет добавить элементы управления во время подключения или выполнения команды. Элементы управления можно определить с помощью кода или файла конфигурации приложения. Для управления поведением попыток повтора можно определить временные номера ошибок и свойства повторных попыток. Кроме того, для фильтрации определенных инструкций SQL можно использовать регулярные выражения.

## <a name="feature-components"></a>Компоненты возможности

Возможность включает три основных компонента:

1. **Базовые API**: разработчики могут использовать эти интерфейсы для реализации собственной логики повторов в объектах <xref:Microsoft.Data.SqlClient.SqlConnection> и <xref:Microsoft.Data.SqlClient.SqlCommand>. Дополнительные сведения см. в статье [Базовые API настраиваемой логики повторных попыток в SqlClient](configurable-retry-logic-core-apis-sqlclient.md).
2. **Предварительно определенная логика повторных попыток**: встроенные методы логики повторных попыток с использованием базовых API доступны из класса `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory`. Дополнительные сведения см. в статье [Внутренние поставщики логики повторных попыток в SqlClient](internal-retry-logic-providers-sqlclient.md).
3. **Схема файла конфигурации**: позволяет указать логику повторных попыток по умолчанию для <xref:Microsoft.Data.SqlClient.SqlConnection> и <xref:Microsoft.Data.SqlClient.SqlCommand> в приложении. Дополнительные сведения см. в статье [Настраиваемый файл конфигурации логики повторных попыток в SqlClient](configurable-retry-logic-config-file-sqlclient.md).

## <a name="quick-start"></a>Быстрый запуск

Чтобы использовать эту возможность, выполните следующие четыре шага:

1. Включите переключатель безопасности в предварительной версии. Дополнительные сведения о том, как включить параметр-переключатель безопасности AppContext, см. в разделе [Включение настраиваемой логики повторных попыток](appcontext-switches.md#enable-configurable-retry-logic).

2. Определите параметры логики повторных попыток с помощью `Microsoft.Data.SqlClient.SqlRetryLogicOption`.  
В этом примере задаются некоторые параметры повторных попыток, а остальная часть этих параметров будет использовать значения по умолчанию.

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_OpenConnection#1](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_OpenConnection.cs#1)]

3. Создайте поставщик логики повторных попыток с помощью объекта `Microsoft.Data.SqlClient.SqlRetryLogicOption`.

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_OpenConnection#2](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_OpenConnection.cs#2)]

4. Присвойте экземпляр `Microsoft.Data.SqlClient.SqlRetryLogicBaseProvider` свойству `Microsoft.Data.SqlClient.SqlConnection.RetryLogicProvider` или `Microsoft.Data.SqlClient.SqlCommand.RetryLogicProvider`.  
В этом примере команда открытия подключения повторит попытку, если одна из временных ошибок во внутреннем списке `Microsoft.Data.SqlClient.SqlConfigurableRetryFactory` возникнет максимум пять раз.

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_OpenConnection#3](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_OpenConnection.cs#3)]

> [!NOTE]
> Эти шаги одинаковы для выполнения команды, кроме того, что вместо этого вы назначаете поставщик логики повторных попыток свойству `Microsoft.Data.SqlClient.SqlCommand.RetryLogicProvider` перед выполнением команды.

## <a name="see-also"></a>См. также раздел

- [Базовые API настраиваемой логики повторных попыток в SqlClient](configurable-retry-logic-core-apis-sqlclient.md)
- [Внутренние поставщики логики повторных попыток в SqlClient](internal-retry-logic-providers-sqlclient.md)
- [Настраиваемый файл конфигурации логики повторных попыток в SqlClient](configurable-retry-logic-config-file-sqlclient.md)
- [Включение настраиваемой логики повторных попыток](appcontext-switches.md#enable-configurable-retry-logic)
- [Настраиваемая логика повторных попыток в SqlClient](configurable-retry-logic.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)

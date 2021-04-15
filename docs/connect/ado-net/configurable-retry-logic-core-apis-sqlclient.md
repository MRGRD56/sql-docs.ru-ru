---
title: Базовые API настраиваемой логики повторных попыток в SqlClient
description: Узнайте, как использовать базовые API настраиваемой логики повторных попыток для реализации пользовательской логики повторных попыток в приложении в Microsoft.Data.SqlClient.
ms.date: 03/22/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-deshtehari
ms.openlocfilehash: 83053651938adfb51640ee9bdb096c2b2c8b4708
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "107003985"
---
# <a name="configurable-retry-logic-core-apis-in-sqlclient"></a>Базовые API настраиваемой логики повторных попыток в SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Если встроенные поставщики логики повторных попыток не покрывают ваши потребности, можно создать собственные пользовательские поставщики. Затем можно назначить эти поставщики объекту `SqlConnection` или `SqlCommand`, чтобы применить пользовательскую логику.

Встроенные поставщики поддерживают три интерфейса, которые можно использовать для реализации пользовательских поставщиков. Пользовательские поставщики логики повторных попыток можно использовать так же, как и соответствующие внутренние поставщики в <xref:Microsoft.Data.SqlClient.SqlConnection> или <xref:Microsoft.Data.SqlClient.SqlCommand>:

1. `Microsoft.Data.SqlClient.SqlRetryIntervalBaseEnumerator`: создает последовательность интервалов времени.
2. `Microsoft.Data.SqlClient.SqlRetryLogicBase`: получает следующий интервал времени для заданного перечислителя, если количество повторных попыток не превышено и соблюдается переходное состояние.
3. `Microsoft.Data.SqlClient.SqlRetryLogicBaseProvider`: применяет логику повтора к операциям с подключением и командами.

> [!CAUTION]
> Реализуя пользовательский поставщик логики повторных попыток, вы отвечаете за управление всеми аспектами, включая параллелизм, производительность и исключения.

## <a name="example"></a>Пример

Реализация в этом примере является максимально простой, чтобы продемонстрировать пошаговую настройку. Здесь не используются расширенные методики, такие как обеспечение потокобезопасности, асинхронности и параллелизма. Для глубокого изучения реальной реализации можно ознакомиться с предварительно определенной логикой повторных попыток в [репозитории Microsoft.Data.SqlClient на сайте GitHub](https://github.com/dotnet/SqlClient/).

1. Определение классов пользовательской настраиваемой логики повторных попыток:

    - **Перечислитель**: определите фиксированную последовательность интервалов времени и расширьте допустимый диапазон времени от 2 минут до 4 минут.

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#6](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#6)]

    - **Логика повторных попыток**: реализуйте логику повторных попыток для любой команды, которая не является частью активной транзакции. Уменьшите число повторных попыток с 60 до 20.

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#7](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#7)]

    - **Поставщик**: реализует поставщик логики повторных попыток для синхронных операций без события `Retrying`. Добавляет <xref:System.TimeoutException> к существующим номерам временных ошибок исключений <xref:Microsoft.Data.SqlClient.SqlException>.

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#8](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#8)]

2. Создайте экземпляр поставщика логики повторных попыток, состоящий из определенных пользовательских типов:

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#4](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#4)]

    - Следующая функция вычислит исключение, используя заданный список повторяемых исключений и специальное исключение <xref:System.TimeoutException>, чтобы определить, является ли оно повторяемым:

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#5](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#5)]

3. Используйте настраиваемую логику повторных попыток:

    - Определите параметры логики повторных попыток:

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#1](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#1)]

    - Создайте настраиваемый поставщик логики повторных попыток:

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#2](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#2)]

    - Назначьте поставщик логики повторных попыток для `Microsoft.Data.SqlClient.SqlConnection.RetryLogicProvider` или `Microsoft.Data.SqlClient.SqlCommand.RetryLogicProvider`:

    [!code-csharp[SqlConfigurableRetryLogic_StepByStep_CustomProvider#3](~/../sqlclient/doc/samples/SqlConfigurableRetryLogic_StepByStep_CustomProvider.cs#3)]

> [!NOTE]
> Не забудьте включить переключатель настраиваемой логики повторных попыток перед его использованием. Дополнительные сведения см. в разделе [Включение настраиваемой логики повторных попыток](appcontext-switches.md#enable-configurable-retry-logic).

## <a name="see-also"></a>См. также раздел

- [Репозиторий Microsoft.Data.SqlClient на сайте GitHub](https://github.com/dotnet/SqlClient/)
- [Настраиваемая логика повторных попыток в SqlClient](configurable-retry-logic.md)
- [Microsoft ADO.NET для SQL Server](microsoft-ado-net-sql-server.md)

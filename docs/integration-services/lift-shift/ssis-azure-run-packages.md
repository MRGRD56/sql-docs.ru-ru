---
title: Выполнение пакетов служб SSIS в Azure | Документы Майкрософт
description: Обзор методов, которые можно использовать для запуска пакетов служб SSIS, развернутых в базе данных SQL Azure.
ms.date: 05/29/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 4f6031cd363db4ae3c26511daf91811c3d133d1e
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/19/2020
ms.locfileid: "92192548"
---
# <a name="run-sql-server-integration-services-ssis-packages-deployed-in-azure"></a>Выполнение пакетов служб SQL Server Integration Services (SSIS), развернутых в Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Для выполнения пакетов служб SSIS, развернутых в каталоге SSISDB на сервере базы данных SQL Azure, можно выбрать один из методов, описанных в этой статье. Пакет можно выполнять напрямую или в составе конвейера фабрики данных Azure. Общие сведения о службах SSIS в Azure см. в статье [Развертывание и выполнение пакетов служб SSIS в Azure](ssis-azure-lift-shift-ssis-packages-overview.md).

- Выполнение пакета напрямую

  - [Запуск с помощью SSMS](#ssms)

  - [Выполнение с помощью хранимых процедур](#sproc)

  - [Выполнение с помощью сценария или кода](#script)

- Выполнение пакета в составе конвейера фабрики данных Azure

  - [Выполнение с помощью действия "Выполнение пакета служб SSIS"](#exec_activity)

  - [Выполнение с помощью действия хранимой процедуры](#sproc_activity)

> [!NOTE]
> Выполнение пакета с помощью `dtexec.exe` не проверялось с пакетами, развернутыми в Azure.

## <a name="run-a-package-with-ssms"></a><a name="ssms"></a> Выполнение пакета с помощью среды SSMS

В среде SQL Server Management Studio (SSMS) щелкните правой кнопкой мыши пакет, развернутый в базе данных каталога служб SSIS (SSISDB), и выберите **Выполнить**, чтобы открыть диалоговое окно **Выполнение пакета**. Дополнительные сведения см. в разделе [Выполнение пакета служб SSIS с помощью SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md).

## <a name="run-a-package-with-stored-procedures"></a><a name="sproc"></a> Выполнение пакета с помощью хранимых процедур

Пакет можно выполнить в любой среде, из которой можно подключиться к базе данных SQL Azure и запустить код Transact-SQL. Для этого вызовите следующие хранимые процедуры.

1. **[catalog].[create_execution]** . Дополнительные сведения см. в статье [catalog.deploy_packages](../system-stored-procedures/catalog-create-execution-ssisdb-database.md).

2. **[catalog].[set_execution_parameter_value]** . Дополнительные сведения см. в статье [catalog.set_execution_parameter_value](../system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md).

3. **[catalog].[start_execution]** . Дополнительные сведения см. в статье [catalog.start_execution](../system-stored-procedures/catalog-start-execution-ssisdb-database.md).

Дополнительные сведения см. в следующих примерах:

- [Выполнение пакета служб SSIS из SSMS с помощью Transact-SQL](../ssis-quickstart-run-tsql-ssms.md)

- [Выполнение пакета служб SSIS из Visual Studio Code с помощью Transact-SQL](../ssis-quickstart-run-tsql-vscode.md)

## <a name="run-a-package-with-script-or-code"></a><a name="script"></a> Выполнение пакета с помощью сценария или кода

Пакет можно выполнить в любой среде разработки, из которой можно вызвать управляемый API. Для этого вызовите метод `Execute` для объекта `Package` в пространстве имен `Microsoft.SQLServer.Management.IntegrationServices`.

Дополнительные сведения см. в следующих примерах:

- [Выполнение пакета служб SSIS с помощью PowerShell](../ssis-quickstart-run-powershell.md)

- [Выполнение пакета служб SSIS с кодом C# в приложении .NET](../ssis-quickstart-run-dotnet.md)

## <a name="run-a-package-with-the-execute-ssis-package-activity"></a><a name="exec_activity"></a> Выполнение пакета с помощью действия "Выполнение пакета служб SSIS"

Дополнительные сведения см. в статье [Выполнение пакета служб SSIS с помощью действия "Выполнение пакета служб SSIS" в фабрике данных Azure](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="run-a-package-with-the-stored-procedure-activity"></a><a name="sproc_activity"></a> Выполнение пакета с помощью действия хранимой процедуры

Дополнительные сведения см. в статье [Выполнение пакета служб SSIS с помощью действия хранимой процедуры в фабрике данных Azure](/azure/data-factory/how-to-invoke-ssis-package-stored-procedure-activity).

## <a name="next-steps"></a>Дальнейшие действия

Узнайте о способах планирования выполнения пакетов служб SSIS, развернутых в Azure. Дополнительные сведения см. в разделе [Планирование выполнения пакетов служб SSIS в Azure](ssis-azure-schedule-packages.md).
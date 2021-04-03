---
title: Установка настраиваемой среды выполнения R
description: Узнайте, как установить настраиваемую среду выполнения R для SQL Server с помощью расширений языка. В настраиваемой среде выполнения Python можно выполнять скрипты машинного обучения.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/16/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: contperf-fy21q3
zone_pivot_groups: sqlml-platforms
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: b6262e5ac3d7090516c1a62661583a64bb6ccbc9
ms.sourcegitcommit: e4b71e5d432a29b6c76ea457b00aa0abd4b6c77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2021
ms.locfileid: "106273479"
---
# <a name="install-an-r-custom-runtime-for-sql-server"></a>Установка настраиваемой среды выполнения R для SQL Server

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Узнайте, как установить настраиваемую среду выполнения R для запуска внешних скриптов R с SQL Server.

+ Windows
+ Ubuntu Linux
+ Red Hat Enterprise Linux (RHEL)
+ SUSE Linux Enterprise Server (SLES) версии 12

Настраиваемая среда выполнения позволяет выполнять скрипты машинного обучения и использует [расширения языка SQL Server](../../language-extensions/language-extensions-overview.md).

Используйте для SQL Server собственную версию среды выполнения R, а не версию среды выполнения по умолчанию, устанавливаемую вместе со [Службами машинного обучения SQL Server](../sql-server-machine-learning-services.md).

::: zone pivot="platform-windows"
[!INCLUDE [R custom runtime - Windows](includes/custom-runtime-r-windows.md)]
::: zone-end

::: zone pivot="platform-linux-ubuntu"
[!INCLUDE [R custom runtime - Linux - Prerequisites](includes/custom-runtime-r-linux-prerequisites.md)]

[!INCLUDE [R custom runtime - Linux - Ubuntu specific steps](includes/custom-runtime-r-linux-ubuntu.md)]

[!INCLUDE [R custom runtime on Linux - Common steps](includes/custom-runtime-r-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-rhel"
[!INCLUDE [R custom runtime - Linux - Prerequisites](includes/custom-runtime-r-linux-prerequisites.md)]

[!INCLUDE [R custom runtime - Linux - RHEL specific steps](includes/custom-runtime-r-linux-rhel.md)]

[!INCLUDE [R custom runtime on Linux - Common steps](includes/custom-runtime-r-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-sles"
[!INCLUDE [R custom runtime - Linux - Prerequisites](includes/custom-runtime-r-linux-prerequisites.md)]

[!INCLUDE [R custom runtime - Linux - SLES specific steps](includes/custom-runtime-r-linux-sles.md)]

[!INCLUDE [R custom runtime on Linux - Common steps](includes/custom-runtime-r-linux-common.md)]
::: zone-end

## <a name="enable-external-script"></a>Включение внешнего скрипта

Внешний скрипт Python можно выполнить с помощью хранимой процедуры [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Чтобы включить внешние скрипты, выполните с помощью [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) приведенную ниже инструкцию.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>Проверка установки

Используйте следующий скрипт SQL для проверки установки и функций настраиваемой среды выполнения R.

```sql
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(R.home());
print(file.path(R.home("bin"), "R"));
print(R.version);
print("Hello RExtension!");'
```

::: zone pivot="platform-linux-rhel"
[!INCLUDE [R custom runtime - Linux - RHEL known issues](includes/custom-runtime-r-linux-known-issues-rhel.md)]
::: zone-end

## <a name="next-steps"></a>Дальнейшие действия

+ [Установка настраиваемой среды выполнения Python для SQL Server](custom-runtime-python.md)
+ [Платформа расширяемости в SQL Server](../concepts/extensibility-framework.md)
+ [Общие сведения о расширениях языка](../../language-extensions/language-extensions-overview.md)
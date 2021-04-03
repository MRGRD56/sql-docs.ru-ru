---
title: Установка настраиваемой среды выполнения Python
description: Узнайте, как установить настраиваемую среду выполнения Python для SQL Server с помощью расширений языка. В настраиваемой среде выполнения Python можно выполнять скрипты машинного обучения.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: contperf-fy21q3
zone_pivot_groups: sqlml-platforms
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 22ba1ff16d22582be39e7b48a297e9dc65f9f554
ms.sourcegitcommit: e4b71e5d432a29b6c76ea457b00aa0abd4b6c77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2021
ms.locfileid: "106273481"
---
# <a name="install-a-python-custom-runtime-for-sql-server"></a>Установка настраиваемой среды выполнения Python для SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

Узнайте, как установить настраиваемую среду выполнения Python для запуска внешних скриптов Python в SQL Server.

+ Windows
+ Ubuntu Linux
+ Red Hat Enterprise Linux (RHEL)
+ SUSE Linux Enterprise Server (SLES)

Настраиваемая среда выполнения позволяет выполнять скрипты машинного обучения и использует [расширения языка SQL Server](../../language-extensions/language-extensions-overview.md).

Используйте для SQL Server собственную версию среды выполнения Python, а не версию среды выполнения по умолчанию, устанавливаемую вместе со [Службами машинного обучения SQL Server](../sql-server-machine-learning-services.md).

::: zone pivot="platform-windows"
[!INCLUDE [Python custom runtime - Windows](includes/custom-runtime-python-windows.md)]
::: zone-end

::: zone pivot="platform-linux-ubuntu"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - Ubuntu specific steps](includes/custom-runtime-python-linux-ubuntu.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-rhel"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - RHEL specific steps](includes/custom-runtime-python-linux-rhel.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

::: zone pivot="platform-linux-sles"
[!INCLUDE [Python custom runtime - Linux - Prerequisites](includes/custom-runtime-python-linux-prerequisites.md)]

[!INCLUDE [Python custom runtime - Linux - SLES specific steps](includes/custom-runtime-python-linux-sles.md)]

[!INCLUDE [Python custom runtime on Linux - Common steps](includes/custom-runtime-python-linux-common.md)]
::: zone-end

## <a name="enable-external-script"></a>Включение внешнего скрипта

Внешний скрипт Python можно выполнить с помощью хранимой процедуры [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Чтобы включить внешние скрипты, выполните с помощью [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) приведенную ниже инструкцию.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>Проверка установки

Используйте приведенный ниже скрипт SQL для проверки установки и функций настраиваемой среды выполнения Python.

```sql
EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## <a name="next-steps"></a>Дальнейшие действия

+ [Установка настраиваемой среды выполнения R для SQL Server](custom-runtime-r.md)
+ [Платформа расширяемости в SQL Server](../concepts/extensibility-framework.md)
+ [Общие сведения о расширениях языка](../../language-extensions/language-extensions-overview.md)

---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/18/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: f6ec3bbcc720fe472cc7146d59c30f0f3fc65ab0
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833759"
---
+ Для RExtension требуется GLIBCXX_3.4.20. Убедитесь, что в Red Hat Enterprise Linux (RHEL) установлена версия **libstdc++.so.6**.

## <a name="install-language-extensions"></a>Установка расширений языка

> [!NOTE]
> Если в SQL Server 2019 установлены [Службы машинного обучения](../../sql-server-machine-learning-services.md), значит, пакет **mssql-server-extensibility** для расширений языка уже установлен и этот шаг можно пропустить.

Выполните приведенную ниже команду, чтобы установить [расширения языка SQL Server](../../../language-extensions/language-extensions-overview.md) для Red Hat Enterprise Linux (RHEL), которые используются в настраиваемой среде выполнения R.

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

## <a name="install-r"></a>Установка R

1. Если установлены [Службы машинного обучения](../../sql-server-machine-learning-services.md), среда R уже установлена в `/opt/microsoft/ropen/3.5.2/lib64/R`. Если вы хотите использовать этот путь в качестве R_HOME, данный шаг можно пропустить.

    Если вы хотите использовать другую среду выполнения R, то перед продолжением установки новой версии необходимо удалить `microsoft-r-open-mro`.

    ```bash
    sudo yum erase microsoft-r-open-mro-3.5.2
    ```

1. Установите [R (версии 3.3 или более поздней)](https://www.r-project.org/) для Red Hat Enterprise Linux (RHEL). По умолчанию R устанавливается в папку **/usr/lib64/R**. Этот путь — ваш **R_HOME**. Если вы установили R в другом месте, зафиксируйте этот путь в качестве **R_HOME**.

    ```bash
    sudo yum install -y R
    ```

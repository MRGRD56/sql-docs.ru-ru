---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/16/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 70bf06deb43e861209e12b6481f3fd414cb9361f
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833776"
---
## <a name="install-language-extensions"></a>Установка расширений языка

> [!NOTE]
> Если в SQL Server 2019 установлены [Службы машинного обучения](../../sql-server-machine-learning-services.md), значит, пакет **mssql-server-extensibility** для расширений языка уже установлен и этот шаг можно пропустить.

Выполните приведенную ниже команду, чтобы установить [расширения языка SQL Server](../../../language-extensions/language-extensions-overview.md) для SUSE Linux Enterprise Server (SLES), которые используются в настраиваемой среде выполнения R.

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-r"></a>Установка R

1. Если установлены [Службы машинного обучения](../../sql-server-machine-learning-services.md), среда R уже установлена в `/opt/microsoft/ropen/3.5.2/lib64/R`. Если вы хотите использовать этот путь в качестве R_HOME, данный шаг можно пропустить.

    Если вы хотите использовать другую среду выполнения R, то перед продолжением установки новой версии необходимо удалить `microsoft-r-open-mro`.

    ```bash
    sudo zypper remove microsoft-r-open-mro-3.4.4
    ```

1. Установите [R (версии 3.3 или более поздней)](https://www.r-project.org/) для SUSE Linux Enterprise Server (SLES). По умолчанию R устанавливается в папку **/usr/lib64/R**. Этот путь — ваш **R_HOME**. Если вы установили R в другом месте, зафиксируйте этот путь в качестве **R_HOME**.

    Чтобы установить R, сделайте следующее:

    ```bash
    sudo zypper ar -f http://download.opensuse.org/repositories/devel:/languages:/R:/patched/openSUSE_12.3/ R-patched
    sudo zypper --gpg-auto-import-keys ref
    sudo zypper install R-core-libs R-core R-core-doc R-patched
    ```

    Предупреждения для **R-tcltk-3.6.1** можно игнорировать, если этот пакет не требуется.

## <a name="install-gcc-c"></a>Установка gcc-c++

Установите **gcc-c++** на SUSE Linux Enterprise Server (SLES). Он используется для **Rcpp** (устанавливается позже).

```bash
sudo zypper install gcc-c++
```

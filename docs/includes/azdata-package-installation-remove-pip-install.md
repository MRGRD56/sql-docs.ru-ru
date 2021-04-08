---
author: MikeRayMSFT
ms.prod: sql
ms.topic: include
ms.date: 01/07/2020
ms.author: mikeray
ms.openlocfilehash: 56271003fbbf4ac8a816413e441239a7a2f8229d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "107003813"
---
### <a name="pythonpip-installation"></a>Установка Python и PIP

Вы можете установить [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] в ОС Linux с помощью yum, apt или zypper либо в MacOS с помощью диспетчеров пакетов установки Homebrew. Пока не появились эти диспетчеры пакетов,для установки требовалось наличие Python и PIP.

>[!IMPORTANT]
>Прежде чем продолжать работу, удалите все установки `azdata`, установленные в глобальной системе Python. Новые установщики или собственные пакеты добавляют к переменной пути значение `azdata`, и очередность вызова предсказать невозможно.
Если в глобальной системе Python уже есть `azdata`, удалите его перед продолжением.

Чтобы просмотреть характеристики текущей установки, выполните следующую команду:

```bash
$ pip list --format columns
```

Если `azdata` устанавливается с помощью PIP, он возвращает пакет и версию. Пример:

```
 Package             Version
------------------- ----------
azdata-cli          15.0.X
azdata-cli-app      15.0.X
azdata-cli-cluster  15.0.X
azdata-cli-core     15.0.X
azdata-cli-hdfs     15.0.X
azdata-cli-notebook 15.0.X
azdata-cli-profile  15.0.X
azdata-cli-spark    15.0.X
azdata-cli-sql      15.0.X
```

В следующем примере удаляется установка PIP для `azdata`.

```bash
$ pip freeze | grep azdata-* | xargs pip uninstall -y
```

Убедившись, что вы успешно удалили все установки `azdata`, установленные с помощью PIP, продолжайте текущую установку.
---
title: Установка Azure Data CLI (azdata) с помощью apt
description: Узнайте, как установить средство Azure Data CLI (azdata) с помощью apt.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 04/07/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d4d1456126a99ca69631c3e817e381c9252fb84c
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "107003820"
---
# <a name="install-azure-data-cli-azdata-with-apt"></a>Установка [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)] с помощью apt

[!INCLUDE[azdata](../../includes/applies-to-version/azdata.md)]

Для дистрибутивов Linux с `apt` существует пакет для `azdata-cli`. Пакет CLI протестирован для версий Linux, которые используют `apt`, а именно:

- Ubuntu 16.04, Ubuntu 18.04, Ubuntu 20.04

[!INCLUDE [azdata-package-installation-remove-pip-install](../../includes/azdata-package-installation-remove-pip-install.md)]

## <a name="install-with-apt"></a>Установка с помощью apt

>[!IMPORTANT]
> Пакет RPM Azure для `azdata-cli` зависит от пакета python3. В вашей системе может быть установлена более ранняя версия Python, чем *требуемая версия 3.6.x*. Если для вас это важно, найдите другой пакет python3 или выполните инструкции по установке вручную с помощью [`pip`](../install/deploy-install-azdata-pip.md).

1. Установите зависимости, необходимые для установки `azdata-cli`.

   ```bash
   sudo apt-get update
   sudo apt-get install gnupg ca-certificates curl wget software-properties-common apt-transport-https lsb-release -y
   ```

2. Импортируйте ключ репозитория Майкрософт.

   ```bash
   curl -sL https://packages.microsoft.com/keys/microsoft.asc |
   gpg --dearmor |
   sudo tee /etc/apt/trusted.gpg.d/microsoft.asc.gpg > /dev/null
   ```

3. Создайте сведения о локальном репозитории.

   Для запуска клиента Ubuntu 16.04 выполните:

    ```bash
    sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/prod.list)"
    ```

   Для запуска клиента Ubuntu 18.04 выполните:

    ```bash
    sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/prod.list)"
    ```

   Для запуска клиента Ubuntu 20.04 выполните:

    ```bash
    sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/20.04/prod.list)"
    ```

4. Установить службы `azdata-cli`.

   ```bash
   sudo apt-get update
   sudo apt-get install -y azdata-cli
   ```

## <a name="verify-install"></a>Проверка установки

```bash
azdata
azdata --version
```

## <a name="update"></a>Update

Обновите `azdata-cli` с помощью команд `apt-get update` и `apt-get install`.

```bash
sudo apt-get update && sudo apt-get install --only-upgrade -y azdata-cli
```

## <a name="uninstall"></a>Удаление

1. Удалите пакет из системы.

   ```bash
   sudo apt-get remove -y azdata-cli
   ```

2. Если вы не планируете переустанавливать `azdata-cli`, удалите сведения о репозитории.

   ```bash
   sudo rm /etc/apt/sources.list.d/azdata-cli.list
   ```

3. Удалите ключ репозитория.

   ```bash
   sudo rm /etc/apt/trusted.gpg.d/dpgswdist.v1.asc.gpg
   ```

4. Удалите зависимости, которые больше не требуются.

   ```bash
   sudo apt autoremove
   ```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о кластерах больших данных см. в статье [Что такое [!INCLUDE[big-data-clusters-2019](../../includes/ssbigdataclusters-ver15.md)]?](../../big-data-cluster/big-data-cluster-overview.md).

Использование azdata со [службами данных с поддержкой Azure Arc](/azure/azure-arc/data/)

---
title: Настройка Службы Azure Kubernetes
titleSuffix: SQL Server Big Data Clusters
description: Узнайте, как настроить Службу Kubernetes Azure (AKS) для развертываний кластера больших данных SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d98eb31e4502ed319e00069cc979154bdc391839
ms.sourcegitcommit: b3788a0e1fff06a6c3954d1060855398ad41b81f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2021
ms.locfileid: "107879234"
---
# <a name="configure-azure-kubernetes-service-for-sql-server-big-data-cluster-deployments"></a>Настройка Службы Kubernetes Azure для развертываний кластера больших данных SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

В этой статье описывается, как настроить Службу Kubernetes Azure (AKS) для развертываний [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)].

AKS упрощает создание, настройку и кластера виртуальных машин, которые предварительно настроены с кластером Kubernetes для запуска контейнерных приложений, и управление им. Это позволяет использовать имеющиеся навыки или пользоваться опытом обширного и постоянно растущего сообщества, а также развертывать приложения на основе контейнеров и управлять ими в Microsoft Azure.

В этой статье описаны шаги по развертыванию Kubernetes в AKS с помощью Azure CLI. Если у вас еще нет подписки Azure, создайте бесплатную учетную запись, прежде чем начинать работу.

> [!TIP]
> Вы также можете создать скрипт для развертывания AKS и кластера больших данных за один шаг. Дополнительные сведения о том, как это сделать, см. в [скрипте Python](quickstart-big-data-cluster-deploy.md) или в [записной книжке](notebooks-deploy.md) Azure Data Studio.

## <a name="prerequisites"></a>Предварительные требования

- [Развертывание средств для работы с большими данными SQL Server 2019](deploy-big-data-tools.md)
   - **Kubectl**
   - **Azure Data Studio**
   - **Расширение SQL Server 2019**
   - **Azure CLI**

- Минимальная версия 1.13 для сервера Kubernetes. Для AKS нужно использовать параметр `--kubernetes-version`, чтобы указать версию, отличную от используемой по умолчанию.

- Чтобы обеспечить успешное развертывание и оптимальное взаимодействие при проверке основных сценариев в AKS, можно использовать один узел или кластер AKS с несколькими узлами, где доступны следующие ресурсы:
   - 8 виртуальных ЦП для всех узлов
   - 64 ГБ памяти на виртуальную машину
   - 24 или более подключенных диска для всех узлов

   > [!TIP]
   > В инфраструктуре Azure предлагается несколько вариантов размера виртуальных машин, дополнительные сведения о тех из них, которые доступны в регионе, где вы планируете выполнить развертывание, см. [здесь](/azure/virtual-machines/windows/sizes).

## <a name="create-a-resource-group"></a>Создание группы ресурсов

Группа ресурсов Azure — это логическая группа, в которой развертываются и управляются ресурсы Azure. Выполните следующие действия, чтобы войти в Azure и создать группу ресурсов для кластера AKS.

1. В командной строке выполните следующую команду и следуйте указаниям для входа в подписку Azure.

    ```azurecli
    az login
    ```

1. Если у вас несколько подписок, можно просмотреть все подписки, выполнив следующую команду.

   ```azurecli
   az account list
   ```

1. Если вы хотите перейти на другую подписку, можно выполнить следующую команду.

   ```azurecli
   az account set --subscription <subscription id>
   ```

1. Найдите регион Azure, в котором вы хотите развернуть кластер и ресурсы, с помощью следующей команды:

   ```azurecli
   az account list-locations -o table
   ```

1. Создайте группу ресурсов с помощью команды **az group create**. В следующем примере создается группа ресурсов с именем `sqlbdcgroup` в расположении именем `westus2`.

   ```azurecli
   az group create --name sqlbdcgroup --location westus2
   ```

## <a name="verify-available-kubernetes-versions"></a>Проверка доступных версий Kubernetes

Используйте самую последнюю доступную версию Kubernetes. Эта версия зависит от расположения развертывания кластера. Приведенная ниже команда возвращает версии Kubernetes, доступные в указанном расположении.

Перед выполнением команды обновите скрипт. Замените `<Azure data center>` расположением кластера.

   **bash**

   ```bash
   az aks get-versions \
   --location <Azure data center> \
   --query orchestrators \
   --o table
   ```

   **PowerShell**

   ```powershell
   az aks get-versions `
   --location <Azure data center> `
   --query orchestrators `
   --o table
   ```

Выберите самую последнюю доступную версию для своего кластера. Запишите номер версии. Он будет использован на следующем шаге.

## <a name="create-a-kubernetes-cluster"></a>Создание кластера Kubernetes

1. Создайте кластер Kubernetes в AKS с помощью команды [az aks create](/cli/azure/aks). В следующем примере создается кластер Kubernetes *kubcluster* с одним узлом агента Linux размером **Standard_L8s**.

   Перед выполнением скрипта замените `<version number>` номером версии, определенным на предыдущем шаге.

   Убедитесь, что вы создали кластер AKS в той же группе ресурсов, которая использовалась в предыдущих разделах.

   **bash:**

   ```bash
   az aks create --name kubcluster \
   --resource-group sqlbdcgroup \
   --generate-ssh-keys \
   --node-vm-size Standard_L8s \
   --node-count 1 \
   --kubernetes-version <version number>
   ```

   **PowerShell:**

   ```powershell
   az aks create --name kubcluster `
   --resource-group sqlbdcgroup `
   --generate-ssh-keys `
   --node-vm-size Standard_L8s `
   --node-count 1 `
   --kubernetes-version <version number>
   ```

   Количество узлов агента Kubernetes можно увеличить или уменьшить, изменив значение `--node-count <n>`, где `<n>` — число узлов агента, которые вы хотите использовать. Сюда не входит главный узел Kubernetes, которым AKS управляет в фоновом режиме. В предыдущем примере в целях оценки используется только один узел. Можно также изменить `--node-vm-size`, чтобы выбрать соответствующий размер виртуальной машины, соответствующий требованиям к рабочей нагрузке. Для вывода списка доступных размеров виртуальных машин в вашем регионе выполните команду `az vm list-sizes --location westus2 -o table`.

   Через несколько минут команда завершается и возвращает сведения о кластере в формате JSON.

   > [!TIP]
   > Если при создании кластера в AKS возникают ошибки, см. [раздел об устранении неполадок](#troubleshoot) в этой статье.

1. Сохраните выходные данные JSON из предыдущей команды для последующего использования.

## <a name="connect-to-the-cluster"></a>Подключение к кластеру

1. Чтобы настроить kubectl для подключения к кластеру Kubernetes, выполните команду [az aks get-credentials](/cli/azure/aks#az_aks_get_credentials). На этом шаге выполняется скачивание учетных данных и настройка интерфейса командной строки kubectl для их использования.

   ```azurecli
   az aks get-credentials --resource-group=sqlbdcgroup --name kubcluster
   ```

1. Чтобы проверить подключение к кластеру, используйте команду [kubectl get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands) для получения списка узлов кластера.  В приведенном ниже примере показаны выходные данные, как если бы у вас были 1 главный узел и 3 узла агентов.

   ```bash
   kubectl get nodes
   ```

## <a name="troubleshooting"></a><a id="troubleshoot"></a> Устранение неполадок

Если у вас возникли проблемы при создании Службы Azure Kubernetes с помощью предыдущих команд, воспользуйтесь следующими способами.

- Убедитесь, что установлена [последняя версия Azure CLI](/cli/azure/install-azure-cli).
- Попробуйте выполнить те же действия, используя другую группу ресурсов и имя кластера.
- См. [документацию по устранению неполадок, связанных с AKS](/azure/aks/troubleshooting).

## <a name="next-steps"></a>Дальнейшие действия

Действия, описанные в этой статье, обеспечивают настройку кластера Kubernetes в AKS. Следующим шагом является развертывание кластера больших данных SQL Server 2019 в кластере AKS. Дополнительные сведения о развертывании кластеров больших данных см. в следующей статье:

[Развертывание [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в Kubernetes](deployment-guidance.md)
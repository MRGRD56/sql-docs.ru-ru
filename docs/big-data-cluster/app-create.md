---
title: Развертывание приложений с помощью azdata
titleSuffix: SQL Server Big Data Clusters
description: Развертывание сценария Python или R в качестве приложения в кластере больших данных SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 890b029833e7d34da7663b9f0e6ccfa63195c6d5
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91725088"
---
# <a name="how-to-deploy-an-app-on-sql-server-big-data-clusters"></a>Развертывание приложения в кластерах больших данных SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Приложения, развернутые в кластерах больших данных SQL Server, не только получают множество преимуществ, таких как вычислительная мощность кластера, но также имеют доступ к большим данным, имеющимся в кластере. Это значительно повышает производительность, так как приложение находится в том же кластере, где находятся данные.

В этой статье описывается, как развернуть скрипт Python или R и управлять им как приложением в кластере больших данных SQL Server.

## <a name="whats-new-and-improved"></a>Новые возможности и усовершенствования

- Единая служебная программа командной строки для управления кластером и приложением.
- Упрощенное развертывание приложений, обеспечивающее детальный контроль с помощью файлов спецификаций.
- Поддержка размещения дополнительных типов приложений — SQL Server Integration Services (SSIS) и MLeap.
- [Расширение Visual Studio Code](app-deployment-extension.md) для управления развертыванием приложений.

Для развертывания приложений и управления ими используется служебная программа командной строки `azdata`. В этой статье приведены примеры развертывания приложений из командной строки. Сведения об использовании этой функции в Visual Studio Code см. в разделе [Расширение Visual Studio Code](app-deployment-extension.md).

Поддерживаются следующие типы приложений.

- **Python** — один из наиболее популярных универсальных языков программирования для различных пользователей, таких как инженеры данных, специалисты по обработке и анализу данных или инженеры DevOps. Он поддерживает множество сценариев, таких как структурирование данных, автоматизацию, прототипирование (в некоторой степени), а также все больше используется для программирования приложений корпоративного уровня, работающих в сочетании с платформами веб-разработки, такими как Flask и Django, для решения различных бизнес-задач.  
- **R** — еще один популярный язык программирования для инженеров данных и специалистов по обработке и анализу данных. По сравнению с Python, R — это более специализированный язык программирования, ориентированный на статистические вычисления и графику.  
- **SQL Server Integration Services (SSIS)**  — высокопроизводительные решения для интеграции данных для создания и отладки пакетов ETL, в которых используется формат файлов пакета служб DTS (DTSX), который представляет собой файловый формат XML, где хранятся инструкции по обработке переноса данных между базами данных и интеграцией внешних источников данных.   
- **MLeap** — это распространенный формат сериализации, который предоставляет все необходимое для выполнения и сериализации конвейеров SparkML, а также других структур, которые затем во время выполнения могут быть загружены для обработки задач машинного обучения практически в реальном времени и близко к данным.  

## <a name="prerequisites"></a>Предварительные требования

- [Кластер больших данных SQL Server 2019](deployment-guidance.md)
- [Служебная программа командной строки azdata](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Возможности

В SQL Server 2019 можно создать, удалить, описать, инициализировать, перечислить, запустить и обновить приложение. В следующей таблице описаны команды развертывания приложения, которые можно использовать с **azdata**.

|Get-Help |Описание |
|:---|:---|
|`azdata login` | Вход в кластер больших данных SQL Server. |
|`azdata app create` | Создание приложения. |
|`azdata app delete` | Удаление приложения. |
|`azdata app describe` | Описание приложения. |
|`azdata app init` | Быстрое создание основы нового приложения. |
|`azdata app list` | Вывод списка приложений. |
|`azdata app run` | Выполнение приложения. |
|`azdata app update`| Обновление приложения. |

Справку по параметру `--help` можно получить, как показано в следующем примере.

```bash
azdata app create --help
```

В следующих разделах эти команды описаны более подробно.

## <a name="sign-in"></a>Вход

Прежде чем развертывать приложения или взаимодействовать с ними, сначала войдите в кластер больших данных SQL Server с помощью команды `azdata login`. Укажите внешний IP-адрес службы `controller-svc-external` (например, `https://ip-address:30080`), а также имя пользователя и пароль для кластера.

```bash
azdata login --controller-endpoint https://<ip-address-of-controller-svc-external>:30080 --controller-username <user-name>
```

## <a name="azure-kubernetes-service-aks"></a>Служба Azure Kubernetes (AKS)

При использовании AKS нужно выполнить следующую команду в окне bash или cmd, чтобы получить IP-адрес службы `controller-svc-external`.


```bash
kubectl get svc controller-svc-external -n <name of your big data cluster>
```

## <a name="kubernetes-clusters-created-with-kubeadm"></a>Кластеры Kubernetes, созданные с помощью kubeadm

Чтобы получить IP-адрес для входа в кластер, выполните следующую команду.

```bash
kubectl get node --selector='node-role.kubernetes.io/master'
```

## <a name="create-an-app-skeleton"></a>Создание основы приложения

Команда **azdata app init** обеспечивает формирование шаблонов с соответствующими артефактами, необходимое для развертывания приложения. В приведенном ниже примере создается приложение add-app с помощью следующей команды.

```bash
azdata app init --name add-app --version v1 --template python
```

При этом создается папка с именем hello.  Вы можете использовать команду `cd` в каталоге и проверить созданные файлы в папке. Спецификация spec. YAML определяет приложение, например имя, версию и исходный код. Вы можете изменить эту спецификацию, чтобы изменить имя, версию, входные и выходные данные.

Ниже приведен пример выходных данных команды init, которые будут отображаться в папке.

```
add-app.py
run-spec.yaml
spec.yaml
```

## <a name="create-an-app"></a>Создание приложения

Чтобы создать приложение, используйте `azdata` с командой `app create`. Эти файлы располагаются локально на компьютере, где вы создаете приложение.

Чтобы создать приложение в кластере больших данных, используйте следующий синтаксис.

```bash
azdata app create --spec <directory containing spec file>
```

Ниже показан пример того, как может выглядеть эта команда.

```bash
azdata app create --spec ./addpy
```

Предполагается, что ваше приложение хранится в папке `addpy`. Эта папка также должна содержать файл спецификации для приложения — `spec.yaml`. Дополнительные сведения о файле `spec.yaml` см. на странице [Развертывание приложений](concept-application-deployment.md).

Чтобы развернуть этот пример приложения, создайте следующие файлы в каталоге с именем `addpy`.

- `add.py`. Скопируйте в этот файл следующий код Python.
   ```py
   #add.py
  def add(x, y):
    result = x+y
    return result
  result=add(x,y)
   ```
- `spec.yaml`. Скопируйте в этот файл следующий код.
   ```yaml
   #spec.yaml
   name: add-app #name of your python script
   version: v1  #version of the app
   runtime: Python #the language this app uses (R or Python)
   src: ./add.py #full path to the location of the app
   entrypoint: add #the function that will be called upon execution
   replicas: 1  #number of replicas needed
   poolsize: 1  #the pool size that you need your app to scale
   inputs:  #input parameters that the app expects and the type
     x: int
     y: int
   output: #output parameter the app expects and the type
     result: int
   ```

Затем выполните следующую команду.

```bash
azdata app create --spec ./addpy
```

Проверить, развернуто ли приложение, можно с помощью команды list.

```bash
azdata app list
```

Если развертывание не завершено, в `state` отображается `WaitingforCreate`, как показано в примере ниже.

```json
[
  {
    "name": "add-app",
    "state": "WaitingforCreate",
    "version": "v1"
  }
]
```

После успешного развертывания состояние `state` должно изменить на `Ready`.

```json
[
  {
    "name": "add-app",
    "state": "Ready",
    "version": "v1"
  }
]
```

## <a name="list-an-app"></a>Перечисление приложения

Вы можете перечислить любые приложения, которые были успешно созданы с помощью команды `app list`.

Следующая команда перечисляет все доступные приложения в кластере больших данных.

```bash
azdata app list
```

Если указать имя и версию, она перечисляет конкретное приложение и его состояние (создание или готовность).

```bash
azdata app list --name <app_name> --version <app_version>
```

Следующий пример демонстрирует использование этой команды.

```bash
azdata app list --name add-app --version v1
```

Выходные данные должны соответствовать следующему примеру.

```json
[
  {
    "name": "add-app",
    "state": "Ready",
    "version": "v1"
  }
]
```

## <a name="delete-an-app"></a>Удаление приложения

Чтобы удалить приложение из кластера больших данных, используйте следующий синтаксис.

```bash
azdata app delete --name add-app --version v1
```

## <a name="next-steps"></a>Дальнейшие действия

Больше о том, как интегрировать приложения, развернутые в [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], в собственные приложения, см. в статьях [Запуск приложений в кластерах больших данных](app-run.md) и [Использование приложений в кластерах больших данных](app-consume.md). Дополнительные примеры можно просмотреть в наборе [примеров развертывания приложений](https://aka.ms/sql-app-deploy).

Дополнительные сведения о [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] см. в статье [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md).
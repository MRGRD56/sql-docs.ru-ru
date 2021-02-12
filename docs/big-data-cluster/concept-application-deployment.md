---
title: Что такое развертывание приложения?
titleSuffix: SQL Server Big Data Clusters
description: Узнайте, как развертывание приложений предоставляет интерфейсы для создания и запуска приложений, а также управления ими в кластере больших данных SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8c1f6b4895e872289095411cfd8ecdaf1e2eb087
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047664"
---
# <a name="what-is-application-deployment-on-a-sql-server-big-data-cluster"></a>Что такое развертывание приложения в кластере больших данных SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Развертывание приложения позволяет развертывать приложения в кластере больших данных SQL Server, предоставляя интерфейсы для создания и выполнения приложений, а также управления ими. Приложения, развертываемые в кластере больших данных SQL Server, могут использовать его вычислительные возможности и обращаться к размещенным в нем данным. Это повышает масштабируемость и производительность приложений, а также позволяет управлять ими в месте размещения данных. Поддерживаемые среды выполнения приложений в [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]: R, Python, SSIS, MLeap.

В следующих разделах описываются архитектура и функциональные возможности развертывания приложения.

## <a name="application-deployment-architecture"></a>Архитектура развертывания приложения

Развертывание приложения состоит из контроллера и обработчиков среды выполнения приложений. При создании приложения предоставляется файл спецификации (`spec.yaml`). Этот файл `spec.yaml` содержит все сведения, которые нужны контроллеру для успешного развертывания приложения. Ниже приведен пример содержимого файла `spec.yaml`.

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

Контроллер проверяет элемент `runtime` в файле `spec.yaml` и вызывает соответствующий обработчик среды выполнения. Обработчик среды выполнения создает приложение. Во-первых, создается объект ReplicaSet Kubernetes, содержащий один или несколько объектов pod, каждый из которых содержит развертываемое приложение. Количество объектов pod определяется параметром `replicas`, указанным в файле `spec.yaml` для приложения. Каждый объект pod может иметь один или несколько пулов. Количество пулов определяется параметром `poolsize`, указанным в файле `spec.yaml`.

Эти параметры влияют на количество запросов, которые могут обрабатываться параллельно. Максимальное количество запросов в определенный момент времени равно значению `replicas`, умноженному на `poolsize`. Если имеется 5 реплик с двумя пулами в каждой, параллельно могут обрабатываться 10 запросов. Графическое представление параметров `replicas` и `poolsize` см. на рисунке ниже.

![Poolsize и replicas](media/big-data-cluster-create-apps/poolsize-vs-replicas.png)

После создания объекта ReplicaSet и запуска пулов создается задание cron, если в файле `spec.yaml` был задан параметр `schedule`. Наконец, создается служба Kubernetes, которую можно использовать для управления приложением и его запуска (см. ниже).

При выполнении приложения служба Kubernetes передает запросы в реплику и возвращает результаты.

## <a name="security-considerations-for-applications-deployments-on-openshift"></a><a id="app-deploy-security"></a> Вопросы безопасности при развертывании приложений в OpenShift

SQL Server 2019 с накопительным пакетом обновления 5 (CU5) поддерживает развертывание кластеров больших данных на Red Hat OpenShift, а также обновленную модель безопасности для кластеров больших данных, поэтому привилегированные контейнеры больше не требуются. Помимо непривилегированных, контейнеры работают как пользователи, не являющиеся корневыми по умолчанию для всех новых развертываний, использующих SQL Server 2019 CU5.

На момент выпуска CU5 установка приложений, развертываемых с помощью интерфейсов [развертывания приложений](), по-прежнему производилась от имени *привилегированного* пользователя. Это обусловлено тем, что во время установки также устанавливаются дополнительные пакеты, используемые приложением. Другой пользовательский код, развертываемый в составе приложения, выполняется от имени пользователя с низким уровнем привилегий. 

Кроме того, имеется дополнительная возможность **CAP_AUDIT_WRITE**, которая необходима для планирования приложений служб SSIS с помощью заданий cron. Если в файле спецификации YAML приложения указано расписание, приложение будет запущено с помощью задания cron, для чего требуется дополнительная возможность.  Приложение можно также активировать по запросу с помощью команды *azdata app run* через вызов веб-службы, для которого не требуется возможность CAP_AUDIT_WRITE. 

> [!NOTE]
> Настраиваемая SCC в [статье, посвященной развертыванию OpenShift](deploy-openshift.md), не включает эту возможность, так как она не требуется для развертывания кластера больших данных по умолчанию. Чтобы включить эту возможность, необходимо сначала обновить пользовательский YAML-файл SCC, включив CAP_AUDIT_WRITE в 

```yml
...
allowedCapabilities:
- SETUID
- SETGID
- CHOWN
- SYS_PTRACE
- AUDIT_WRITE
...
```

## <a name="how-to-work-with-application-deployment"></a>Работа с развертыванием приложения

Развертывание приложения имеет два основных интерфейса: 
- [интерфейс командной строки [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)];](app-create.md)
- [расширение для Visual Studio Code и Azure Data Studio.](app-deployment-extension.md)

Приложение может также выполняться с помощью веб-службы RESTful. Дополнительные сведения см. в статье [Использование приложений в кластерах больших данных](app-consume.md).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о создании и выполнении приложений в [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] см. в следующих статьях:

- [Развертывание приложений с помощью azdata](app-create.md)
- [Развертывание приложений с помощью расширения "Развертывание приложения"](app-deployment-extension.md)
- [Использование приложений в кластерах больших данных](app-consume.md)

Дополнительные сведения о [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] см. в следующем обзоре:

- [Что такое [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
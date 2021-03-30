---
title: Руководства по развертыванию
titleSuffix: SQL Server Big Data Clusters
description: Узнайте, как развертывать кластеры больших данных SQL Server в Kubernetes.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9ca8eff773bff7900a1228665d65d911b52d5a8f
ms.sourcegitcommit: c242f423cc3b776c20268483cfab0f4be54460d4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105551490"
---
# <a name="how-to-deploy-big-data-clusters-2019-on-kubernetes"></a>Развертывание [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] в Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Кластер больших данных SQL Server развертывается в виде контейнеров Docker в кластере Kubernetes. Здесь описаны этапы настройки.

- Настройте кластер Kubernetes на одной виртуальной машине, в кластере виртуальных машин, в службе Azure Kubernetes (AKS), в Red Hat OpenShift или в Azure Red Hat OpenShift (ARO).
- Установите средство настройки кластера [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] на клиентском компьютере.
- Разверните кластер больших данных SQL Server в кластере Kubernetes.

## <a name="supported-platforms"></a>Поддерживаемые платформы

Полный список платформ Kubernetes, проверенных для развертывания кластеров больших данных SQL Server, см. в разделе [Поддерживаемые платформы](release-notes-big-data-cluster.md#supported-platforms).

### <a name="sql-server-editions"></a>Выпуски SQL Server

|Выпуск|Примечания|
|---------|---------|
|Enterprise<br/>Standard<br/>Разработчик| Выпуск кластера больших данных определяется выпуском основного экземпляра SQL Server. Во время развертывания выпуск Developer развертывается по умолчанию. После развертывания выпуск можно изменить. См. [Настройка основного экземпляра SQL Server](./configure-sql-server-master-instance.md). |

## <a name="kubernetes"></a><a id="prereqs"></a> Kubernetes

### <a name="kubernetes-cluster-setup"></a><a id="kubernetes"></a> Настройка кластера Kubernetes

Если у вас уже есть кластер Kubernetes, соответствующий приведенным выше предварительным требованиям, можно сразу перейти к [этапу развертывания](#deploy). В этом разделе предполагается базовое понимание концепций Kubernetes.  Подробные сведения о Kubernetes см. в [документации по Kubernetes](https://kubernetes.io/docs/home).

Вы можете развернуть Kubernetes указанными ниже способами.

| Расположение развертывания Kubernetes | Описание | Ссылка |
|---|---|---|
| **Службы Azure Kubernetes (AKS)** | Управляемая служба контейнеров Kubernetes в Azure. | [Инструкции](deploy-on-aks.md) |
| **Один или несколько компьютеров (`kubeadm`)** | Кластер Kubernetes, развернутый на физических компьютерах или виртуальных машинах с помощью `kubeadm` | [Инструкции](deploy-with-kubeadm.md) |
|**Azure Red Hat OpenShift** | Управляемое предложение OpenShift, выполняемое в Azure. | [Инструкции](deploy-openshift.md)|
|**Red Hat OpenShift**|Гибридное облако, корпоративная платформа приложений Kubernetes.| [Инструкции](deploy-openshift.md)|

> [!TIP]
> Вы также можете создать скрипт для развертывания AKS и кластера больших данных за один шаг. Дополнительные сведения о том, как это сделать, см. в [скрипте Python](quickstart-big-data-cluster-deploy.md) или в [записной книжке](notebooks-deploy.md) Azure Data Studio.

### <a name="verify-kubernetes-configuration"></a>Проверка конфигурации Kubernetes

Выполните команду `kubectl`, чтобы просмотреть конфигурацию кластера. Убедитесь, что kubectl указывает на правильный контекст кластера.

```bash
kubectl config view
```

> [!Important] 
> При развертывании в кластере Kubernetes с несколькими узлами, начальная загрузка которого выполняется с помощью `kubeadm`, перед развертыванием кластера больших данных необходимо убедиться, что часы во всех узлах Kubernetes, участвующих в развертывании, синхронизированы. Кластер больших данных имеет встроенные свойства обеспечения работоспособности разных служб, которые зависят от времени, поэтому любые отклонения во времени могут привести к неправильной работе системы.

После настройки кластера Kubernetes можно перейти к развертыванию нового кластера больших данных SQL Server. Если вы обновляете предыдущий выпуск, см. статью [Обновление[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-upgrade.md).

## <a name="ensure-you-have-storage-configured"></a>Убедитесь, что хранилище настроено

В большинстве случаев развертывания кластеров больших данных должны иметь постоянное хранилище. В настоящее время необходимо убедиться, как именно вы собираетесь подготовить постоянное хранилище в кластере Kubernetes перед развертыванием BDC.

При развертывании в AKS настройка хранилища не требуется. AKS предоставляет встроенные классы хранения с динамической подготовкой. Вы можете настроить класс хранения (`default` или `managed-premium`) в файле конфигурации развертывания. Встроенные профили используют класс хранения `default`. При развертывании в кластере Kubernetes, развернутом с помощью `kubeadm`, необходимо убедиться в наличии достаточного объема хранилища для кластера, доступного для требуемого масштабирования и настроенного для использования. Если вы хотите настроить использование хранилища, сделайте это перед продолжением. См. раздел [Сохраняемость данных при использовании кластера больших данных SQL Server в Kubernetes](concept-data-persistence.md).

## <a name="install-sql-server-2019-big-data-tools"></a>Установка средств для работы с большими данными SQL Server 2019

Перед развертыванием кластера больших данных SQL Server 2019 [установите средства для работы с большими данными](deploy-big-data-tools.md).

- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]
- `kubectl`
- Azure Data Studio
- [Расширение Data Virtualization](../azure-data-studio/extensions/data-virtualization-extension.md) для Azure Data Studio


## <a name="deployment-overview"></a><a id="deploy"></a> Общие сведения о развертывании

Большинство параметров кластера больших данных определяется в файле конфигурации развертывания JSON. Вы можете использовать профиль развертывания по умолчанию для кластеров AKS и Kubernetes, созданных с помощью `kubeadm`, или настроить собственный файл конфигурации развертывания для использования во время установки. По соображениям безопасности параметры проверки подлинности передаются через переменные среды.

Следующие разделы содержат дополнительные сведения о настройке развертываний кластера больших данных, а также примеры распространенных настроек. Кроме того, вы всегда можете изменить пользовательский файл конфигурации развертывания в редакторе, таком как VS Code.

## <a name="default-configurations"></a><a id="configfile"></a> Конфигурации по умолчанию

Параметры развертывания кластера больших данных определяются в файлах конфигурации JSON. Можно начать пользовательскую настройку развертывания кластеров с встроенных профилей развертывания, которые доступны в [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. 

> [!NOTE]
> Образы контейнеров, необходимые для развертывания кластера больших данных, размещаются в реестре контейнеров Microsoft (`mcr.microsoft.com`) в репозитории `mssql/bdc`. По умолчанию эти параметры уже включены в файл конфигурации `control.json` в каждом из профилей развертывания, включенных в [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Кроме того, тег образа контейнера для каждого выпуска также предварительно заполняется в том же файле конфигурации. Если необходимо извлечь образы контейнеров в собственный частный реестр контейнеров и изменить параметры реестра контейнеров или репозитория, следуйте инструкциям в статье [Автономная установка](deploy-offline.md).

Выполните следующую команду, чтобы найти доступные шаблоны:

```
azdata bdc config list -o table 
```

В SQL Server 2019 CU5 доступны следующие шаблоны. 

| Профиль развертывания | Среда Kubernetes |
|---|---|
| `aks-dev-test` | Развертывание кластера больших данных SQL Server в службе Azure Kubernetes Service (AKS)|
| `aks-dev-test-ha` | Развертывание кластера больших данных SQL Server в службе Azure Kubernetes (AKS). Критически важные службы, такие как основной экземпляр SQL Server и узел имен HDFS, настроены для обеспечения высокой доступности.|
| `aro-dev-test`|Развертывание кластера больших данных SQL Server в Azure Red Hat OpenShift для разработки и тестирования. <br/><br/>Впервые представлено в SQL Server 2019 CU5.|
| `aro-dev-test-ha`|Развертывание кластера больших данных SQL Server с высоким уровнем доступности в кластере Red Hat OpenShift для разработки и тестирования. <br/><br/>Впервые представлено в SQL Server 2019 CU5.|
| `kubeadm-dev-test` | Развертывание кластера больших данных SQL Server в кластере Kubernetes, созданном с помощью kubeadm на одной или нескольких физических или виртуальных машинах.|
| `kubeadm-prod`| Развертывание кластера больших данных SQL Server в кластере Kubernetes, созданном с помощью kubeadm на одной или нескольких физических или виртуальных машинах. Используйте этот шаблон, чтобы включить интеграцию служб кластеров больших данных с Active Directory. Критически важные службы, такие как основной экземпляр SQL Server и узел имен HDFS, настроены для обеспечения высокой доступности.  |
| `openshift-dev-test`|Развертывание кластера больших данных SQL Server в кластере Red Hat OpenShift для разработки и тестирования. <br/><br/>Впервые представлено в SQL Server 2019 CU5.|
| `openshift-prod`|Развертывание кластера больших данных SQL Server с высоким уровнем доступности в кластере Red Hat OpenShift. <br/><br/>Впервые представлено в SQL Server 2019 CU5.|

Кластер больших данных можно развернуть, выполнив команду `azdata bdc create`. Вам будет предложено выбрать одну из конфигураций по умолчанию, а затем выполнить развертывание, следуя указаниям.

При первом запуске [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] нужно включить `--accept-eula=yes`, чтобы принять условия лицензионного соглашения (EULA).

```bash
azdata bdc create --accept-eula=yes
```

В этом сценарии вам будет предложено ввести параметры, не входящие в конфигурацию по умолчанию, например пароли. 

> [!IMPORTANT]
> Имя кластера больших данных по умолчанию — `mssql-cluster`. Это важно знать, чтобы выполнить любую из команд `kubectl`, задающих пространство имен Kubernetes с параметром `-n`.

## <a name="custom-configurations"></a><a id="customconfig"></a> Пользовательские конфигурации

Кроме того, можно настроить развертывание специально для выполнения нужных вам рабочих нагрузок. Нельзя изменить масштаб (число реплик) или параметры хранилища для служб кластеров больших данных после развертывания, поэтому необходимо тщательно спланировать конфигурацию развертывания, чтобы избежать проблем с емкостью. Чтобы настроить развертывание, выполните следующие действия:

1. Начните с одного из стандартных профилей развертывания, соответствующих вашей среде Kubernetes. Для их перечисления можно использовать команду `azdata bdc config list`:

   ```bash
   azdata bdc config list
   ```

1. Чтобы настроить развертывание, создайте копию профиля развертывания с помощью команды `azdata bdc config init`. Например, следующая команда создает копию файлов конфигурации развертывания `aks-dev-test` в целевом каталоге `custom`:

   ```bash
   azdata bdc config init --source aks-dev-test --target custom
   ```

   >[!TIP]
   >`--target` указывает каталог, содержащий файлы конфигурации `bdc.json` и `control.json`, на основе параметра `--source`.

1. Чтобы настроить параметры в профиле конфигурации развертывания, можно изменить этот файл в средстве, которое подходит для редактирования JSON-файлов, например VS Code. Для автоматизации на основе скриптов можно также изменить пользовательский профиль развертывания с помощью команды `azdata bdc config`. Например, следующая команда изменяет пользовательский профиль развертывания, чтобы изменить имя развернутого кластера с используемого по умолчанию (`mssql-cluster`) на `test-cluster`.  

   ```bash
   azdata bdc config replace --config-file custom/bdc.json --json-values "metadata.name=test-cluster"
   ```

   > [!TIP]
   > Вы также можете передать имя кластера во время развертывания с помощью параметра *--name* для команды *azdata create bdc*. Параметры в команде имеют приоритет над значениями в файлах конфигурации.
   >
   > Полезным инструментом для поиска путей JSON является [JSONPath Online Evaluator](https://jsonpath.com/).
   >
   Кроме передачи пар "ключ — значение", можно также указать встроенные значения JSON или передать файлы исправлений JSON. Дополнительные сведения см. в статье [Настройка параметров развертывания для кластеров больших данных](deployment-custom-configuration.md).

1. Передайте пользовательский файл конфигурации в `azdata bdc create`. Обратите внимание, что требуется задать необходимые [переменные среды](#env), в противном случае терминал предложит ввести эти значения:

   ```bash
   azdata bdc create --config-profile custom --accept-eula yes
   ```

> Дополнительные сведения о структуре файла конфигурации развертывания см. в [справочнике по файлу конфигурации развертывания](reference-deployment-config.md). Дополнительные примеры конфигурации см. в статье [Настройка параметров развертывания для кластеров больших данных](deployment-custom-configuration.md).

## <a name="environment-variables"></a><a id="env"></a> Переменные среды

Указанные ниже переменные среды используются для параметров безопасности, которые не хранятся в файле конфигурации развертывания. Обратите внимание, что параметры Docker, за исключением учетных данных, можно задать в файле конфигурации.

| Переменная среды | Требование |Описание |
|---|---|---|
| `AZDATA_USERNAME` | Обязательное значение |Имя пользователя администратора кластера больших данных SQL Server. Данные для входа sysadmin с тем же именем создаются в основном экземпляре SQL Server. В целях безопасности учетная запись `sa` отключена. <br/><br/>[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]|
| `AZDATA_PASSWORD` | Обязательно |Пароль для учетной записи пользователя, созданной выше. В кластерах, развернутых до версии SQL Server 2019 CU5, один и тот же пароль используется для пользователя `root`, для защиты шлюза Knox и HDFS. |
| `ACCEPT_EULA`| Требуется для первого использования [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].| Выберите "Да". При задании в качестве переменной среды применяет лицензионное соглашение как к SQL Server, так и к [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Если параметр не задан в качестве переменной среды, можно включить `--accept-eula=yes` при первом использовании команды [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].|
| `DOCKER_USERNAME` | Необязательно | Имя пользователя для доступа к образам контейнера в случае, если они хранятся в частном репозитории. Дополнительные сведения о том, как использовать частный репозиторий Docker для развертывания кластеров больших данных, см. в разделе [Автономные развертывания](deploy-offline.md).|
| `DOCKER_PASSWORD` | Необязательно |Пароль для доступа к указанному выше частному репозиторию. |

Эти переменные среды необходимо задать до вызова команду `azdata bdc create`. Если какая-либо из переменных не задана, вам будет предложено ввести ее.

В следующем примере показано, как задать переменные среды для Linux (bash) и Windows (PowerShell).

```bash
export AZDATA_USERNAME=admin
export AZDATA_PASSWORD=<password>
export ACCEPT_EULA=yes
```

```PowerShell
SET AZDATA_USERNAME=admin
SET AZDATA_PASSWORD=<password>
```

> [!NOTE]
> В кластерах, развернутых до версии SQL Server 2019 CU5, для шлюза Knox необходимо использовать пользователя `root` с указанным выше паролем. `root` — это единственный пользователь, поддерживаемый этим методом обычной проверки подлинности (имя пользователя и пароль).
> [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]
> Чтобы подключиться к SQL Server с обычной проверкой подлинности, используйте те же значения, что и для [переменных среды](#env) AZDATA_USERNAME и AZDATA_PASSWORD. 

После задания переменных среды нужно запустить `azdata bdc create`, чтобы активировать развертывание. В этом примере используется профиль конфигурации кластера, созданный ранее.

```bash
azdata bdc create --config-profile custom --accept-eula yes
```

Нужно учитывать следующее.

- Заключите пароль в двойные кавычки, если он содержит специальные символы. Вы можете задать для `AZDATA_PASSWORD` любое значение, но убедитесь, что пароль достаточно сложен и не содержит символы `!`, `&` или `'`. Обратите внимание, что разделители в виде двойных кавычек работают только в командах bash.
- Учетная запись `AZDATA_USERNAME` обладает правами системного администратора на главном экземпляре SQL Server, создаваемом во время установки. После создания контейнера SQL Server указанную вами переменную среды `AZDATA_PASSWORD` можно обнаружить, запустив `echo $AZDATA_PASSWORD` в контейнере. В целях безопасности измените пароль.

## <a name="unattended-install"></a><a id="unattended"></a> Автоматическая установка

Для автоматического развертывания нужно задать все необходимые переменные среды, использовать файл конфигурации и вызвать команду `azdata bdc create` с параметром `--accept-eula yes`. Примеры в предыдущем разделе демонстрируют синтаксис для автоматической установки.

## <a name="monitor-the-deployment"></a><a id="monitor"></a> Мониторинг развертывания

Во время начальной загрузки кластера командное окно клиента возвращает состояние развертывания. В процессе развертывания вы увидите ряд сообщений об ожидании pod контроллера.

```output
Waiting for cluster controller to start.
```

В течение 15–30 минут вы должны получить уведомление о том, что pod контроллера работает.

```output
Cluster controller endpoint is available at 11.111.111.11:30080.
Cluster control plane is ready.
```

> [!IMPORTANT]
> Полное развертывание может занять много времени из-за временных затрат на скачивание образов контейнеров для компонентов кластера больших данных. Однако это не должно занять несколько часов. При возникновении проблем с развертыванием см. статью [Мониторинг и устранение неполадок [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](cluster-troubleshooting-commands.md).

После завершения развертывания выходные данные указывают на успешное выполнение.

```output
Cluster deployed successfully.
```

> [!TIP]
> Именем по умолчанию для развернутого кластера больших данных является `mssql-cluster`, если только оно не изменено в пользовательской конфигурации.

## <a name="retrieve-endpoints"></a><a id="endpoints"></a> Получение конечных точек

После успешного завершения скрипта развертывания можно получить адреса внешних конечных точек для кластера больших данных, выполнив указанные ниже действия.

1. После развертывания найдите IP-адрес конечной точки контроллера либо в стандартных выходных данных развертывания, либо в выходных данных EXTERNAL-IP следующей команды `kubectl`.

   ```bash
   kubectl get svc controller-svc-external -n <your-big-data-cluster-name>
   ```

   > [!TIP]
   > Если вы не изменили имя по умолчанию во время развертывания, используйте `-n mssql-cluster` в предыдущей команде. `mssql-cluster` — это имя по умолчанию для кластера больших данных.

1. Войдите в кластер больших данных с помощью [azdata login](../azdata/reference/reference-azdata.md). Задайте для параметра `--endpoint` значение внешнего IP-адреса конечной точки контроллера.

   ```bash
   azdata login --endpoint https://<ip-address-of-controller-svc-external>:30080 --username <user-name>
   ```

   Укажите имя пользователя и пароль, настроенные для администратора кластера больших данных (AZDATA_USERNAME и AZDATA_PASSWORD) во время развертывания.

   > [!TIP]
   > Если вы являетесь администратором кластера Kubernetes и имеете доступ к файлу конфигурации кластера (файл конфигурации kube), можно настроить текущий контекст так, чтобы он указывал на целевой кластер Kubernetes. В этом случае можно выполнить вход с помощью `azdata login -n <namespaceName>`, где `namespace` — имя кластера больших данных. Вам будет предложено ввести учетные данные, если они не указаны в команде login.
   
1. Выполните команду [azdata bdc endpoint list](../azdata/reference/reference-azdata-bdc-endpoint.md), чтобы получить список с описанием каждой конечной точки и соответствующими значениями IP-адреса и порта. 

   ```bash
   azdata bdc endpoint list -o table
   ```

   Следующий список содержит пример выходных данных этой команды.

   ```output
   Description                                             Endpoint                                                   Ip              Name               Port    Protocol
   ------------------------------------------------------  ---------------------------------------------------------  --------------  -----------------  ------  ----------
   Gateway to access HDFS files, Spark                     https://11.111.111.111:30443                               11.111.111.111  gateway            30443   https
   Spark Jobs Management and Monitoring Dashboard          https://11.111.111.111:30443/gateway/default/sparkhistory  11.111.111.111  spark-history      30443   https
   Spark Diagnostics and Monitoring Dashboard              https://11.111.111.111:30443/gateway/default/yarn          11.111.111.111  yarn-ui            30443   https
   Application Proxy                                       https://11.111.111.111:30778                               11.111.111.111  app-proxy          30778   https
   Management Proxy                                        https://11.111.111.111:30777                               11.111.111.111  mgmtproxy          30777   https
   Log Search Dashboard                                    https://11.111.111.111:30777/kibana                        11.111.111.111  logsui             30777   https
   Metrics Dashboard                                       https://11.111.111.111:30777/grafana                       11.111.111.111  metricsui          30777   https
   Cluster Management Service                              https://11.111.111.111:30080                               11.111.111.111  controller         30080   https
   SQL Server Master Instance Front-End                    11.111.111.111,31433                                       11.111.111.111  sql-server-master  31433   tcp
   HDFS File System Proxy                                  https://11.111.111.111:30443/gateway/default/webhdfs/v1    11.111.111.111  webhdfs            30443   https
   Proxy for running Spark statements, jobs, applications  https://11.111.111.111:30443/gateway/default/livy/v1       11.111.111.111  livy               30443   https
   ```

Вы также можете получить все конечные точки службы, развернутые для кластера, выполнив следующую команду `kubectl`.

```bash
kubectl get svc -n <your-big-data-cluster-name>
```

## <a name="verify-the-cluster-status"></a><a id="status"></a> Проверка состояния кластера

После развертывания можно проверить состояние кластера с помощью команды [azdata bdc status show](../azdata/reference/reference-azdata-bdc-status.md).

```bash
azdata bdc status show
```

> [!TIP]
> Чтобы выполнить команды состояния, нужно сначала войти в систему с помощью команды `azdata login`, которая была приведена в предыдущем разделе о конечных точках.

Ниже приведен пример выходных данных этой команды.

```output
Bdc: ready                                                                                                                                                                                                          Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Services: ready                                                                                                                                                                                                     Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Servicename    State    Healthstatus    Details

 sql            ready    healthy         -
 hdfs           ready    healthy         -
 spark          ready    healthy         -
 control        ready    healthy         -
 gateway        ready    healthy         -
 app            ready    healthy         -


 Sql Services: ready                                                                                                                                                                                                 Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 master          ready    healthy         StatefulSet master is healthy
 compute-0       ready    healthy         StatefulSet compute-0 is healthy
 data-0          ready    healthy         StatefulSet data-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy


 Hdfs Services: ready                                                                                                                                                                                                Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 nmnode-0        ready    healthy         StatefulSet nmnode-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy
 sparkhead       ready    healthy         StatefulSet sparkhead is healthy


 Spark Services: ready                                                                                                                                                                                               Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 sparkhead       ready    healthy         StatefulSet sparkhead is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy


 Control Services: ready                                                                                                                                                                                             Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 controldb       ready    healthy         -
 control         ready    healthy         -
 metricsdc       ready    healthy         DaemonSet metricsdc is healthy
 metricsui       ready    healthy         ReplicaSet metricsui is healthy
 metricsdb       ready    healthy         StatefulSet metricsdb is healthy
 logsui          ready    healthy         ReplicaSet logsui is healthy
 logsdb          ready    healthy         StatefulSet logsdb is healthy
 mgmtproxy       ready    healthy         ReplicaSet mgmtproxy is healthy


 Gateway Services: ready                                                                                                                                                                                             Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 gateway         ready    healthy         StatefulSet gateway is healthy


 App Services: ready                                                                                                                                                                                                 Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 appproxy        ready    healthy         ReplicaSet appproxy is healthy
```

Можно также получить более подробные сведения о состоянии с помощью следующих команд.

- Команда [azdata bdc control status show](../azdata/reference/reference-azdata-bdc-control-status.md) возвращает состояние работоспособности для всех компонентов, связанных со службой управления.
```
azdata bdc control status show
```
Образец вывода:
```output
Control: ready                                                                                                                                                                                                      Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Resources: ready                                                                                                                                                                                                    Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 controldb       ready    healthy         -
 control         ready    healthy         -
 metricsdc       ready    healthy         DaemonSet metricsdc is healthy
 metricsui       ready    healthy         ReplicaSet metricsui is healthy
 metricsdb       ready    healthy         StatefulSet metricsdb is healthy
 logsui          ready    healthy         ReplicaSet logsui is healthy
 logsdb          ready    healthy         StatefulSet logsdb is healthy
 mgmtproxy       ready    healthy         ReplicaSet mgmtproxy is healthy
```

- `azdata bdc sql status show` возвращает состояние работоспособности для всех ресурсов со службой SQL Server
```
azdata bdc sql status show
```
Образец вывода:
```output
Sql: ready                                                                                                                                                                                                          Health Status:  healthy
 ===========================================================================================================================================================================================================================================
 Resources: ready                                                                                                                                                                                                    Health Status:  healthy
 -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Resourcename    State    Healthstatus    Details

 master          ready    healthy         StatefulSet master is healthy
 compute-0       ready    healthy         StatefulSet compute-0 is healthy
 data-0          ready    healthy         StatefulSet data-0 is healthy
 storage-0       ready    healthy         StatefulSet storage-0 is healthy
```

> [!IMPORTANT]
> При использовании параметра `--all` выходные данные этих команд содержат URL-адреса панелей мониторинга Kibana и Grafana для более подробного анализа.

В дополнение к использованию [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] можно также использовать Azure Data Studio для поиска конечных точек и сведений о состоянии. Дополнительные сведения о просмотре состояния кластера с помощью [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] и Azure Data Studio см. в статье [Просмотр состояния кластера больших данных](view-cluster-status.md).

## <a name="connect-to-the-cluster"></a><a id="connect"></a> Подключение к кластеру

Дополнительные сведения о подключении к кластеру больших данных см. в статье [Подключение к кластеру больших данных SQL Server с помощью Azure Data Studio](connect-to-big-data-cluster.md).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о развертывании кластера больших данных см. в следующих статьях.

- [Настройка параметров развертывания для кластеров больших данных](deployment-custom-configuration.md)
- [Выполнение автономного развертывания кластера больших данных SQL Server](deploy-offline.md)
- [Семинар. Архитектура [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] Майкрософт](https://github.com/microsoft/sqlworkshops-bdc)
- [Вопросы и ответы по кластерам больших данных](big-data-cluster-faq.yml)  
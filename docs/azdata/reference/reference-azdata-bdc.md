---
title: Справочник по azdata bdc
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 42a14ddd634ea31db608b6c68f9f3b8638984cc3
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556476"
---
# <a name="azdata-bdc"></a>azdata bdc

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata bdc create](#azdata-bdc-create) | Создание кластера больших данных.
[azdata bdc delete](#azdata-bdc-delete) | Удаление кластера больших данных.
[azdata bdc upgrade](#azdata-bdc-upgrade) | Обновление образов, развернутых в каждом контейнере в кластере больших данных SQL Server.
[azdata bdc config](reference-azdata-bdc-config.md) | Команды настройки.
[azdata bdc endpoint](reference-azdata-bdc-endpoint.md) | Команды конечной точки.
[azdata bdc debug](reference-azdata-bdc-debug.md) | Команды отладки.
[azdata bdc status](reference-azdata-bdc-status.md) | Команды состояния BDC.
[azdata bdc control](reference-azdata-bdc-control.md) | Команды службы контроля.
[azdata bdc sql](reference-azdata-bdc-sql.md) | Команды службы SQL.
[azdata bdc hdfs](reference-azdata-bdc-hdfs.md) | Команды службы HDFS.
[azdata bdc spark](reference-azdata-bdc-spark.md) | Команды службы Spark.
[azdata bdc gateway](reference-azdata-bdc-gateway.md) | Команды службы шлюза.
[azdata bdc app](reference-azdata-bdc-app.md) | Команды службы приложений.
[azdata bdc settings](reference-azdata-bdc-settings.md) | Команды параметров BDC.
[azdata bdc hdfs](reference-azdata-bdc-hdfs.md) | Модуль HDFS предоставляет команды для доступа к файловой системе HDFS.
[azdata bdc spark](reference-azdata-bdc-spark.md) | Команды Spark позволяют пользователю взаимодействовать с системой Spark, создавая сеансы, инструкции и пакеты и управляя ими.
## <a name="azdata-bdc-create"></a>azdata bdc create
Создание кластера больших данных SQL Server. В системе должна быть конфигурация Kubernetes, а также следующие переменные среды: ['AZDATA_USERNAME', 'AZDATA_PASSWORD'].
```bash
azdata bdc create [--name -n] 
                  [--config-profile -c]  
                  
[--accept-eula -a]  
                  
[--node-label -l]  
                  
[--force -f]
```
### <a name="examples"></a>Примеры
Интерактивный процесс развертывания кластера больших данных — выводятся запросы требуемых значений.
```bash
azdata bdc create
```
Развертывание BDC с аргументами и настраиваемым профилем конфигурации, инициализированным с помощью `azdata bdc config init`.
```bash
azdata bdc create --accept-eula yes --config-profile ./path/to/config/profile
```
Развертывание BDC с указанным настраиваемым именем кластера и профилем конфигурации по умолчанию aks-dev-test.
```bash
azdata bdc create --name <cluster_name> --accept-eula yes --config-profile aks-dev-test
```
Развертывание кластера больших данных с аргументами; запросы не выводятся, так как используется флаг --force.
```bash
azdata bdc create --accept-eula yes --config-profile aks-dev-test --force
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--name -n`
Имя кластера больших данных, используемого для пространств имен Kubernetes.
#### `--config-profile -c`
Профиль конфигурации кластера больших данных, используемый для развертывания кластера: ['aro-dev-test-ha', 'openshift-prod', 'aro-dev-test', 'openshift-dev-test', 'kubeadm-dev-test', 'kubeadm-prod', 'aks-dev-test-ha', 'aks-dev-test']
#### `--accept-eula -a`
Вы принимаете условия лицензии? [да/нет]. Если вы не хотите использовать этот аргумент, можно присвоить переменной среды ACCEPT_EULA значение "yes". Условия лицензии для azdata можно просмотреть по адресу https://aka.ms/eula-azdata-en.
#### `--node-label -l`
Метка узлов кластера больших данных, используемая для указания узлов, в которых будет выполняться развертывание.
#### `--force -f`
Принудительное создание; пользователь не будет получать запросы на ввод значений; все ошибки будут выводиться в рамках потока stderr.
### <a name="global-arguments"></a>Глобальные аргументы
#### `--debug`
Повышение уровня детализации журнала для включения всех журналов отладки.
#### `--help -h`
Отображение этого справочного сообщения и выход.
#### `--output -o`
Формат вывода.  Допустимые значения: json, jsonc, table, tsv.  Значение по умолчанию: json.
#### `--query -q`
Строка запроса JMESPath. Дополнительные сведения и примеры см. в разделе [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Повышение уровня детализации журнала. Чтобы включить полные журналы отладки, используйте параметр --debug.
## <a name="azdata-bdc-delete"></a>azdata bdc delete
Удаление кластера больших данных SQL Server. В системе должна быть конфигурация Kubernetes.
```bash
azdata bdc delete --name -n 
                  [--force -f]
```
### <a name="examples"></a>Примеры
Удаление BDC.
```bash
azdata bdc delete --name <cluster_name>
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--name -n`
Имя кластера больших данных, используемого для пространства имен Kubernetes.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--force -f`
Принудительное удаление кластера больших данных.
### <a name="global-arguments"></a>Глобальные аргументы
#### `--debug`
Повышение уровня детализации журнала для включения всех журналов отладки.
#### `--help -h`
Отображение этого справочного сообщения и выход.
#### `--output -o`
Формат вывода.  Допустимые значения: json, jsonc, table, tsv.  Значение по умолчанию: json.
#### `--query -q`
Строка запроса JMESPath. Дополнительные сведения и примеры см. в разделе [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Повышение уровня детализации журнала. Чтобы включить полные журналы отладки, используйте параметр --debug.
## <a name="azdata-bdc-upgrade"></a>azdata bdc upgrade
Обновление образов, развернутых в каждом контейнере в кластере больших данных SQL Server. Обновленные образы создаются на основе переданного образа Docker. Если обновленные образы не из того же репозитория образов Docker, из которого образы, развернутые в текущий момент, то необходимо указать параметр "repository".
```bash
azdata bdc upgrade --name -n 
                   --tag -t  
                   
[--repository -r]  
                   
[--controller-timeout -k]  
                   
[--stability-threshold -s]  
                   
[--component-timeout -p]  
                   
[--force -f]
```
### <a name="examples"></a>Примеры
Обновление BDC до нового образа с помощью тега "cu2" из того же репозитория.
```bash
azdata bdc upgrade -t cu2
```
Обновление BDC до новых образов с помощью тега "cu2" из нового репозитория "foo/bar/baz".
```bash
azdata bdc upgrade -t cu2 -r foo/bar/baz
```
Обновление BDC до новых образов с тегом cu2 из того же репозитория. Обновление выполняется через 30 минут после обновления контроллера и через 30 минут после обновления базы данных контроллера. Затем он ждет, пока контроллер и база данных контроллера будут работать в течение трех минут без сбоев при обновлении остальной части кластера. Каждый последующий этап обновления займет 40 минут.
```bash
azdata bdc upgrade -t cu2 --controller-timeout=30 --component-timeout=40 --stability-threshold=3
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--name -n`
Имя кластера больших данных, используемого для пространств имен Kubernetes.
#### `--tag -t`
Тег целевого образа Docker для обновления всех контейнеров в кластере.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--repository -r`
Репозиторий Docker, откуда все контейнеры в кластере должны получать свои образы.
#### `--controller-timeout -k`
Количество минут, в течение которых следует ожидать обновления базы данных контроллера или контроллера перед откатом обновления.
#### `--stability-threshold -s`
Количество минут ожидания после обновления, прежде чем оно будет отмечено как стабильное.
#### `--component-timeout -p`
Время ожидания в минутах для каждого этапа обновления (после обновления контроллера) для завершения перед приостановкой обновления.
#### `--force -f`
Если этот параметр указан, проверка работоспособности кластера перед началом обновления не производится.
### <a name="global-arguments"></a>Глобальные аргументы
#### `--debug`
Повышение уровня детализации журнала для включения всех журналов отладки.
#### `--help -h`
Отображение этого справочного сообщения и выход.
#### `--output -o`
Формат вывода.  Допустимые значения: json, jsonc, table, tsv.  Значение по умолчанию: json.
#### `--query -q`
Строка запроса JMESPath. Дополнительные сведения и примеры см. в разделе [http://jmespath.org/](http://jmespath.org).
#### `--verbose`
Повышение уровня детализации журнала. Чтобы включить полные журналы отладки, используйте параметр --debug.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md). 

Дополнительные сведения об установке средства **azdata** см. в разделе [Установка azdata](..\install\deploy-install-azdata.md).


---
title: Справка по командам azdata bdc spark settings
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc spark settings.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 66c80bc7d77ebc1f1f5c18e5fe972e4c6d61419d
ms.sourcegitcommit: 129c084add904fd3f7e9ab35a800c3fd8b1a8927
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100567320"
---
# <a name="azdata-bdc-spark-settings"></a>azdata bdc spark settings

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В этой статье приводятся справочные сведения о командах **spark settings** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды
|Команда|Описание|
| --- | --- |
[azdata bdc spark settings set](#azdata-bdc-spark-settings-set) | Задает параметры в области службы Spark.
[azdata bdc spark settings show](#azdata-bdc-spark-settings-show) | Отображает параметры в области службы Spark и при необходимости параметры Spark для указанных ресурсов.

## <a name="azdata-bdc-spark-settings-set"></a>azdata bdc spark settings set
Позволяет задать параметр в области службы или области ресурса. Укажите полное имя параметра и его значение. Параметр не применяется к работающему кластеру больших данных. Чтобы применить его, выполните обновление.
```bash
azdata bdc spark settings set --settings -s 
                        
```
### <a name="examples"></a>Примеры
Изменение числа ядер по умолчанию для драйвера службы Spark на 1 и размера памяти для драйвера на 1664m. 
```bash 
azdata bdc spark settings set --settings spark-defaults-conf.spark.driver.cores=1,spark-defaults-conf.spark.driver.memory=1664m 
``` 
Изменение числа ядер исполнителей по умолчанию на 1 для пула носителей 
```bash 
azdata bdc spark  settings set --settings spark-defaults-conf.spark.executor.cores=1 –resources storage-0 
``` 

### <a name="required-parameters"></a>Обязательные параметры
#### `--settings -s`
Задает определенное значение для указанных параметров. Если параметров несколько, они разделяются запятыми.
### <a name="optional-parameters"></a>Необязательные параметры 
#### `--resources` 
Задает определенные параметры для указанных ресурсов. Можно указать несколько ресурсов через запятую. 

### <a name="global-arguments"></a>Глобальные аргументы
#### `--debug`
Повышение уровня детализации журнала для включения всех журналов отладки.
#### `--help -h`
Отображение этого справочного сообщения и выход.
#### `--output -o`
Формат вывода.  Допустимые значения: json, jsonc, table, tsv.  Значение по умолчанию: json.
#### `--query -q`
Строка запроса JMESPath. Дополнительные сведения и примеры см. в разделе [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Повышение уровня детализации журнала. Чтобы включить полные журналы отладки, используйте параметр --debug.

## <a name="azdata-bdc-spark-settings-show"></a>azdata bdc spark settings show
Отображает параметры кластера больших данных в области службы `spark` (либо при необходимости в области ресурса). По умолчанию эта команда отображает параметры, настроенные пользователем в области службы. Доступны фильтры для отображения всех параметров (управляемых системой и настраиваемых), настраиваемых параметров или ожидающих параметров. Чтобы просмотреть определенный параметр в области службы или ресурса, укажите его имя. Чтобы просмотреть параметры всех ресурсов, входящих в службу, используйте параметр recursive. 
```bash

azdata bdc spark settings show 
[--settings -s]
[--filter-option -f]  
[--recursive -rec]
[--include-details -i]  
[--description -d]
```
### <a name="examples"></a>Примеры
Отображение настроенных пользователем параметров в области службы Spark.
```bash
azdata bdc spark settings show
```
Отображение текущего и настраиваемого значений, определяющих число ядер для драйвера Spark в пуле носителей. 
```bash
azdata bdc spark settings show --settings spark-defaults-conf.spark.driver.cores --resources storage-0 --include-details
```
Отображение любого настраиваемого параметра памяти для службы Spark.
```bash
azdata bdc spark settings show --settings *memory* --resources storage-0 
```
Отображение ожидающих изменений параметров в области службы Spark и области ресурса.
```bash
azdata bdc spark settings show --filter-options=pending --recursive --include-details
```
### <a name="optional-parameters"></a>Необязательные параметры 
#### `--filter-options | -f` 
Параметры для фильтрации отображаемых параметров уровня службы или ресурса вместо только настроенных пользователем параметров. Доступны фильтры для отображения всех параметров (управляемых системой и настраиваемых пользователями), всех настраиваемых параметров или ожидающих параметров. Доступные варианты: `userConfigured`, `all`, `pending`, `configurable`.
#### `--settings | -s` 
Отображает сведения о параметрах с указанными именами. 
#### `--include-details | -i` 
Включает дополнительные сведения о параметрах, выбранных для отображения. 
#### `--description | -d` 
Включает описание параметра. Должен использоваться с параметром --include-details. 
#### `--resources | -r` 
Отображает сведения о параметрах указанных ресурсов. Можно указать несколько ресурсов через запятую. 
#### `--recursive | -rec` 
Отображает сведения о параметрах в указанной области (службы или ресурса службы), а также параметрах всех компонентов более низкого уровня (ресурсов). 

### <a name="global-arguments"></a>Глобальные аргументы
#### `--debug`
Повышение уровня детализации журнала для включения всех журналов отладки.
#### `--help -h`
Отображение этого справочного сообщения и выход.
#### `--output -o`
Формат вывода.  Допустимые значения: json, jsonc, table, tsv.  Значение по умолчанию: json.
#### `--query -q`
Строка запроса JMESPath. Дополнительные сведения и примеры см. в разделе [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Повышение уровня детализации журнала. Чтобы включить полные журналы отладки, используйте параметр --debug.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md). 

Дополнительные сведения об установке средства **azdata** см. в статье [Установка azdata для управления кластерами больших данных SQL Server 2019](../install/deploy-install-azdata.md).

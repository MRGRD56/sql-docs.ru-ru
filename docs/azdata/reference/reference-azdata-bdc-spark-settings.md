---
title: Справка по командам azdata bdc spark settings
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc spark settings.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 157154685979cb0a99d68bb78b6348305ee916cf
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556587"
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
Позволяет задать параметр в области службы или области ресурса. Укажите полное имя параметра и его значение. Параметр не применяется к работающему кластеру больших данных. Для этого выполните команду apply.
```bash
azdata bdc spark settings set [--resources -r] 
                              [--settings -s]
```
### <a name="examples"></a>Примеры
Изменение числа ядер по умолчанию для драйвера службы Spark на 1 и размера памяти для драйвера на 1664m.
```bash
azdata bdc spark settings set --settings spark-defaults-conf.spark.driver.cores=1,spark-defaults-conf.spark.driver.memory=1664m
```
Изменение числа ядер исполнителей по умолчанию на 1 для пула носителей
```bash
azdata bdc spark settings set --settings spark-defaults-conf.spark.executor.cores=1 –resources storage-0
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--resources -r`
Задает определенные параметры для указанных ресурсов. Можно указать несколько ресурсов через запятую.
#### `--settings -s`
Задает определенное значение для указанных параметров. Если параметров несколько, они разделяются запятыми.
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
## <a name="azdata-bdc-spark-settings-show"></a>azdata bdc spark settings show
Отображает параметры кластера больших данных в области службы Spark (либо при необходимости в области ресурса). По умолчанию эта команда отображает параметры, настроенные пользователем в области службы. Доступны фильтры для отображения всех параметров (управляемых системой и настраиваемых), настраиваемых параметров или ожидающих параметров. Чтобы просмотреть определенный параметр в области службы или ресурса, укажите его имя. Чтобы просмотреть параметры всех ресурсов, входящих в службу, используйте параметр recursive.
```bash
azdata bdc spark settings show [--resources -r] 
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
### <a name="optional-parameters"></a>Необязательные параметры
#### `--resources -r`
Отображает сведения о параметрах указанных ресурсов. Можно указать несколько ресурсов через запятую.
#### `--settings -s`
Отображает сведения о параметрах с указанными именами.
#### `--filter-option -f`
Отфильтруйте отображаемые параметры области кластера, а не только параметры, настроенные пользователем. Доступны фильтры для отображения всех параметров (управляемых системой и настраиваемых пользователями), всех настраиваемых параметров или ожидающих параметров.
`userConfigured`
#### `--recursive -rec`
Отображает сведения о параметрах в указанной области (службы или ресурса службы), а также параметрах всех компонентов более низкого уровня (ресурсов).
#### `--include-details -i`
Включает дополнительные сведения о параметрах, выбранных для отображения.
#### `--description -d`
Включает описание параметра.
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

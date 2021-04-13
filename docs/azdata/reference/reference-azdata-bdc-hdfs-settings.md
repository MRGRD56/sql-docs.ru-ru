---
title: Справка по командам azdata bdc hdfs settings
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc hdfs settings.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 128035616f3cd7ea1cb53a4bc8042db59bc09e03
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556697"
---
# <a name="azdata-bdc-hdfs-settings"></a>azdata bdc hdfs settings

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В этой статье приводятся справочные сведения о командах **hdfs settings** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata bdc hdfs settings set](#azdata-bdc-hdfs-settings-set) | Задает параметры в области службы Spark.
[azdata bdc hdfs settings show](#azdata-bdc-hdfs-settings-show) | Отображает параметры в области службы HDFS и при необходимости параметры HDFS для указанных ресурсов.
## <a name="azdata-bdc-hdfs-settings-set"></a>azdata bdc hdfs settings set
Позволяет задать параметр в области службы или области ресурса. Укажите полное имя параметра и его значение. Параметр не применяется к работающему кластеру больших данных. Для этого выполните команду apply.
```bash
azdata bdc hdfs settings set [--resources -r] 
                             [--settings -s]
```
### <a name="examples"></a>Примеры
Отключение сквозного чтения тома.
```bash
azdata bdc hdfs settings set --settings hdfs-site dfs.datanode.provided.volume.readthrough=false
```
Задание коэффициента блочной репликации по умолчанию равным 3 для пула носителей.
```bash
azdata bdc hdfs settings set --settings hdfs-site.dfs.replication=3 –resources storage-0
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
## <a name="azdata-bdc-hdfs-settings-show"></a>azdata bdc hdfs settings show
Отображает параметры кластера больших данных в области службы HDFS (либо при необходимости в области ресурса). По умолчанию эта команда отображает параметры, настроенные пользователем в области службы. Доступны фильтры для отображения всех параметров (управляемых системой и настраиваемых), настраиваемых параметров или ожидающих параметров. Чтобы просмотреть определенный параметр в области службы или ресурса, укажите его имя. Чтобы просмотреть параметры всех ресурсов, входящих в службу, используйте параметр recursive.
```bash
azdata bdc hdfs settings show [--resources -r] 
                              [--settings -s]  
                              
[--filter-option -f]  
                              
[--recursive -rec]  
                              
[--include-details -i]  
                              
[--description -d]
```
### <a name="examples"></a>Примеры
Отображение настроенных пользователем параметров в области службы HDFS.
```bash
azdata bdc hdfs settings show
```
Отображение коэффициента репликации для HDFS в пуле носителей.
```bash
azdata bdc hdfs settings show --settings hdfs-site.dfs.replication --resources storage-0
```
Отображение ожидающих изменения параметров в области службы HDFS и области ресурса.
```bash
azdata bdc hdfs settings show --filter-options=pending --recursive
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

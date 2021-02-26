---
title: Справка по командам azdata bdc sql settings
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc sql settings.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 208f136f246cbcd6af612fbd4867a2a50c0f0cae
ms.sourcegitcommit: 129c084add904fd3f7e9ab35a800c3fd8b1a8927
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100567315"
---
# <a name="azdata-bdc-sql-settings"></a>azdata bdc sql settings

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В этой статье приводятся справочные сведения о командах **sql settings** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды
|Команда|Описание|
| --- | --- |
[azdata bdc sql settings set](#azdata-bdc-sql-settings-set) | Задает параметры службы SQL.
[azdata bdc sql settings show](#azdata-bdc-sql-settings-show) | Отображает параметры в области службы SQL и при необходимости параметры SQL для указанных ресурсов.

## <a name="azdata-bdc-sql-settings-set"></a>azdata bdc sql settings set
Позволяет задать параметр в области службы или области ресурса. Укажите полное имя параметра и его значение. Параметр не применяется к работающему кластеру больших данных. Чтобы применить его, выполните обновление.
```bash
azdata bdc sql settings set --settings -s 
                        
```
### <a name="examples"></a>Примеры
Включение агента SQL в главном экземпляре SQL Server.
```bash 
azdata bdc sql settings set --settings mssql.sqlagent.enabled=true --resources master 
``` 
Задание количества процессоров для SQL Server в пуле данных.
```bash 
azdata bdc sql settings set --settings mssql.numberOfCpus=10 –resources data-0 
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

## <a name="azdata-bdc-sql-settings-show"></a>azdata bdc sql settings show
Отображает параметры кластера больших данных в области службы `sql` (либо при необходимости в области ресурса). По умолчанию эта команда отображает параметры, настроенные пользователем в области службы. Доступны фильтры для отображения всех параметров (управляемых системой и настраиваемых), настраиваемых параметров или ожидающих параметров. Чтобы просмотреть определенный параметр в области службы или ресурса, укажите его имя. Чтобы просмотреть параметры всех ресурсов, входящих в службу, используйте параметр recursive. 
```bash

azdata bdc sql settings show 
[--settings -s]
[--filter-option -f]  
[--recursive -rec]
[--include-details -i]  
[--description -d]
```
### <a name="examples"></a>Примеры
Отображение настроенных пользователем параметров в области службы SQL: 
```bash
azdata bdc sql settings show
```
Отображение максимальной памяти для сервера SQL в пуле данных.
```bash
azdata bdc sql settings show --settings mssql.maxServerMemory --resources data-0 
```
Отображение ожидающих изменений параметров в области службы SQL и области ресурса.
```bash
azdata bdc sql settings show --filter-options=pending --recursive --include-details
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

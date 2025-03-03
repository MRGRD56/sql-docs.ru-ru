---
title: Справка по командам azdata bdc settings
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc settings.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 22562417eceb524bdafc9485f725712b202771e9
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557177"
---
# <a name="azdata-bdc-settings"></a>azdata bdc settings

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В этой статье приводятся справочные сведения о командах **settings** уровня кластера в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata bdc settings set](#azdata-bdc-settings-set) | Задает параметры в области кластера.
[azdata bdc settings apply](#azdata-bdc-settings-apply) | Применяет ожидающие изменения параметры к кластеру больших данных.
[azdata bdc settings cancel-apply](#azdata-bdc-settings-cancel-apply) | Отменяет параметры, примененные к кластеру больших данных.
[azdata bdc settings show](#azdata-bdc-settings-show) | Отображает параметры области кластера или все параметры при использовании параметра --recursive.
[azdata bdc settings revert](#azdata-bdc-settings-revert) | Отменяет ожидающие изменения параметры для кластера больших данных во всех областях.
## <a name="azdata-bdc-settings-set"></a>azdata bdc settings set
Задает параметр в области кластера. Укажите полное имя параметра и его значение. Чтобы применить параметр и обновить параметры кластера больших данных, выполните команду apply.
```bash
azdata bdc settings set --settings -s 
                        
```
### <a name="examples"></a>Примеры
Задание значения по умолчанию для параметра bdc.telemetry.customerFeedback.
```bash
azdata bdc settings set --settings bdc.telemetry.customerFeedback=false
```
### <a name="required-parameters"></a>Обязательные параметры
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
## <a name="azdata-bdc-settings-apply"></a>azdata bdc settings apply
Применяет ожидающие изменения параметры к кластеру больших данных.
```bash
azdata bdc settings apply [--force -f] 
                          
```
### <a name="examples"></a>Примеры
Применяет ожидающие изменения параметры к кластеру больших данных.
```bash
azdata bdc settings apply
```
Принудительное применение (пользователь не получает запросы подтверждения; все ошибки выводятся в stderr).
```bash
azdata bdc settings apply --force
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--force -f`
Принудительное применение (пользователь не получает запросы подтверждения; все ошибки выводятся в stderr).
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
## <a name="azdata-bdc-settings-cancel-apply"></a>azdata bdc settings cancel-apply
В случае сбоя при применении параметров кластер больших данных возвращается в последнее работоспособное состояние. В случае применения к работающему кластеру эта команда не действует. После отмены ранее ожидающие изменения параметров останутся в состоянии ожидания.
```bash
azdata bdc settings cancel-apply [--force -f] 
                                 
```
### <a name="examples"></a>Примеры
Отмена параметров, примененных к кластеру больших данных.
```bash
azdata bdc settings cancel-apply
```
Принудительная отмена применения (пользователь не получает запросы подтверждения; все ошибки выводятся в stderr).
```bash
azdata bdc settings cancel-apply --force
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--force -f`
Принудительная отмена применения (пользователь не получает запросы подтверждения; все ошибки выводятся в stderr).
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
## <a name="azdata-bdc-settings-show"></a>azdata bdc settings show
Выводит параметры уровня кластера больших данных. По умолчанию эта команда отображает параметры, настроенные пользователем в области кластера. Доступны и другие фильтры: для отображения всех параметров (управляемых системой, настраиваемых пользователями и унаследованных), всех настраиваемых параметров или ожидающих параметров. Чтобы просмотреть определенный параметр области кластера, укажите его имя. Чтобы просмотреть параметры во всех областях (кластера, службы и ресурса), добавьте параметр recursive.
```bash
azdata bdc settings show [--settings -s] 
                         [--filter-option -f]  
                         
[--recursive -rec]  
                         
[--include-details -i]  
                         
[--description -d]
```
### <a name="examples"></a>Примеры
Отображение состояния сбора телеметрии для кластера больших данных.
```bash
azdata bdc settings show --settings bdc.telemetry.customerFeedback
```
Отображение параметров, настроенных пользователями для кластера больших данных во всех областях.
```bash
azdata bdc settings show --recursive
```
Отображение всех ожидающих параметров для кластера больших данных во всех областях.
```bash
azdata bdc settings show –filter-option=pending --recursive
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--settings -s`
Отображает сведения о параметрах с указанными именами.
#### `--filter-option -f`
Отфильтруйте отображаемые параметры области кластера, а не только параметры, настроенные пользователем. Доступны фильтры для отображения всех параметров (управляемых системой и настраиваемых пользователями), всех настраиваемых параметров или ожидающих параметров.
`userConfigured`
#### `--recursive -rec`
Отображает сведения о параметрах в области кластера и областях более низкого уровня (службы, ресурсы).
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
## <a name="azdata-bdc-settings-revert"></a>azdata bdc settings revert
Отменяет ожидающие изменения параметры для кластера больших данных во всех областях.
```bash
azdata bdc settings revert [--force -f] 
                           
```
### <a name="examples"></a>Примеры
Отмена ожидающих изменений параметров для кластера больших данных во всех областях.
```bash
azdata bdc settings revert
```
Принудительная отмена (пользователь не получает запросы подтверждения; все ошибки выводятся в stderr).
```bash
azdata bdc settings revert --force
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--force -f`
Принудительная отмена (пользователь не получает запросы подтверждения; все ошибки выводятся в stderr).
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

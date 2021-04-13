---
title: Справка по командам azdata bdc gateway settings
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc gateway settings.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 829c099e854fca32019aee53c82e20640417bf97
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556718"
---
# <a name="azdata-bdc-gateway-settings"></a>azdata bdc gateway settings

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В этой статье приводятся справочные сведения о командах **gateway settings** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata bdc gateway settings set](#azdata-bdc-gateway-settings-set) | Задает параметры в области службы шлюза.
[azdata bdc gateway settings show](#azdata-bdc-gateway-settings-show) | Отображает параметры в области службы шлюза и при необходимости параметры шлюза для указанных ресурсов.
## <a name="azdata-bdc-gateway-settings-set"></a>azdata bdc gateway settings set
Позволяет задать параметр в области службы или области ресурса. Укажите полное имя параметра и его значение. Параметр не применяется к работающему кластеру больших данных. Для этого выполните команду apply.
```bash
azdata bdc gateway settings set [--resources -r] 
                                [--settings -s]
```
### <a name="examples"></a>Примеры
Задание времени ожидания сокета для httpclient равным 100 с для ресурса шлюза.
```bash
azdata bdc gateway settings set --settings gateway-site.gateway.httpclient.socketTimeout=100s –resources gateway
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
## <a name="azdata-bdc-gateway-settings-show"></a>azdata bdc gateway settings show
Отображает параметры кластера больших данных в области службы шлюза (либо при необходимости в области ресурса). По умолчанию эта команда отображает параметры, настроенные пользователем в области службы. Доступны фильтры для отображения всех параметров (управляемых системой и настраиваемых), настраиваемых параметров или ожидающих параметров. Чтобы просмотреть определенный параметр в области службы или ресурса, укажите его имя. Чтобы просмотреть параметры всех ресурсов, входящих в службу, используйте параметр recursive.
```bash
azdata bdc gateway settings show [--resources -r] 
                                 [--settings -s]  
                                 
[--filter-option -f]  
                                 
[--recursive -rec]  
                                 
[--include-details -i]  
                                 
[--description -d]
```
### <a name="examples"></a>Примеры
Отображение настроенных пользователем параметров в области ресурса шлюза.
```bash
azdata bdc gateway settings show --resource gateway
```
Отображение времени ожидания для шлюза.
```bash
azdata bdc gateway settings show --settings gateway-site.gateway.httpclient.socketTimeout --resources gateway
```
Отображение ожидающих изменений параметров для ресурса шлюза.
```bash
azdata bdc gateway settings show --filter-options=pending –-resource gateway --include-details
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

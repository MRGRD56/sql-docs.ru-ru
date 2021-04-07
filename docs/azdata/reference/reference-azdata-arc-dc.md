---
title: Справочник по azdata arc dc
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata arc dc.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 1f1464d84655b942f294f71af53cf55b226372c5
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557437"
---
# <a name="azdata-arc-dc"></a>azdata arc dc

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata arc dc create](#azdata-arc-dc-create) | Создание контроллера данных.
[azdata arc dc delete](#azdata-arc-dc-delete) | Удаление контроллера данных.
[azdata arc dc endpoint](reference-azdata-arc-dc-endpoint.md) | Команды конечной точки.
[azdata arc dc status](reference-azdata-arc-dc-status.md) | Команды состояния.
[azdata arc dc config](reference-azdata-arc-dc-config.md) | Команды настройки.
[azdata arc dc debug](reference-azdata-arc-dc-debug.md) | Команды отладки.
[azdata arc dc export](#azdata-arc-dc-export) | Экспорт метрик, журналов или сведений об использовании.
[azdata arc dc upload](#azdata-arc-dc-upload) | Отправка экспортированного файла данных.
## <a name="azdata-arc-dc-create"></a>azdata arc dc create
Создание контроллера данных. Необходимо иметь в системе файл kube config, а также переменные среды ['AZDATA_USERNAME', 'AZDATA_PASSWORD'].
```bash
azdata arc dc create 
```
### <a name="examples"></a>Примеры
Развертывание контроллера данных.
```bash
azdata arc dc create
```
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
## <a name="azdata-arc-dc-delete"></a>azdata arc dc delete
Удаление контроллера данных. Необходимо иметь в системе файл kube config.
```bash
azdata arc dc delete 
```
### <a name="examples"></a>Примеры
Развертывание контроллера данных.
```bash
azdata arc dc delete
```
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
## <a name="azdata-arc-dc-export"></a>azdata arc dc export
Экспорт метрик, журналов или сведений об использовании в файл.
```bash
azdata arc dc export 
```
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
## <a name="azdata-arc-dc-upload"></a>azdata arc dc upload
Отправка файла данных, экспортируемого из контроллера данных в Azure.
```bash
azdata arc dc upload 
```
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


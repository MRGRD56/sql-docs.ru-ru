---
title: Справочник по azdata arc resource-kind
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata arc resource-kind.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 5c5fb49777adcc3aa8daa99e8ac28342300d0bb7
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556855"
---
# <a name="azdata-arc-resource-kind"></a>azdata arc resource-kind

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata arc resource-kind list](#azdata-arc-resource-kind-list) | Перечисляет доступные виды настраиваемых ресурсов для Arc, которые можно определять и создавать.
[azdata arc resource-kind get](#azdata-arc-resource-kind-get) | Получает файл шаблона для вида ресурса Arc.
## <a name="azdata-arc-resource-kind-list"></a>azdata arc resource-kind list
Перечисляет доступные виды настраиваемых ресурсов для Arc, которые можно определять и создавать. После перечисления вы можете получить файл шаблона, необходимый для определения или создания настраиваемого ресурса.
```bash
azdata arc resource-kind list 
```
### <a name="examples"></a>Примеры
Пример команды для перечисления доступных видов настраиваемых ресурсов для Arc.
```bash
azdata arc resource-kind list
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
## <a name="azdata-arc-resource-kind-get"></a>azdata arc resource-kind get
Получает файл шаблона для вида ресурса Arc.
```bash
azdata arc resource-kind get 
```
### <a name="examples"></a>Примеры
Пример команды, позволяющей получить файл шаблона определения настраиваемого ресурса (CRD) для вида ресурса Arc.
```bash
azdata arc resource-kind get --kind sqldb
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


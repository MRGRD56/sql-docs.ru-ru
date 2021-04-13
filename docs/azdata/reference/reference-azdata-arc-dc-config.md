---
title: Справочник по azdata arc dc config
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata arc dc config.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ce7a88d0ca83a4edb6f02f08f8bb0b7f2d36d6f7
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557507"
---
# <a name="azdata-arc-dc-config"></a>azdata arc dc config

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata arc dc config init](#azdata-arc-dc-config-init) | Инициализирует профиль конфигурации контроллера данных, который можно использовать с командой control create.
[azdata arc dc config list](#azdata-arc-dc-config-list) | Выводит список доступных профилей конфигурации.
[azdata arc dc config add](#azdata-arc-dc-config-add) | Добавляет значение для пути JSON в файле конфигурации.
[azdata arc dc config remove](#azdata-arc-dc-config-remove) | Удаляет значение пути JSON в файле конфигурации.
[azdata arc dc config replace](#azdata-arc-dc-config-replace) | Заменяет значение пути JSON в файле конфигурации.
[azdata arc dc config patch](#azdata-arc-dc-config-patch) | Вносит исправление в файл конфигурации на основе файла исправления JSON.
## <a name="azdata-arc-dc-config-init"></a>azdata arc dc config init
Инициализирует профиль конфигурации контроллера данных, который можно использовать с командой control create. В аргументах можно указать определенный источник профиля конфигурации.
```bash
azdata arc dc config init 
```
### <a name="examples"></a>Примеры
Интерактивный процесс инициализации конфигурации контроллера данных — выводятся подсказки для ввода необходимых значений.
```bash
azdata arc dc config init
```
Команда arc dc config init с аргументами создает профиль конфигурации aks-dev-test в ./custom.
```bash
azdata arc dc config init --source azure-arc-kubeadm --path custom
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
## <a name="azdata-arc-dc-config-list"></a>azdata arc dc config list
Выводит список доступных профилей конфигурации для использования в `arc dc config init`.
```bash
azdata arc dc config list 
```
### <a name="examples"></a>Примеры
Выводит имена всех доступных профилей конфигурации.
```bash
azdata arc dc config list
```
Выводит код JSON определенного профиля конфигурации.
```bash
azdata arc dc config list --config-profile aks-dev-test
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
## <a name="azdata-arc-dc-config-add"></a>azdata arc dc config add
Добавляет значение по пути JSON в файле конфигурации.  Все приведенные ниже примеры выполняются в Bash.  Если используется другая оболочка командной строки, может потребоваться экранировать кавычки соответствующим образом.  В качестве альтернативы можно воспользоваться возможностями файла исправления.
```bash
azdata arc dc config add 
```
### <a name="examples"></a>Примеры
Пример 1. Добавление хранилища контроллера данных.
```bash
azdata arc dc config add --path custom/control.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
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
## <a name="azdata-arc-dc-config-remove"></a>azdata arc dc config remove
Удаляет значение по пути JSON в файле конфигурации.  Все приведенные ниже примеры выполняются в Bash.  Если используется другая оболочка командной строки, может потребоваться экранировать кавычки соответствующим образом.  В качестве альтернативы можно воспользоваться возможностями файла исправления.
```bash
azdata arc dc config remove 
```
### <a name="examples"></a>Примеры
Пример 1. Удаление хранилища контроллера данных.
```bash
azdata arc dc config remove --path custom/control.json --json-path ".spec.storage"
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
## <a name="azdata-arc-dc-config-replace"></a>azdata arc dc config replace
Заменяет значение по пути JSON в файле конфигурации.  Все приведенные ниже примеры выполняются в Bash.  Если используется другая оболочка командной строки, может потребоваться экранировать кавычки соответствующим образом.  В качестве альтернативы можно воспользоваться возможностями файла исправления.
```bash
azdata arc dc config replace 
```
### <a name="examples"></a>Примеры
Пример 1. Замена порта одной конечной точки (конечной точки контроллера данных).
```bash
azdata arc dc config replace --path custom/control.json --json-values "$.spec.endpoints[?(@.name=="Controller")].port=30080"
```
Пример 2. Замена хранилища контроллера данных.
```bash
azdata arc dc config replace --path custom/control.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
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
## <a name="azdata-arc-dc-config-patch"></a>azdata arc dc config patch
Вносит исправление в файл конфигурации в соответствии с указанным файлом исправления. Дополнительные сведения о том, как следует составлять пути, см. на сайте http://jsonpatch.com/. При выполнении операции замены в пути могут использоваться условные выражения посредством библиотеки jsonpath https://jsonpath.com/. Все файлы исправлений JSON должны начинаться с ключа "patch", который указывает на массив исправлений с соответствующими операциями (добавление, замена, удаление), путями и значениями. Для операции удаления не требуется значение, только путь. См. примеры ниже.
```bash
azdata arc dc config patch 
```
### <a name="examples"></a>Примеры
Пример 1. Замена порта одной конечной точки (конечной точки контроллера данных) с помощью файла исправления.
```bash
azdata arc dc config patch --path custom/control.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":"$.spec.endpoints[?(@.name=="Controller")].port","value":30080}]}
```
Пример 2. Замена хранилища контроллера данных на файл исправления.
```bash
azdata arc dc config patch --path custom/control.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":".spec.storage","value":{"accessMode":"ReadWriteMany","className":"managed-premium","size":"10Gi"}}]}
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


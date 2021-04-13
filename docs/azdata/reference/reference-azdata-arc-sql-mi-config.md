---
title: Справочник по azdata arc sql mi config
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata arc sql mi config.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: aff2a45cdd35d0bac4293670cb3e5963c7300d42
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557357"
---
# <a name="azdata-arc-sql-mi-config"></a>azdata arc sql mi config

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata arc sql mi config init](#azdata-arc-sql-mi-config-init) | Инициализирует файлы определения настраиваемого ресурса (CRD) и спецификации для управляемого экземпляра SQL.
[azdata arc sql mi config add](#azdata-arc-sql-mi-config-add) | Добавляет значение для пути JSON в файле конфигурации.
[azdata arc sql mi config remove](#azdata-arc-sql-mi-config-remove) | Удаляет значение пути JSON в файле конфигурации.
[azdata arc sql mi config replace](#azdata-arc-sql-mi-config-replace) | Заменяет значение пути JSON в файле конфигурации.
[azdata arc sql mi config patch](#azdata-arc-sql-mi-config-patch) | Вносит исправление в файл конфигурации на основе файла исправления JSON.
## <a name="azdata-arc-sql-mi-config-init"></a>azdata arc sql mi config init
Инициализирует файлы определения CRD и спецификации для управляемого экземпляра SQL.
```bash
azdata arc sql mi config init --path -p 
                              
```
### <a name="examples"></a>Примеры
Инициализирует файлы определения CRD и спецификации для управляемого экземпляра SQL.
```bash
azdata arc sql mi config init --path ./template
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--path -p`
Путь для записи определения CRD и спецификации для управляемого экземпляра SQL.
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
## <a name="azdata-arc-sql-mi-config-add"></a>azdata arc sql mi config add
Добавляет значение по пути JSON в файле конфигурации.  Все приведенные ниже примеры выполняются в Bash.  Если используется другая оболочка командной строки, может потребоваться экранировать кавычки соответствующим образом.  В качестве альтернативы можно воспользоваться возможностями файла исправления.
```bash
azdata arc sql mi config add --path -p 
                             --json-values -j
```
### <a name="examples"></a>Примеры
Пример 1. Добавление хранилища.
```bash
azdata arc sql mi config add --path custom/spec.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--path -p`
Путь к спецификации настраиваемого ресурса, например custom/spec.json.
#### `--json-values -j`
Список пар "ключ–значение" с путями JSON и значениями: ключ1.подключ1=значение1,ключ2.подключ2=значение2. Вы можете указать значения JSON в коде, например key='{"тип":"кластер","имя":"тестовый-кластер"}', или предоставить путь к файлу, например key=./values.json. При добавлении не поддерживаются условные выражения.  Если указываемое вами встроенное значение само представляет собой пару "ключ — значение" с символами "=" и ",", эти символы необходимо экранировать.  Например, key1="key2\=val2\,key3\=val3". Примеры путей см. на сайте http://jsonpatch.com/.  Для доступа к массиву необходимо указать индекс, например ключ.0=значение
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
## <a name="azdata-arc-sql-mi-config-remove"></a>azdata arc sql mi config remove
Удаляет значение по пути JSON в файле конфигурации.  Все приведенные ниже примеры выполняются в Bash.  Если используется другая оболочка командной строки, может потребоваться экранировать кавычки соответствующим образом.  В качестве альтернативы можно воспользоваться возможностями файла исправления.
```bash
azdata arc sql mi config remove --path -p 
                                --json-path -j
```
### <a name="examples"></a>Примеры
Пример 1. Удаление хранилища.
```bash
azdata arc sql mi config remove --path custom/spec.json --json-path ".spec.storage"
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--path -p`
Путь к спецификации настраиваемого ресурса, например custom/spec.json.
#### `--json-path -j`
Список путей JSON на основе библиотеки jsonpatch, в котором перечислены значения, которые необходимо удалить, например: ключ.подключ1,ключ2.подключ2. При удалении не поддерживаются условные выражения. Примеры путей см. на сайте http://jsonpatch.com/.  Для доступа к массиву необходимо указать индекс, например ключ.0=значение
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
## <a name="azdata-arc-sql-mi-config-replace"></a>azdata arc sql mi config replace
Заменяет значение по пути JSON в файле конфигурации.  Все приведенные ниже примеры выполняются в Bash.  Если используется другая оболочка командной строки, может потребоваться экранировать кавычки соответствующим образом.  В качестве альтернативы можно воспользоваться возможностями файла исправления.
```bash
azdata arc sql mi config replace --path -p 
                                 --json-values -j
```
### <a name="examples"></a>Примеры
Пример 1. Замена порта одной конечной точки.
```bash
azdata arc sql mi config replace --path custom/spec.json --json-values "$.spec.endpoints[?(@.name=="Controller")].port=30080"
```
Пример 2. Замена хранилища.
```bash
azdata arc sql mi config replace --path custom/spec.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--path -p`
Путь к спецификации настраиваемого ресурса, например custom/spec.json.
#### `--json-values -j`
Список пар "ключ–значение" с путями JSON и значениями: ключ1.подключ1=значение1,ключ2.подключ2=значение2. Вы можете указать значения JSON в коде, например key='{"тип":"кластер","имя":"тестовый-кластер"}', или предоставить путь к файлу, например key=./values.json. При замене условные выражения поддерживаются посредством библиотеки jsonpath.  Для этого путь должен начинаться с символа $. Это позволяет использовать условия, например, следующего вида: -j $.key1.key2[?(@.key3=="некотороеЗначение"].key4=value. Если указываемое вами встроенное значение само представляет собой пару "ключ — значение" с символами "=" и ",", эти символы необходимо экранировать.  Например, key1="key2\=val2\,key3\=val3". Примеры приведены ниже. Дополнительные справочные сведения см. на сайте https://jsonpath.com/.
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
## <a name="azdata-arc-sql-mi-config-patch"></a>azdata arc sql mi config patch
Вносит исправление в файл конфигурации в соответствии с указанным файлом исправления. Дополнительные сведения о том, как следует составлять пути, см. на сайте http://jsonpatch.com/. При выполнении операции замены в пути могут использоваться условные выражения посредством библиотеки jsonpath https://jsonpath.com/. Все файлы исправлений JSON должны начинаться с ключа "patch", который указывает на массив исправлений с соответствующими операциями (добавление, замена, удаление), путями и значениями. Для операции удаления не требуется значение, только путь. См. примеры ниже.
```bash
azdata arc sql mi config patch --path -p 
                               --patch-file
```
### <a name="examples"></a>Примеры
Пример 1. Замена порта одной конечной точки с помощью файла исправления.
```bash
azdata arc sql mi config patch --path custom/spec.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":"$.spec.endpoints[?(@.name=="Controller")].port","value":30080}]}
```
Пример 2. Замена хранилища с помощью файла исправления.
```bash
azdata arc sql mi config patch --path custom/spec.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":".spec.storage","value":{"accessMode":"ReadWriteMany","className":"managed-premium","size":"10Gi"}}]}
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--path -p`
Путь к спецификации настраиваемого ресурса, например custom/spec.json.
#### `--patch-file`
Путь к файлу исправления JSON на основе библиотеки jsonpatch: http://jsonpatch.com/. Файл исправления JSON должен начинаться с ключа "patch", значением которого является массив операций исправления, подлежащих выполнению. Для пути операции исправления можно использовать точечную нотацию, например ключ1.ключ2 для большинства операций. Если необходимо выполнить операцию замены и для замены значения в массиве требуется условное выражение, используйте нотацию jsonpath, указав в начале пути символ $. Это позволяет использовать условия, например, следующего вида: $.key1.key2[?(@.key3=="некотороеЗначение"].key4. См. примеры ниже. Дополнительные справочные сведения об условных выражениях см. на сайте https://jsonpath.com/.
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


---
title: Справочник по azdata app
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata app.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 5c6dfa43eb652c94e820a773c5604058f1346d6a
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557037"
---
# <a name="azdata-app"></a>azdata app

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata app template](reference-azdata-app-template.md) | Шаблоны.
[azdata app init](#azdata-app-init) | Быстрое создание основы нового приложения.
[azdata app create](#azdata-app-create) | Создание приложения.
[azdata app update](#azdata-app-update) | Обновление приложения.
[azdata app list](#azdata-app-list) | Вывод списка приложений.
[azdata app delete](#azdata-app-delete) | Удаление приложения.
[azdata app run](#azdata-app-run) | Выполнение приложения.
[azdata app describe](#azdata-app-describe) | Описание приложения.
## <a name="azdata-app-init"></a>azdata app init
Позволяет быстро создать основу нового приложения и файлы спецификаций на основе сред выполнения.
```bash
azdata app init [--spec -s] 
                [--name -n]  
                
[--version -v]  
                
[--template -t]  
                
[--destination -d]  
                
[--url -u]
```
### <a name="examples"></a>Примеры
Создание только файла `spec.yaml` приложения.
```bash
azdata app init --spec
```
Создание новой схемы приложения R на основе шаблона `r`.
```bash
azdata app init --name reduce --template r
```
Создание новой схемы приложения Python на основе шаблона `python`.
```bash
azdata app init --name reduce --template python
```
Создание новой схемы приложения SSIS на основе шаблона `ssis`.
```bash
azdata app init --name reduce --template ssis            
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--spec -s`
Создание только файла spec.yaml приложения.
#### `--name -n`
Имя приложения.
#### `--version -v`
Версия приложения.
#### `--template -t`
Имя шаблона. Чтобы получить полный список имен поддерживаемых шаблонов, выполните команду `azdata app template list`.
#### `--destination -d`
Место для размещения основы приложения. По умолчанию текущий рабочий каталог.
#### `--url -u`
Указание другого расположения репозитория шаблонов. По умолчанию: https://github.com/Microsoft/SQLBDC-AppDeploy.git
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
## <a name="azdata-app-create"></a>azdata app create
Создание приложения.
```bash
azdata app create --spec -s 
                  
```
### <a name="examples"></a>Примеры
Создание приложения из каталога, содержащего допустимую спецификацию развертывания spec.yaml.
```bash
azdata app create --spec /path/to/dir/with/spec/yaml
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--spec -s`
Путь к каталогу с YAML-файлом спецификации, описывающим приложение.
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
## <a name="azdata-app-update"></a>azdata app update
Обновление приложения.
```bash
azdata app update [--spec -s] 
                  [--yes -y]
```
### <a name="examples"></a>Примеры
Обновление существующего приложения из каталога с допустимым файлом спецификации приложения spec.yaml.
```bash
azdata app update --spec /path/to/dir/with/spec/yaml    
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--spec -s`
Путь к каталогу с YAML-файлом спецификации, описывающим приложение.
#### `--yes -y`
Не запрашивать подтверждение при обновлении приложения из файла spec.yaml CWD.
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
## <a name="azdata-app-list"></a>azdata app list
Вывод списка приложений.
```bash
azdata app list [--name -n] 
                [--version -v]
```
### <a name="examples"></a>Примеры
Вывод списка приложений по имени и версии.
```bash
azdata app list --name reduce  --version v1
```
Вывод списка всех версий приложения по имени.
```bash
azdata app list --name reduce
```
Вывод списка всех версий приложения по имени.
```bash
azdata app list
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--name -n`
Имя приложения.
#### `--version -v`
Версия приложения.
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
## <a name="azdata-app-delete"></a>azdata app delete
Удаление приложения.
```bash
azdata app delete --name -n 
                  --version -v
```
### <a name="examples"></a>Примеры
Удаление приложения по имени и версии.
```bash
azdata app delete --name reduce --version v1    
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--name -n`
Имя приложения.
#### `--version -v`
Версия приложения.
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
## <a name="azdata-app-run"></a>azdata app run
Запуск приложения.
```bash
azdata app run --name -n 
               --version -v  
               
[--inputs]
```
### <a name="examples"></a>Примеры
Запуск приложения без входных параметров.
```bash
azdata app run --name reduce --version v1
```
Запуск приложения с одним входным параметром.
```bash
azdata app run --name reduce --version v1 --inputs x=10
```
Запуск приложения с несколькими входными параметрами.
```bash
azdata app run --name reduce --version v1 --inputs x=10,y5.6    
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--name -n`
Имя приложения.
#### `--version -v`
Версия приложения.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--inputs`
Входные параметры приложения в формате CSV `name=value`.
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
## <a name="azdata-app-describe"></a>azdata app describe
Описание приложения.
```bash
azdata app describe [--spec -s] 
                    [--name -n]  
                    
[--version -v]
```
### <a name="examples"></a>Примеры
Описание приложения.
```bash
azdata app describe --name reduce --version v1    
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--spec -s`
Путь к каталогу с YAML-файлом спецификации, описывающим приложение.
#### `--name -n`
Имя приложения.
#### `--version -v`
Версия приложения.
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


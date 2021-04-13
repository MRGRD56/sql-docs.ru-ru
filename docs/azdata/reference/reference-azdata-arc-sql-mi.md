---
title: Справочник по azdata arc sql mi
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata arc sql mi.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2b6a8d15233198dbcdd3aad064e5f7668f39eaef
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556818"
---
# <a name="azdata-arc-sql-mi"></a>azdata arc sql mi

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata arc sql mi create](#azdata-arc-sql-mi-create) | Создание управляемого экземпляра SQL.
[azdata arc sql mi edit](#azdata-arc-sql-mi-edit) | Изменение конфигурации управляемого экземпляра SQL.
[azdata arc sql mi delete](#azdata-arc-sql-mi-delete) | Удаление управляемого экземпляра SQL.
[azdata arc sql mi show](#azdata-arc-sql-mi-show) | Отображение сведений об управляемом экземпляре SQL.
[azdata arc sql mi list](#azdata-arc-sql-mi-list) | Перечисление управляемых экземпляров SQL.
[azdata arc sql mi config](reference-azdata-arc-sql-mi-config.md) | Команды настройки.
## <a name="azdata-arc-sql-mi-create"></a>azdata arc sql mi create
Чтобы задать пароль для управляемого экземпляра SQL, задайте переменную среды AZDATA_PASSWORD.
```bash
azdata arc sql mi create --name -n 
                         [--path]  
                         
[--replicas]  
                         
[--cores-limit -cl]  
                         
[--cores-request -cr]  
                         
[--memory-limit -ml]  
                         
[--memory-request -mr]  
                         
[--storage-class-data -scd]  
                         
[--storage-class-logs -scl]  
                         
[--storage-class-data-logs -scdl]  
                         
[--storage-class-backups -scb]  
                         
[--volume-size-data -vsd]  
                         
[--volume-size-logs -vsl]  
                         
[--volume-size-data-logs -vsdl]  
                         
[--volume-size-backups -vsb]  
                         
[--no-external-endpoint]  
                         
[--no-wait]
```
### <a name="examples"></a>Примеры
Создание управляемого экземпляра SQL.
```bash
azdata arc sql mi create -n sqlmi1
```
Создание управляемого экземпляра SQL с тремя репликами в сценарии с высоким уровнем доступности.
```bash
azdata arc sql mi create -n sqlmi2 --replicas 3
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--name -n`
Имя управляемого экземпляра SQL.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--path`
Путь к src-файлу для файла JSON управляемого экземпляра SQL.
#### `--replicas`
Число реплик, развертываемых для обеспечения высокого уровня доступности. Допустимые значения: 3 или 1. Значение по умолчанию — 1.
#### `--cores-limit -cl`
Лимит ядер для управляемого экземпляра (целое число).
#### `--cores-request -cr`
Запрашиваемое число ядер управляемого экземпляра (целое число).
#### `--memory-limit -ml`
Лимит емкости для управляемого экземпляра (целое число).
#### `--memory-request -mr`
Запрашиваемая емкость управляемого экземпляра в виде объема памяти в ГБ (целое число).
#### `--storage-class-data -scd`
Класс хранения, используемый для данных (.mdf). Если значение не указано, класс хранения указан не будет и Kubernetes будет использовать класс по умолчанию.
#### `--storage-class-logs -scl`
Класс хранения, используемый для журналов (/var/log). Если значение не указано, класс хранения указан не будет и Kubernetes будет использовать класс по умолчанию.
#### `--storage-class-data-logs -scdl`
Класс хранения, используемый для журналов баз данных (.ldf). Если значение не указано, класс хранения указан не будет и Kubernetes будет использовать класс по умолчанию.
#### `--storage-class-backups -scb`
Класс хранения, используемый для резервных копий (/var/opt/mssql/backups). Если значение не указано, класс хранения указан не будет и Kubernetes будет использовать класс по умолчанию.
#### `--volume-size-data -vsd`
Размер тома хранилища для данных, выраженный как положительное число, за которым следует обозначение Ki (килобайт), Mi (мегабайт) или Gi (гигабайт).
#### `--volume-size-logs -vsl`
Размер тома хранилища для журналов, выраженный как положительное число, за которым следует обозначение Ki (килобайт), Mi (мегабайт) или Gi (гигабайт).
#### `--volume-size-data-logs -vsdl`
Размер тома хранилища для журналов данных, выраженный как положительное число, за которым следует обозначение Ki (килобайт), Mi (мегабайт) или Gi (гигабайт).
#### `--volume-size-backups -vsb`
Размер тома хранилища для резервных копий, выраженный как положительное число, за которым следует обозначение Ki (килобайт), Mi (мегабайт) или Gi (гигабайт).
#### `--no-external-endpoint`
Если задано, внешняя служба не создается. В противном случае внешняя служба будет создана с использованием того же типа службы, что и контроллер данных.
#### `--no-wait`
Если указано, команда не будет ожидать готового состояния экземпляра перед возвратом результата.
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
## <a name="azdata-arc-sql-mi-edit"></a>azdata arc sql mi edit
Изменение конфигурации управляемого экземпляра SQL.
```bash
azdata arc sql mi edit --name -n 
                       [--path]  
                       
[--cores-limit -cl]  
                       
[--cores-request -cr]  
                       
[--memory-limit -ml]  
                       
[--memory-request -mr]  
                       
[--no-wait]
```
### <a name="examples"></a>Примеры
Изменение конфигурации управляемого экземпляра SQL.
```bash
azdata arc sql mi edit --path ./spec.json -n sqlmi1
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--name -n`
Имя редактируемого управляемого экземпляра SQL. Изменить имя, под которым развертывается ваш экземпляр, невозможно.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--path`
Путь к src-файлу для файла JSON управляемого экземпляра SQL.
#### `--cores-limit -cl`
Лимит ядер для управляемого экземпляра (целое число).
#### `--cores-request -cr`
Запрашиваемое число ядер управляемого экземпляра (целое число).
#### `--memory-limit -ml`
Лимит емкости для управляемого экземпляра (целое число).
#### `--memory-request -mr`
Запрашиваемая емкость управляемого экземпляра в виде объема памяти в ГБ (целое число).
#### `--no-wait`
Если указано, команда не будет ожидать готового состояния экземпляра перед возвратом результата.
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
## <a name="azdata-arc-sql-mi-delete"></a>azdata arc sql mi delete
Удаление управляемого экземпляра SQL.
```bash
azdata arc sql mi delete --name -n 
                         
```
### <a name="examples"></a>Примеры
Удаление управляемого экземпляра SQL.
```bash
azdata arc sql mi delete -n sqlmi1
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--name -n`
Имя удаляемого управляемого экземпляра SQL.
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
## <a name="azdata-arc-sql-mi-show"></a>azdata arc sql mi show
Отображение сведений об управляемом экземпляре SQL.
```bash
azdata arc sql mi show --name -n 
                       [--path -p]
```
### <a name="examples"></a>Примеры
Отображение сведений об управляемом экземпляре SQL.
```bash
azdata arc sql mi show -n sqlmi1
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--name -n`
Имя отображаемого управляемого экземпляра SQL.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--path -p`
Путь для записи полной спецификации управляемого экземпляра SQL. Если не указать его, спецификация записывается в стандартный поток вывода.
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
## <a name="azdata-arc-sql-mi-list"></a>azdata arc sql mi list
Перечисление управляемых экземпляров SQL.
```bash
azdata arc sql mi list 
```
### <a name="examples"></a>Примеры
Перечисление управляемых экземпляров SQL.
```bash
azdata arc sql mi list
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


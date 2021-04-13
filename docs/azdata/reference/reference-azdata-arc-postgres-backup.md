---
title: Справочник по azdata arc postgres backup
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata arc postgres backup.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: e303d6549898084c791d7dd203533d9bd18b50cd
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556957"
---
# <a name="azdata-arc-postgres-backup"></a>azdata arc postgres backup

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata arc postgres backup create](#azdata-arc-postgres-backup-create) | Создание резервной копии группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
[azdata arc postgres backup delete](#azdata-arc-postgres-backup-delete) | Удаление резервной копии группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
[azdata arc postgres backup list](#azdata-arc-postgres-backup-list) | Получение списка резервных копий группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
[azdata arc postgres backup restore](#azdata-arc-postgres-backup-restore) | Восстановление резервной копии группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
## <a name="azdata-arc-postgres-backup-create"></a>azdata arc postgres backup create
Создание резервной копии группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
```bash
azdata arc postgres backup create --server-name -sn 
                                  [--name -n]  
                                  
[--no-wait]
```
### <a name="examples"></a>Примеры
Создает резервную копию для службы "pg".
```bash
azdata arc postgres backup create -sn pg
```
Создает именованную резервную копию для службы "pg".
```bash
azdata arc postgres backup create -sn pg -n backup1
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--server-name -sn`
Имя группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--name -n`
Имя резервной копии. Это необязательный параметр.
#### `--no-wait`
Если указан данный параметр, команда не будет ждать, пока резервное копирование завершится.
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
## <a name="azdata-arc-postgres-backup-delete"></a>azdata arc postgres backup delete
Удаление резервной копии группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
```bash
azdata arc postgres backup delete --server-name -sn 
                                  [--name -n]  
                                  
[-id]
```
### <a name="examples"></a>Примеры
Удаление резервной копии группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
```bash
azdata arc postgres backup delete -sn pg -id e07dd3940e374bd9acbc86869cbc123d
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--server-name -sn`
Имя группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--name -n`
Имя резервной копии. Этот параметр является взаимоисключающим с -id.
#### `-id`
Идентификатор удаляемой резервной копии. Этот параметр является взаимоисключающим с --name.
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
## <a name="azdata-arc-postgres-backup-list"></a>azdata arc postgres backup list
Получение списка резервных копий группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
```bash
azdata arc postgres backup list --server-name -sn 
                                
```
### <a name="examples"></a>Примеры
Получение списка резервных копий группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
```bash
azdata arc postgres backup list -sn pg
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--server-name -sn`
Имя группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
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
## <a name="azdata-arc-postgres-backup-restore"></a>azdata arc postgres backup restore
Восстановление резервной копии группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
```bash
azdata arc postgres backup restore --server-name -sn 
                                   [--backup-id -id]  
                                   
[--source-server-name -ssn]  
                                   
[--time -t]
```
### <a name="examples"></a>Примеры
Восстановление резервной копии по идентификатору
```bash
azdata arc postgres backup restore -sn pg -id 123e4567e89b12d3a456426655440000
```
Восстановление резервной копии по времени (восстановление до точки во времени)
```bash
azdata arc postgres backup restore -sn pg-dst -ssn pg-src --time "2020-11-18 17:25:34Z"
```
Восстановление резервной копии по интервалу времени (восстановление до точки во времени)
```bash
azdata arc postgres backup restore -sn pg-dst -ssn pg-src --time 1d
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--server-name -sn`
Имя группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--backup-id -id`
Идентификатор резервной копии. Если этот параметр не указан, будет восстановлена последняя резервная копия.
#### `--source-server-name -ssn`
Имя исходной группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc. Если этот параметр не указан, резервная копия будет восстановлена на месте в группе серверов, определяемой параметром --server-name.
#### `--time -t`
Момент времени, до которого нужно выполнить восстановление. Задается как метка времени или как число и суффикс (m для минут, h для часов, d для дней и w для недель). Пример: 1,5h означает 90 минут назад. Если этот параметр задан, необходимо указать --source-server-name, чтобы восстановить резервную копию из отдельной группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
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


---
title: Справочник по azdata arc postgres server backup
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata arc postgres server backup.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3f07009577054d4eaa6336a7268d5ca6b0ccc085
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556917"
---
# <a name="azdata-arc-postgres-server-backup"></a>azdata arc postgres server backup

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata arc postgres server backup create](#azdata-arc-postgres-server-backup-create) | Создает резервную копию группы серверов PostgreSQL.
[azdata arc postgres server backup delete](#azdata-arc-postgres-server-backup-delete) | Удаляет резервную копию группы серверов PostgreSQL.
[azdata arc postgres server backup restore](#azdata-arc-postgres-server-backup-restore) | Восстанавливает резервную копию группы серверов PostgreSQL.
[azdata arc postgres server backup restorestatus](#azdata-arc-postgres-server-backup-restorestatus) | Возвращает состояние последней операции восстановления, если таковая выполнялась.
[azdata arc postgres server backup show](#azdata-arc-postgres-server-backup-show) | Отображает сведения о резервной копии группы серверов PostgreSQL.
## <a name="azdata-arc-postgres-server-backup-create"></a>azdata arc postgres server backup create
Создает резервную копию группы серверов PostgreSQL.
```bash
azdata arc postgres server backup create 
```
### <a name="examples"></a>Примеры
Создает резервную копию для службы "pg".
```bash
azdata arc postgres server backup create -sn pg
```
Создает именованную резервную копию для службы "pg".
```bash
azdata arc postgres server backup create -sn pg -n backup1
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
## <a name="azdata-arc-postgres-server-backup-delete"></a>azdata arc postgres server backup delete
Удаляет резервную копию группы серверов PostgreSQL.
```bash
azdata arc postgres server backup delete 
```
### <a name="examples"></a>Примеры
Удаляет резервную копию группы серверов PostgreSQL.
```bash
azdata arc postgres backup delete -sn pg -id e07dd3940e374bd9acbc86869cbc123d
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
## <a name="azdata-arc-postgres-server-backup-restore"></a>azdata arc postgres server backup restore
Восстанавливает резервную копию группы серверов PostgreSQL.
```bash
azdata arc postgres server backup restore 
```
### <a name="examples"></a>Примеры
Восстанавливает последнюю резервную копию.
```bash
azdata arc postgres server restore -sn pg```
Restore a backup by ID
```bash
azdata arc postgres server restore -sn pg -id 123e4567e89b12d3a456426655440000
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
## <a name="azdata-arc-postgres-server-backup-restorestatus"></a>azdata arc postgres server backup restorestatus
Возвращает состояние последней операции восстановления, если таковая выполнялась.
```bash
azdata arc postgres server backup restorestatus 
```
### <a name="examples"></a>Примеры
Позволяет получить последнее состояния восстановления для службы "pg" по идентификатору.
```bash
azdata arc postgres server backup restorestatus -sn pg -id 123e4567e89b12d3a456426655440000
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
## <a name="azdata-arc-postgres-server-backup-show"></a>azdata arc postgres server backup show
Отображает сведения о резервной копии группы серверов PostgreSQL.
```bash
azdata arc postgres server backup show 
```
### <a name="examples"></a>Примеры
Позволяет получить резервную копию для службы "pg" по идентификатору.
```bash
azdata arc postgres server backup show -sn pg -id 123e4567e89b12d3a456426655440000
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



---
title: Справка по командам azdata arc sql endpoint
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata arc sql endpoint.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 13ff758f200bd675bc21c020b9c957fb286ecfd6
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556843"
---
# <a name="azdata-arc-sql-endpoint"></a>azdata arc sql endpoint

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata arc sql endpoint list](#azdata-arc-sql-endpoint-list) | Выводит список конечных точек SQL.
## <a name="azdata-arc-sql-endpoint-list"></a>azdata arc sql endpoint list
Выводит список конечных точек SQL.
```bash
azdata arc sql endpoint list [--name -n] 
                             
```
### <a name="examples"></a>Примеры
Вывод списка конечных точек для управляемого экземпляра SQL.
```bash
azdata arc sql endpoint list -n sqlmi1
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--name -n`
Имя отображаемого экземпляра SQL. Если этот параметр не указан, будут показаны все конечные точки для всех экземпляров.
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


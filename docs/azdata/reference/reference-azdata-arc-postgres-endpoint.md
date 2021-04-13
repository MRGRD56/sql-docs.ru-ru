---
title: Справочник по azdata arc postgres endpoint
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata arc postgres endpoint.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: dc94dad852cf016ac9bcb47935544df2a97eebd7
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557427"
---
# <a name="azdata-arc-postgres-endpoint"></a>azdata arc postgres endpoint

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata arc postgres endpoint list](#azdata-arc-postgres-endpoint-list) | Получение списка конечных точек группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
## <a name="azdata-arc-postgres-endpoint-list"></a>azdata arc postgres endpoint list
Получение списка конечных точек группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
```bash
azdata arc postgres endpoint list [--name -n] 
                                  []
```
### <a name="examples"></a>Примеры
Получение списка конечных точек группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
```bash
azdata arc postgres endpoint list -n postgres01
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--name -n`
Имя группы серверов с Гипермасштабированием для PostgreSQL с поддержкой Azure Arc.
#### <a name=""></a>``
--engine-version можно использовать вместе с параметром --name для обнаружения гипермасштабированной группы серверов PostgreSQL, если две группы серверов с подсистемами разных версий имеют одинаковое имя. --engine-version — необязательный параметр, который должен иметь значение 11 или 12, когда он используется для обнаружения группы серверов.
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


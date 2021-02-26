---
title: Справка по командам azdata arc sql endpoint
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata arc sql endpoint.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 190bc4360d8f8cd35f0ceda04887a1fc912243e3
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342530"
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
Строка запроса JMESPath. Дополнительные сведения и примеры см. в разделе [http://jmespath.org/](http://jmespath.org/).
#### `--verbose`
Повышение уровня детализации журнала. Чтобы включить полные журналы отладки, используйте параметр --debug.

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md). 

Дополнительные сведения об установке средства **azdata** см. в разделе [Установка azdata](..\install\deploy-install-azdata.md).

---
title: Справочник по командам azdata bdc hdfs key
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc hdfs key.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 194003f64bf3433eb590c11f3a3279049544757d
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556687"
---
# <a name="azdata-bdc-hdfs-key"></a>azdata bdc hdfs key

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata bdc hdfs key create](#azdata-bdc-hdfs-key-create) | Создает ключ HDFS.
[azdata bdc hdfs key list](#azdata-bdc-hdfs-key-list) | Выводит список всех ключей зоны шифрования Hadoop.
[azdata bdc hdfs key roll](#azdata-bdc-hdfs-key-roll) | Выполните смену ключа HDFS.
## <a name="azdata-bdc-hdfs-key-create"></a>azdata bdc hdfs key create
Создает ключ HDFS с указанным именем и размером.
```bash
azdata bdc hdfs key create --name -n 
                           [--size -size]
```
### <a name="examples"></a>Примеры
Чтобы создать 256-битный ключ с именем key1, выполните команду "azdata hdfs key create --name key1 --size 256".
```bash
azdata hdfs key create --name key1 --size 256
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--name -n`
Имя ключа зоны шифрования Hadoop. 
### <a name="optional-parameters"></a>Необязательные параметры
#### `--size -size`
Длина ключа шифрования Hadoop в битах. Длина по умолчанию — 256 бит.
`256`
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
## <a name="azdata-bdc-hdfs-key-list"></a>azdata bdc hdfs key list
Выводит список всех ключей зоны шифрования Hadoop.
```bash
azdata bdc hdfs key list 
```
### <a name="examples"></a>Примеры
Чтобы получить список всех ключей зоны шифрования Hadoop, выполните команду "azdata bdc hdfs key list".
```bash
azdata bdc hdfs key list
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
## <a name="azdata-bdc-hdfs-key-roll"></a>azdata bdc hdfs key roll
Выполняет смену ключа HDFS с указанным именем.
```bash
azdata bdc hdfs key roll --name -n 
                         
```
### <a name="examples"></a>Примеры
Чтобы сменить ключ с именем key1, выполните команду "azdata hdfs key roll --name key1".
```bash
azdata hdfs key roll --name key1
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--name -n`
Имя ключа зоны шифрования, который нужно сменить на новую версию. 
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


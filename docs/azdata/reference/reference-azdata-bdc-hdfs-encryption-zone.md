---
title: Справка по командам azdata bdc hdfs encryption-zone
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc hdfs encryption-zone.
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7c094d489cb925e280f5a44a8234d70fc817554b
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342521"
---
# <a name="azdata-bdc-hdfs-encryption-zone"></a>azdata bdc hdfs encryption-zone

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды
|Команда|Описание|
| --- | --- |
[azdata bdc hdfs encryption-zone create](#azdata-bdc-hdfs-encryption-zone-create) | Преобразует папку HDFS в зону шифрования.
[azdata bdc hdfs encryption-zone list](#azdata-bdc-hdfs-encryption-zone-list) | Выводит список всех зон шифрования и связанных с ними ключей.
[azdata bdc hdfs encryption-zone get-file-encryption-info](#azdata-bdc-hdfs-encryption-zone-get-file-encryption-info) | Получает сведения о шифровании для указанного пути к файлу HDFS.
[azdata bdc hdfs encryption-zone status](#azdata-bdc-hdfs-encryption-zone-status) | Получает состояние повторного шифрования для кластера HDFS.
[azdata bdc hdfs encryption-zone reencrypt](#azdata-bdc-hdfs-encryption-zone-reencrypt) | Управляет повторным шифрованием зоны шифрования, заданной аргументом --path.
## <a name="azdata-bdc-hdfs-encryption-zone-create"></a>azdata bdc hdfs encryption-zone create
Преобразует папку HDFS в зону шифрования, используя предоставленный ключ для шифрования файлов в зоне шифрования.
```bash
azdata bdc hdfs encryption-zone create --path -p 
                                       --keyname -k
```
### <a name="examples"></a>Примеры
Преобразование существующей папки /user/securefolder в зону шифрования с помощью ключа securelake
```bash
azdata bdc hdfs encryption-zone create --path /home/securefolder --keyname securelake
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--path -p`
Путь к папке HDFS, которая будет преобразована в зону шифрования.
#### `--keyname -k`
Ключ KMS Hadoop, используемый для защиты зоны шифрования.
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
## <a name="azdata-bdc-hdfs-encryption-zone-list"></a>azdata bdc hdfs encryption-zone list
Выводит список всех зон шифрования и связанных с ними ключей.
```bash
azdata bdc hdfs encryption-zone list 
```
### <a name="examples"></a>Примеры
Вывод списка всех зон шифрования.
```bash
azdata bdc hdfs encryption-zone list
```
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
## <a name="azdata-bdc-hdfs-encryption-zone-get-file-encryption-info"></a>azdata bdc hdfs encryption-zone get-file-encryption-info
Получает сведения о шифровании для указанного пути к файлу HDFS.
```bash
azdata bdc hdfs encryption-zone get-file-encryption-info --path -p 
                                                         
```
### <a name="examples"></a>Примеры
Получение сведений о шифровании для файла /user/securefolder/data.csv.
```bash
azdata bdc hdfs encryption-zone get-file-encryption-info --path /user/securefolder/data.csv
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--path -p`
Путь к файлу HDFS, для которого необходимо получить сведения о шифровании.
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
## <a name="azdata-bdc-hdfs-encryption-zone-status"></a>azdata bdc hdfs encryption-zone status
Получает состояние повторного шифрования для кластера HDFS.
```bash
azdata bdc hdfs encryption-zone status 
```
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
## <a name="azdata-bdc-hdfs-encryption-zone-reencrypt"></a>azdata bdc hdfs encryption-zone reencrypt
Управляет повторным шифрованием зоны шифрования, заданной аргументом --path.
```bash
azdata bdc hdfs encryption-zone reencrypt --path -p 
                                          --action -a
```
### <a name="examples"></a>Примеры
Запуск повторного шифрования для зоны шифрования securelake.
```bash
azdata bdc hdfs encryption-zone reencrypt --path /securelake --action start
```
Отмена повторного шифрования для зоны шифрования securelake.
```bash
azdata bdc hdfs encryption-zone reencrypt --path /securelake --action cancel
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--path -p`
Путь к папке зоны шифрования HDFS, которую необходимо повторно зашифровать
#### `--action -a`
Необходимое действие повторного шифрования. Допустимые значения: start и cancel.
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

---
title: Справка по azdata bdc spark batch
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc spark batch.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a233aae9ae8c7ecb144ddb3b85c8f0bd2f1c6897
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556617"
---
# <a name="azdata-bdc-spark-batch"></a>azdata bdc spark batch

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata bdc spark batch create](#azdata-bdc-spark-batch-create) | Создание пакета Spark.
[azdata bdc spark batch list](#azdata-bdc-spark-batch-list) | Вывод списка всех пакетов в Spark.
[azdata bdc spark batch info](#azdata-bdc-spark-batch-info) | Получение сведений об активном пакете Spark.
[azdata bdc spark batch log](#azdata-bdc-spark-batch-log) | Получение журналов выполнения для пакета Spark.
[azdata bdc spark batch state](#azdata-bdc-spark-batch-state) | Получение состояния выполнения для пакета Spark.
[azdata bdc spark batch delete](#azdata-bdc-spark-batch-delete) | Удаление пакета Spark.
## <a name="azdata-bdc-spark-batch-create"></a>azdata bdc spark batch create
Создает пакетное задание Spark для выполнения предоставленного кода.
```bash
azdata bdc spark batch create --file -f 
                              [--class-name -c]  
                              
[--arguments -a]  
                              
[--jar-files -j]  
                              
[--py-files -p]  
                              
[--files]  
                              
[--driver-memory]  
                              
[--driver-cores]  
                              
[--executor-memory]  
                              
[--executor-cores]  
                              
[--executor-count]  
                              
[--archives]  
                              
[--queue -q]  
                              
[--name -n]  
                              
[--config]
```
### <a name="examples"></a>Примеры
Создание пакета Spark.
```bash
azdata spark batch create --code "2+2"
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--file -f`
Путь к выполняемому файлу.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--class-name -c`
Имя класса для выполнения при передаче в одном или нескольких JAR-файлах.
#### `--arguments -a`
Список аргументов.  Передача списка значений в кодировке JSON.  Пример: "["entry1", "entry2"]".
#### `--jar-files -j`
Список путей к JAR-файлам.  Передача списка значений в кодировке JSON.  Пример: "["entry1", "entry2"]".
#### `--py-files -p`
Список путей к файлам Python.  Передача списка значений в кодировке JSON.  Пример: "["entry1", "entry2"]".
#### `--files`
Список путей к файлам.  Передача списка значений в кодировке JSON.  Пример: "["entry1", "entry2"]".
#### `--driver-memory`
Размер памяти, выделяемой драйверу.  Вместе со значением необходимо указать единицы измерения.  Пример: 512M или 2G.
#### `--driver-cores`
Количество ядер ЦП, выделяемых драйверу.
#### `--executor-memory`
Количество памяти, выделяемой исполнителю.  Вместе со значением необходимо указать единицы измерения.  Пример: 512M или 2G.
#### `--executor-cores`
Количество ядер ЦП, выделяемых исполнителю.
#### `--executor-count`
Количество запускаемых экземпляров исполнителя.
#### `--archives`
Список путей к архивам.  Передача списка значений в кодировке JSON.  Пример: "["entry1", "entry2"]".
#### `--queue -q`
Имя очереди Spark, в которой должен выполняться сеанс.
#### `--name -n`
Имя сеанса Spark.
#### `--config`
Список пар "имя–значение", содержащих значения конфигурации Spark.  Кодируется как словарь JSON.  Пример: "{"имя":"значение", "имя2":"значение2"}".
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
## <a name="azdata-bdc-spark-batch-list"></a>azdata bdc spark batch list
Вывод списка всех пакетов в Spark.
```bash
azdata bdc spark batch list 
```
### <a name="examples"></a>Примеры
Вывод списка всех активных пакетов.
```bash
azdata spark batch list
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
## <a name="azdata-bdc-spark-batch-info"></a>azdata bdc spark batch info
Возвращает сведения о пакете Spark с указанным идентификатором.  Идентификатор пакета возвращается командой spark batch create.
```bash
azdata bdc spark batch info --batch-id -i 
                            
```
### <a name="examples"></a>Примеры
Получение сведений о пакете с идентификатором 0.
```bash
azdata spark batch info --batch-id 0
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--batch-id -i`
Идентификационный номер пакета Spark.
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
## <a name="azdata-bdc-spark-batch-log"></a>azdata bdc spark batch log
Возвращает записи журнала для пакета Spark с указанным идентификатором.  Идентификатор пакета возвращается командой spark batch create.
```bash
azdata bdc spark batch log --batch-id -i 
                           
```
### <a name="examples"></a>Примеры
Получение журнала для пакета с идентификатором 0.
```bash
azdata spark batch log --batch-id 0
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--batch-id -i`
Идентификационный номер пакета Spark.
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
## <a name="azdata-bdc-spark-batch-state"></a>azdata bdc spark batch state
Возвращает состояние пакета Spark с указанным идентификатором.  Идентификатор пакета возвращается командой spark batch create.
```bash
azdata bdc spark batch state --batch-id -i 
                             
```
### <a name="examples"></a>Примеры
Получение состояния пакета с идентификатором 0.
```bash
azdata spark batch state --batch-id 0
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--batch-id -i`
Идентификационный номер пакета Spark.
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
## <a name="azdata-bdc-spark-batch-delete"></a>azdata bdc spark batch delete
Удаляет пакет Spark. Идентификатор пакета возвращается командой spark batch create.
```bash
azdata bdc spark batch delete --batch-id -i 
                              
```
### <a name="examples"></a>Примеры
Удаление пакета.
```bash
azdata spark batch delete --batch-id 0
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--batch-id -i`
Идентификационный номер пакета Spark.
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


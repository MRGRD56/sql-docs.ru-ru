---
title: Справка по azdata bdc spark session
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc spark session.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9243909d6a7399b8f9a27d86de2895356e327ba7
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557147"
---
# <a name="azdata-bdc-spark-session"></a>azdata bdc spark session

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata bdc spark session create](#azdata-bdc-spark-session-create) | Создание сеанса Spark.
[azdata bdc spark session list](#azdata-bdc-spark-session-list) | Вывод списка всех активных сеансов Spark.
[azdata bdc spark session info](#azdata-bdc-spark-session-info) | Получение сведений об активном сеансе Spark.
[azdata bdc spark session log](#azdata-bdc-spark-session-log) | Получение журналов выполнения для активного сеанса Spark.
[azdata bdc spark session state](#azdata-bdc-spark-session-state) | Получение состояния выполнения для активного сеанса Spark.
[azdata bdc spark session delete](#azdata-bdc-spark-session-delete) | Удаление сеанса Spark.
## <a name="azdata-bdc-spark-session-create"></a>azdata bdc spark session create
Создает интерактивный сеанс Spark. Вызывающая сторона должна указать тип сеанса Spark. Сеанс сохраняется по истечении времени выполнения azdata, и его необходимо удалить с помощью команды spark session delete.
```bash
azdata bdc spark session create [--session-kind -k] 
                                [--jar-files -j]  
                                
[--py-files -p]  
                                
[--files -f]  
                                
[--driver-memory]  
                                
[--driver-cores]  
                                
[--executor-memory]  
                                
[--executor-cores]  
                                
[--executor-count]  
                                
[--archives -a]  
                                
[--queue -q]  
                                
[--name -n]  
                                
[--config -c]  
                                
[--timeout-seconds -t]
```
### <a name="examples"></a>Примеры
Создание сеанса.
```bash
azdata spark session create --session-kind pyspark
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--session-kind -k`
Название типа создаваемого сеанса.  Одно из следующих значений: spark, pyspark, sparkr или sql.
#### `--jar-files -j`
Список путей к JAR-файлам.  Передача списка значений в кодировке JSON.  Пример: "["entry1", "entry2"]".
#### `--py-files -p`
Список путей к файлам Python.  Передача списка значений в кодировке JSON.  Пример: "["entry1", "entry2"]".
#### `--files -f`
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
#### `--archives -a`
Список путей к архивам.  Передача списка значений в кодировке JSON.  Пример: "["entry1", "entry2"]".
#### `--queue -q`
Имя очереди Spark, в которой должен выполняться сеанс.
#### `--name -n`
Имя сеанса Spark.
#### `--config -c`
Список пар "имя–значение", содержащих значения конфигурации Spark.  Кодируется как словарь JSON.  Пример: "{"имя":"значение", "имя2":"значение2"}".
#### `--timeout-seconds -t`
Время ожидания простоя сеанса в секундах.
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
## <a name="azdata-bdc-spark-session-list"></a>azdata bdc spark session list
Вывод списка всех активных сеансов Spark.
```bash
azdata bdc spark session list 
```
### <a name="examples"></a>Примеры
Вывод списка всех активных сеансов.
```bash
azdata spark session list
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
## <a name="azdata-bdc-spark-session-info"></a>azdata bdc spark session info
Возвращает сведения об активном сеансе Spark с указанным идентификатором.  Идентификатор сеанса возвращается командой spark session create.
```bash
azdata bdc spark session info --session-id -i 
                              
```
### <a name="examples"></a>Примеры
Получение сведений о сеансе с идентификатором 0.
```bash
azdata spark session info --session-id 0
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--session-id -i`
Идентификатор сеанса Spark.
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
## <a name="azdata-bdc-spark-session-log"></a>azdata bdc spark session log
Возвращает записи журнала для активного сеанса Spark с указанным идентификатором.  Идентификатор сеанса возвращается командой spark session create.
```bash
azdata bdc spark session log --session-id -i 
                             
```
### <a name="examples"></a>Примеры
Получение журнала для сеанса с идентификатором 0.
```bash
azdata spark session log --session-id 0
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--session-id -i`
Идентификатор сеанса Spark.
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
## <a name="azdata-bdc-spark-session-state"></a>azdata bdc spark session state
Возвращает состояние для активного сеанса Spark с указанным идентификатором.  Идентификатор сеанса возвращается командой spark session create.
```bash
azdata bdc spark session state --session-id -i 
                               
```
### <a name="examples"></a>Примеры
Получение состояния сеанса с идентификатором 0.
```bash
azdata spark session state --session-id 0
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--session-id -i`
Идентификатор сеанса Spark.
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
## <a name="azdata-bdc-spark-session-delete"></a>azdata bdc spark session delete
Удаляет интерактивный сеанс Spark. Идентификатор сеанса возвращается командой spark session create.
```bash
azdata bdc spark session delete --session-id -i 
                                
```
### <a name="examples"></a>Примеры
Удаление сеанса.
```bash
azdata spark session delete --session-id 0
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--session-id -i`
Идентификатор сеанса Spark.
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


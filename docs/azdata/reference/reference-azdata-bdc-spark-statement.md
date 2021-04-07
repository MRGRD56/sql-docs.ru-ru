---
title: Справочник по azdata bdc spark statement
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc spark statement.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 5b789129ea562c5f56850c424db01910108d2474
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557174"
---
# <a name="azdata-bdc-spark-statement"></a>azdata bdc spark statement

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata bdc spark statement list](#azdata-bdc-spark-statement-list) | Список всех инструкций в заданном сеансе Spark.
[azdata bdc spark statement create](#azdata-bdc-spark-statement-create) | Создание инструкции Spark в заданном сеансе.
[azdata bdc spark statement info](#azdata-bdc-spark-statement-info) | Получение сведений о запрошенной инструкции в заданном сеансе Spark.
[azdata bdc spark statement cancel](#azdata-bdc-spark-statement-cancel) | Отмена инструкции в рамках заданного сеанса Spark.
## <a name="azdata-bdc-spark-statement-list"></a>azdata bdc spark statement list
Список всех инструкций в заданном сеансе Spark.
```bash
azdata bdc spark statement list --session-id -i 
                                
```
### <a name="examples"></a>Примеры
Список всех инструкции сеанса.
```bash
azdata spark statement list --session-id 0
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
## <a name="azdata-bdc-spark-statement-create"></a>azdata bdc spark statement create
Создание и выполнение инструкции в заданном сеансе.  Если выполнение производится быстро, результат содержит выходные данные выполнения.  В противном случае результат можно получить с помощью команды spark session info после завершения инструкции.
```bash
azdata bdc spark statement create --session-id -i 
                                  --code -c
```
### <a name="examples"></a>Примеры
Выполнение инструкции.
```bash
azdata spark statement create --session-id 0 --code "2+2"
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--session-id -i`
Идентификатор сеанса Spark.
#### `--code -c`
Строка с кодом, выполняемым в рамках инструкции.
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
## <a name="azdata-bdc-spark-statement-info"></a>azdata bdc spark statement info
Возвращает состояние и результаты выполнения, если инструкция завершена. Идентификатор инструкции возвращается из команды spark statement create.
```bash
azdata bdc spark statement info --session-id -i 
                                --statement-id -s
```
### <a name="examples"></a>Примеры
Получение сведений об инструкции для сеанса с идентификатором 0 и идентификатора инструкции 0.
```bash
azdata spark statement info --session-id 0 --statement-id 0
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--session-id -i`
Идентификатор сеанса Spark.
#### `--statement-id -s`
Идентификатор инструкции Spark в заданном идентификаторе сеанса.
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
## <a name="azdata-bdc-spark-statement-cancel"></a>azdata bdc spark statement cancel
Отмена инструкции в рамках заданного сеанса Spark. Идентификатор инструкции возвращается из команды spark statement create.
```bash
azdata bdc spark statement cancel --session-id -i 
                                  --statement-id -s
```
### <a name="examples"></a>Примеры
Отмена инструкции.
```bash
azdata spark statement cancel --session-id 0 --statement-id 0
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--session-id -i`
Идентификатор сеанса Spark.
#### `--statement-id -s`
Идентификатор инструкции Spark в заданном идентификаторе сеанса.
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


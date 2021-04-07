---
title: Справочник по расширению azdata
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам расширения azdata.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 376be35c1fc6329d0c78d9c02bcf89d665581c2e
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556494"
---
# <a name="azdata-extension"></a>Расширение azdata

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata extension add](#azdata-extension-add) | Добавление расширения.
[azdata extension remove](#azdata-extension-remove) | Удаление расширения.
[azdata extension list](#azdata-extension-list) | Вывод списка установленных расширений.
## <a name="azdata-extension-add"></a>azdata extension add
Добавление расширения.
```bash
azdata extension add --source -s 
                     [--index]  
                     
[--pip-proxy]  
                     
[--pip-extra-index-urls]  
                     
[--yes -y]
```
### <a name="examples"></a>Примеры
Добавление расширения из URL-адреса.
```bash
azdata extension add --source https://contoso.com/some_ext-0.0.1-py2.py3-none-any.whl```
Add extension from local disk.
```bash
azdata extension add --source ~/some_ext-0.0.1-py2.py3-none-any.whl```
Add extension from local disk and use pip proxy for dependencies.
```bash
azdata extension add --source ~/some_ext-0.0.1-py2.py3-none-any.whl --pip-proxy https://user:pass@proxy.server:8080
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--source -s`
Путь к модулю расширения на диске или URL-адрес расширения
### <a name="optional-parameters"></a>Необязательные параметры
#### `--index`
Базовый URL-адрес папки пакетов Python (по умолчанию https://pypi.org/simple). Адрес должен указывать на репозиторий, совместимый со стандартом PEP 503 (простой API-интерфейс репозитория) или локальный каталог в таком же формате.
#### `--pip-proxy`
Прокси-сервер для pip, используемый для зависимостей расширений, в формате [пользователь:пароль@]прокси-сервер:порт
#### `--pip-extra-index-urls`
Разделенный пробелами список дополнительных URL-адресов используемых каталогов пакетов. Адрес должен указывать на репозиторий, совместимый со стандартом PEP 503 (простой API-интерфейс репозитория) или локальный каталог в таком же формате.
#### `--yes -y`
Не запрашивать подтверждение.
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
## <a name="azdata-extension-remove"></a>azdata extension remove
Удаление расширения.
```bash
azdata extension remove --name -n 
                        [--yes -y]
```
### <a name="examples"></a>Примеры
Удаление расширения.
```bash
azdata extension remove --name some-ext
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--name -n`
Имя расширения
### <a name="optional-parameters"></a>Необязательные параметры
#### `--yes -y`
Не запрашивать подтверждение.
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
## <a name="azdata-extension-list"></a>azdata extension list
Вывод списка установленных расширений.
```bash
azdata extension list 
```
### <a name="examples"></a>Примеры
Вывод списка расширений.
```bash
azdata extension list
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


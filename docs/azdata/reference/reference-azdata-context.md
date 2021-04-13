---
title: Справочник по контекстам azdata
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам контекста azdata.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3d7f09126ed0c3475718c8182a27b6b49b2f379b
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557067"
---
# <a name="azdata-context"></a>Контекст azdata

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata context list](#azdata-context-list) | Список доступных контекстов в профиле пользователя.
[azdata context delete](#azdata-context-delete) | Удаление контекста с заданным пространством имен из профиля пользователя.
[azdata context set](#azdata-context-set) | Установка контекста с заданным пространством имен в качестве активного контекста в профиле пользователя.
## <a name="azdata-context-list"></a>azdata context list
Можно установить или удалить любой из этих контекстов с помощью команды `azdata context set` или `azdata context delete`. Для входа в новый контекст используется команда `azdata login`.
```bash
azdata context list [--active -a] 
                    
```
### <a name="examples"></a>Примеры
Список всех доступных контекстов в профиле пользователя.
```bash
azdata context list
```
Список активных контекстов в профиле пользователя.
```bash
azdata context list --active
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--active -a`
Выводить только текущий активный контекст.
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
## <a name="azdata-context-delete"></a>azdata context delete
Если удаленный контекст был активен, пользователю нужно будет задать новый активный контекст. Для просмотра контекстов, доступных для установки или удаления, используется команда `azdata context list`. Если указано пространство имен, совпадающее с несколькими пространствами имен контекста, необходимо предоставить все параметры (--namespace --username --endpoint), чтобы уникальным образом определить удаляемый контекст.
```bash
azdata context delete --namespace -ns 
                      [--endpoint -e]  
                      
[--username -u]
```
### <a name="examples"></a>Примеры
Удаляет контекст в профиле пользователя на основе уникального пространства имен.
```bash
azdata context delete --namespace contextNamespace
```
Удаляет контекст в профиле пользователя на основе пространства имен, имени пользователя и конечной точки контроллера.
```bash
azdata context set --namespace contextNamespace --username johndoe --endpoint https://<ip or domain name>:30080
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--namespace -ns`
Пространство имен контекста, который нужно удалить.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--endpoint -e`
Конечная точка контроллера кластера "https://host:port".
#### `--username -u`
Имя пользователя учетной записи.
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
## <a name="azdata-context-set"></a>azdata context set
Для просмотра контекстов, доступных для установки, используется команда `azdata context list`. Если никакие контексты не отображаются, необходимо создать контекст в профиле пользователя, выполнив вход с помощью команды `azdata login`. То, куда выполнен вход, станет активным контекстом. При входе в несколько сущностей можно переключаться между активными контекстами с помощью этой команды. Чтобы увидеть текущий активный контекст, используется команда `azdata context list --active`. Если указано пространство имен, совпадающее с несколькими пространствами имен контекста, необходимо предоставить все параметры (--namespace --username --endpoint), чтобы уникальным образом определить задаваемый контекст.
```bash
azdata context set --namespace -ns 
                   [--endpoint -e]  
                   
[--username -u]
```
### <a name="examples"></a>Примеры
Задает активный контекст в профиле пользователя на основе уникального пространства имен.
```bash
azdata context set --namespace contextNamespace
```
Задает активный контекст в профиле пользователя на основе пространства имен, имени пользователя и конечной точки контроллера.
```bash
azdata context set --namespace contextNamespace --username johndoe --endpoint https://<ip or domain name>:30080     
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--namespace -ns`
Пространство имен контекста, который нужно установить.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--endpoint -e`
Конечная точка контроллера кластера "https://host:port".
#### `--username -u`
Имя пользователя учетной записи.
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


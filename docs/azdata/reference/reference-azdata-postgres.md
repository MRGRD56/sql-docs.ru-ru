---
title: Справочник по azdata postgres
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata postgres.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 951ad9294352418e4a43d442e5a4df6f0fd3543a
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556497"
---
# <a name="azdata-postgres"></a>azdata postgres

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata postgres shell](#azdata-postgres-shell) | Интерфейс оболочки командной строки для Postgres. См. раздел https://www.pgcli.com/.
[azdata postgres query](#azdata-postgres-query) | Эта команда запроса позволяет выполнять команды PostgreSQL в сеансе баз данных.
## <a name="azdata-postgres-shell"></a>azdata postgres shell
Интерфейс оболочки командной строки для Postgres. См. раздел https://www.pgcli.com/.
```bash
azdata postgres shell [--dbname -d] 
                      [--host]  
                      
[--port -p]  
                      
[--password -w]  
                      
[--no-password]  
                      
[--single-connection]  
                      
[--username -u]  
                      
[--pgclirc]  
                      
[--dsn]  
                      
[--list-dsn]  
                      
[--row-limit]  
                      
[--less-chatty]  
                      
[--prompt]  
                      
[--prompt-dsn]  
                      
[--list -l]  
                      
[--auto-vertical-output]  
                      
[--warn]  
                      
[--no-warn]
```
### <a name="examples"></a>Примеры
Пример командной строки для запуска интерактивного взаимодействия.
```bash
azdata postgres shell
```
Пример командной строки с указанием базы данных и пользователя.
```bash
azdata postgres shell --dbname <database> --username <username> --host <host>
```
Пример командной строки, позволяющей начать использовать полную строку подключения.
```bash
azdata postgres shell --dbname postgres://user:passw0rd@example.com:5432/master 
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--dbname -d`
Имя базы данных, к которой нужно подключиться.
#### `--host`
Адрес узла базы данных Postgres.
#### `--port -p`
Номер порта, на котором экземпляр Postgres ожидает передачи данных.
#### `--password -w`
Принудительный запрос пароля.
#### `--no-password`
Пароль никогда запрашиваться не будет.
#### `--single-connection`
Для завершений не будет использоваться отдельное подключение.
#### `--username -u`
Имя пользователя для подключения к базе данных Postgres.
#### `--pgclirc`
Расположение файла pgclirc.
#### `--dsn`
Использование имени DSN, настроенного в разделе [alias_dsn] файла pgclirc.
#### `--list-dsn`
Перечисление имен DSN, настроенных в разделе [alias_dsn] файла pgclirc.
#### `--row-limit`
Задание порога для запроса о лимите строк. Задайте 0, чтобы отключить запрос.
#### `--less-chatty`
Пропуск приветствия при запуске и прощания при выходе.
#### `--prompt`
Формат запроса (по умолчанию "\u@\h:\d> ").
#### `--prompt-dsn`
Формат запроса для подключений с использованием псевдонимов DSN (по умолчанию "\u@\h:\d> ").
#### `--list -l`
Перечисление доступных баз данных и последующий выход.
#### `--auto-vertical-output`
Автоматическое переключение на вертикальный режим вывода, если результат шире, чем ширина терминала.
#### `--warn`
Предупреждение перед выполнением запроса с разрушением.
#### `--no-warn`
Предупреждение перед выполнением запроса с разрушением.
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
## <a name="azdata-postgres-query"></a>azdata postgres query
Эта команда запроса позволяет выполнять команды PostgreSQL в сеансе баз данных.
```bash
azdata postgres query --q -q 
                      [--host]  
                      
[--dbname -d]  
                      
[--port -p]  
                      
[--username -u]
```
### <a name="examples"></a>Примеры
Перечисление всех таблиц в information_schema.
```bash
azdata postgres query --host <host> --username <username> -q "SELECT * FROM information_schema.tables"
```
### <a name="required-parameters"></a>Обязательные параметры
#### `--q -q`
Запрос PostgreSQL, который нужно выполнить.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--host`
Адрес узла базы данных Postgres.
`localhost`
#### `--dbname -d`
База данных, в которой нужно выполнить запрос.
#### `--port -p`
Номер порта, на котором экземпляр Postgres ожидает передачи данных.
`5432`
#### `--username -u`
Имя пользователя для подключения к базе данных Postgres.
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


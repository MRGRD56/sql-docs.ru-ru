---
title: Справочник по azdata sql
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata sql.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2a55bb03e6e597efe8216948b651382ff01dc50c
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106557017"
---
# <a name="azdata-sql"></a>azdata sql

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata sql shell](#azdata-sql-shell) | CLI SQL позволяет пользователям взаимодействовать с SQL Server и Azure SQL с помощью T-SQL.
[azdata sql query](#azdata-sql-query) | CLI SQL позволяет пользователям взаимодействовать с SQL Server и Azure SQL с помощью T-SQL.
## <a name="azdata-sql-shell"></a>azdata sql shell
CLI SQL позволяет пользователям взаимодействовать с SQL Server и Azure SQL с помощью T-SQL.
```bash
azdata sql shell [--username -u] 
                 [--database -d]  
                 
[--server -s]  
                 
[--integrated -e]  
                 
[--mssqlclirc]  
                 
[--row-limit]  
                 
[--less-chatty]  
                 
[--auto-vertical-output]  
                 
[--encrypt -n]  
                 
[--trust-server-certificate -c]  
                 
[--connect-timeout -l]  
                 
[--application-intent -k]  
                 
[--multi-subnet-failover -m]  
                 
[--packet-size]  
                 
[--dac-connection -a]  
                 
[--input-file -i]  
                 
[--output-file]  
                 
[--enable-sqltoolsservice-logging]  
                 
[--prompt]
```
### <a name="examples"></a>Примеры
Пример командной строки для запуска интерактивного взаимодействия.
```bash
azdata sql shell
```
Пример командной строки с использованием указанных сервера, пользователя и базы данных
```bash
azdata sql shell --server localhost --username sa --database master         
```
### <a name="optional-parameters"></a>Необязательные параметры
#### `--username -u`
Имя пользователя для подключения к базе данных.
#### `--database -d`
Имя базы данных, к которой нужно подключиться.
#### `--server -s`
Имя или адрес экземпляра SQL Server.
#### `--integrated -e`
Использование встроенной проверки подлинности в Windows.
#### `--mssqlclirc`
Расположение файла конфигурации mssqlclirc.
#### `--row-limit`
Задание порога для запроса о лимите строк. Задайте 0, чтобы отключить запрос.
#### `--less-chatty`
Пропуск приветствия при запуске и прощания при выходе.
#### `--auto-vertical-output`
Автоматическое переключение на вертикальный режим вывода, если результат шире, чем ширина окна терминала.
#### `--encrypt -n`
SQL Server использует SSL-шифрование для всех данных, если на сервере установлен сертификат.
#### `--trust-server-certificate -c`
Канал будет шифроваться при проходе по цепочке сертификатов для проверки доверия.
#### `--connect-timeout -l`
Время (в секундах) ожидания установки подключения перед завершением запроса.
#### `--application-intent -k`
Объявление типа рабочей нагрузки приложения при подключении к базе данных в группе доступности SQL Server.
#### `--multi-subnet-failover -m`
Если приложение подключается к группе доступности Always On в разных подсетях, задание этого параметра ускорит определение активного сервера и подключение к нему.
#### `--packet-size`
Размер (в байтах) сетевых пакетов, используемых для обмена данными с SQL Server.
#### `--dac-connection -a`
Подключитесь к SQL Server с помощью выделенного административного соединения.
#### `--input-file -i`
Задание файла, содержащего пакет инструкций SQL для обработки.
#### `--output-file`
Задание файла, получающего выходные данные из запроса.
#### `--enable-sqltoolsservice-logging`
Включение ведения журнала диагностики для SqlToolsService.
#### `--prompt`
Формат запроса (по умолчанию: \d>
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
## <a name="azdata-sql-query"></a>azdata sql query
CLI SQL позволяет пользователям взаимодействовать с SQL Server и Azure SQL с помощью T-SQL.
```bash
azdata sql query -q 
                 [--database -d]  
                 
[--username -u]  
                 
[--server -s]  
                 
[--integrated -e]
```
### <a name="examples"></a>Примеры
Пример командной строки для выбора списка имен таблиц.
```bash
azdata sql query --server localhost --username sa --database master -q "SELECT name FROM SYS.TABLES"
```
### <a name="required-parameters"></a>Обязательные параметры
#### `-q`
Запрос T-SQL для выполнения.
### <a name="optional-parameters"></a>Необязательные параметры
#### `--database -d`
Имя базы данных, к которой нужно подключиться.
`master`
#### `--username -u`
Имя пользователя для подключения к базе данных.
#### `--server -s`
Имя или адрес экземпляра SQL Server.
#### `--integrated -e`
Использование встроенной проверки подлинности в Windows.
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


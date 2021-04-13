---
title: Справочник по azdata bdc hdfs
titleSuffix: SQL Server big data clusters
description: Справочная статья по командам azdata bdc hdfs.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 04/06/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 27f27123d5b39efdc4c607a119858048dfc87859
ms.sourcegitcommit: 7e5414d8005e7b07e537417582fb4132b5832ded
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "106556707"
---
# <a name="azdata-bdc-hdfs"></a>azdata bdc hdfs

Применяется к [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

В следующей статье приводятся справочные сведения по командам **sql** в средстве **azdata**. Дополнительные сведения о других командах **azdata** см. в [справочнике по azdata](reference-azdata.md).

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
[azdata bdc hdfs status](reference-azdata-bdc-hdfs-status.md) | Команды состояния службы HDFS.
[azdata bdc hdfs settings](reference-azdata-bdc-hdfs-settings.md) | Команды параметров BDC для службы HDFS.
[azdata bdc hdfs encryption-zone](reference-azdata-bdc-hdfs-encryption-zone.md) | Управление зонами шифрования в HDFS.
[azdata bdc hdfs key](reference-azdata-bdc-hdfs-key.md) | Управление ключами зоны шифрования Hadoop.
[azdata bdc hdfs shell](#azdata-bdc-hdfs-shell) | Оболочка HDFS представляет собой простую интерактивную командную оболочку для файловой системы HDFS.
[azdata bdc hdfs ls](#azdata-bdc-hdfs-ls) | Перечисление состояния заданного файла или каталога.
[azdata bdc hdfs exists](#azdata-bdc-hdfs-exists) | Определение существования файла или каталога.  Возвращает значение true, если объект существует, и false в противном случае.
[azdata bdc hdfs mkdir](#azdata-bdc-hdfs-mkdir) | Создание каталога по указанному пути.
[azdata bdc hdfs mv](#azdata-bdc-hdfs-mv) | Перемещение указанного файла или пути в заданное расположение.
[azdata bdc hdfs create](#azdata-bdc-hdfs-create) | Создание текстового файла в указанном расположении.  Простое текстовое содержимое можно добавить с помощью параметра данных.
[azdata bdc hdfs cat](#azdata-bdc-hdfs-cat) | Чтение содержимого файла.  Смещение и длина в байтах являются необязательными параметрами.
[azdata bdc hdfs rm](#azdata-bdc-hdfs-rm) | Удаление файла или каталога.
[azdata bdc hdfs rmr](#azdata-bdc-hdfs-rmr) | Рекурсивное удаление файла или каталога.
[azdata bdc hdfs chmod](#azdata-bdc-hdfs-chmod) | Изменение разрешения для указанного файла или каталога.
[azdata bdc hdfs chown](#azdata-bdc-hdfs-chown) | Изменение владельца или группы для указанного файла.
[azdata bdc hdfs cp](#azdata-bdc-hdfs-cp) | Копирование файла или каталога между локальным компьютером и HDFS.
[azdata bdc hdfs mount](reference-azdata-bdc-hdfs-mount.md) | Управление подключением удаленных хранилищ в HDFS.
## <a name="azdata-bdc-hdfs-shell"></a>azdata bdc hdfs shell
Оболочка HDFS представляет собой простую интерактивную командную оболочку для файловой системы HDFS.
```bash
azdata bdc hdfs shell 
```
### <a name="examples"></a>Примеры
Запуск оболочки.
```bash
azdata bdc hdfs shell
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
## <a name="azdata-bdc-hdfs-ls"></a>azdata bdc hdfs ls
Перечисление состояния заданного файла или каталога.
```bash
azdata bdc hdfs ls --path -p 
                   
```
### <a name="examples"></a>Примеры
Перечисление состояния
```bash
azdata bdc hdfs ls --path "tmp/"
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--path -p`
Путь к перечислению состояния.
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
## <a name="azdata-bdc-hdfs-exists"></a>azdata bdc hdfs exists
Определение существования файла или каталога.  Возвращает значение true, если объект существует, и false в противном случае.
```bash
azdata bdc hdfs exists --path -p 
                       
```
### <a name="examples"></a>Примеры
Проверка существования файла или каталога.
```bash
azdata bdc hdfs exists --path "tmp/"
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--path -p`
Путь для проверки существования.
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
## <a name="azdata-bdc-hdfs-mkdir"></a>azdata bdc hdfs mkdir
Создание каталога по указанному пути.
```bash
azdata bdc hdfs mkdir --path -p 
                      
```
### <a name="examples"></a>Примеры
Создание каталога.
```bash
azdata bdc hdfs mkdir --path "tmp/"
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--path -p`
Имя создаваемого каталога.
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
## <a name="azdata-bdc-hdfs-mv"></a>azdata bdc hdfs mv
Перемещение указанного файла или пути в заданное расположение.
```bash
azdata bdc hdfs mv --source-path -s 
                   --target-path -t
```
### <a name="examples"></a>Примеры
Перемещение файла или каталога.
```bash
azdata bdc hdfs mv --source-path "tmp/" --target-path "dest/"
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--source-path -s`
Перемещаемый каталог.
#### `--target-path -t`
Расположение, куда требуется переместить.
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
## <a name="azdata-bdc-hdfs-create"></a>azdata bdc hdfs create
Создание текстового файла в указанном расположении.  Простое текстовое содержимое можно добавить с помощью параметра данных.
```bash
azdata bdc hdfs create --path -p 
                       --data -d
```
### <a name="examples"></a>Примеры
Создание файла.
```bash
azdata bdc hdfs create --path "tmp/test.txt" --data "This is a test."
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--path -p`
Имя создаваемого файла.
#### `--data -d`
Содержимое файла.  Предназначено для простого текстового содержимого.
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
## <a name="azdata-bdc-hdfs-cat"></a>azdata bdc hdfs cat
Чтение содержимого файла.  Смещение и длина в байтах являются необязательными параметрами.
```bash
azdata bdc hdfs cat --path -p 
                    --offset  
                    
--length -l
```
### <a name="examples"></a>Примеры
Чтение файла.
```bash
azdata bdc hdfs cat --path "tmp/test.txt"
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--path -p`
Имя считываемого файла.
#### `--offset`
Размер байтового смещения в считываемом файле.
#### `--length -l`
Длина считываемых данных.
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
## <a name="azdata-bdc-hdfs-rm"></a>azdata bdc hdfs rm
Удаление файла или каталога.
```bash
azdata bdc hdfs rm --path -p 
                   
```
### <a name="examples"></a>Примеры
Удаление файла или каталога.
```bash
azdata bdc hdfs rm --path "tmp/"
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--path -p`
Имя удаляемого файла.
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
## <a name="azdata-bdc-hdfs-rmr"></a>azdata bdc hdfs rmr
Рекурсивное удаление файла или каталога.
```bash
azdata bdc hdfs rmr --path -p 
                    
```
### <a name="examples"></a>Примеры
Рекурсивное удаление каталога.
```bash
azdata bdc hdfs rmr --path "tmp/"
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--path -p`
Имя рекурсивно удаляемого файла.
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
## <a name="azdata-bdc-hdfs-chmod"></a>azdata bdc hdfs chmod
Изменение разрешения для указанного файла или каталога.
```bash
azdata bdc hdfs chmod --path -p 
                      --permission
```
### <a name="examples"></a>Примеры
Изменение разрешения для файла или каталога.
```bash
azdata bdc hdfs chmod --permission 775 --path "tmp/test.txt"
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--path -p`
Имя файла или каталога, для которого нужно задать разрешения.
#### `--permission`
Задаваемый октет разрешений.  Например, "775".
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
## <a name="azdata-bdc-hdfs-chown"></a>azdata bdc hdfs chown
Изменение владельца или группы для указанного файла.
```bash
azdata bdc hdfs chown --path -p 
                      --owner  
                      
--group -g
```
### <a name="examples"></a>Примеры
Изменение владельца и группы.
```bash
azdata bdc hdfs chown --owner hdfs --group superusergroup --path "tmp/test.txt"
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--path -p`
Имя файла или каталога, для которого нужно изменить владельца.
#### `--owner`
Задаваемое имя владельца.
#### `--group -g`
Задаваемое имя группы.
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
## <a name="azdata-bdc-hdfs-cp"></a>azdata bdc hdfs cp
Копирование файла или каталога между локальным компьютером и HDFS.  Если во входных данных задан каталог, то копируется полное дерево каталога.  Если такой целевой файл или каталог существует, команда завершится ошибкой.  Чтобы указать удаленный каталог HDFS, необходимо добавить "hdfs:" в качестве префикса пути.
```bash
azdata bdc hdfs cp --from-path -f 
                   --to-path -t
```
### <a name="examples"></a>Примеры
Копирование файла или каталога между локальным компьютером и HDFS.
```bash
azdata bdc hdfs cp --from_path "tmp/test.txt" --to-path "hdfs:/user/me/test.txt"
```
### <a name="required-parameters"></a>Необходимые параметры
#### `--from-path -f`
Имя пути, из которого производится копирование.  Добавьте к пути префикс "hdfs:", чтобы указать путь HDFS.
#### `--to-path -t`
Имя пути, в который производится копирование.  Добавьте к пути префикс "hdfs:", чтобы указать путь HDFS.
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


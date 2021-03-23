---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: a79f648631ade25b94c5f0af37bd493127d92eac
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104879595"
---
## <a name="custom-installation-of-python"></a>Настраиваемая установка Python

> [!NOTE]
> Если вы установили Python 3.7 в расположение `/usr/lib/python3.7` по умолчанию, этот раздел можно пропустить и сразу перейти к разделу [Регистрация расширения языка](#register-language-extension-linux).

Если вы скомпилировали собственную версию Python 3.7, выполните приведенные ниже команды, чтобы сообщить SQL Server о настраиваемой установке.

### <a name="add-environment-variable"></a>Добавление переменной среды

Измените службу **mssql-launchpadd**, добавив переменную среды **PYTHONHOME** в файл `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`.

1. Откройте файл с помощью systemctl.

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Вставьте следующий текст в открывающийся файл `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`. Установите значение параметра **PYTHONHOME** в соответствии с путем установки настраиваемой версии Python.

    ```
    [Service]
    Environment="PYTHONHOME=<path to the python3.7 lib>"
    ```

1. Сохраните файл и закройте редактор.

Убедитесь, что удается загрузить `libpython3.7m.so.1.0`.

1. Создайте файл custom-python.conf в `/etc/ld.so.conf.d`.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-python.conf
    ```

1. В открывшийся файл добавьте путь к **libpython3.7m.so.1.0** из настраиваемой установки Python.

    ```
    <path to the python3.7 lib>
    ```

1. Сохраните новый файл и закройте редактор.

1. Запустите `ldconfig` и убедитесь, что можно загрузить `libpython3.7m.so.1.0`, выполнив следующую команду и проверив наличие всех зависимых библиотек.

    ```bash
    sudo ldconfig
    ldd <path to the python3.7 lib>/libpython3.7m.so.1.0
    ```

### <a name="grant-access-to-python-folder"></a>Предоставление доступа к папке Python

Присвойте параметру `datadirectories` в разделе "Расширяемость" файла `/var/opt/mssql/mssql.conf` значение настраиваемой установки Python.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories <path to python3.7>
```

### <a name="restart-mssql-launchpadd"></a>Перезапуск службы mssql-launchpadd

Чтобы перезапустить службу **mssql-launchpadd**, выполните приведенную ниже команду.

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="register-language-extension-linux"></a>

## <a name="register-language-extension"></a>Регистрация расширения языка

Выполните описанные ниже действия, чтобы скачать и зарегистрировать расширение языка Python, которое используется для настраиваемой среды выполнения Python.

1. Скачайте файл **python-lang-extension-linux-release.zip** из [репозитория GitHub для расширений языка SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    Для среды разработки или тестирования также можно использовать отладочную версию (**python-lang-extension-linux-debug.zip**). Отладочная версия сохраняет в журнал подробные сведения, которые помогают изучать ошибки, и не рекомендуется для рабочих сред.

1. С помощью [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) подключитесь к экземпляру SQL Server и выполните приведенную ниже команду T-SQL, чтобы зарегистрировать расширение языка Python с помощью инструкции [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md). 

    Измените путь в этой инструкции таким образом, чтобы он указывал расположение скачанного ZIP-файла с расширением языка (**python-lang-extension-linux-release.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-linux-release.zip', FILE_NAME = 'libPythonExtension.so.1.1');
    GO
    ```

    Выполните эту инструкцию для каждой базы данных, в которой вы намерены использовать расширение языка Python.

    > [!NOTE]
    > **Python** является зарезервированным словом, то есть вы не сможете указать такое имя для нового внешнего языка. Выберите другое имя. Например, в приведенной выше инструкции это имя **myPython**.

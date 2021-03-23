---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/18/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: b62528c2ef43f2d5cbf860ad50bb94b2d81dc732
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833750"
---
## <a name="custom-installation-of-r"></a>Выборочная установка R

::: zone pivot="platform-linux-ubuntu"
> [!NOTE]
> Если вы установили R в расположение по умолчанию **/usr/lib/R**, этот раздел можно пропустить и сразу перейти к разделу [Установка пакета Rcpp](#install-rcpp-package-linux).
::: zone-end

### <a name="update-the-environment-variables"></a>Обновление переменных среды

Сначала измените службу **mssql-launchpadd**, чтобы добавить переменную среды **R_HOME** в файл `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`.

1. Откройте файл с помощью systemctl.

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Вставьте следующий текст в открывающийся файл `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`. Установите значение параметра **R_HOME** в соответствии с настраиваемым путем установки R.

    ```text
    [Service]
    Environment="R_HOME=<path to R>"
    ```

1. Сохраните и закройте файл.

Убедитесь, что удается загрузить `libR.so`.

1. Создайте файл custom-r.conf в **/etc/ld.so.conf.d**.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-r.conf
    ```

1. В открывшийся файл добавьте путь к **libR.so** из настраиваемой установки R.

    ```
    <path to the R lib>
    ```

1. Сохраните новый файл и закройте редактор.

1. Запустите `ldconfig` и убедитесь, что можно загрузить **libR.so**, выполнив следующую команду и проверив наличие всех зависимых библиотек.

    ```bash
    sudo ldconfig
    ldd <path to the R lib>/libR.so
    ```

### <a name="grant-access-to-the-custom-r-installation-folder"></a>Предоставление доступа к настраиваемой папке установки R

Присвойте параметру `datadirectories` в разделе "Расширяемость" файла `/var/opt/mssql/mssql.conf` значение настраиваемой установки R.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories <path to R>
```

### <a name="restart-mssql-launchpadd-service"></a>Перезапуск службы mssql-launchpadd

Чтобы перезапустить службу **mssql-launchpadd**, выполните приведенную ниже команду.

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="install-rcpp-package-linux"></a>

## <a name="install-rcpp-package"></a>Установка пакета Rcpp

Чтобы установить пакет **Rcpp**, выполните указанные ниже действия.

1. Запустите R из оболочки:

    ```bash
    sudo ${R_HOME}/bin/R
    ```

1. Чтобы установить пакет Rcpp в папке ${R_HOME}\library, выполните приведенный ниже скрипт.

  ```R
  install.packages("Rcpp", lib = "${R_HOME}/library");
  ```

## <a name="register-language-extension"></a>Регистрация расширения языка

Выполните описанные ниже действия, чтобы скачать и зарегистрировать расширение языка R, которое используется для настраиваемой среды выполнения R.

1. Скачайте файл **R-lang-extension-linux-release.zip** из [репозитория GitHub для расширений языка SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    Для среды разработки или тестирования также можно использовать отладочную версию (**R-lang-extension-linux-debug.zip**). Отладочная версия сохраняет в журнал подробные сведения, которые помогают изучать ошибки, и не рекомендуется для рабочих сред.

1. С помощью [Azure Data Studio](../../../azure-data-studio/what-is-azure-data-studio.md) подключитесь к экземпляру SQL Server и выполните приведенную ниже команду T-SQL, чтобы зарегистрировать расширение языка R с помощью инструкции [CREATE EXTERNAL LANGUAGE](../../../t-sql/statements/create-external-language-transact-sql.md). 

    Измените путь в этой инструкции таким образом, чтобы он указывал расположение скачанного ZIP-файла с расширением языка (**R-lang-extension-linux-release.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myR]
    FROM (CONTENT = N'/path/to/R-lang-extension-linux-release.zip', FILE_NAME = 'libRExtension.so.1.1');
    GO
    ```

    Выполните эту инструкцию для каждой базы данных, в которой вы намерены использовать расширение языка R.

    > [!NOTE]
    > **R** является зарезервированным словом, то есть вы не сможете указать такое имя для нового внешнего языка. Выберите другое имя. Например, в приведенной выше инструкции это имя **myR**.

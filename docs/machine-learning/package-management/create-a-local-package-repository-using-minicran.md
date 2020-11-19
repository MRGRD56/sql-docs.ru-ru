---
title: Создание репозитория с помощью miniCRAN
titleSuffix: SQL machine learning
description: Узнайте, как установить пакеты R в автономном режиме с помощью пакета miniCRAN, чтобы создать локальный репозиторий пакетов и зависимостей.
ms.prod: sql
ms.technology: machine-learning
ms.date: 11/20/2019
ms.topic: how-to
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: 8a23b12f1cd42a1c6f67a09708481134d8d893d4
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870472"
---
# <a name="create-a-local-r-package-repository-using-minicran"></a>Создание локального репозитория пакетов R с помощью miniCRAN
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

В этой статье рассказано, как установить пакеты R в автономном режиме с помощью пакета [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html), чтобы создать локальный репозиторий пакетов и зависимостей. Пакет **miniCRAN** определяет и скачивает пакеты и зависимости в одну папку, которая копируется на другие компьютеры для автономной установки пакетов R.

Можно указать один пакет или несколько; **miniCRAN** рекурсивно считывает дерево зависимостей для этих пакетов. Затем он скачивает только перечисленные пакеты и их зависимости из CRAN или аналогичных репозиториев.

По завершении **miniCRAN** создает согласованный внутри себя репозиторий, состоящий из выбранных пакетов и всех необходимых зависимостей. Этот локальный репозиторий можно переместить на сервер и перейти к установке пакетов без подключения к Интернету.

Опытные пользователи R часто ищут список пакетов-зависимостей в файле DESCRIPTION скачанного пакета. Однако пакеты, перечисленные в разделе **Imports**, могут иметь зависимости второго уровня. По этой причине рекомендуется использовать **miniCRAN** для формирования полного набора необходимых пакетов.

## <a name="why-create-a-local-repository"></a>Цель создания локального репозитория

Цель создания локального репозитория пакетов — формирование единого расположения, которое администратор сервера или другие пользователи организации (особенно те, у которых нет доступа к Интернету) могут использовать для установки новых пакетов R на сервере. После создания репозитория его можно изменять, добавляя новые пакеты или обновляя существующие.

Репозитории пакетов полезны в следующих сценариях:

- **Безопасность** — Многие пользователи R привыкли произвольно скачивать и устанавливать новые пакеты R из CRAN или одного из его зеркальных сайтов. Однако в целях безопасности рабочие серверы, где работает [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], обычно не имеют подключения к Интернету.

- **Упрощенная автономная установка**. Для установки пакета на автономном сервере необходимо также скачать все пакеты-зависимости. Использование miniCRAN упрощает получение всех зависимостей в правильном формате и позволяет избежать ошибок с зависимостями.

- **Улучшенное управление версиями**. В многопользовательской среде существуют веские причины избегать неограниченной установки разных версий пакетов на сервере. Используйте локальный репозиторий, чтобы обеспечить согласованный набор пакетов для пользователей.

## <a name="install-minicran"></a>Установка miniCRAN

Сам пакет **miniCRAN** зависит от 18 других пакетов CRAN, среди которых есть пакет **RCurl**, имеющий системную зависимость от пакета **curl-devel**. Аналогично, пакет **XML** зависит от **libxml2-devel**. Для разрешения зависимостей рекомендуется сначала создать локальный репозиторий на компьютере с полным доступом к Интернету.

Выполните следующие команды на компьютере с базовым пакетом R, инструментами R и подключением к Интернету. Предполагается, что это не компьютер SQL Server. Следующие команды устанавливают пакеты **miniCRAN** и **igraph**. В этом примере проверяется, установлен ли пакет, но можно обойти инструкции `if` и установить пакеты напрямую.

```R
if(!require("miniCRAN")) install.packages("miniCRAN") 
if(!require("igraph")) install.packages("igraph") 
library("miniCRAN")
```

## <a name="set-the-cran-mirror-and-mran-snapshot"></a>Настройка зеркального отображения CRAN и моментального снимка MRAN

Укажите зеркальный сайт, который будет использоваться при получении пакетов. Например, можно использовать сайт MRAN или любой другой сайт в вашем регионе, который содержит необходимые пакеты. Если скачивание завершается ошибкой, попробуйте другой зеркальный сайт.

```R
CRAN_mirror <- c(CRAN = "https://cran.cnr.berkeley.edu")
```

## <a name="create-a-local-folder"></a>Создание локальной папки

Создайте локальную папку для хранения собранных пакетов. Если это действие повторяется часто, стоит использовать описательное имя, например "miniCRANZooPackages" или "miniCRANMyRPackageV2".

Укажите папку в качестве локального репозитория. Синтаксис R использует прямую косую черту для имен путей, что противоречит соглашениям, используемым в Windows.

```R
local_repo <- "C:/miniCRANZooPackages"
```

## <a name="add-packages-to-the-local-repo"></a>Добавление пакетов в локальный репозиторий

После установки и загрузки **miniCRAN** создайте список, указывающий дополнительные пакеты, которые требуется скачать.

**Не стоит** добавлять зависимости в этот начальный список. Пакет **igraph**, используемый **miniCRAN**, автоматически создает список зависимостей. Дополнительные сведения об использовании созданного графа зависимостей см. в разделе [Использование miniCRAN для указания зависимостей пакетов](https://cran.r-project.org/web/packages/miniCRAN/vignettes/miniCRAN-dependency-graph.html).

1. Добавьте целевые пакеты "zoo" и "forecast" в переменную.

    ```R
    pkgs_needed <- c("zoo", "forecast")
    ```

2. При необходимости можно построить график зависимостей. Это необязательно, но может быть информативно.

    ```R
    plot(makeDepGraph(pkgs_needed))
    ```

3. Создайте локальный репозиторий. При необходимости измените версию R на версию, установленную на экземпляре SQL Server. Если вы обновляли компоненты, ваша версия может быть новее исходной. Дополнительные сведения см. в статье [Получение сведений о пакете R](../package-management/r-package-information.md).

    ```R
    pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);
    makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.3");
    ```

   На основе этих сведений пакет miniCRAN создает структуру папок, необходимую для последующего копирования пакетов в [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)].

На этом этапе у вас должна быть папка, содержащая необходимые пакеты, а также все необходимые дополнительные пакеты. Папка должна содержать коллекцию архивированных пакетов. Не распаковывайте пакеты и не переименовывайте файлы.

При необходимости выполните следующий код, чтобы вывести список пакетов в локальном репозитории miniCRAN.

```R
pdb <- as.data.frame(pkgAvail(local_repo, type = "win.binary", Rversion = "3.3"), stringsAsFactors = FALSE);
head(pdb);
pdb$Package;
pdb[, c("Package", "Version", "License")]
```

## <a name="add-packages-to-the-instance-library"></a>Добавление пакетов в библиотеку экземпляров

После создания локального репозитория с нужными пакетами переместите репозиторий пакетов на компьютер SQL Server. В следующей процедуре описывается установка пакетов с помощью средств R.

::: moniker range=">sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions"
> [!NOTE]
> Для установки пакетов рекомендуется использовать **sqlmlutils**. См. статью [Установка новых пакетов R с помощью sqlmlutils](install-additional-r-packages-on-sql-server.md).
::: moniker-end

1. Целиком скопируйте папку, содержащую репозиторий miniCRAN, на сервер, на котором планируется устанавливать пакеты. Папка обычно имеет следующую структуру: 

   `<miniCRAN root>/bin/windows/contrib/version/<all packages>`

   В этой процедуре мы предполагаем, что папка находится на корневом диске.

2. Откройте средство R, связанное с экземпляром (например, можно использовать RGUI. exe). Щелкните правой кнопкой мыши и выберите команду **Запуск от имени администратора**, чтобы разрешить средству вносить обновления в систему.

   ::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
   - Например, расположением файлов по умолчанию для RGUI является `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

   ::: moniker range"=sql-server-2017||=sqlallproducts-allversions"
   - Например, расположением файлов для RGUI является `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

   ::: moniker range=">sql-server-2017||=sqlallproducts-allversions"
   - Например, расположением файлов для RGUI является `C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\bin\x64`.
   ::: moniker-end

3. Получите путь к библиотеке экземпляров и добавьте его в список путей к библиотекам.

   ::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
   Например,

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL13.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

   ::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
   Например,

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL14.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

   ::: moniker range=">sql-server-2017||=sqlallproducts-allversions"
   Например,

   ```R
   outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL15.MSSQLSERVER/R_SERVICES/library"
   ```

   ::: moniker-end

4. Укажите новое расположение на сервере, куда вы скопировали репозиторий **miniCRAN**, в качестве `server_repo`.

    В этом примере предполагается, что репозиторий скопирован во временную папку на сервере.

    ```R
    inputlib <- "C:/miniCRANZooPackages"
    ```

5. Так как вы работаете в новой рабочей области R на сервере, необходимо также добавить список пакетов для установки.

    ```R
    mypackages <- c("zoo", "forecast")
    ```

6. Установите пакеты, указав путь к локальной копии репозитория miniCRAN.

    ```R
    install.packages(mypackages, repos = file.path("file://", normalizePath(inputlib, winslash = "/")), lib = outputlib, type = "win.binary", dependencies = TRUE);
    ```

7. Из библиотеки экземпляров можно просмотреть установленные пакеты с помощью команды наподобие следующей:

    ```R
    installed.packages()
    ```

## <a name="see-also"></a>См. также раздел

+ [Получение сведений о пакете R](../package-management/r-package-information.md)
+ [Учебники по R](../tutorials/r-tutorials.md)
---
title: 'Ubuntu: Установка SQL Server в Linux'
description: В этом кратком руководстве рассказывается, как установить SQL Server 2017 или SQL Server 2019 в Ubuntu, а затем создать и запросить базу данных с помощью средства sqlcmd.
author: VanMSFT
ms.author: vanto
ms.date: 04/07/2021
ms.topic: conceptual
ms.prod: sql
ms.custom: seo-lt-2019
ms.technology: linux
ms.assetid: 31c8c92e-12fe-4728-9b95-4bc028250d85
ms.openlocfilehash: 4d96b75d35af95f646ba41864ee9df846642338f
ms.sourcegitcommit: d8cbbeffa3faa110e02056ff97dc7102b400ffb3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2021
ms.locfileid: "107003920"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-ubuntu"></a>Краткое руководство. Установка SQL Server и создание базы данных в Ubuntu
[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]


<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

В этом кратком руководстве вы установите SQL Server 2017 в Ubuntu 16.04/18.04. Затем вы подключитесь с помощью **sqlcmd** для создания первой базы данных и выполнения запросов.

> [!TIP]
> Для выполнения этого руководства требуется ввод данных пользователем и подключение к Интернету. Если вас интересуют процедуры автоматической или автономной установки, см. [руководство по установке SQL Server на Linux](sql-server-linux-setup.md). Список поддерживаемых платформ см. в [заметках о выпуске](sql-server-linux-release-notes.md).

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

В этом кратком руководстве показано, как установить SQL Server 2019 в Ubuntu 16.04, 18.04 или 20.04. Затем вы подключитесь с помощью **sqlcmd** для создания первой базы данных и выполнения запросов.

> [!NOTE]
> Ubuntu 20.04 поддерживается, начиная с SQL Server 2019 с накопительным пакетом обновления 20 (CU10).

> [!TIP]
> Для выполнения этого руководства требуется ввод данных пользователем и подключение к Интернету. Если вас интересуют процедуры автоматической или автономной установки, см. [руководство по установке SQL Server на Linux](sql-server-linux-setup.md). Список поддерживаемых платформ см. в [заметках о выпуске](sql-server-linux-release-notes-2019.md).

::: moniker-end

## <a name="prerequisites"></a>Предварительные требования

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Требуется компьютер, на котором установлена ОС Ubuntu 16.04 или 18.04 и имеется **по крайней мере 2 ГБ** памяти.

Чтобы установить Ubuntu 18.04 на собственный компьютер, перейдите на страницу <http://releases.ubuntu.com/bionic/>. Можно также создать виртуальные машины Ubuntu в Azure. См. статью [Создание виртуальных машин Linux и управление ими с помощью Azure CLI](/azure/virtual-machines/linux/tutorial-manage-vm).

> [!NOTE]
> В настоящее время [подсистема Windows для Linux](/windows/wsl/about) для Windows 10 не поддерживается в качестве цели установки.

Сведения о других требованиях к системе см. в статье [Требования к системе для SQL Server на Linux](sql-server-linux-setup.md#system).

> [!NOTE]
> Ubuntu 18.04 поддерживается, начиная с SQL Server 2017 с накопительным пакетом обновления 20 (CU20). Если вы хотите использовать инструкции, приведенные в этой статье, с Ubuntu 18.04, убедитесь, что используется правильный [путь к репозиторию](sql-server-linux-change-repo.md) `18.04` вместо `16.04`.
>
> Если вы используете SQL Server с более ранней версией, конфигурация возможна с [изменениями](/archive/blogs/sql_server_team/installing-sql-server-2017-for-linux-on-ubuntu-18-04-lts).

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Требуется компьютер с установленной версией Ubuntu 16.04, 18.04 или 20.04 и **минимум 2 ГБ** памяти.

Чтобы установить Ubuntu 20.04 на свой компьютер, перейдите на страницу <https://releases.ubuntu.com/20.04/>. Можно также создать виртуальные машины Ubuntu в Azure. См. статью [Создание виртуальных машин Linux и управление ими с помощью Azure CLI](/azure/virtual-machines/linux/tutorial-manage-vm).

> [!NOTE]
> В настоящее время [подсистема Windows для Linux](/windows/wsl/about) для Windows 10 не поддерживается в качестве цели установки.

Сведения о других требованиях к системе см. в статье [Требования к системе для SQL Server на Linux](sql-server-linux-setup.md#system).

::: moniker-end

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server"></a><a id="install"></a>Установка SQL Server

> [!NOTE]
> Следующие команды для SQL Server 2017 ссылаются на репозиторий Ubuntu 18.04. Если вы используете Ubuntu 16.04, в приведенном ниже пути замените `/ubuntu/18.04/` на `/ubuntu/16.04/`.

Чтобы настроить SQL Server в Ubuntu, выполните следующие команды в терминале для установки пакета **mssql-server**:

1. Импортируйте открытые ключи GPG из репозитория:

   ```bash
   wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Зарегистрируйте репозиторий Ubuntu для Microsoft SQL Server:
   
   Для Ubuntu 16.04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"
   ```
   
   Для Ubuntu 18.04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017.list)"
   ```

   > [!TIP]
   > Если вы хотите установить SQL Server 2019, необходимо зарегистрировать вместо этого репозиторий SQL Server 2019. Используйте следующую команду для установки SQL Server 2019:
   >
   > Для Ubuntu 16.04:
   >
   > ```bash
   > sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
   > ```
   >
   > Для Ubuntu 18.04:
   >
   > ```bash
   > sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019.list)"
   > ```

3. Выполните следующие команды для установки SQL Server:

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server
   ```

4. Когда установка пакета завершится, выполните команду **mssql-conf setup** и следуйте указаниям, чтобы задать пароль системного администратора и выбрать выпуск.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!TIP]
   > Следующие выпуски SQL Server 2017 имеют бесплатные лицензии: Evaluation, Developer и Express.

   > [!NOTE]
   > Для учетной записи системного администратора необходимо установить надежный пароль (минимальная длина — 8 символов; должен содержать строчные и прописные буквы, десятичные цифры и (или) символы, отличные от букв и цифр).

5. По завершении настройки убедитесь в том, что служба работает.

   ```bash
   systemctl status mssql-server --no-pager
   ```

6. Если вы планируете подключаться удаленно, может потребоваться открыть в брандмауэре TCP-порт SQL Server (по умолчанию 1433).

В результате сервер SQL Server будет запущен на компьютере Ubuntu и готов к использованию!

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server"></a><a id="install"></a>Установка SQL Server

> [!NOTE]
> Следующие команды для SQL Server 2019 ссылаются на репозиторий Ubuntu 20.04. Если вы используете Ubuntu 18.04 или 16.04, измените приведенный ниже путь на `/ubuntu/18.04/` или `/ubuntu/16.04/` вместо `/ubuntu/20.04/`.

Чтобы настроить SQL Server в Ubuntu, выполните следующие команды в терминале для установки пакета **mssql-server**:

1. Импортируйте открытые ключи GPG из репозитория:

   ```bash
   wget -qO- https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Зарегистрируйте репозиторий Microsoft SQL Server Ubuntu для SQL Server 2019:
   
   Для Ubuntu 16.04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2019.list)"
   ```
   
   Для Ubuntu 18.04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019.list)"
   ```

   Для Ubuntu 20.04:
   
   ```bash
   sudo add-apt-repository "$(wget -qO- https://packages.microsoft.com/config/ubuntu/20.04/mssql-server-2019.list)"
   ```

3. Выполните следующие команды для установки SQL Server:

   ```bash
   sudo apt-get update
   sudo apt-get install -y mssql-server
   ```

4. Когда установка пакета завершится, выполните команду **mssql-conf setup** и следуйте указаниям, чтобы задать пароль системного администратора и выбрать выпуск.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > Для учетной записи системного администратора необходимо установить надежный пароль (минимальная длина — 8 символов; должен содержать строчные и прописные буквы, десятичные цифры и (или) символы, отличные от букв и цифр).

5. По завершении настройки убедитесь в том, что служба работает.

   ```bash
   systemctl status mssql-server --no-pager
   ```

6. Если вы планируете подключаться удаленно, может потребоваться открыть в брандмауэре TCP-порт SQL Server (по умолчанию 1433).

В результате SQL Server 2019 будет запущен на компьютере Ubuntu и готов к использованию!

::: moniker-end

## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>Установка программ командной строки SQL Server

Чтобы создать базу данных, необходимо подключиться с помощью средства, которое позволяет выполнять инструкции Transact-SQL в SQL Server. Ниже приведены инструкции по установке программ командной строки SQL Server: [sqlcmd](../tools/sqlcmd-utility.md) и [bcp](../tools/bcp-utility.md).

Чтобы установить **mssql-tools** в Ubuntu, выполните указанные ниже действия. 

1. Импортируйте открытые ключи GPG из репозитория.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Зарегистрируйте репозиторий Ubuntu для Майкрософт.
   
   Для Ubuntu 16.04:
   
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

   Для Ubuntu 18.04:
   
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

   Для Ubuntu 20.04:
   
   ```bash
   curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

1. Обновите список источников и выполните команду установки с помощью пакета разработчика unixODBC. Дополнительные сведения см. в разделе [Установка драйвера Microsoft ODBC для SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

   ```bash
   sudo apt-get update 
   sudo apt-get install mssql-tools unixodbc-dev
   ```

   > [!Note] 
   > Чтобы произвести обновление до последней версии **mssql-tools**, выполните следующие команды:
   >
   > ```bash
   > sudo apt-get update 
   > sudo apt-get install mssql-tools 
   > ```

1. **Необязательно**: Добавьте путь `/opt/mssql-tools/bin/` в переменную среды **PATH** в оболочке bash.

   Чтобы программы **sqlcmd и bcp** были доступны из оболочки bash в рамках сеансов входа в систему, измените переменную среды **PATH** в файле **~/.bash_profile** с помощью следующей команды:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Чтобы программы **sqlcmd и bcp** были доступны из оболочки bash в рамках интерактивных сеансов и сеансов без входа в систему, измените переменную среды **PATH** в файле **~/.bashrc** с помощью следующей команды:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]

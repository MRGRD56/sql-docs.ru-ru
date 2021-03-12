---
title: Установка Microsoft ODBC Driver for SQL Server (Linux)
description: Узнайте, как установить Microsoft ODBC Driver for SQL Server на клиентах Linux, чтобы обеспечить подключение к базе данных.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver, installing
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d4e2ee02544955cab007d542bca75b8f14cfeeee
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770462"
---
# <a name="install-the-microsoft-odbc-driver-for-sql-server-linux"></a>Установка Microsoft ODBC Driver for SQL Server (Linux)

В этой статье объясняется, как установить Microsoft ODBC Driver for SQL Server в Linux. В ней также содержатся инструкции для необязательных средств командной строки для SQL Server (`bcp` и `sqlcmd`) и заголовков разработки unixODBC.

В этой статье приведены команды для установки драйвера ODBC из оболочки bash. Сведения о том, как загрузить пакеты напрямую, см. в разделе [Скачивание драйвера ODBC Driver for SQL Server](../download-odbc-driver-for-sql-server.md).

## <a name="microsoft-odbc-17"></a><a id="17"></a> Microsoft ODBC 17

В следующих разделах объясняется, как установить драйвер Microsoft ODBC 17 из оболочки bash в различных дистрибутивах Linux.

- [Alpine Linux](#alpine17)
- [Debian](#debian17)
- [Red Hat Enterprise Linux и Oracle](#redhat17)
- [SUSE Linux Enterprise Server](#suse17)
- [Ubuntu](#ubuntu17)

> [!IMPORTANT]
> Если вы установили пакет `msodbcsql` версии 17, который был доступен непродолжительное время, его следует удалить перед установкой пакета `msodbcsql17`. Это позволит избежать конфликтов. Пакет `msodbcsql17` можно установить параллельно с пакетом `msodbcsql` версии 13.

### <a name="alpine-linux"></a><a id="alpine17"></a> Alpine Linux

```bash
#Download the desired package(s)
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.2.1-1_amd64.apk
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.7.1.1-1_amd64.apk


#(Optional) Verify signature, if 'gpg' is missing install it using 'apk add gnupg':
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.2.1-1_amd64.sig
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.7.1.1-1_amd64.sig

curl https://packages.microsoft.com/keys/microsoft.asc  | gpg --import -
gpg --verify msodbcsql17_17.7.2.1-1_amd64.sig msodbcsql17_17.7.2.1-1_amd64.apk
gpg --verify mssql-tools_17.7.1.1-1_amd64.sig mssql-tools_17.7.1.1-1_amd64.apk


#Install the package(s)
sudo apk add --allow-untrusted msodbcsql17_17.7.2.1-1_amd64.apk
sudo apk add --allow-untrusted mssql-tools_17.7.1.1-1_amd64.apk
```

> [!NOTE]
> Для поддержки Alpine требуется драйвер версии 17.5 или более поздней.

### <a name="debian"></a><a id="debian17"></a> Debian

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Debian 8
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 9
curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 10
curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
# optional: kerberos library for debian-slim distributions
sudo apt-get install libgssapi-krb5-2
```

> [!NOTE]
> Вместо настройки переменной среды ACCEPT_EULA вы можете создать переменную debconf с именем "msodbcsql/ACCEPT_EULA": `echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | sudo debconf-set-selections`

### <a name="red-hat-enterprise-server-and-oracle-linux"></a><a id="redhat17"></a> Red Hat Enterprise Server и Oracle Linux

```bash
sudo su

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#RedHat Enterprise Server 6
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 7 and Oracle Linux 7
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 8 and Oracle Linux 8
curl https://packages.microsoft.com/config/rhel/8/prod.repo > /etc/yum.repos.d/mssql-release.repo

exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server"></a><a id="suse17"></a> SUSE Linux Enterprise Server

```bash
sudo su
curl -O https://packages.microsoft.com/keys/microsoft.asc
rpm --import microsoft.asc

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#SUSE Linux Enterprise Server 11 SP4
#Ensure SUSE Linux Enterprise 11 Security Module has been installed 
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo

#SUSE Linux Enterprise Server 12
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo

#SUSE Linux Enterprise Server 15
zypper ar https://packages.microsoft.com/config/sles/15/prod.repo
#(Only for driver 17.3 and below)
SUSEConnect -p sle-module-legacy/15/x86_64

exit
sudo ACCEPT_EULA=Y zypper install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="ubuntu"></a><a id="ubuntu17"></a> Ubuntu

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Ubuntu 16.04
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 18.04
curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 20.04
curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 20.10
curl https://packages.microsoft.com/config/ubuntu/20.10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

> [!NOTE]
> - Для поддержки Ubuntu 18.04 требуется драйвер версии 17.2 или более поздней.
> - Для поддержки Ubuntu 18.10 требуется драйвер версии 17.3 или более поздней.

> [!NOTE]
> Вместо настройки переменной среды ACCEPT_EULA вы можете создать переменную debconf с именем "msodbcsql/ACCEPT_EULA": `echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | sudo debconf-set-selections`

## <a name="previous-versions"></a>Предыдущие версии

В следующих разделах приведены инструкции по установке предыдущих версий драйвера Microsoft ODBC в Linux. Рассматриваются следующие версии драйверов.

- [Microsoft ODBC Driver 13.1 for SQL Server](#13.1)
- [Microsoft ODBC Driver 13 for SQL Server](#13)
- [Microsoft ODBC Driver 11 for SQL Server](#11)

## <a name="odbc-131"></a><a id="13.1"></a> ODBC 13.1

В следующих разделах объясняется, как установить драйвер Microsoft ODBC 13.1 из оболочки bash в различных дистрибутивах Linux.

### <a name="debian-8"></a>Debian 8

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-11"></a>SUSE Linux Enterprise Server 11

```bash
sudo su
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```bash
sudo su
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1610"></a>Ubuntu 16.10

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

## <a name="odbc-13"></a><a id="13"></a> ODBC 13

В следующих разделах объясняется, как установить драйвер Microsoft ODBC 13 из оболочки bash в различных дистрибутивах Linux.

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```bash
sudo su 
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo 
zypper update 
sudo ACCEPT_EULA=Y zypper install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
zypper install unixODBC-utf16-devel
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="offline-installation"></a>Автономная установка

Если необходимо установить драйвер [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC версии 13 на компьютере без подключения к Интернету, потребуется разрешить зависимости пакетов вручную. Драйвер [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC версии 13 имеет следующие прямые зависимости:
- Ubuntu: libc6 (>= 2.21), libstdc++6 (>= 4.9), libkrb5-3, libcurl3, openssl, debconf (>= 0.5), unixodbc (>= 2.3.1-1)
- Red Hat: ```glibc, e2fsprogs, krb5-libs, openssl, unixODBC```
- SUSE: ```glibc, libuuid1, krb5, openssl, unixODBC```

Каждый из этих пакетов, в свою очередь, имеет собственные зависимости, которые могут отсутствовать в системе. Для решения этой проблемы в общем случае следует обратиться к документации по диспетчеру пакетов используемого дистрибутива: [Redhat](https://wiki.centos.org/HowTos/CreateLocalRepos), [Ubuntu](https://unix.stackexchange.com/questions/87130/how-to-quickly-create-a-local-apt-repository-for-random-packages-using-a-debian) или [SUSE](https://en.opensuse.org/Portal:Zypper).

Другое распространенное решение — вручную скачать все зависимые пакеты в одну папку на компьютере установки, а затем вручную установить каждый пакет по очереди, завершив пакетом драйвера [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC версии 13.

#### <a name="redhat-linux-enterprise-server-7"></a>Redhat Linux Enterprise Server 7

- Скачайте последнюю версию `msodbcsql` `.rpm` с сайта [https://packages.microsoft.com/rhel/7/prod/](https://packages.microsoft.com/rhel/7/prod/).
- Установите зависимости и драйвер.
  
```bash
yum install glibc e2fsprogs krb5-libs openssl unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

#### <a name="ubuntu-1604"></a>Ubuntu 16.04

- Скачайте последнюю версию `msodbcsql` `.deb` с сайта [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/).
- Установите зависимости и драйвер.

```bash
sudo apt-get install libc6 libstdc++6 libkrb5-3 libcurl3 openssl debconf unixodbc unixodbc-dev #install dependencies
sudo dpkg -i msodbcsql_13.1.X.X-X_amd64.deb #install the Driver
```

#### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

- Скачайте последнюю версию `msodbcsql` `.rpm` с сайта [https://packages.microsoft.com/sles/12/prod/](https://packages.microsoft.com/sles/12/prod/).
- Установите зависимости и драйвер.

```bash
zypper install glibc, libuuid1, krb5, openssl, unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

После установки пакета можно проверить, находит ли драйвер [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC версии 13 все нужные зависимости. Для этого выполните команду ldd и проверьте наличие отсутствующих библиотек в выходных данных:

```bash
ldd /opt/microsoft/msodbcsql/lib64/libmsodbcsql-*
```

## <a name="odbc-11"></a><a id="11"></a> ODBC 11

В следующих разделах объясняется, как установить Microsoft ODBC Driver 11 в Linux. Для использования драйвера сначала установите диспетчер драйверов unixODBC. Дополнительные сведения: [Установка диспетчера драйверов](../../../connect/odbc/linux-mac/installing-the-driver-manager.md).

### <a name="installation-steps"></a>Процесс установки  

> [!IMPORTANT]  
> Эти инструкции ссылаются на `msodbcsql-11.0.2270.0.tar.gz` (файл установки для Red Hat Linux). В случае установке предварительной версии для SUSE Linux файл называется `msodbcsql-11.0.2260.0.tar.gz`.  
  
Порядок установки драйвера

1. Убедитесь, что у вас есть корневое разрешение.  

2. Перейдите в каталог, куда был скачан файл с именем `msodbcsql-11.0.2270.0.tar.gz`. Убедитесь в наличии файла \*.TAR.GZ, который соответствует вашей версии Linux. Чтобы извлечь файлы, выполните следующую команду: `tar xvzf msodbcsql-11.0.2270.0.tar.gz`.  
  
3. Перейдите в каталог `msodbcsql-11.0.2270.0`, где должен находиться файл **install.sh**.  
  
4. Чтобы просмотреть список доступных параметров установки, выполните следующую команду: **./install.sh**.  
  
5. Создайте резервную копию **odbcinst.ini**. Установка драйвера обновляет **odbcinst.ini**. Файл odbcinst.ini содержит список драйверов, которые зарегистрированы с помощью диспетчера драйверов unixODBC. Чтобы определить на компьютере расположение файла odbcinst.ini, выполните следующую команду: ```odbc_config --odbcinstini```.  
  
6. Перед установкой драйвера выполните следующую команду: `./install.sh verify`. Выходные данные команды `./install.sh verify` показывают, есть ли на компьютере ПО, необходимое для поддержки драйвера ODBC на Linux.  
  
7. Когда вы будете готовы установить драйвер ODBC на Linux, выполните команду: `./install.sh install`. Если вам нужно будет дополнительно указать команду установки (`bin-dir` или `lib-dir`), сделайте это после параметра **install**.  
  
8. После просмотра лицензионного соглашения введите **YES** для продолжения установки.  
  
При установке драйвер помещается в папку `/opt/microsoft/msodbcsql/11.0.2270.0`. Драйвер и его вспомогательные файлы должны находиться в папке `/opt/microsoft/msodbcsql/11.0.2270.0`.  
  
Для проверки, что драйвер ODBC в Linux зарегистрирован, выполните следующую команду: ```odbcinst -q -d -n "ODBC Driver 11 for SQL Server"```.  
  
### <a name="uninstall"></a>Удаление  
  
Вы можете удалить драйвер ODBC 11 на Linux, выполнив следующие команды:  
  
1. `rm -f /usr/bin/sqlcmd`
  
2. `rm -f /usr/bin/bcp`  
  
3. `rm -rf /opt/microsoft/msodbcsql`  
  
4. `odbcinst -u -d -n "ODBC Driver 11 for SQL Server"`
  
## <a name="driver-files"></a>Файлы драйвера

Драйвер ODBC в Linux состоит из следующих компонентов.

|Компонент|Описание|  
|---------------|-----------------|  
|libmsodbcsql-17.X.so.X.X или libmsodbcsql-13.X.so.X.X|Общий объект (`so`) файла динамической библиотеки, содержащий все функциональные возможности драйвера. Этот файл устанавливается в папке `/opt/microsoft/msodbcsql17/lib64/` для версии 17 драйвера и в папке `/opt/microsoft/msodbcsql/lib64/` для версии 13.|  
|`msodbcsqlr17.rll` либо `msodbcsqlr13.rll`|Сопутствующий файл ресурса для библиотеки драйвера. Этот файл устанавливается в папке `[driver .so directory]../share/resources/en_US/`.| 
|msodbcsql.h|Файл заголовка, содержащий все новые определения, необходимые для использования драйвера.<br /><br /> **Примечание.**  Нельзя сочетать в одной программе ссылки на msodbcsql.h и odbcss.h.<br /><br /> Файл msodbcsql.h устанавливается в папке `/opt/microsoft/msodbcsql17/include/` для версии 17 драйвера и в папке `/opt/microsoft/msodbcsql/include/` для версии 13. |
|LICENSE.txt|Текстовый файл с условиями лицензионного соглашения. Этот файл помещается в папку `/usr/share/doc/msodbcsql17/` для версии 17 драйвера и в папку `/usr/share/doc/msodbcsql/` для версии 13.|
|RELEASE_NOTES|Текстовый файл с заметками о выпуске. Этот файл помещается в папку `/usr/share/doc/msodbcsql17/` для версии 17 драйвера и в папку `/usr/share/doc/msodbcsql/` для версии 13.|

## <a name="resource-file-loading"></a>Загрузка файла ресурсов

Чтобы драйвер работал, он должен загрузить файл ресурсов. Этот файл имеет имя `msodbcsqlr17.rll` или `msodbcsqlr13.rll` в зависимости от версии драйвера. Файл `.rll` располагается по пути относительно расположения самого драйвера (`so` или `dylib`), указанного в таблице выше. Кроме того, начиная с версии 17.1 драйвер пытается загрузить файл `.rll` из каталога по умолчанию, если его не удалось загрузить по относительному пути. Путь к файлу ресурсов по умолчанию в Linux: `/opt/microsoft/msodbcsql17/share/resources/en_US/`.

## <a name="troubleshooting"></a>Устранение неполадок

Если не удается установить подключение к SQL Server с помощью драйвера ODBC, см. статью, посвященную известным проблемам при [устранении неполадок подключения](known-issues-in-this-version-of-the-driver.md#connectivity).

## <a name="next-steps"></a>Дальнейшие действия

После установки драйвера можно попробовать [пример приложения C++ ODBC](../../odbc/cpp-code-example-app-connect-access-sql-db.md). Подробнее о разработке приложений ODBC см. в разделе [Разработка приложений](../../../odbc/reference/develop-app/developing-applications.md).

Дополнительные сведения см. в статьях с [заметками о выпуске](release-notes-odbc-sql-server-linux-mac.md) и [требованиями к системе](system-requirements.md) для драйвера ODBC.

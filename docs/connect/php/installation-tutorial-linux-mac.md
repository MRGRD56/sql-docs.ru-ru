---
title: Установка Linux и macOS для драйверов для PHP
description: В этих инструкциях вы узнаете, как установить драйверы Майкрософт для PHP для SQL Server на Linux или macOS.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
manager: v-mabarw
ms.openlocfilehash: dfb8494912ae2f11590ec8a8b679f0f23064f930
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076456"
---
# <a name="linux-and-macos-installation-tutorial-for-the-microsoft-drivers-for-php-for-sql-server"></a>Руководство по установке драйверов Майкрософт для PHP для SQL Server в Linux и MacOS
В следующих инструкциях предполагается чистая среда и показано, как установить PHP 8.0, драйвер Microsoft ODBC, веб-сервер Apache и драйверы Майкрософт для PHP для SQL Server в Ubuntu 16.04, 18.04 и 20.04, RedHat 7 и 8, Debian 9 и 10, SUSE 12 и 15, Alpine 3.11 и 3.12 и macOS 10.14, 10.15 и 11.0. В этих инструкциях рекомендуется установка драйверов с помощью PECL, но вы можете скачать предварительно созданные двоичные файлы со страницы проекта [драйверов Майкрософт для PHP для SQL Server](https://github.com/Microsoft/msphpsql/releases) на сайте GitHub и установить их по инструкциям из статьи [Loading the Microsoft Drivers for PHP for SQL Server](../../connect/php/loading-the-php-sql-driver.md) (Загрузка драйверов Майкрософт для PHP для SQL Server). Описание процесса загрузки расширений и причины, по которым расширения не добавляются в файл php.ini, см. в статье [о загрузке драйверов](../../connect/php/loading-the-php-sql-driver.md#loading-the-driver-at-php-startup).

С помощью следующих инструкций производится установка PHP 8.0 по умолчанию с помощью `pecl install`, если доступны пакеты PHP 8.0. Возможно, потребуется сначала выполнить `pecl channel-update pecl.php.net`. Некоторые поддерживаемые дистрибутивы Linux по умолчанию используют PHP 7.1 или более ранних версий, которые не поддерживаются последней версией драйверов PHP для SQL Server. См. примечания в начале каждого раздела насчет установки вместо этого PHP 7.4 или 7.3.

Также включены инструкции по установке диспетчера процессов PHP FastCGI (PHP-FPM) в Ubuntu. Это необходимо, если вместо Apache используется веб-сервер nginx.

Хотя эти инструкции содержат команды для установки драйверов SQLSRV и PDO_SQLSRV, драйверы можно устанавливать и выполнять независимо. Пользователи, имеющие опыт работы с настройкой конфигурации, могут настроить эти инструкции индивидуально для SQLSRV или PDO_SQLSRV. Оба драйвера имеют одинаковые зависимости, кроме указанных ниже.

## <a name="contents-of-this-page"></a>Содержимое этой страницы

- [Установка драйверов в Ubuntu 16.04, 18.04 и 20.04](#installing-the-drivers-on-ubuntu-1604-1804-and-2004)
- [Установка драйверов с помощью PHP-FPM в Ubuntu](#installing-the-drivers-with-php-fpm-on-ubuntu)
- [Установка драйверов в Red Hat 7 и 8](#installing-the-drivers-on-red-hat-7-and-8)
- [Установка драйверов в Debian 9 и 10](#installing-the-drivers-on-debian-9-and-10)
- [Установка драйверов в Suse 12 и 15](#installing-the-drivers-on-suse-12-and-15)
- [Установка драйверов в Alpine 3.11 и 3.12](#installing-the-drivers-on-alpine-311-and-312)
- [Установка драйверов в macOS Mojave, Catalina и Big Sur](#installing-the-drivers-on-macos-mojave-catalina-and-big-sur)

## <a name="installing-the-drivers-on-ubuntu-1604-1804-and-2004"></a>Установка драйверов в Ubuntu 16.04, 18.04 и 20.04

> [!NOTE]
> Чтобы установить PHP 7.4 или 7.3, замените 8.0 на 7.4 или 7.3 в следующих командах.

### <a name="step-1-install-php"></a>Шаг 1. Установка PHP
```bash
sudo su
add-apt-repository ppa:ondrej/php -y
apt-get update
apt-get install php8.0 php8.0-dev php8.0-xml -y --allow-unauthenticated
```
### <a name="step-2-install-prerequisites"></a>Шаг 2. Установка необходимых компонентов
Установите драйвер ODBC для Ubuntu, следуя инструкциям в статье [Установка Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Шаг 3. Установка драйверов PHP для Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/8.0/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/8.0/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 8.0 sqlsrv pdo_sqlsrv
```

Если в системе только одна версия PHP, последний шаг можно упростить: `phpenmod sqlsrv pdo_sqlsrv`.

### <a name="step-4-install-apache-and-configure-driver-loading"></a>Шаг 4. Установка Apache и настройка загрузки драйвера
```bash
sudo su
apt-get install libapache2-mod-php8.0 apache2
a2dismod mpm_event
a2enmod mpm_prefork
a2enmod php8.0
exit
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Шаг 5. Перезапуск Apache и тестирование примера скрипта
```bash
sudo service apache2 restart
```
Чтобы протестировать установку, воспользуйтесь разделом [Тестирование установки](#testing-your-installation) в конце этого документа.

## <a name="installing-the-drivers-with-php-fpm-on-ubuntu"></a>Установка драйверов с помощью PHP-FPM в Ubuntu

> [!NOTE]
> Чтобы установить PHP 7.4 или 7.3, замените 8.0 на 7.4 или 7.3 в следующих командах.

### <a name="step-1-install-php"></a>Шаг 1. Установка PHP
```bash
sudo su
add-apt-repository ppa:ondrej/php -y
apt-get update
apt-get install php8.0 php8.0-dev php8.0-fpm php8.0-xml -y --allow-unauthenticated
```
Проверьте состояние службы PHP-FPM, выполнив следующее:
```bash
systemctl status php8.0-fpm
```
### <a name="step-2-install-prerequisites"></a>Шаг 2. Установка необходимых компонентов
Установите драйвер ODBC для Ubuntu, следуя инструкциям в статье [Установка Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Шаг 3. Установка драйверов PHP для Microsoft SQL Server
```bash
sudo pecl config-set php_ini /etc/php/8.0/fpm/php.ini
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/8.0/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/8.0/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 8.0 sqlsrv pdo_sqlsrv
```
Если в системе только одна версия PHP, последний шаг можно упростить: `phpenmod sqlsrv pdo_sqlsrv`.

Убедитесь, что `sqlsrv.ini` и `pdo_sqlsrv.ini` находятся в `/etc/php/8.0/fpm/conf.d/`:
```bash
ls /etc/php/8.0/fpm/conf.d/*sqlsrv.ini
```
Перезапустите службу PHP-FPM:
```bash
sudo systemctl restart php8.0-fpm
```

### <a name="step-4-install-and-configure-nginx"></a>Шаг 4. Установка и настройка nginx
```bash
sudo apt-get update
sudo apt-get install nginx
sudo systemctl status nginx
```
Чтобы настроить nginx, необходимо изменить файл `/etc/nginx/sites-available/default`. Добавьте `index.php` в список под разделом со следующим текстом: `# Add index.php to the list if you are using PHP`:
```
# Add index.php to the list if you are using PHP
index index.html index.htm index.nginx-debian.html index.php;
```
Затем раскомментируйте и измените раздел после `# pass PHP scripts to FastCGI server` следующим образом:
```
# pass PHP scripts to FastCGI server
#
location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.0-fpm.sock;
}
```
### <a name="step-5-restart-nginx-and-test-the-sample-script"></a>Шаг 5. Перезапуск nginx и тестирование примера скрипта
```bash
sudo systemctl restart nginx.service
```
Чтобы протестировать установку, воспользуйтесь разделом [Тестирование установки](#testing-your-installation) в конце этого документа.

## <a name="installing-the-drivers-on-red-hat-7-and-8"></a>Установка драйверов в Red Hat 7 и 8

### <a name="step-1-install-php"></a>Шаг 1. Установка PHP

Чтобы установить PHP в Red Hat 7, выполните следующее:
> [!NOTE]
> Чтобы установить PHP 7.4 или 7.3, замените remi-php80 строкой remi-php74 или remi-php73 соответственно в следующих командах.
```bash
sudo su
yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install https://rpms.remirepo.net/enterprise/remi-release-7.rpm
subscription-manager repos --enable=rhel-7-server-optional-rpms
yum install yum-utils
yum-config-manager --enable remi-php80
yum update
# Note: The php-pdo package is required only for the PDO_SQLSRV driver
yum install php php-pdo php-xml php-pear php-devel re2c gcc-c++ gcc
```

Чтобы установить PHP в Red Hat 8, выполните следующее:
> [!NOTE]
> Чтобы установить PHP 7.4 или 7.3, замените remi-8.0 на remi-7.4 или remi-7.3 соответственно в следующих командах.
```bash
sudo su
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm
dnf install yum-utils
dnf module reset php
dnf module install php:remi-8.0
subscription-manager repos --enable codeready-builder-for-rhel-8-x86_64-rpms
dnf update
# Note: The php-pdo package is required only for the PDO_SQLSRV driver
dnf install php-pdo php-pear php-devel
```

### <a name="step-2-install-prerequisites"></a>Шаг 2. Установка необходимых компонентов
Установите драйвер ODBC для Red Hat 7 или 8, следуя инструкциям в статье [Установка Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Шаг 3. Установка драйверов PHP для Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/30-pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/20-sqlsrv.ini
exit
```

Можно также установить их из репозитория Remi:
```bash
sudo yum install php-sqlsrv
```
### <a name="step-4-install-apache"></a>Шаг 4. Установка Apache
```bash
sudo yum install httpd
```
SELinux устанавливается по умолчанию и выполняется в принудительном режиме. Чтобы разрешить Apache подключаться к базам данных через SELinux, выполните следующую команду:
```bash
sudo setsebool -P httpd_can_network_connect_db 1
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Шаг 5. Перезапуск Apache и тестирование примера скрипта
```bash
sudo apachectl restart
```
Чтобы протестировать установку, воспользуйтесь разделом [Тестирование установки](#testing-your-installation) в конце этого документа.

## <a name="installing-the-drivers-on-debian-9-and-10"></a>Установка драйверов в Debian 9 и 10

> [!NOTE]
> Чтобы установить PHP 7.4 или 7.3, замените 8.0 на 7.4 или 7.3 в следующих командах.

### <a name="step-1-install-php"></a>Шаг 1. Установка PHP
```bash
sudo su
apt-get install curl apt-transport-https
wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list
apt-get update
apt-get install -y php8.0 php8.0-dev php8.0-xml php8.0-intl
```
### <a name="step-2-install-prerequisites"></a>Шаг 2. Установка необходимых компонентов
Установите драйвер ODBC для Debian, следуя инструкциям в статье [Установка Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md). 

Кроме того, может потребоваться создать правильный языковой стандарт, чтобы выходные данные PHP правильно отображались в браузере. Например, для языкового стандарта en_US UTF-8 выполните следующие команды:
```bash
sudo su
sed -i 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/g' /etc/locale.gen
locale-gen
```
Может потребоваться добавить `/usr/sbin` в `$PATH`, так как там находится исполняемый файл `locale-gen`.

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Шаг 3. Установка драйверов PHP для Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
printf "; priority=20\nextension=sqlsrv.so\n" > /etc/php/8.0/mods-available/sqlsrv.ini
printf "; priority=30\nextension=pdo_sqlsrv.so\n" > /etc/php/8.0/mods-available/pdo_sqlsrv.ini
exit
sudo phpenmod -v 8.0 sqlsrv pdo_sqlsrv
```

Если в системе только одна версия PHP, последний шаг можно упростить: `phpenmod sqlsrv pdo_sqlsrv`. Как и в случае с `locale-gen`, `phpenmod` находится в `/usr/sbin`, поэтому может потребоваться добавить этот каталог в `$PATH`.

### <a name="step-4-install-apache-and-configure-driver-loading"></a>Шаг 4. Установка Apache и настройка загрузки драйвера
```bash
sudo su
apt-get install libapache2-mod-php8.0 apache2
a2dismod mpm_event
a2enmod mpm_prefork
a2enmod php8.0
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Шаг 5. Перезапуск Apache и тестирование примера скрипта
```bash
sudo service apache2 restart
```
Чтобы протестировать установку, воспользуйтесь разделом [Тестирование установки](#testing-your-installation) в конце этого документа.

## <a name="installing-the-drivers-on-suse-12-and-15"></a>Установка драйверов в Suse 12 и 15

> [!NOTE]
> В следующих инструкциях замените `<SuseVersion>` на свою версию SUSE. Если вы используете SUSE Enterprise Linux 15, это будет SLE_15_SP1 или SLE_15_SP2. Для SUSE 12 используйте SLE_12_SP4 (или выше, если применимо). Не все версии PHP доступны для всех версий SUSE Linux. В `http://download.opensuse.org/repositories/devel:/languages:/php` указано, какие версии SUSE имеют доступную версию PHP по умолчанию, а в `http://download.opensuse.org/repositories/devel:/languages:/php:/` — какие еще версии PHP доступны для разных версий SUSE.

> [!NOTE]
> Пакеты для PHP 7.4 или более поздней версии недоступны для SUSE 12, а пакет для PHP 8.0 пока не доступен для SUSE 15.
> Чтобы установить PHP 7.3, замените URL-адрес репозитория в команде ниже следующим URL-адресом: `https://download.opensuse.org/repositories/devel:/languages:/php:/php73/<SuseVersion>/devel:languages:php:php73.repo`.

### <a name="step-1-install-php"></a>Шаг 1. Установка PHP
```bash
sudo su
zypper -n ar -f https://download.opensuse.org/repositories/devel:languages:php/<SuseVersion>/devel:languages:php.repo
zypper --gpg-auto-import-keys refresh
zypper -n install php7 php7-devel php7-openssl
```
### <a name="step-2-install-prerequisites"></a>Шаг 2. Установка необходимых компонентов
Установите драйвер ODBC для SUSE, следуя инструкциям в статье [Установка Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Шаг 3. Установка драйверов PHP для Microsoft SQL Server
> [!NOTE]
> Если вы получаете сообщение об ошибке вида `Connection to 'pecl.php.net:443' failed: Unable to find the socket transport "ssl"`, измените скрипт pecl в папке /usr/bin/pecl, удалив аргумент `-n` в последней строке. Этот аргумент запрещает PECL загружать ini-файлы при вызове PHP, что мешает загрузить расширение OpenSSL PECL.

```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/sqlsrv.ini
exit
```
### <a name="step-4-install-apache-and-configure-driver-loading"></a>Шаг 4. Установка Apache и настройка загрузки драйвера
```bash
sudo su
zypper install apache2 apache2-mod_php7
a2enmod php7
echo "extension=sqlsrv.so" >> /etc/php7/apache2/php.ini
echo "extension=pdo_sqlsrv.so" >> /etc/php7/apache2/php.ini
exit
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Шаг 5. Перезапуск Apache и тестирование примера скрипта
```bash
sudo systemctl restart apache2
```
Чтобы протестировать установку, воспользуйтесь разделом [Тестирование установки](#testing-your-installation) в конце этого документа.

## <a name="installing-the-drivers-on-alpine-311-and-312"></a>Установка драйверов в Alpine 3.11 и 3.12

> [!NOTE]
> Версия PHP по умолчанию — 7.3. Версия PHP 7.4 или более поздняя может быть доступна в тестовых или пограничных репозиториях для Alpine. Вместо этого можно скомпилировать PHP из источника.

### <a name="step-1-install-php"></a>Шаг 1. Установка PHP
Пакеты PHP для Alpine находятся в репозитории `edge/community`. Проверьте раздел [Включить репозиторий сообщества](https://wiki.alpinelinux.org/wiki/Enable_Community_Repository) на их вики-странице. Добавьте следующую строку в `/etc/apk/repositories`, заменив `<mirror>` на URL-адрес зеркального отображения репозитория Alpine:
```bash
http://<mirror>/alpine/edge/community
```
Далее выполните:
```bash
sudo su
apk update
# Note: The php7-pdo package is required only for the PDO_SQLSRV driver
apk add php7 php7-dev php7-pear php7-pdo php7-openssl autoconf make g++
```
### <a name="step-2-install-prerequisites"></a>Шаг 2. Установка необходимых компонентов
Установите драйвер ODBC для Alpine, следуя инструкциям в статье [Установка Microsoft ODBC Driver for SQL Server (Linux)](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md). 

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Шаг 3. Установка драйверов PHP для Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
sudo su
echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/10_pdo_sqlsrv.ini
echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/00_sqlsrv.ini
```

### <a name="step-4-install-apache-and-configure-driver-loading"></a>Шаг 4. Установка Apache и настройка загрузки драйвера
```bash
sudo apk add php7-apache2 apache2
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Шаг 5. Перезапуск Apache и тестирование примера скрипта
```bash
sudo rc-service apache2 restart
```
Чтобы протестировать установку, воспользуйтесь разделом [Тестирование установки](#testing-your-installation) в конце этого документа.


## <a name="installing-the-drivers-on-macos-mojave-catalina-and-big-sur"></a>Установка драйверов в macOS Mojave, Catalina и Big Sur

Установите brew, как описано ниже, если у вас ее еще нет:
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

> [!NOTE]
> Чтобы установить PHP 7.4 или 7.3, замените php@8.0 на php@7.4 или php@7.3 соответственно в следующих командах.

### <a name="step-1-install-php"></a>Шаг 1. Установка PHP

```bash
brew tap
brew tap homebrew/core
brew install php@8.0
```
PHP теперь должен быть указан в пути. Запустите `php -v` и убедитесь, что используется правильная версия PHP. Если в пути нет PHP или есть PHP неправильной версии, выполните следующую команду:
```bash
brew link --force --overwrite php@8.0
```

### <a name="step-2-install-prerequisites"></a>Шаг 2. Установка необходимых компонентов
Установите драйвер ODBC для macOS, следуя инструкциям в статье [Установка Microsoft ODBC Driver for SQL Server (macOS)](../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md). 

Кроме того, может потребоваться установить средства make для GNU:
```bash
brew install autoconf automake libtool
```

### <a name="step-3-install-the-php-drivers-for-microsoft-sql-server"></a>Шаг 3. Установка драйверов PHP для Microsoft SQL Server
```bash
sudo pecl install sqlsrv
sudo pecl install pdo_sqlsrv
```
### <a name="step-4-install-apache-and-configure-driver-loading"></a>Шаг 4. Установка Apache и настройка загрузки драйвера
```bash
brew install apache2
```
Чтобы найти файл конфигурации Apache (`httpd.conf`) для установки Apache, выполните 
```bash
/usr/local/bin/apachectl -V | grep SERVER_CONFIG_FILE
``` 
Следующие команды добавляют необходимую конфигурацию в `httpd.conf`. Не забудьте указать путь, возвращенный предыдущей командой, вместо `/usr/local/etc/httpd/httpd.conf`:
```bash
echo "LoadModule php7_module /usr/local/opt/php@8.0/lib/httpd/modules/libphp7.so" >> /usr/local/etc/httpd/httpd.conf
(echo "<FilesMatch .php$>"; echo "SetHandler application/x-httpd-php"; echo "</FilesMatch>";) >> /usr/local/etc/httpd/httpd.conf
```
### <a name="step-5-restart-apache-and-test-the-sample-script"></a>Шаг 5. Перезапуск Apache и тестирование примера скрипта
```bash
sudo apachectl restart
```
Чтобы протестировать установку, воспользуйтесь разделом [Тестирование установки](#testing-your-installation) в конце этого документа.

## <a name="testing-your-installation"></a>Тестирование установки

Чтобы протестировать этот пример скрипта, создайте файл с именем testsql.php в корневом каталоге документов системы. Это каталог `/var/www/html/` в Ubuntu, Debian и Redhat, `/srv/www/htdocs` в SUSE, `/var/www/localhost/htdocs` в Alpine и `/usr/local/var/www` в macOS. Скопируйте приведенный ниже скрипт, заменив имя сервера, имя базы данных, имя пользователя и пароль правильными значениями.

### <a name="sqlsrv-example"></a>Пример SQLSRV

```php
<?php
$serverName = "yourServername";
$connectionOptions = array(
    "database" => "yourDatabase",
    "uid" => "yourUsername",
    "pwd" => "yourPassword"
);

function exception_handler($exception) {
    echo "<h1>Failure</h1>";
    echo "Uncaught exception: " , $exception->getMessage();
    echo "<h1>PHP Info for troubleshooting</h1>";
    phpinfo();
}

set_exception_handler('exception_handler');

// Establishes the connection
$conn = sqlsrv_connect($serverName, $connectionOptions);
if ($conn === false) {
    die(formatErrors(sqlsrv_errors()));
}

// Select Query
$tsql = "SELECT @@Version AS SQL_VERSION";

// Executes the query
$stmt = sqlsrv_query($conn, $tsql);

// Error handling
if ($stmt === false) {
    die(formatErrors(sqlsrv_errors()));
}
?>

<h1> Success Results : </h1>

<?php
while ($row = sqlsrv_fetch_array($stmt, SQLSRV_FETCH_ASSOC)) {
    echo $row['SQL_VERSION'] . PHP_EOL;
}

sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);

function formatErrors($errors)
{
    // Display errors
    echo "<h1>SQL Error:</h1>";
    echo "Error information: <br/>";
    foreach ($errors as $error) {
        echo "SQLSTATE: ". $error['SQLSTATE'] . "<br/>";
        echo "Code: ". $error['code'] . "<br/>";
        echo "Message: ". $error['message'] . "<br/>";
    }
}
?>
```

### <a name="pdo_sqlsrv-example"></a>Пример PDO_SQLSRV

```php
<?php
try {
    $serverName = "yourServername";
    $databaseName = "yourDatabase";
    $uid = "yourUsername";
    $pwd = "yourPassword";
    
    $conn = new PDO("sqlsrv:server = $serverName; Database = $databaseName;", $uid, $pwd);

    // Select Query
    $tsql = "SELECT @@Version AS SQL_VERSION";

    // Executes the query
    $stmt = $conn->query($tsql);
} catch (PDOException $exception1) {
    echo "<h1>Caught PDO exception:</h1>";
    echo $exception1->getMessage() . PHP_EOL;
    echo "<h1>PHP Info for troubleshooting</h1>";
    phpinfo();
}

?>

<h1> Success Results : </h1>

<?php
try {
    while ($row = $stmt->fetch(PDO::FETCH_ASSOC)) {
        echo $row['SQL_VERSION'] . PHP_EOL;
    }
} catch (PDOException $exception2) {
    // Display errors
    echo "<h1>Caught PDO exception:</h1>";
    echo $exception2->getMessage() . PHP_EOL;
}

unset($stmt);
unset($conn);
?>
```

Перейдите в браузере на страницу https://localhost/testsql.php (https://localhost:8080/testsql.php в Mac OS). Теперь вы сможете подключиться к базе данных SQL Server или SQL Azure. Если вы не видите сообщение об успешном выполнении с информацией о версии SQL, выполните основные действия по устранению неполадок, запустив скрипт из командной строки:

```bash
php testsql.php
```

Если выполнение из командной строки прошло успешно, но в браузере ничего не отображается, проверьте [файлы журнала Apache](https://linuxize.com/post/apache-log-files/#location-of-the-log-files). Если потребуется дополнительная поддержка, см. места, где можно найти необходимую информацию, в разделе [Ресурсы поддержки](support-resources-for-the-php-sql-driver.md).

## <a name="see-also"></a>См. также:  
[Getting Started with the Microsoft Drivers for PHP for SQL Server](../../connect/php/getting-started-with-the-php-sql-driver.md) (Начало работы с драйверами Майкрософт для PHP для SQL Server)

[Загрузка драйверов Майкрософт для PHP для SQL Server](../../connect/php/loading-the-php-sql-driver.md)

[Системные требования драйверов Майкрософт для PHP для SQL Server](../../connect/php/system-requirements-for-the-php-sql-driver.md)

---
title: Подключение из Linux или macOS
description: Узнайте, как создать подключение к базе данных из Linux или macOS с помощью Microsoft ODBC Driver for SQL Server.
ms.custom: ''
ms.date: 09/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connect to linux
- configure odbc.ini
ms.assetid: f95cdbce-e7c2-4e56-a9f7-8fa3a920a125
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d1bdbcbb34be9cbfa075ead7e1cd03ec813a5d9d
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247302"
---
# <a name="connecting-from-linux-or-macos"></a>Подключение из Linux или macOS

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

В этой статье описывается, как можно создать подключение к базе данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="connection-properties"></a>Свойства подключения  

Ключевые слова и атрибуты строки подключения и имени DSN, поддерживаемые в Linux и macOS, можно найти [здесь](../dsn-connection-string-attribute.md).

> [!IMPORTANT]  
> При подключении к базе данных, которая использует зеркальное отображение базы данных (имеет партнера по обеспечению отработки отказа), не указывайте имя базы данных в строке подключения. Вместо этого отправьте команду **use**_имя_базы_данных_, чтобы подключиться к базе данных перед выполнением запросов.  
  
Значение, передаваемое в ключевое слово **Driver**, может быть одним из следующих:  
  
- именем, использованным при установке драйвера;

- путем к библиотеке драйвера, которая была указана в INI-файле шаблона, используемого для установки драйвера.  

Использовать имена DSN не обязательно. Вы можете использовать имя DSN для определения ключевых слов строки подключения с именем `DSN` с последующим созданием соответствующей ссылки в строке подключения. Чтобы создать имя DSN, создайте (при необходимости) и измените файл **~/.odbc.ini** (`.odbc.ini` в домашнем каталоге) для имени DSN пользователя, доступного только для текущего пользователя, или `/etc/odbc.ini` для системного имени DSN (требуются права администратора). Ниже приведен пример файла, который показывает минимальное количество необходимых записей для имени DSN:  

```ini
# [DSN name]
[MSSQLTest]  
Driver = ODBC Driver 17 for SQL Server  
# Server = [protocol:]server[,port]  
Server = tcp:localhost,1433
#
# Note:  
# Port is not a valid keyword in the odbc.ini file  
# for the Microsoft ODBC driver on Linux or macOS
#  
```  

Чтобы подключиться с помощью приведенного выше имени DSN в строке подключения, следует указать ключевое слово `DSN` следующим образом: `DSN=MSSQLTest;UID=my_username;PWD=my_password`.  
Строка подключения выше будет эквивалентна строке подключения, определенной без ключевого слова `DSN`, например: `Driver=ODBC Driver 17 for SQL Server;Server=tcp:localhost,1433;UID=my_username;PWD=my_password`.

При необходимости можно указать протокол и порт для подключения к серверу. Например, **Server = tcp:** _имя_сервера_ **,12345**. Обратите внимание, что единственным протоколом, поддерживаемым драйверами Linux и macOS, является `tcp`.

Чтобы подключиться к именованному экземпляру через статический порт, используйте <b>Server=</b>*имя_сервера*,**номер_порта**. Подключение к динамическому порту не поддерживается в версиях ниже 17.4.

Кроме того, можно добавить сведения о DSN в файл шаблона и выполнить следующую команду, чтобы добавить его в `~/.odbc.ini`:
 - **odbcinst -i -s -f** _файл_шаблона_  

Полную документацию по INI-файлам и `odbcinst` см. в [документации unixODBC](http://www.unixodbc.org/odbcinst.html). Сведения о записях в файле `odbc.ini`, характерных для ODBC Driver for SQL Server и поддерживаемых в Linux и macOS, см. в статье [Ключевые слова и атрибуты строки подключения и имени DSN](../dsn-connection-string-attribute.md).

Можно проверить, что драйвер работает, используя `isql` для проверки подключения или следующую команду:
 - **bcp master.INFORMATION_SCHEMA.TABLES out OutFile.dat -S <server> -U <name> -P <password>**  

## <a name="using-tlsssl"></a>Использование TLS/SSL  

Для шифрования подключений к [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] можно использовать протокол TLS, ранее называемый SSL. TLS защищает имена пользователей и пароли [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] по сети. Кроме того, TLS проверяет идентификатор сервера для защиты от атак "злоумышленник в середине".  

Включение шифрования повышает безопасность за счет снижения производительности.

См. сведения о [шифровании подключений в SQL Server](/previous-versions/sql/sql-server-2008-r2/ms189067(v=sql.105)) и [использовании шифрования без проверки](../../../relational-databases/native-client/features/using-encryption-without-validation.md).

Независимо от параметров для **Encrypt** и **TrustServerCertificate** учетные данные входа на сервер (имя пользователя и пароль) всегда шифруются. Следующая таблица показывает эффект от параметров **Encrypt** и **TrustServerCertificate** .  

||**TrustServerCertificate=no**|**TrustServerCertificate=yes**|  
|-|-------------------------------------|------------------------------------|  
|**Encrypt=no**|Сертификат сервера не проверяется.<br /><br />Данные, передаваемые между клиентом и сервером, не шифруются.|Сертификат сервера не проверяется.<br /><br />Данные, передаваемые между клиентом и сервером, не шифруются.|  
|**Encrypt=yes**|Сертификат сервера проверяется.<br /><br />Данные, передаваемые между клиентом и сервером, шифруются.<br /><br />Имя (или IP-адрес) в общем имени субъекта (CN) или альтернативном имени субъекта (SAN) в TLS/SSL-сертификате [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] должно точно совпадать с именем (или IP-адресом) сервера, указанным в строке подключения.|Сертификат сервера не проверяется.<br /><br />Данные, передаваемые между клиентом и сервером, шифруются.|  

По умолчанию зашифрованные соединения всегда проверяют сертификат сервера. Однако при подключении к серверу с самозаверяющим сертификатом также добавьте параметр `TrustServerCertificate`, чтобы обойти проверку сертификата по списку доверенных центров сертификации:  

```
Driver={ODBC Driver 17 for SQL Server};Server=ServerNameHere;Encrypt=YES;TrustServerCertificate=YES  
```  
  
TLS использует библиотеку OpenSSL. Следующая таблица содержит минимально поддерживаемые версии OpenSSL, а также расположения хранилища доверия сертификатов по умолчанию для каждой платформы:

|Платформа|Минимальная версия OpenSSL|Расположение хранилища доверия сертификатов по умолчанию|  
|------------|---------------------------|--------------------------------------------|
|Debian 10|1.1.1|/etc/ssl/certs|
|Debian 9|1.1.0|/etc/ssl/certs|
|Debian 8.71|1.0.1|/etc/ssl/certs|
|OS X 10.11, macOS 10.12–10.15|1.0.2|/usr/local/etc/openssl/certs|
|Red Hat Enterprise Linux 8|1.1.1|/etc/pki/tls/cert.pem|
|Red Hat Enterprise Linux 7|1.0.1|/etc/pki/tls/cert.pem|
|Red Hat Enterprise Linux 6|1.0.0-10|/etc/pki/tls/cert.pem|
|SUSE Linux Enterprise 15|1.1.0|/etc/ssl/certs|
|SUSE Linux Enterprise 11, 12|1.0.1|/etc/ssl/certs|
|Ubuntu 18.10, 19.04, 19.10, 20.04|1.1.1|/etc/ssl/certs|
|Ubuntu 18.04|1.1.0|/etc/ssl/certs|
|Ubuntu 16.04, 16.10, 17.10|1.0.2|/etc/ssl/certs|
|Ubuntu 14.04|1.0.1|/etc/ssl/certs|

Можно также указать шифрование в строке подключения с помощью параметра `Encrypt` при использовании **SQLDriverConnect** для подключения.

## <a name="adjusting-the-tcp-keep-alive-settings"></a>Настройка параметров поддержания активности TCP

Начиная с ODBC Driver версии 17.4, можно настроить частоту отправки драйвером пакетов проверки активности и их пересылки, если ответ не получен.
Чтобы настроить, добавьте следующие параметры в раздел драйвера в `odbcinst.ini` или в раздел имени DSN в `odbc.ini`. При подключении с помощью имени DSN драйвер будет использовать параметры в разделе имени DSN, если они есть. В противном случае или если подключение выполняется только со строкой подключения, драйвер будет использовать параметры из раздела драйвера в `odbcinst.ini`. Если параметр отсутствует в обоих расположениях, драйвер использует значение по умолчанию.

- `KeepAlive=<integer>` управляет частотой попыток протокола TCP проверить работоспособность неактивного подключения путем отправки пакета keep-alive. Значение по умолчанию — **30** секунд.

- `KeepAliveInterval=<integer>` определяет интервал, разделяющий повторные передачи пакета keep-alive, пока не происходит получение ответа.  Значение по умолчанию составляет **15** секунд.

## <a name="see-also"></a>См. также:

- [Установка Microsoft ODBC Driver for SQL Server в Linux](installing-the-microsoft-odbc-driver-for-sql-server.md)
- [Установка Microsoft ODBC Driver for SQL Server в macOS](install-microsoft-odbc-driver-sql-server-macos.md)
- [Указания по программированию](programming-guidelines.md)

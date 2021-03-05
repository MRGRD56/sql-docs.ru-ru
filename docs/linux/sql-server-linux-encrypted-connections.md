---
title: Шифрование соединений с SQL Server на Linux
description: SQL Server на Linux использует протокол TLS для шифрования данных, передаваемых по сети между клиентским приложением и экземпляром SQL Server.
ms.date: 06/29/2020
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
helpviewer_keywords:
- Linux, encrypted connections
ms.openlocfilehash: 1040fec65e6ffce580a33b6ef581c4f22a3903ef
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186418"
---
# <a name="encrypting-connections-to-sql-server-on-linux"></a>Шифрование соединений с SQL Server на Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на Linux может использовать протокол TLS для шифрования данных, передаваемых по сети между клиентским приложением и экземпляром [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] поддерживает одни и те же протоколы TLS в Windows и Linux: TLS 1.2, 1.1 и 1.0. Однако действия по настройке TLS зависят от конкретной операционной системы, где выполняется [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

## <a name="requirements-for-certificates"></a>Требования к сертификатам 
Прежде чем приступить к работе, нужно убедиться, что сертификаты соответствуют следующим требованиям.
- Текущее системное время должно превышать значение свойства сертификата "Действителен с" и не превышать значение свойства "Действителен до".
- Сертификат должен быть предназначен для проверки подлинности сервера. Для этого требуется задать свойству сертификата "Улучшенный ключ" значение "Проверка подлинности сервера (1.3.6.1.5.5.7.3.1)".
- Сертификат должен быть создан с помощью параметра KeySpec свойства AT_KEYEXCHANGE. Обычно в свойстве применения ключа сертификата (KEY_USAGE) также предусмотрено шифрование ключа (CERT_KEY_ENCIPHERMENT_KEY_USAGE).
- Свойство "Субъект" сертификата должно указывать, что общее имя (CN) совпадает с именем узла или полным доменным именем (FQDN) сервера. Примечание. Поддерживаются групповые сертификаты.

## <a name="configuring-the-openssl-libraries-for-use-optional"></a>Настройка библиотек OpenSSL для использования (необязательно)
В `/opt/mssql/lib/` каталоге можно создать символьные ссылки, указывающие, какие библиотеки `libcrypto.so` и `libssl.so` следует использовать для шифрования. Это удобно, если нужно заставить SQL Server использовать определенную версию OpenSSL, отличную от предоставленной системой для использования по умолчанию. Если эти символьные ссылки отсутствуют, SQL Server загрузит настроенные в системе библиотеки OpenSSL по умолчанию.

Эти символьные ссылки должны называться `libcrypto.so` и `libssl.so` и находиться в каталоге `/opt/mssql/lib/`.

## <a name="overview"></a>Обзор
Протокол TLS используется для шифрования подключений от клиентского приложения к [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. При правильной настройке TLS обеспечивает как конфиденциальность, так и целостность данных при взаимодействии клиента и сервера.  Подключения TLS могут инициироваться клиентом или сервером. 

## <a name="client-initiated-encryption"></a>Шифрование, инициированное клиентом 
- **Создание сертификата** (значение /CN должно соответствовать полному доменному имени вашего узла SQL Server)

> [!NOTE]
> В этом примере используется самозаверяющий сертификат, который не следует использовать в рабочих сценариях. Нужно использовать сертификаты ЦС.<br>
> Убедитесь в том, что папки, в которых сохраняются сертификаты и закрытые ключи, доступны для пользователя или группы mssql и для них задано разрешение 700 (drwx-----). Вы можете создать папки вручную с разрешением 700 (drwx------) и пользователем или группой mssql в качестве владельца либо с разрешением 755 (drwxr-xr-x) и другим пользователем в качестве владельца, но с сохранением доступа для группы пользователей mssql. Например, можно создать папку с именем sslcert по пути /var/opt/mssql/, а затем сохранить сертификат и закрытый ключ с разрешением 600 для файлов, как показано ниже. 

```bash
openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
sudo chown mssql:mssql mssql.pem mssql.key 
sudo chmod 600 mssql.pem mssql.key 
# in this case we are saving the certificate to the certs folder under /etc/ssl/ which has the following permission 755(drwxr-xr-x)
sudo mv mssql.pem /etc/ssl/certs/ drwxr-xr-x 
# in this case we are saving the private key to the private folder under /etc/ssl/ with permissions set to 755(drwxr-xr-x)
sudo mv mssql.key /etc/ssl/private/ 
```

- **Настройка SQL Server**

```bash
systemctl stop mssql-server 
sudo cat /var/opt/mssql/mssql.conf 
sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
sudo /opt/mssql/bin/mssql-conf set network.forceencryption 0 
systemctl start mssql-server 
```

- **Регистрация сертификата на клиентском компьютере (Windows, Linux или macOS)**

    -   Если вы используете сертификат, подписанный центром сертификации, на клиентский компьютер нужно скопировать сертификат центра сертификации (ЦС), а не сертификат пользователя. 
    -   Если вы используете самозаверяющий сертификат, просто скопируйте PEM-файл в следующие папки, соответствующие используемому дистрибутиву, и выполните команды, чтобы включить их. 
        - **Ubuntu**: скопируйте сертификат в `/usr/share/ca-certificates/`, переименуйте его расширение на CRT и используйте `dpkg-reconfigure ca-certificates`, чтобы включить его как сертификат системного ЦС. 
        - **RHEL**: скопируйте сертификат в `/etc/pki/ca-trust/source/anchors/` и используйте `update-ca-trust`, чтобы включить его в качестве системного сертификата ЦС.
        - **SUSE**: скопируйте сертификат в `/usr/share/pki/trust/anchors/` и используйте `update-ca-certificates`, чтобы включить его в качестве системного сертификата ЦС.
        - **Windows**:  импортируйте PEM-файл в качестве сертификата в раздел "Текущий пользователь" -> "Доверенные корневые центры сертификации" -> "Сертификаты".
        - **macOS**: 
           - скопируйте сертификат в `/usr/local/etc/openssl/certs`.
           - Выполните следующую команду, чтобы получить хэш-значение: `/usr/local/Cellar/openssl/1.0.2l/openssl x509 -hash -in mssql.pem -noout`.
           - Переименуйте сертификат в value. Например: `mv mssql.pem dc2dd900.0`. Убедитесь, что dc2dd900.0 указано в `/usr/local/etc/openssl/certs`.
    
-   **Примеры строк подключения** 

    - **[!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)]**   
  ![Диалоговое окно подключения SSMS](media/sql-server-linux-encrypted-connections/ssms-encrypt-connection.png "Диалоговое окно подключения SSMS")  
  
    - **SQLCMD** 

        `sqlcmd  -S <sqlhostname> -N -U sa -P '<YourPassword>'`

    - **ADO.NET** 

        `"Encrypt=True; TrustServerCertificate=False;"`

    - **ODBC** 

        `"Encrypt=Yes; TrustServerCertificate=no;"`

    - **JDBC** 

        `"encrypt=true; trustServerCertificate=false;"`

## <a name="server-initiated-encryption"></a>Шифрование, инициированное сервером 

- **Создание сертификата** (значение /CN должно соответствовать полному доменному имени вашего узла SQL Server)

```bash
openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
sudo chown mssql:mssql mssql.pem mssql.key 
sudo chmod 600 mssql.pem mssql.key   
sudo mv mssql.pem /etc/ssl/certs/ 
sudo mv mssql.key /etc/ssl/private/ 
```

- **Настройка SQL Server**

```bash
systemctl stop mssql-server 
sudo cat /var/opt/mssql/mssql.conf 
sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
sudo /opt/mssql/bin/mssql-conf set network.forceencryption 1
systemctl start mssql-server 
```

-   **Примеры строк подключения** 

    - **SQLCMD**

        `sqlcmd  -S <sqlhostname> -U sa -P '<YourPassword>'`

    - **ADO.NET** 

        `"Encrypt=False; TrustServerCertificate=False;"`

    - **ODBC** 

        `"Encrypt=no; TrustServerCertificate=no;"`

    - **JDBC** 

        `"encrypt=false; trustServerCertificate=false;"`

> [!NOTE]
> Задайте для **TrustServerCertificate** значение true, если клиенту не удается подключиться к ЦС для проверки подлинности сертификата.

## <a name="common-connection-errors"></a>Распространенные ошибки при подключении  

|Сообщение об ошибке |Fix |
|--- |--- |
|Цепочка сертификатов не была выпущена доверенным центром сертификации.  |Эта ошибка возникает, когда клиенты не могут проверить подпись сертификата, представленного SQL Server во время подтверждения TLS. Убедитесь, что клиент доверяет напрямую сертификату [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] или доверяет ЦС, подписавшему этот сертификат SQL Server. |
|Неправильное имя целевого субъекта.  |Убедитесь, что значение в поле общего имени в сертификате SQL Server соответствует имени сервера, указанному в строке подключения клиента. |  
|существующее соединение было принудительно завершено удаленным узлом. |Эта ошибка может возникать, если клиент не поддерживает версию протокола TLS, необходимую SQL Server. Например, если параметр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] настроен для использования TLS 1.2, убедитесь, что клиенты также поддерживают протокол TLS 1.2. |
| | |   

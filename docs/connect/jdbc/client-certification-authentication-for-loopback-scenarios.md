---
description: Проверка подлинности сертификата клиента для сценариев замыкания на себя
title: Проверка подлинности сертификата клиента для сценариев замыкания на себя | Документация Майкрософт
ms.custom: ''
ms.date: 07/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: peterbae
ms.author: v-hyba
ms.openlocfilehash: 168034034e5f2abe53bd47029b99fa78c250336b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836971"
---
# <a name="client-certificate-authentication-for-loopback-scenarios"></a>Проверка подлинности сертификата клиента для сценариев замыкания на себя

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

В SQL Server 2016 была добавлена новая хранимая процедура с именем sp_execute_external_script (SPEES). Эта хранимая процедура позволяет SQL Server запускать и выполнять внешний сценарий за пределами SQL Server как расширение. В ней предусмотрена поддержка скриптов R и Python, в которых есть библиотеки, которые могут использовать драйвер JDBC для подключения к SQL Server. Хотя SQL Servers в Windows может использовать встроенную проверку подлинности Windows для проверки подлинности этих подключений с теми же учетными данными, что и пользователь, инициировавший запрос, SQL Server в Linux не может делать то же самое. Поэтому в такой ситуации добавляется проверка подлинности сертификата клиента, чтобы разрешить пользователям проходить проверку подлинности с помощью сертификата и ключа.

## <a name="connecting-using-client-certificate-authentication"></a>Подключение с помощью проверки подлинности с помощью сертификата клиента

Драйвер JDBC добавляет три свойства подключения для этой функции:

* clientCertificate — определяет сертификат, который будет использоваться для аутентификации. Драйвер JDBC будет поддерживать расширения файлов PFX, PEM, DER и CER.

Формат
```
clientCertificate=<file_location>
``` 
Этот драйвер использует файл сертификата. Для сертификатов в форматах PEM, DER и CER требуется атрибут clientKey. Расположение файла может быть относительным или абсолютным.
 
* clientKey — указывает расположение файла закрытого ключа для сертификатов PEM, DER или CER, заданных атрибутом clientCertificate.

Формат
```
clientKey=<file_location>
```
Указывает расположение файла закрытого ключа. Если файл закрытого ключа защищен паролем, необходимо ввести пароль-ключ. Расположение файла может быть относительным или абсолютным.

* clientKeyPassword — необязательная строка пароля для доступа к закрытому ключу файла clientKey.

Эта функция официально поддерживается только для сценариев проверки подлинности на базе Linux SQL Server 2019 и выше.

## <a name="see-also"></a>См. также раздел

[Подключение к SQL Server с помощью JDBC Driver](../../connect/jdbc/connecting-to-sql-server-with-the-jdbc-driver.md)  
[sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)

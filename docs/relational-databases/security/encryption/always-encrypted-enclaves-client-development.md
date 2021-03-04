---
description: Разработка приложений с помощью Always Encrypted с безопасными анклавами
title: Разработка приложений с помощью Always Encrypted с безопасными анклавами | Документация Майкрософт
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
dev_langs:
- CSharp
ms.assetid: 9595eb66-284c-4474-828f-8961a05ce989
author: jaszymas
ms.author: jaszymas
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 14bfbdd1a9c6bd176c89513674c5680388ef1452
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838932"
---
# <a name="develop-applications-using-always-encrypted-with-secure-enclaves"></a>Разработка приложений с помощью Always Encrypted с безопасными анклавами
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted с безопасными анклавами](always-encrypted-enclaves.md) расширяет возможности [Always Encrypted](always-encrypted-database-engine.md) для реализации более широких возможностей запросов приложений к зашифрованным столбцам базы данных. Используемые технологии безопасных анклавов позволяют исполнителю запросов в [!INCLUDE[ssde-md](../../../includes/ssde-md.md)] делегировать вычисления в зашифрованных столбцах безопасному анклаву внутри процесса [!INCLUDE[ssde-md](../../../includes/ssde-md.md)].

## <a name="prerequisites"></a>Предварительные требования

- Экземпляр [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] или база данных и сервер в [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] должны быть правильно настроены для поддержки анклавов и аттестации. Дополнительные сведения см. в статье [Настройка безопасного анклава и аттестации](configure-always-encrypted-enclaves.md#set-up-the-secure-enclave-and-attestation).
- Необходимо получить URL-адрес аттестации для имеющейся среды у администратора службы аттестации.

  - Если вы используете [!INCLUDE[ssnoversion-md](../../../includes/ssnoversion-md.md)] и службу защитника узлов (HGS), см. сведения в разделе об [определении и совместном использовании URL-адреса аттестации HGS](../../../relational-databases/security/encryption/always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Если вы используете [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] и Аттестацию Microsoft Azure, см. сведения об [определении URL-адреса аттестации для политики аттестации](./always-encrypted-enclaves.md?view=sql-server-ver15#secure-enclave-attestation).

- Приложение должно использовать версию драйвера клиента SQL, поддерживающую безопасные анклавы. Дополнительные сведения см. в разделах ниже.

- Для подключения к базе данных необходимо настроить протокол аттестации и URL-адрес аттестации. Сведения о настройке протокола аттестации и URL-адреса аттестации зависят от используемого драйвера клиента.

## <a name="client-drivers-for-always-encrypted-with-secure-enclaves"></a>Драйверы клиента для Always Encrypted с безопасными анклавами

Для разработки приложений с помощью Always Encrypted с защищенными анклавами требуется версия драйвера клиента SQL, поддерживающая безопасные анклавы. Драйвер клиента играет следующую ключевую роль.

- Перед отправкой запроса, который использует безопасный анклав, в [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] для выполнения, драйвер инициирует аттестацию анклава для проверки его подлинности и безопасной работы для обработки конфиденциальных данных. Дополнительные сведения об аттестации см. в статье [Аттестация безопасного анклава](always-encrypted-enclaves.md#secure-enclave-attestation).
- После завершения аттестации драйвер клиента устанавливает защищенный сеанс с анклавом путем согласования общего секрета.
- Драйвер использует общий секрет для шифрования ключей шифрования столбцов, которые потребуются анклаву для обработки запроса, и отправляет ключи [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], который пересылает их в безопасный анклав для расшифровки. 
- Наконец, драйвер отправляет запрос для выполнения, который активирует вычисления в безопасном анклаве.

## <a name="next-steps"></a>Дальнейшие действия

Следующие драйверы клиента поддерживают Always Encrypted с безопасными анклавами.

- Поставщик данных Microsoft .NET для SQL Server в .NET Framework 4.6 или более поздней версии и .NET Core 2.1 или более поздней версии. 
    - Дополнительные сведения см. в статье [Использование Always Encrypted с поставщиком данных Microsoft .NET для SQL Server](../../../connect/ado-net/sql/sqlclient-support-always-encrypted.md).
    - Пошаговое руководство см. в статье [Учебник. Develop a .NET application using Always Encrypted with secure enclaves](../../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md) (Разработка приложения NET с помощью Always Encrypted с безопасными анклавами).
    - См. также статью [Пример использования поставщика Azure Key Vault с поддержкой Always Encrypted с безопасными анклавами](../../../connect/ado-net/sql/azure-key-vault-enclave-example.md).
- Microsoft ODBC Driver for SQL Server версии 17.4 или выше. 
    - Дополнительные сведения см. в статье [Использование функции Always Encrypted с драйвером ODBC](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md). 
    - Сведения о включении вычислений анклава для подключения к базе данных с помощью ODBC см. в разделе [Включение Always Encrypted с безопасными анклавами](../../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md#enabling-always-encrypted-with-secure-enclaves).
- Microsoft ODBC Driver for SQL Server версии 8.2 или выше.
    - Дополнительные сведения см. в статье [Использование Always Encrypted с безопасными анклавами с драйвером JDBC](../../../connect/jdbc/using-always-encrypted-with-secure-enclaves-with-the-jdbc-driver.md).
- Поставщик данных .NET Framework для SQL Server в .NET Framework 4.7.2 и выше. 
    - Дополнительные сведения см. в статье [Использование Always Encrypted с поставщиком данных .NET Framework для SQL Server](../../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md).
    - Пошаговое руководство см. в статье [Учебник. Разработка приложения .NET Framework с помощью Always Encrypted с безопасными анклавами](../tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)

## <a name="see-also"></a>См. также раздел

- [Устранение распространенных неполадок Always Encrypted с безопасными анклавами](always-encrypted-enclaves-troubleshooting.md)
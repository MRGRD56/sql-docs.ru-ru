---
description: Узнайте, как использовать драйвер JDBC для SQL Server в режиме FIPS, чтобы обеспечить для приложения соответствие требованиям FIPS 140.
title: Режим FIPS в JDBC
ms.custom: ''
ms.date: 03/21/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: genemi
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f11527e1b6e5917ed7b74624dc5e92ed39c27047
ms.sourcegitcommit: ebe81e2daa544f41c8ababb66a91c218ad0c2a0a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/01/2021
ms.locfileid: "106177005"
---
# <a name="fips-mode"></a>Режим FIPS

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Microsoft JDBC Driver for SQL Server поддерживает запуск в виртуальных машинах Java, настроенных на *соответствие FIPS 140*.

## <a name="prerequisites"></a>Предварительные требования

- Виртуальная машина Java с конфигурацией FIPS.
- Соответствующий TLS/SSL-сертификат.
- Соответствующие файлы политики.
- Соответствующие параметры конфигурации.

## <a name="fips-configured-jvm"></a>Виртуальная машина Java с конфигурацией FIPS

Как правило, приложения могут настроить файл `java.security` для использования поставщиков шифрования, соответствующих FIPS. Сведения об обеспечении соответствия FIPS 140 см. в документации по вашей виртуальной машине Java.

Чтобы просмотреть утвержденные модули для конфигурации FIPS, ознакомьтесь с [этой статьей](https://csrc.nist.gov/Projects/cryptographic-module-validation-program/Validated-Modules).

Поставщики могут выполнять некоторые дополнительные действия по настройке виртуальной машины Java в соответствии с FIPS.

## <a name="appropriate-tls-certificate"></a>Соответствующий TLS-сертификат

Чтобы подключиться к SQL Server в режиме FIPS, требуется действительный TLS/SSL-сертификат. Установите или импортируйте его в хранилище ключей Java на клиентском компьютере (виртуальная машина Java), где включен режим FIPS.

### <a name="importing-tls-certificate-in-java-keystore"></a>Импорт TLS-сертификата в хранилище ключей Java

Для FIPS, скорее всего, потребуется импортировать сертификат (CERT) в формате PKCS или в формате, характерном для поставщика.
Используйте следующий фрагмент кода, чтобы импортировать TLS/SSL-сертификат и сохранить его в рабочем каталоге в соответствующем формате хранилища ключей. _TRUST\_STORE\_PASSWORD_ — пароль для хранилища ключей Java.

```java
public void saveGenericKeyStore(
        String provider,
        String trustStoreType,
        String certName,
        String certPath
        ) throws KeyStoreException, CertificateException,
            NoSuchAlgorithmException, NoSuchProviderException,
            IOException
{
    KeyStore ks = KeyStore.getInstance(trustStoreType, provider);
    FileOutputStream os = new FileOutputStream("./MyTrustStore_" + trustStoreType);
    ks.load(null, null);
    ks.setCertificateEntry(certName, getCertificate(certPath));
    ks.store(os, TRUST_STORE_PASSWORD.toCharArray());
    os.flush();
    os.close();
}

private Certificate getCertificate(String pathName)
        throws FileNotFoundException, CertificateException
{
    FileInputStream fis = new FileInputStream(pathName);
    CertificateFactory cf = CertificateFactory.getInstance("X.509");
    return cf.generateCertificate(fis);
}
```

В следующем примере импортируется TLS/SSL-сертификат Azure в формате PKCS12 с помощью поставщика BouncyCastle. Сертификат импортируется в рабочий каталог с именем _MyTrustStore\_PKCS12_ с помощью следующего фрагмента кода:

`saveGenericKeyStore(BCFIPS, PKCS12, "SQLAzure SSL Certificate Name", "SQLAzure.cer");`

## <a name="appropriate-policy-files"></a>Соответствующие файлы политики

Для некоторых поставщиков FIPS требуется неограниченная политика JAR-файлов. В таких случаях скачайте для Sun и Oracle файлы политики юрисдикции неограниченной стойкости Java Cryptography Extension (JCE) для [JRE 8](https://www.oracle.com/technetwork/java/javase/downloads/jce8-download-2133166.html) или [JRE 7](https://www.oracle.com/technetwork/java/javase/downloads/jce-7-download-432124.html).

## <a name="appropriate-configuration-parameters"></a>Соответствующие параметры конфигурации

Чтобы запустить драйвер JDBC в режиме, соответствующем FIPS, настройте свойства подключения, как показано в следующей таблице.

### <a name="properties"></a>Свойства

|Свойство|Тип|По умолчанию|Описание|Примечания|
|---|---|---|---|---|
|`encrypt`|логическое значение ["true / false"]|"false"|Свойство для шифрования виртуальной машины Java с поддержкой FIPS должно иметь значение **true**.||
|`TrustServerCertificate`|логическое значение ["true / false"]|"false"|Для FIPS пользователь должен проверить цепочку сертификатов, поэтому ему нужно использовать значение **false** для этого свойства. ||
|`trustStore`|Строка|null|Путь к файлу хранилища ключей Java, куда был импортирован сертификат. Если установить сертификат в системе, не нужно ничего передавать. Драйвер использует файлы cacerts или jssecacerts.||
|`trustStorePassword`|Строка|null|Пароль, используемый для проверки целостности данных trustStore.||
|`fips`|логическое значение ["true / false"]|"false"|Свойство для виртуальной машины Java с поддержкой FIPS должно иметь значение **true**.|Добавлено в версии 6.1.4 (стабильный выпуск 6.2.2).|
|`fipsProvider`|Строка|null|Поставщик FIPS, настроенный в виртуальной машине Java. Например, BCFIPS или SunPKCS11-NSS. |Добавлено в версии 6.1.2 (стабильный выпуск 6.2.2), не рекомендуется в версии 6.4.0. Дополнительные сведения см. в [этом блоге](https://github.com/Microsoft/mssql-jdbc/pull/460).|
|`trustStoreType`|Строка|JKS|Для режима FIPS задайте для хранилища доверия тип PKCS12 или тип, определенный поставщиком FIPS. |Добавлено в версии 6.1.2 (стабильный выпуск 6.2.2).|

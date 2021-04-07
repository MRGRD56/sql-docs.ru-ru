---
description: Справочник по API Always Encrypted для JDBC Driver
title: Сведения об API-интерфейсах функции Always Encrypted в драйвере JDBC и о том, как их можно использовать для шифрования и защиты данных в приложении Java.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 6962a2aa-9508-4d4f-a78c-905e2bc68615
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d44b4b4858b71af4e633f7057ca77fd37ea0ce75
ms.sourcegitcommit: 524a0f0cc9533188f4b14d2e78ba1cfe816b3b9a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/27/2021
ms.locfileid: "105633226"
---
# <a name="always-encrypted-api-reference-for-the-jdbc-driver"></a>Справочник по API Always Encrypted для JDBC Driver

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Функция Always Encrypted позволяет клиентам шифровать конфиденциальные данные в клиентских приложениях, не раскрывая ключи шифрования серверу. Драйвер с поддержкой Always Encrypted, установленный на клиентском компьютере, реализует эту функцию за счет автоматического шифрования и расшифровки конфиденциальных данных в клиентском приложении.

Драйвер шифрует данные из конфиденциальных столбцов перед их передачей в SQL Server и автоматически переписывает запросы, чтобы сохранить семантику приложения. Аналогичным образом драйвер прозрачно расшифровывает данные, хранящиеся в столбцах зашифрованной базы данных, которые содержатся в результатах запроса. Дополнительные сведения в статьях [Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md) и [Using Always Encrypted with the JDBC driver](using-always-encrypted-with-the-jdbc-driver.md) (Использование функции Always Encrypted с драйвером JDBC).

> [!NOTE]
> Always Encrypted поддерживается только Microsoft JDBC Driver 6.0 или более поздних версий для SQL Server с Базой данных SQL Azure для SQL Server 2016 или более поздних версий.

## <a name="always-encrypted-api-references"></a>Справочники по API Always Encrypted

Существует несколько дополнений и изменений для API драйвера JDBC для использования в клиентских приложениях, использующих функцию Always Encrypted.

### <a name="sqlserverconnection-class"></a>SQLServerConnection, класс

|Имя|Описание|
|----------|-----------------|
|Новое ключевое слово строки подключения:<br /><br /> columnEncryptionSetting|columnEncryptionSetting=Enabled включает функцию Always Encrypted для подключения, а columnEncryptionSetting=Disabled отключает ее. Допустимыми являются значения Enabled/Disabled. По умолчанию установлено значение Disabled.|
|Новое ключевое слово строки подключения (MS JDBC 7.4 и более поздних версий)<br /><br /> keyVaultProviderClientId <br /><br /> keyVaultProviderClientKey |keyVaultProviderClientId=\<ClientID>;keyVaultProviderClientKey=\<ClientKey> <br/><br/> Регистрирует SQLServerColumnEncryptionAzureKeyVaultProvider и использует значения ClientID и ClientKey для получения главного ключа столбца из Azure Key Vault.|
|Новые методы:<br /><br />`public static void setColumnEncryptionTrustedMasterKeyPaths(Map<String, List\<String>> trustedKeyPaths)`<br /><br />`public static void updateColumnEncryptionTrustedMasterKeyPaths(String server, List\<String> trustedKeyPaths)`<br /><br />`public static void removeColumnEncryptionTrustedMasterKeyPaths(String server)`|Позволяет задавать, обновлять и удалять список доверенных путей ключа для сервера базы данных. Если при обработке запроса приложения драйвер получает путь ключа, которого нет в списке, запрос завершается с ошибкой. Это свойство обеспечивает дополнительную защиту от атак на систему безопасности, включающих отправку скомпрометированным сервером фиктивных путей ключа, что может привести к утечке учетных данных хранилища ключей.|
|Новый метод:<br /><br />`public static Map<String, List\<String>> getColumnEncryptionTrustedMasterKeyPaths()`|Возвращает список доверенных путей ключа для сервера базы данных.|
|Новый метод:<br /><br />`public static void registerColumnEncryptionKeyStoreProviders (Map\<String, SQLServerColumnEncryptionKeyStoreProvider> clientKeyStoreProviders)`|Позволяет регистрировать пользовательские поставщики хранилища ключей. Это словарь, сопоставляющий имена поставщиков хранилища ключей с реализациями поставщиков хранилища ключей.<br /><br /> Для использования хранилища ключей виртуальной машины Java необходимо создать экземпляр объекта SQLServerColumnEncryptionJVMKeyStoreProvider с помощью учетных данных хранилища ключей виртуальной машины Java и зарегистрировать его с помощью драйвера. Имя этого поставщика должно быть в виде MSSQL_JVM_KEYSTORE.<br /><br /> Чтобы использовать хранилище Azure Key Vault, необходимо создать экземпляр объекта SQLServerColumnEncryptionAzureKeyStoreProvider и зарегистрировать его с помощью драйвера. Имя этого поставщика должно быть AZURE_KEY_VAULT.|
|Новый метод:<br /><br />`public static void unregisterColumnEncryptionKeyStoreProviders (Map\<String, SQLServerColumnEncryptionKeyStoreProvider> clientKeyStoreProviders)`|Позволяет отменить регистрацию всех поставщиков пользовательских хранилищ ключей путем очистки словаря, в котором имена поставщиков хранилищ ключей сопоставляются с реализациями этих поставщиков.|
|`public final boolean getSendTimeAsDatetime()`|Возвращает значение свойства подключения sendTimeAsDatetime.|
|`public void setSendTimeAsDatetime(boolean sendTimeAsDateTimeValue)`|Изменяет значение свойства подключения sendTimeAsDatetime.|

### <a name="sqlserverconnectionpoolproxy-class"></a>Класс SQLServerConnectionPoolProxy

|Имя|Описание|
|----------|-----------------|
|`public final boolean getSendTimeAsDatetime()` | Возвращает значение свойства подключения sendTimeAsDatetime.|
|`public void setSendTimeAsDatetime(boolean sendTimeAsDateTimeValue)` | Изменяет значение свойства подключения sendTimeAsDatetime.|

### <a name="sqlserverdatasource-class"></a>SQLServerDataSource, класс

|Имя|Описание|
|----------|-----------------|
|`public void setColumnEncryptionSetting(String columnEncryptionSetting)`|Включает или отключает функцию Always Encrypted для объекта источника данных.<br /><br /> По умолчанию установлено значение Disabled.|
|`public String getColumnEncryptionSetting()`|Получает параметр функции Always Encrypted для объекта источника данных.|
|`public void setKeyStoreAuthentication(String keyStoreAuthentication)`|Задает имя, идентифицирующее хранилище ключей. Поддерживается только значение **JavaKeyStorePassword** для идентификации хранилища ключей Java.<br/><br/>Значение по умолчанию — NULL.|
|`public String getKeyStoreAuthentication()`|Возвращает значение параметра keyStoreAuthentication для объекта источника данных.|
|`public void setKeyStoreSecret(String keyStoreSecret)`|Задает пароль для хранилища ключей Java. Пароль для хранилища ключей и ключ должны совпадать. Для keyStoreAuthentication необходимо задать **JavaKeyStorePassword**.|
|`public void setKeyStoreLocation(String keyStoreLocation)`|Задает расположение, включая имя файла для хранилища ключей Java. Для keyStoreAuthentication необходимо задать **JavaKeyStorePassword**.|
|`public String getKeyStoreLocation()`|Извлекает keyStoreLocation для хранилища ключей Java.|

### <a name="sqlservercolumnencryptionjavakeystoreprovider-class"></a>Класс SQLServerColumnEncryptionJavaKeyStoreProvider

Реализация поставщика хранилища ключей для хранилища ключей Java. Этот класс позволяет использовать сертификаты, находящиеся в хранилище ключей Java, в качестве главных ключей столбца.

**Конструкторы**

|Имя|Описание|
|----------|-----------------|
|`public SQLServerColumnEncryptionJavaKeyStoreProvider (String keyStoreLocation, char[] keyStoreSecret)`|Поставщик хранилища ключей для хранилища ключей Java.|

**Методы**

|Имя|Описание|
|----------|-----------------|
|`public byte[] decryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte[] encryptedColumnEncryptionKey)`|Расшифровывает указанное зашифрованное значение ключа шифрования столбца. Зашифрованное значение должно быть зашифровано с помощью сертификата по указанному пути ключа и с использованием указанного алгоритма.<br /><br /> **Путь ключа должен иметь один из следующих форматов:**<br /><br /> Отпечаток:<отпечаток_сертификата><br /><br /> Псевдоним:<псевдоним_сертификата><br /><br /> (Переопределяет `SQLServerColumnEncryptionKeyStoreProvider`. decryptColumnEncryptionKey(String, String, Byte[]).)|
|`public byte[] encryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte[] plainTextColumnEncryptionKey)`|Шифрует ключ шифрования столбца с помощью сертификата по указанному пути ключа и с использованием указанного алгоритма.<br /><br /> **Путь ключа должен иметь один из следующих форматов:**<br /><br /> Отпечаток:<отпечаток_сертификата><br /><br /> Псевдоним:<псевдоним_сертификата><br /><br /> (Переопределяет `SQLServerColumnEncryptionKeyStoreProvider`. encryptColumnEncryptionKey(String, String, Byte[]).)|
|`public boolean verifyColumnEncryptionKey (String masterKeyPath, boolean allowEnclaveComputations, byte[] signature)`|Проверяет подпись ключа шифрования столбца с помощью сертификата.<br /><br /> **Путь ключа должен иметь один из следующих форматов:**<br /><br /> Отпечаток:<отпечаток_сертификата><br /><br /> Псевдоним:<псевдоним_сертификата><br /><br /> (Переопределяет `SQLServerColumnEncryptionKeyStoreProvider`. verifyColumnEncryptionKey(String, boolean, Byte[]).)|
|`public void setName (String name)`|Указывает имя поставщика хранилища ключей.|
|`public String getName ()`|Возвращает имя поставщика хранилища ключей.|

### <a name="sqlservercolumnencryptionazurekeyvaultprovider-class"></a>Класс SQLServerColumnEncryptionAzureKeyVaultProvider

Реализация поставщика хранилища ключей для Azure Key Vault. Этот класс позволяет использовать ключи, находящиеся в Azure Key Vault, в качестве главных ключей столбца.

**Конструкторы**

|Имя|Описание:|
|----------|-----------------|
|`public SQLServerColumnEncryptionAzureKeyVaultProvider ()`|Формирует SQLServerColumnEncryptionAzureKeyVaultProvider для проверки подлинности в Azure Key Vault.|
|`public SQLServerColumnEncryptionAzureKeyVaultProvider (String clientId)`|Формирует SQLServerColumnEncryptionAzureKeyVaultProvider для проверки подлинности в Azure Key Vault, используя идентификатор клиента, запрашивающего маркер.|
|`public SQLServerColumnEncryptionAzureKeyVaultProvider (String clientId, String clientKey)`|Формирует SQLServerColumnEncryptionAzureKeyVaultProvider для проверки подлинности в Azure Key Vault, используя идентификатор и ключ клиента, запрашивающего маркер.|
|`public SQLServerColumnEncryptionAzureKeyVaultProvider (TokenCredential tokenCredential)`|Формирует SQLServerColumnEncryptionAzureKeyVaultProvider для проверки подлинности в Azure Key Vault, используя предоставленное значение TokenCredential.|

**Методы**

|Имя|Описание:|
|----------|-----------------|
|`public byte[] decryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte[] encryptedColumnEncryptionKey)` | Расшифровывает ключ шифрования зашифрованного столбца (CEK). Расшифровка выполняется с помощью алгоритма шифрования RSA, который использует асимметричный ключ, заданный с помощью пути к главному ключу.<br />(Переопределяет `SQLServerColumnEncryptionKeyStoreProvider`. decryptColumnEncryptionKey(String, String, Byte[]).) |
| `public byte[] encryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte[] columnEncryptionKey)` | Шифрует ключ шифрования столбца, присваивая указанный главный ключ столбца указанному алгоритму.<br />(Переопределяет `SQLServerColumnEncryptionKeyStoreProvider`. encryptColumnEncryptionKey(String, String, Byte[]).) |
|`public void setName (String name)`|Указывает имя поставщика хранилища ключей.|
|`public String getName ()`|Возвращает имя поставщика хранилища ключей.|

### <a name="sqlserverkeyvaultauthenticationcallback-interface"></a>Интерфейс SQLServerKeyVaultAuthenticationCallback

Этот интерфейс содержит один метод для проверки подлинности Azure Key Vault, который должен быть реализован пользователем.

**Методы**

|Имя|Описание|
|----------|-----------------|
|`public String getAccessToken(String authority, String resource, String scope);`|Этот метод необходимо переопределить. Метод используется для получения маркера доступа к Azure Key Vault.|

### <a name="sqlservercolumnencryptionkeystoreprovider-class"></a>Класс SQLServerColumnEncryptionKeyStoreProvider

Расширьте этот класс для реализации пользовательского поставщика хранилища ключей.

|name|Описание|
|----------|-----------------|
|SQLServerColumnEncryptionKeyStoreProvider|Базовый класс для всех поставщиков хранилища ключей. Пользовательский поставщик должен наследовать от этого класса и переопределять его функции-члены, а затем регистрировать его с помощью SQLServerConnection. registerColumnEncryptionKeyStoreProviders().|

**Методы**

|Имя|Описание|
|----------|-----------------|
|`public abstract byte[] decryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte [] encryptedColumnEncryptionKey)`|Метод базового класса для расшифровки указанного зашифрованного значения ключа шифрования столбца. Зашифрованное значение должно быть зашифровано с помощью главного ключа столбца по указанному пути ключа и с помощью указанного алгоритма.|
|`public abstract byte[] encryptColumnEncryptionKey (String masterKeyPath, String encryptionAlgorithm, byte[]  columnEncryptionKey)`|Метод базового класса для шифрования ключа шифрования столбца с помощью главного ключа столбца по указанному пути ключа и с помощью указанного алгоритма.|
|`public abstract void setName(String name)`|Указывает имя поставщика хранилища ключей.|
|`public abstract String getName()`|Возвращает имя поставщика хранилища ключей.|

### <a name="new-or-overloaded-methods-in-sqlserverpreparedstatement-class"></a>Новые или перегруженные методы в классе SQLServerPreparedStatement.

|Имя|Описание|
|----------|-----------------|
|`public void setBigDecimal(int parameterIndex, BigDecimal x, int precision, int scale)`<br /><br /> `public void setObject(int parameterIndex, Object x, int targetSqlType, Integer precision, int scale)`<br /><br /> `public void setObject(int parameterIndex, Object x, SQLType targetSqlType, Integer precision, Integer scale)`<br /><br /> `public void setTime(int parameterIndex, java.sql.Time x, int scale)`<br /><br /> `public void setTimestamp(int parameterIndex, java.sql.Timestamp x, int scale)` <br />`public void setDateTimeOffset(int parameterIndex, microsoft.sql.DateTimeOffset x, int scale)`|Эти методы перегружаются с помощью аргумента точности или масштабирования или и того, и другого, чтобы поддерживать функцию Always Encrypted для конкретных типов данных, для которых требуются сведения о точности и масштабировании.|
|`public void setMoney(int parameterIndex, BigDecimal x)`<br /><br /> `public void setSmallMoney(int parameterIndex, BigDecimal x)`<br /><br /> `public void setUniqueIdentifier(int parameterIndex, String guid)`<br /><br /> `public void setDateTime(int parameterIndex, java.sql.Timestamp x)`<br /><br /> `public void setSmallDateTime(int parameterIndex, java.sql.Timestamp x)`|Эти методы обеспечивают поддержку функции Always Encrypted в типах данных money, smallmoney, uniqueidentifier, datetime и smalldatetime. <br/><br/>Существующий метод `setTimestamp()` используется для отправки значений параметров в зашифрованный столбец datetime2. Для зашифрованных столбцов datetime и smalldatetime используются новые методы `setDateTime()` и `setSmallDateTime()` соответственно.|
|`public final void setBigDecimal(int parameterIndex, BigDecimal x, int precision, int scale, boolean forceEncrypt)`<br /><br /> `public final void setMoney(int parameterIndex, BigDecimal x, boolean forceEncrypt)`<br /><br /> `public final void setSmallMoney(int parameterIndex, BigDecimal x, boolean forceEncrypt)`<br /><br /> `public final void setBoolean(int parameterIndex, boolean x, boolean forceEncrypt)`<br /><br /> `public final void setByte(int parameterIndex, byte x, boolean forceEncrypt)`<br /><br /> `public final void setBytes(int parameterIndex, byte x[], boolean forceEncrypt)`<br /><br /> `public final void setUniqueIdentifier(int parameterIndex, String guid, boolean forceEncrypt)`<br /><br /> `public final void setDouble(int parameterIndex, double x, boolean forceEncrypt)`<br /><br /> `public final void setFloat(int parameterIndex, float x, boolean forceEncrypt)`<br /><br /> `public final void setInt(int parameterIndex, int value, boolean forceEncrypt)`<br /><br /> `public final void setLong(int parameterIndex, long x, boolean forceEncrypt)`<br /><br /> `public final setObject(int parameterIndex, Object x, int targetSqlType, Integer precision, int scale, boolean forceEncrypt)`<br /><br /> `public final void setObject(int parameterIndex, Object x, SQLType targetSqlType, Integer precision, Integer scale, boolean forceEncrypt)`<br /><br /> `public final void setShort(int parameterIndex, short x, boolean forceEncrypt)`<br /><br /> `public final void setString(int parameterIndex, String str, boolean forceEncrypt)`<br /><br /> `public final void setNString(int parameterIndex, String value, boolean forceEncrypt)`<br /><br /> `public final void setTime(int parameterIndex, java.sql.Time x, int scale, boolean forceEncrypt)`<br /><br /> `public final void setTimestamp(int parameterIndex, java.sql.Timestamp x, int scale, boolean forceEncrypt)`<br /><br /> `public final void setDateTimeOffset(int parameterIndex, microsoft.sql.DateTimeOffset x, int scale, boolean forceEncrypt)`<br /><br /> `public final void setDateTime(int parameterIndex, java.sql.Timestamp x, boolean forceEncrypt)`<br /><br /> `public final void setSmallDateTime(int parameterIndex, java.sql.Timestamp x, boolean forceEncrypt)`<br /><br /> `public final void setDate(int parameterIndex, java.sql.Date x, java.util.Calendar cal, boolean forceEncrypt)`<br /><br /> `public final void setTime(int parameterIndex, java.sql.Time x, java.util.Calendar cal, boolean forceEncrypt)`<br /><br /> `public final void setTimestamp(int parameterIndex, java.sql.Timestamp x, java.util.Calendar cal, boolean forceEncrypt)`|Присваивает именованному параметру указанное значение типа Java.<br /><br /> Если логическому параметру `forceEncrypt` задано значение true, параметр запроса будет установлен только в том случае, если назначенный столбец является зашифрованным, а функция Always Encrypted включена в подключении или в инструкции.<br /><br /> Если для логического параметра forceEncrypt задано значение false, драйвер не будет принудительно выполнять шифрование параметров.|

### <a name="new-or-overloaded-methods-in-sqlservercallablestatement-class"></a>Новые или перегруженные методы в классе SQLServerCallableStatement

|Имя|Описание|
|----------|-----------------|
|`public void registerOutParameter(int parameterIndex, int sqlType, int precision, int scale)`<br /><br /> `public void registerOutParameter(int parameterIndex, SQLType sqlType, int precision, int scale)`<br /><br /> `public void registerOutParameter(String parameterName, int sqlType, int precision, int scale)`<br /><br /> `public void registerOutParameter(String parameterName, SQLType sqlType, int precision, int scale)`<br />`public void setBigDecimal(String parameterName, BigDecimal bd, int precision, int scale)`<br /><br /> `public void setTime(String parameterName, java.sql.Time t, int scale)`<br /><br /> `public void setTimestamp(String parameterName, java.sql.Timestamp t, int scale)`<br /><br /> `public void setDateTimeOffset(String parameterName, microsoft.sql.DateTimeOffset t, int scale)`<br/><br/>`public final void setObject(String sCol, Object x, int targetSqlType, Integer precision, int scale)`|Эти методы перегружаются с помощью аргумента точности или масштабирования или и того, и другого, чтобы поддерживать функцию Always Encrypted для конкретных типов данных, для которых требуются сведения о точности и масштабировании.|
|`public void setDateTime(String parameterName, java.sql.Timestamp x)`<br /><br /> `public void setSmallDateTime(String parameterName, java.sql.Timestamp x)`<br /><br /> `public void setUniqueIdentifier(String parameterName, String guid)`<br /><br /> `public void setMoney(String parameterName, BigDecimal bd)`<br /><br /> `public void setSmallMoney(String parameterName, BigDecimal bd)`<br/><br/>`public Timestamp getDateTime(int index)`<br/><br/>`public Timestamp getDateTime(String sCol)`<br/><br/>`public Timestamp getDateTime(int index, Calendar cal)`<br/><br/>`public Timestamp getSmallDateTime(int index)`<br/><br/>`public Timestamp getSmallDateTime(String sCol)`<br/><br/>`public Timestamp getSmallDateTime(int index, Calendar cal)`<br/><br/>`public Timestamp getSmallDateTime(String name, Calendar cal)`<br/><br/>`public BigDecimal getMoney(int index)`<br/><br/>`public BigDecimal getMoney(String sCol)`<br/><br/>`public BigDecimal getSmallMoney(int index)`<br/><br/>`public BigDecimal getSmallMoney(String sCol)`|Эти методы обеспечивают поддержку функции Always Encrypted в типах данных money, smallmoney, uniqueidentifier, datetime и smalldatetime. <br/><br/>Существующий метод `setTimestamp()` используется для отправки значений параметров в зашифрованный столбец datetime2. Для зашифрованных столбцов datetime и smalldatetime используются новые методы `setDateTime()` и `setSmallDateTime()` соответственно.|
|`public void setObject(String parameterName, Object o, int n, int m, boolean forceEncrypt)`<br /><br /> `public void setObject(String parameterName, Object obj, SQLType jdbcType, int scale, boolean forceEncrypt)`<br /><br /> `public void setDate(String parameterName, java.sql.Date x, Calendar c, boolean forceEncrypt)`<br /><br /> `public void setTime(String parameterName, java.sql.Time t, int scale, boolean forceEncrypt)`<br /><br /> `public void setTime(String parameterName, java.sql.Time x, Calendar c, boolean forceEncrypt)`<br /><br /> `public void setDateTime(String parameterName, java.sql.Timestamp x, boolean forceEncrypt)`<br /><br /> `public void setDateTimeOffset(String parameterName, microsoft.sql.DateTimeOffset t, int scale, boolean forceEncrypt)`<br /><br /> `public void setSmallDateTime(String parameterName, java.sql.Timestamp x, boolean forceEncrypt)`<br /><br /> `public void setTimestamp(String parameterName, java.sql.Timestamp t, int scale, boolean forceEncrypt)`<br /><br /> `public void setTimestamp(String parameterName, java.sql.Timestamp x, boolean forceEncrypt)`<br /><br /> `public void setUniqueIdentifier(String parameterName, String guid, boolean forceEncrypt)`<br /><br /> `public void setBytes(String parameterName, byte[] b, boolean forceEncrypt)`<br /><br /> `public void setByte(String parameterName, byte b, boolean forceEncrypt)`<br /><br /> `public void setString(String parameterName, String s, boolean forceEncrypt)`<br /><br /> `public final void setNString(String parameterName, String value, boolean forceEncrypt)<br /><br /> public void setMoney(String parameterName, BigDecimal bd, boolean forceEncrypt)`<br /><br /> `public void setSmallMoney(String parameterName, BigDecimal bd, boolean forceEncrypt)`<br /><br /> `public void setBigDecimal(String parameterName, BigDecimal bd, int precision, int scale, boolean forceEncrypt)`<br /><br /> `public void setDouble(String parameterName, double d, boolean forceEncrypt)`<br /><br /> `public void setFloat(String parameterName, float f, boolean forceEncrypt)`<br /><br /> `public void setInt(String parameterName, int i, boolean forceEncrypt)`<br /><br /> `public void setLong(String parameterName, long l, boolean forceEncrypt)`<br /><br /> `public void setShort(String parameterName, short s, boolean forceEncrypt)`<br /><br /> `public void setBoolean(String parameterNames, boolean b, boolean forceEncrypt)`<br/><br/>`public void setTimeStamp(String sCol, java.sql.Timestamp x, Calendar c, Boolean forceEncrypt)`|Присваивает именованному параметру указанное значение типа Java.<br /><br /> Если логическому параметру forceEncrypt задано значение true, параметр запроса будет установлен только в том случае, если назначенный столбец является зашифрованным, а функция Always Encrypted включена в подключении или в инструкции.<br /><br /> Если для логического параметра forceEncrypt задано значение false, драйвер не будет принудительно выполнять шифрование параметров.|

### <a name="new-or-overloaded-methods-in-sqlserverresultset-class"></a>Новые или перегруженные методы в классе SQLServerResultSet

|Имя|Описание:|
|----------|-----------------|
|`public String getUniqueIdentifier(int columnIndex)`<br/><br/>`public String getUniqueIdentifier(String columnLabel)`<br/><br/>   `public java.sql.Timestamp getDateTime(int columnIndex)` <br/><br/> `public java.sql.Timestamp getDateTime(String columnName)`   <br/><br/> `public java.sql.Timestamp getDateTime(int columnIndex, Calendar cal)`   <br/><br/>`public java.sql.Timestamp getDateTime(String colName, Calendar cal)`    <br/><br/>`public java.sql.Timestamp getSmallDateTime(int columnIndex)`    <br/><br/> `public java.sql.Timestamp getSmallDateTime(String columnName)`   <br/><br/> `public java.sql.Timestamp getSmallDateTime(int columnIndex, Calendar cal)`   <br/><br/> `public java.sql.Timestamp getSmallDateTime(String colName, Calendar cal)`   <br/><br/>  `public BigDecimal getMoney(int columnIndex)`  <br/><br/> `public BigDecimal getMoney(String columnName)`   <br/><br/> `public BigDecimal getSmallMoney(int columnIndex)`   <br/><br/>  `public BigDecimal getSmallMoney(String columnName)`  <br/><br/>`public void updateMoney(String columnName, BigDecimal x)`    <br/><br/>  `public void updateSmallMoney(String columnName, BigDecimal x)`  <br/><br/>     `public void updateDateTime(int index, java.sql.Timestamp x)` <br/><br/> `public void updateSmallDateTime(int index, java.sql.Timestamp x)` |Эти методы обеспечивают поддержку функции Always Encrypted в типах данных money, smallmoney, uniqueidentifier, datetime и smalldatetime. <br/><br/>Существующий метод `updateTimestamp()` используется для обновления зашифрованных столбцов datetime2. Для зашифрованных столбцов datetime и smalldatetime используются новые методы `updateDateTime()` и `updateSmallDateTime()` соответственно.|
|`public void updateBoolean(int index, boolean x, boolean forceEncrypt)`  <br/><br/>  `public void updateByte(int index, byte x, boolean forceEncrypt)`  <br/><br/>  `public void updateShort(int index, short x, boolean forceEncrypt)`  <br/><br/> `public void updateInt(int index, int x, boolean forceEncrypt)`   <br/><br/>  `public void updateLong(int index, long x, boolean forceEncrypt)`  <br/><br/> `public void updateFloat(int index, float x, boolean forceEncrypt)`   <br/><br/> `public void updateDouble(int index, double x, boolean forceEncrypt)`   <br/><br/> `public void updateMoney(int index, BigDecimal x, boolean forceEncrypt)`   <br/><br/>  `public void updateMoney(String columnName, BigDecimal x, boolean forceEncrypt)`  <br/><br/> `public void updateSmallMoney(int index, BigDecimal x, boolean forceEncrypt)`   <br/><br/>  `public void updateSmallMoney(String columnName, BigDecimal x, boolean forceEncrypt)`  <br/><br/> `public void updateBigDecimal(int index, BigDecimal x, Integer precision, Integer scale, boolean forceEncrypt)`   <br/><br/>  `public void updateString(int columnIndex, String stringValue, boolean forceEncrypt)`  <br/><br/>  `public void updateNString(int columnIndex, String nString, boolean forceEncrypt)`  <br/><br/>  `public void updateNString(String columnLabel, String nString, boolean forceEncrypt)`  <br/><br/> `public void updateBytes(int index, byte x[], boolean forceEncrypt)   <br/><br/>  public void updateDate(int index, java.sql.Date x, boolean forceEncrypt)`  <br/><br/> `public void updateTime(int index, java.sql.Time x, Integer scale, boolean forceEncrypt)`   <br/><br/> `public void updateTimestamp(int index, java.sql.Timestamp x, int scale, boolean forceEncrypt)`   <br/><br/> `public void updateDateTime(int index, java.sql.Timestamp x, Integer scale, boolean forceEncrypt)`   <br/><br/> `public void updateSmallDateTime(int index, java.sql.Timestamp x, Integer scale, boolean forceEncrypt)`   <br/><br/>  `public void updateDateTimeOffset(int index, microsoft.sql.DateTimeOffset x, Integer scale, boolean forceEncrypt)`  <br/><br/> `public void updateUniqueIdentifier(int index, String x, boolean forceEncrypt)`    <br/><br/>  `public void updateObject(int index, Object x, int precision, int scale, boolean forceEncrypt)`  <br/><br/>  `public void updateObject(int index, Object obj, SQLType targetSqlType, int scale, boolean forceEncrypt)`  <br/><br/> `public void updateBoolean(String columnName, boolean x, boolean forceEncrypt)`    <br/><br/>  `public void updateByte(String columnName, byte x, boolean forceEncrypt)`  <br/><br/>  `public void updateShort(String columnName, short x, boolean forceEncrypt)`  <br/><br/> `public void updateInt(String columnName, int x, boolean forceEncrypt)`   <br/><br/>   `public void updateLong(String columnName, long x, boolean forceEncrypt)` <br/><br/>  `public void updateFloat(String columnName, float x, boolean forceEncrypt)`  <br/><br/>  `public void updateDouble(String columnName, double x, boolean forceEncrypt)  <br/><br/> public void updateBigDecimal(String columnName, BigDecimal x, boolean forceEncrypt)`   <br/><br/>  `public void updateBigDecimal(String columnName, BigDecimal x, Integer precision, Integer scale, boolean forceEncrypt)`  <br/><br/> `public void updateString(String columnName, String x, boolean forceEncrypt)`   <br/><br/>  `public void updateBytes(String columnName, byte x[], boolean forceEncrypt)`  <br/><br/> `public void updateDate(String columnName, java.sql.Date x, boolean forceEncrypt)`   <br/><br/>  `public void updateTime(String columnName, java.sql.Time x, int scale, boolean forceEncrypt)`  <br/><br/>  `public void updateTimestamp(String columnName, java.sql.Timestamp x, int scale, boolean forceEncrypt)`  <br/><br/> `public void updateDateTime(String columnName, java.sql.Timestamp x, int scale, boolean forceEncrypt)`   <br/><br/>  `public void updateSmallDateTime(String columnName, java.sql.Timestamp x, int scale, boolean forceEncrypt)`  <br/><br/>  `public void updateDateTimeOffset(String columnName, microsoft.sql.DateTimeOffset x, int scale, boolean forceEncrypt)`  <br/><br/>  `public void updateUniqueIdentifier(String columnName, String x, boolean forceEncrypt)`<br/><br/>`public void updateObject(String columnName, Object x, int precision, int scale, boolean forceEncrypt)`<br/><br/>`public void updateObject(String columnName, Object obj, SQLType targetSqlType, int scale, boolean forceEncrypt)`|Обновляет именованный столбец, используя указанное значение Java.<br/><br/>Если логическому параметру `forceEncrypt` задано значение true, столбец будет настроен только в том случае, если он является зашифрованным, а функция Always Encrypted включена в подключении или в инструкции.<br/><br/>Если логическому параметру `forceEncrypt` задано значение false, драйвер не будет принудительно выполнять шифрование параметров.|

### <a name="new-types-in-microsoftsqltypes-class"></a>Новые типы в классе microsoft.sql.Types.

|Имя|Описание|
|----------|-----------------|
|DATETIME, SMALLDATETIME, MONEY, SMALLMONEY, GUID|Используйте эти типы в качестве целевых типов SQL при отправке значений параметров в **зашифрованные** столбцы datetime, smalldatetime, Money, smallmoney и `setObject()/updateObject()` uniqueidentifier с помощью методов API.|

### <a name="sqlserverstatementcolumnencryptionsetting-enum"></a>Перечисление SQLServerStatementColumnEncryptionSetting

Определяет способ отправки и получения данных при чтении и записи зашифрованных столбцов. В зависимости от конкретного запроса влияние на производительность может быть уменьшено за счет обхода обработки драйвером данных в режиме Always Encrypted при использовании незашифрованных столбцов. Эти параметры нельзя использовать для обхода шифрования и получения доступа к данным в виде открытого текста.

**Синтаксис**

```java
Public enum  SQLServerStatementColumnEncryptionSetting
```

**Члены**

|Имя|Описание|
|----------|-----------------|
|UseConnectionSetting|Указывает, что команда должна использовать по умолчанию параметр "Всегда зашифрованы" в строке подключения.|
|Активировано|Разрешает режим "Всегда зашифрованы" для запроса.|
|ResultSetOnly|Указывает, что только результаты выполнения команды должны обрабатываться в драйвере в режиме "Всегда зашифрованы". Используйте это значение, если команда не имеет параметров, требующих шифрования.|
|Выключено|Отменяет режим "Всегда зашифрованы" для запроса.|

Параметр уровня инструкции AE добавляется в класс SQLServerConnection и в класс SQLServerConnectionPoolProxy. Следующие методы в этих классах перегружаются с помощью нового параметра.

|name|Описание|
|----------|-----------------|
|`public Statement createStatement(int nType, int nConcur, int statementHoldability, SQLServerStatementColumnEncryptionSetting stmtColEncSetting)`|Создает объект Statement, который будет формировать объекты ResultSet с заданным типом, параллелизмом, удержанием и параметром шифрования столбца.|
|`public CallableStatement prepareCall(String sql, int nType, int nConcur, int statementHoldability, SQLServerStatementColumnEncryptionSetting stmtColEncSetiing)`|Создает объект CallableStatement с заданным параметром шифрования столбца, который будет формировать объекты ResultSet с заданным типом, параллелизмом и удержанием.|
|`public PreparedStatement prepareStatement(String sql, int autogeneratedKeys, SQLServerStatementColumnEncryptionSetting stmtColEncSetting)`|Создает объект PreparedStatement с заданным параметром шифрования столбца, который имеет возможность получать автоматически созданные ключи.|
|`public PreparedStatement prepareStatement(String sql, String[] columnNames, SQLServerStatementColumnEncryptionSetting stmtColEncSetting)`|Создает объект PreparedStatement с заданным параметром шифрования столбца, который будет формировать объекты ResultSet с заданными именами столбцов.|
|`public PreparedStatement prepareStatement(String sql, int[] columnIndexes, SQLServerStatementColumnEncryptionSetting stmtColEncSetting`|Создает объект PreparedStatement с заданным параметром шифрования столбца, который будет формировать объекты ResultSet с заданными индексами столбцов.|
|`public PreparedStatement prepareStatement(String sql, int nType, int nConcur, int nHold, SQLServerStatementColumnEncryptionSetting stmtColEncSetting)`|Создает объект PreparedStatement с заданным параметром шифрования столбца, который будет формировать объекты ResultSet с заданным типом, параллелизмом и удержанием.|

> [!NOTE]
> Если для запроса отключена функция Always Encrypted, а в запросе есть параметры, которые должны быть зашифрованы (параметры, соответствующие зашифрованным столбцам), запрос завершится ошибкой.
>
> Если функция Always Encrypted для запроса отключена и запрос возвращает результаты из зашифрованных столбцов, запрос возвратит зашифрованные значения. Зашифрованные значения будут иметь тип данных varbinary.

## <a name="see-also"></a>См. также раздел

[Использование функции Always Encrypted с JDBC Driver](using-always-encrypted-with-the-jdbc-driver.md)

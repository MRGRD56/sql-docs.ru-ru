---
title: Выполнение инструкций Transact-SQL с помощью безопасных анклавов
description: Выполнение инструкций языка описания данных (DDL) для настройки шифрования столбцов или управления индексами в зашифрованных столбцах, и выполнения запросов к зашифрованным столбцам
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15 || = sqlallproducts-allversions'
ms.openlocfilehash: 130d2a9f2d92a77ab2d2e033f8e5f4fb9f88ef9f
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837673"
---
# <a name="run-transact-sql-statements-using-secure-enclaves"></a>Выполнение инструкций Transact-SQL с помощью безопасных анклавов

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted с безопасными анклавами](always-encrypted-enclaves.md) позволяет некоторым инструкциям Transact-SQL (T-SQL) выполнять конфиденциальные вычисления в зашифрованных столбцах базы данных в безопасном анклаве на стороне сервера.

## <a name="statements-using-secure-enclaves"></a>Инструкции, использующие безопасные анклавы

Безопасные анклавы используются следующими типами инструкций T-SQL.

### <a name="ddl-statements-using-secure-enclaves"></a>Инструкции DDL, использующие безопасные анклавы

Безопасные анклавы требуются для выполнения следующих типов [инструкций языка описания данных (DDL)](../../../t-sql/statements/statements.md#data-definition-language).

- Инструкции [ALTER TABLE column_definition (Transact-SQL)](../../../t-sql/statements/alter-table-column-definition-transact-sql.md), инициирующие криптографические операции на месте с помощью ключей с поддержкой анклава. Дополнительные сведения см. в статье [Настройка шифрования столбцов на месте с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-configure-encryption.md).
- [Инструкции CREATE INDEX (Transact-SQL)](../../../t-sql/statements/create-index-transact-sql.md) и [ALTER INDEX (Transact-SQL)](../../../t-sql/statements/alter-index-transact-sql.md), которые создают или изменяют индексы в столбцах с поддержкой анклава с помощью случайного шифрования. Дополнительные сведения см. в статье [Создание и использование индексов в столбцах с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-create-use-indexes.md).
  
### <a name="dml-statements-using-secure-enclaves"></a>Инструкции DML, использующие безопасные анклавы

Безопасные анклавы требуются для выполнения следующих [инструкций языка обработки данных (DML)](../../../t-sql/statements/statements.md#data-manipulation-language) или запросов к столбцам с поддержкой анклава.

- Запросы, использующие один или несколько указанных далее операторов Transact-SQL, поддерживаемых в безопасных анклавах.
  - [Операторы сравнения](../../../mdx/comparison-operators.md)
  - [Оператор BETWEEN (Transact-SQL)](../../../t-sql/language-elements/between-transact-sql.md)
  - [IN (Transact-SQL)](../../../t-sql/language-elements/in-transact-sql.md)
  - [LIKE (Transact-SQL)](../../../t-sql/language-elements/like-transact-sql.md)
  - [DISTINCT](../../../t-sql/queries/select-transact-sql.md#c-using-distinct-with-select)
  - [Joins](../../performance/joins.md) - [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] поддерживает только соединения вложенных циклов. [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] поддерживает соединения вложенных циклов, хэшей и объединений.
  - [SELECT — предложение ORDER BY (Transact-SQL)](../../../t-sql/queries/select-order-by-clause-transact-sql.md). Поддерживается в [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]. В [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]не поддерживается.
  - [SELECT — предложение GROUP BY (Transact-SQL)](../../../t-sql/queries/select-group-by-transact-sql.md). Поддерживается в [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)]. В [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)]не поддерживается.
- Запросы, которые вставляют, обновляют или удаляют строки, которые, в свою очередь, инициируют вставку ключа индекса в столбец с поддержкой анклава или удаление ключа из столбца. Дополнительные сведения см. в статье [Создание и использование индексов в столбцах с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-create-use-indexes.md).

> [!NOTE]
> Операции с индексами и конфиденциальными запросами DML, использующими анклавы, поддерживаются только в столбцах с поддержкой анклава, применяющих случайное шифрование. Детерминированное шифрование не поддерживается.
>
> В [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)] конфиденциальным запросам, использующим анклавы в столбцах строк символов (`char`, `nchar`), требуются параметры сортировки binary2 (BIN2), настроенные для столбца. В [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] требуется использовать параметры сортировки BIN2 или UTF-8.

### <a name="dbcc-commands-using-secure-enclaves"></a>Команды DBCC, использующие безопасные анклавы

Безопасные анклавы могут также потребоваться для выполнения административных команд [DBCC (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-transact-sql.md), предполагающих проверку целостности индексов, если база данных содержит индексы в столбцах с поддержкой анклава с использованием случайного шифрования. Например, [DBCC CHECKDB (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) и [DBCC CHECKTABLE (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).

## <a name="prerequisites-for-running-statements-using-secure-enclaves"></a>Необходимые условия для выполнения инструкций, использующих безопасные анклавы

Ваша среда должна удовлетворять следующим требованиям для поддержки выполнения инструкций, использующих безопасный анклав.

- Экземпляр [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] или база данных и сервер в [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] должны быть правильно настроены для поддержки анклавов и аттестации. Дополнительные сведения см. в статье [Настройка безопасного анклава и аттестации](configure-always-encrypted-enclaves.md#set-up-the-secure-enclave-and-attestation).
- Необходимо получить URL-адрес аттестации для имеющейся среды у администратора службы аттестации.

  - Если вы используете [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] и службу защитника узлов (HGS), см. сведения в разделе об [определении и совместном использовании URL-адреса аттестации HGS](always-encrypted-enclaves-host-guardian-service-deploy.md#step-6-determine-and-share-the-hgs-attestation-url).
  - Если вы используете [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)] и Аттестацию Microsoft Azure, см. сведения об [определении URL-адреса аттестации для политики аттестации](./always-encrypted-enclaves.md?view=sql-server-ver15#secure-enclave-attestation).

- Если подключение к базе данных осуществляется с помощью приложения, необходимо использовать драйвер клиента, который поддерживает Always Encrypted с безопасными анклавами. Приложение должно подключаться к базе данных с включенной функцией Always Encrypted для подключения к базе данных и использовать правильно настроенные протокол аттестации и URL-адрес аттестации. Подробные сведения см. в статье [Разработка приложений с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-client-development.md).
- Если вы используете SQL Server Management Studio (SSMS) или Azure SQL Data Studio, при подключении к базе данных необходимо включить Always Encrypted и настроить протокол аттестации и URL-адрес аттестации. Дополнительные сведения см. в следующих разделах.

> [!NOTE]
> Если используются кэшированные ключи шифрования столбцов, подключение к базе данных с настроенными функцией Always Encrypted и аттестацией не требуется для следующих операций: запросы DDL, которые создают или изменяют индексы, запросы DML, которые обновляют индексы, и команды DBCC, которые проверяют целостность индекса. Дополнительные сведения см. в разделе [Вызов операций индексирования с использованием кэшированных ключей шифрования столбцов](always-encrypted-enclaves-create-use-indexes.md#invoke-indexing-operations-using-cached-column-encryption-keys).

### <a name="prerequisites-for-running-t-sql-statements-using-enclaves-in-ssms"></a>Необходимые условия для выполнения инструкций T-SQL, использующих анклавы, в SSMS

Минимальные требования к версии для SQL Server Management Studio:

- SSMS 18.3 при использовании [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].
- SSMS 18.8 при использовании [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

Выполнять инструкции необходимо в окне запросов, использующем подключение с Always Encrypted и правильно настроенный URL-адрес аттестации.

1. В диалоговом окне **Соединение с сервером** укажите имя сервера, выберите метод аутентификации и введите учетные данные.
2. Нажмите кнопку **Параметры >>** и выберите вкладку **Always Encrypted**.
3. Установите флажок **Включить Always Encrypted (шифрование столбцов)** и укажите URL-адрес аттестации анклава. Например, `https://hgs.bastion.local/Attestation` или `https://contososqlattestation.uks.attest.azure.net/attest/SgxEnclave`.

    ![Подключение к серверу с аттестацией с помощью SSMS](./media/always-encrypted-enclaves/column-encryption-setting.png)

4. Щелкните **Подключить**.
5. Если отобразится запрос на включение параметризации для запросов Always Encrypted, нажмите кнопку **Включить**, что позволит выполнять параметризованные запросы DML. Дополнительные сведения см. в разделе [Параметризация для Always Encrypted](always-encrypted-query-columns-ssms.md#param).

Дополнительные сведения см. в разделе [Включение и отключение функции Always Encrypted, применяемой для подключения к базе данных](always-encrypted-query-columns-ssms.md#en-dis).

### <a name="prerequisites-for-running-t-sql-statements-using-enclaves-in-azure-data-studio"></a>Необходимые условия для выполнения инструкций T-SQL, использующих анклавы, в Azure Data Studio

Рекомендуется использовать минимальную версию **1.23** или выше.

Выполнять инструкции необходимо в окне запросов, использующем подключение с включенной функцией Always Encrypted и правильно настроенными протоколом аттестации и URL-адресом аттестации.

1. В диалоговом окне **Подключения** нажмите кнопку **Дополнительно...** .
2. Чтобы включить Always Encrypted для подключения, задайте для поля **Always Encrypted** значение **Включено**.
3. Укажите протокол аттестации и URL-адрес аттестации.
    - Если вы используете [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)], укажите в поле **Протокол аттестации** значение **Служба защиты узла** и введите URL-адрес аттестации службы защиты узла в поле **URL-адрес аттестации анклава**.
    - Если вы используете [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], укажите в поле **Протокол аттестации**  значение **Аттестация Azure** и введите URL-адрес аттестации, ссылающийся на политику в Аттестации Microsoft Azure, в поле **URL-адрес аттестации анклава**.

    ![Подключение к серверу с аттестацией с помощью Azure Data Studio](./media/always-encrypted-enclaves/azure-data-studio-connect-with-enclaves.png)

4. Нажмите кнопку **OK**, чтобы закрыть **Расширенные свойства**.

Дополнительные сведения см. в разделе [Включение и отключение функции Always Encrypted, применяемой для подключения к базе данных](always-encrypted-query-columns-ads.md#enabling-and-disabling-always-encrypted-for-a-database-connection).

Если вы планируете выполнять параметризованные запросы DML, необходимо также включить [параметризацию для Always Encrypted](always-encrypted-query-columns-ads.md#parameterization-for-always-encrypted).

## <a name="examples"></a>Примеры

В этом разделе приводятся примеры запросов DML, использующих анклавы. 

В примерах используется приведенная ниже схема.

```sql
CREATE SCHEMA [HR];
GO

CREATE TABLE [HR].[Jobs](
 [JobID] [int] IDENTITY(1,1) PRIMARY KEY,
 [JobTitle] [nvarchar](50) NOT NULL,
 [MinSalary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [MaxSalary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
);
GO

CREATE TABLE [HR].[Employees](
 [EmployeeID] [int] IDENTITY(1,1) PRIMARY KEY,
 [SSN] [char](11) ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [FirstName] [nvarchar](50) NOT NULL,
 [LastName] [nvarchar](50) NOT NULL,
 [Salary] [money] ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL,
 [JobID] [int] NULL,
 FOREIGN KEY (JobID) REFERENCES [HR].[Jobs] (JobID)
);
GO
```

### <a name="exact-match-search"></a>Поиск точного совпадения

Приведенный ниже запрос выполняет поиск точного совпадения в зашифрованном строковом столбце `SSN`.

```sql
DECLARE @SSN char(11) = '795-73-9838';
SELECT * FROM [HR].[Employees] WHERE [SSN] = @SSN;
GO
```

### <a name="pattern-matching-search"></a>Поиск сопоставлений по шаблону

В приведенном ниже запросе выполняется поиск сопоставлений по шаблону в зашифрованном строковом столбце `SSN`, где нужно найти сотрудников с указанными последними цифрами номера социального страхования.

```sql
DECLARE @SSN char(11) = '795-73-9838';
SELECT * FROM [HR].[Employees] WHERE [SSN] = @SSN;
GO
```

### <a name="range-comparison"></a>Сравнение диапазонов

В приведенном ниже запросе выполняется сравнение диапазонов в зашифрованном столбце `Salary`, где нужно найти сотрудников с заработной платой в указанном диапазоне.

```sql
DECLARE @MinSalary money = 40000;
DECLARE @MaxSalary money = 45000;
SELECT * FROM [HR].[Employees] WHERE [Salary] > @MinSalary AND [Salary] < @MaxSalary;
GO
```

### <a name="joins"></a>Соединения

В приведенном ниже запросе выполняется присоединение таблиц `Employees` и `Jobs` с помощью зашифрованного столбца `Salary`. Запрос получает сотрудников с заработной платой вне диапазона зарплаты для должности сотрудника.

```sql
SELECT * FROM [HR].[Employees] e
JOIN [HR].[Jobs] j
ON e.[JobID] = j.[JobID] AND e.[Salary] > j.[MaxSalary] OR e.[Salary] < j.[MinSalary];
GO
```

### <a name="sorting"></a>Сортировка

В приведенном ниже запросе сортируются записи сотрудников на основе зашифрованного столбца `Salary` и выводятся 10 сотрудников с самыми высокими окладами.
> [!NOTE]
> Сортировка зашифрованных столбцов поддерживается в [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], но не в [!INCLUDE[sql-server-2019](../../../includes/sssql19-md.md)].

```sql
SELECT TOP(10) * FROM [HR].[Employees]
ORDER BY [Salary] DESC;
GO
```

## <a name="next-steps"></a>Next Steps

- [Разработка приложений с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>См. также раздел

- [Устранение распространенных неполадок Always Encrypted с безопасными анклавами](always-encrypted-enclaves-troubleshooting.md)
- [Учебник. Начало работы с Always Encrypted и безопасными анклавами в SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Учебник. Начало работы с Always Encrypted и безопасными анклавами в Базе данных SQL Azure](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Настройка шифрования столбцов на месте с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-configure-encryption.md)
- [Создание и использование индексов в столбцах с помощью Always Encrypted с безопасными анклавами](always-encrypted-enclaves-create-use-indexes.md)
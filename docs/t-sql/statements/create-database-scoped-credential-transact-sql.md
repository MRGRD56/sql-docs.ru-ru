---
description: CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)
title: CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 09/25/2019
ms.prod: sql
ms.prod_service: synapse-analytics, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DATABASE SCOPED CREDENTIAL
- DATABASE_SCOPED_CREDENTIAL_TSQL
- SCOPED_TSQL
- CREATE_DATABASE_SCOPED_CREDENTIAL
- CREATE_DATABASE_SCOPED_CREDENTIAL_TSQL
- SCOPED_CREDENTIAL_TSQL
- SCOPED_CREDENTIAL
helpviewer_keywords:
- DATABASE SCOPED CREDENTIAL statement
- credentials [SQL Server], DATABASE SCOPED CREDENTIAL statement
ms.assetid: fe830577-11ca-44e5-953b-2d589d54d045
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=aps-pdw-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0f5f3bc676b2ba7c37df9ce463c99a1954a671a0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753354"
---
# <a name="create-database-scoped-credential-transact-sql"></a>CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Создает учетные данные для базы данных. Учетные данные базы данных не сопоставляются с именем входа сервера или пользователем базы данных. База данных использует эти учетные данные для доступа к внешнему расположению каждый раз при выполнении операции, требующей доступа.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
CREATE DATABASE SCOPED CREDENTIAL credential_name
WITH IDENTITY = 'identity_name'
    [ , SECRET = 'secret' ]

```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

*credential_name* Указывает имя создаваемых учетных данных для базы данных. Аргумент *credential_name* не может начинаться с символа номера (#). Системные учетные данные начинаются с символов ##.

IDENTITY **='** _identity\_name_ **'** . Указывает имя учетной записи для использования при подключении за пределами сервера. Чтобы импортировать файл из хранилища BLOB-объектов Azure с использованием общего ключа, имя удостоверения должно быть `SHARED ACCESS SIGNATURE`. Для загрузки данных в Azure Synapse Analytics в качестве удостоверения можно использовать любое допустимое значение. Дополнительные сведения о подписанных URL-адресах см. в статье [Использование подписанных URL-адресов](/azure/storage/storage-dotnet-shared-access-signature-part-1). При использовании Kerberos (Windows Active Directory или MIT KDC) не указывайте доменное имя в аргументе IDENTITY. В этом аргументе должно быть только имя учетной записи.

> [!IMPORTANT]
> Единственный внешний источник данных PolyBase, поддерживающий проверку подлинности Kerberos, — Hadoop. Все другие внешние источники данных (SQL Server, Oracle, Teradata, MongoDB, универсальный ODBC) поддерживают только обычную проверку подлинности.

> [!NOTE]
> Инструкция WITH IDENTITY не требуется, если для контейнера в хранилище BLOB-объектов Azure включен анонимный доступ. См. пример запроса к хранилищу BLOB-объектов Azure в разделе [Импорт данных в таблицу из файла, который находится в хранилище BLOB-объектов Azure](../functions/openrowset-transact-sql.md#j-importing-into-a-table-from-a-file-stored-on-azure-blob-storage).

SECRET **='** _secret_ **'** . Указывает секретный код, необходимый для исходящей проверки подлинности. `SECRET` требуется для импорта файла из хранилища больших двоичных объектов Azure. Для загрузки из хранилища BLOB-объектов Azure в Azure Synapse Analytics или Parallel Data Warehouse в качестве секретного ключа необходимо использовать ключ хранилища Azure.
> [!WARNING]
> Значение ключа SAS может начинаться с '?' (вопросительный знак). При использовании ключа SAS необходимо удалить начальный символ '?'. В противном случае действия могут быть заблокированы.

## <a name="remarks"></a>Remarks

Учетные данные для базы данных являются записью, которая содержит сведения для проверки подлинности, необходимые для подключения к ресурсу извне [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Большинство учетных данных включают имя пользователя и пароль Windows.

Перед созданием учетных данных для базы данных в базе данных должен находиться главный ключ для защиты учетных данных. Дополнительные сведения см. в разделе [CREATE MASTER KEY (Transact-SQL)](../../t-sql/statements/create-master-key-transact-sql.md).

Если IDENTITY является пользователем Windows, секретный код может быть паролем. Секретный код шифруется главным ключом службы. Если главный ключ службы формируется повторно, секретный код повторно шифруется, используя новый главный ключ службы.

Дополнительные сведения об учетных данных для базы данных см. в представлении каталога [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md).

Далее приводятся некоторые варианты использования учетных данных для базы данных.

- [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] использует учетные данные для базы данных для доступа к не являющемуся открытым хранилищу больших двоичных объектов или к кластерам Hadoop с PolyBase, защищенным Kerberos. Дополнительные сведения см. в разделе [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md).

- [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)] использует учетные данные для базы данных для доступа к не являющемуся открытым хранилищу больших двоичных объектов Azure с PolyBase. Дополнительные сведения см. в разделе [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md).

- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] использует учетные данные для базы данных для своего компонента глобального запроса. Это возможность выполнять запросы к нескольким сегментам базы данных.

- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] использует учетные данные для базы данных для записи файлов расширенных событий в хранилище BLOB-объектов Azure.

- [!INCLUDE[ssSDS](../../includes/sssds-md.md)] использует учетные данные для базы данных для эластичных пулов. Дополнительные сведения см. в статье о [решении проблем с бурным ростом эластичных баз данных](/azure/azure-sql/database/elastic-pool-overview).

- [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) используют учетные данные для базы данных для доступа к данным из хранилища BLOB-объектов Azure. Дополнительные сведения см. в разделе [Примеры массового доступа к данным в хранилище BLOB-объектов Azure](../../relational-databases/import-export/examples-of-bulk-access-to-data-in-azure-blob-storage.md). 

## <a name="permissions"></a>Разрешения

Требуется разрешение **CONTROL** для базы данных.

## <a name="examples"></a>Примеры

### <a name="a-creating-a-database-scoped-credential-for-your-application"></a>A. Создание учетных данных для базы данных для вашего приложения

В следующем примере создаются учетные данные с именем `AppCred` для базы данных. В эти учетные данные для базы данных входят имя пользователя Windows `Mary5` и пароль.

```sql
-- Create a db master key if one does not already exist, using your own password.
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';

-- Create a database scoped credential.
CREATE DATABASE SCOPED CREDENTIAL AppCred WITH IDENTITY = 'Mary5',
    SECRET = '<EnterStrongPasswordHere>';
```

### <a name="b-creating-a-database-scoped-credential-for-a-shared-access-signature"></a>Б. Создание учетных данных для базы данных для подписанного URL-адреса

В следующем примере создаются учетные данные для базы данных, которые можно использовать для создания [внешнего источника данных](../../t-sql/statements/create-external-data-source-transact-sql.md), выполняющего массовые операции, такие как [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) и [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md). Подписанные URL-адреса нельзя использовать с PolyBase в SQL Server, APS или Azure Synapse Analytics.

```sql
-- Create a db master key if one does not already exist, using your own password.
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';

-- Create a database scoped credential.
CREATE DATABASE SCOPED CREDENTIAL MyCredentials
WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
SECRET = 'QLYMgmSXMklt%2FI1U6DcVrQixnlU5Sgbtk1qDRakUBGs%3D';
```

### <a name="c-creating-a-database-scoped-credential-for-polybase-connectivity-to-azure-data-lake-store"></a>В. Создание учетных данных для базы данных для подключения PolyBase к Azure Data Lake Store

В следующем примере создаются учетные данные для базы данных, которые можно использовать для создания [внешнего источника данных](../../t-sql/statements/create-external-data-source-transact-sql.md), используемого PolyBase в [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].

Azure Data Lake Store использует приложение Azure Active Directory для проверки подлинности в службе.
[Создайте приложение AAD](/azure/data-lake-store/data-lake-store-authenticate-using-active-directory) и задокументируйте client_id, OAuth_2.0_Token_EndPoint и ключ, прежде чем создавать учетные данные для базы данных.

```sql
-- Create a db master key if one does not already exist, using your own password.
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';

-- Create a database scoped credential.
CREATE DATABASE SCOPED CREDENTIAL ADL_User
WITH
    IDENTITY = '<client_id>@\<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;
```

## <a name="more-information"></a>Дополнительные сведения

- [Учетные данные (компонент Database Engine)](../../relational-databases/security/authentication-access/credentials-database-engine.md)
- [ALTER DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-credential-transact-sql.md)
- [DROP DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-scoped-credential-transact-sql.md)
- [sys.database_scoped_credentials](../../relational-databases/system-catalog-views/sys-database-scoped-credentials-transact-sql.md)
- [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)
- [sys.credentials (Transact-SQL)](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)

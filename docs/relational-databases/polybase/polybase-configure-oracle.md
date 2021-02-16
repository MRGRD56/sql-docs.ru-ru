---
title: 'Доступ к внешним данным: Oracle — PolyBase'
description: В этой статье показано, как использовать PolyBase для создания внешнего источника данных для доступа к данным Oracle.
ms.date: 12/13/2019
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: 77478f132083417b20f3a722804f497ecf373d73
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351784"
---
# <a name="configure-polybase-to-access-external-data-in-oracle"></a>Настройка PolyBase для доступа к внешним данным в Oracle

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

В этой статье описывается использование PolyBase в экземпляре SQL Server для запроса внешних данных в Oracle.

## <a name="prerequisites"></a>Предварительные требования

Если вы не установили PolyBase, см. раздел [Установка PolyBase](polybase-installation.md).

  [Главный ключ](../../t-sql/statements/create-master-key-transact-sql.md) необходимо создать перед созданием учетных данных для базы данных. 

## <a name="configure-an-oracle-external-data-source"></a>Настройка внешнего источника данных Oracle

Чтобы запросить данные из источника данных Oracle, необходимо создать внешние таблицы, позволяющие ссылаться на внешние данные. Этот раздел содержит пример кода для создания таких внешних таблиц.

В рамках этого раздела используются следующие команды Transact-SQL:

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 
- [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)


1. Создайте учетные данные в области базы данных для доступа к источнику Oracle.

    ```sql
    /*  specify credentials to external data source
    *  IDENTITY: user name for external source. 
    *  SECRET: password for external source.
    */
    CREATE DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'username', Secret = 'password';
    ```
    
   > [!IMPORTANT] 
   > Соединитель ODBC Oracle для PolyBase поддерживает только обычную проверку подлинности, но не проверку подлинности Kerberos. 

1. Создайте внешний источник данных с помощью инструкции [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md).

    ```sql
    /* 
    * LOCATION: Location string should be of format '<vendor>://<server>[:<port>]'.
    * PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    * CONNECTION_OPTIONS: Specify driver location
    * CREDENTIAL: the database scoped credential, created above.
    */  
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH ( LOCATION = 'oracle://<server address>[:<port>]',
    -- PUSHDOWN = ON | OFF,
    CREDENTIAL = credential_name)
    ```

1. **Необязательно**. Создайте статистику внешней таблицы.

    Чтобы обеспечить оптимальную производительность запросов, мы советуем создать статистику столбцов внешней таблицы, особенно тех, которые используются для объединения, применения фильтров и статистических выражений.

    ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY) WITH FULLSCAN; 
    ```

>[!IMPORTANT] 
>После создания внешнего источника данных можно использовать команду [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md), чтобы создать таблицу с поддержкой запросов по этому источнику. 

Дополнительные сведения и примеры см. в следующих разделах:

- [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md).
- [Общие сведения об SQL Server PolyBase](polybase-guide.md).

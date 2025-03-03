---
title: Неподдерживаемые функции ядра СУБД
description: Узнайте, какие функции и возможности ядра СУБД больше не поддерживаются в SQL Server 2019 (15.x), SQL Server 2016 (13.x) и предыдущих версиях.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- VIA protocol
- unsupported features [SQL Server]
- SQL Mail
- discontinued functionality [SQL Server]
- RESTORE WITH DBO_ONLY
- BACKUP WITH PASSWORD
- user instances enabled
- BACKUP WITH MEDIAPASSWORD
- AWE
- SQL-DMO
- '*= and =*'
- 80 compatibility levels
- COMPUTE BY
- user instance timeout
- sp_dropalias
- COMPUTE
- SSL
- WITH APPEND
- sys.database_principal_aliases
- sp_dboption
- DATABASEPROPERTY
- FASTFIRSTROW hint
- SET DISABLE_DEF_CNST_CHK
ms.assetid: d686cdf0-d11d-4dba-9ec8-de1a5f189f25
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= sql-server-linux-2017  || >= sql-server-2016'
ms.openlocfilehash: e1c5f8b15d40ff26cf1a164e806f7fa735b28319
ms.sourcegitcommit: 00af0b6448ba58e3685530f40bc622453d3545ac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104673521"
---
# <a name="discontinued-database-engine-functionality-in-sql-server"></a>Нерекомендуемые функции ядра СУБД в SQL Server
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

  В этом разделе описаны функции компонента [!INCLUDE[ssDE](../includes/ssde-md.md)] , которые больше не доступны в [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)].  

## <a name="discontinued-features-in-sssql19"></a>Неподдерживаемые функции в [!INCLUDE[sssql19](../includes/sssql19-md.md)]  

- Больше не поддерживаются следующие параметры конфигурации области базы данных:

  - `DISABLE_BATCH_MODE_ADAPTIVE_JOIN`
  - `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK`
  - `DISABLE_INTERLEAVED_EXECUTION_TVF`

Сведения о текущих параметрах конфигурации см. в разделе [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

>[!NOTE]
>В [!INCLUDE[sssql14](../includes/sssql17-md.md)] не появились новые неподдерживаемые функции.

## <a name="discontinued-features-in-sssql15-md"></a>Неподдерживаемые функции в [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]

- [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] — это 64-разрядное приложение. 32-разрядная установка больше не поддерживается, хотя некоторые элементы работают как 32-разрядные.  

- Уровень совместимости 90 не поддерживается. Дополнительные сведения см. в разделе [Уровень совместимости инструкции ALTER DATABASE (Transact-SQL)](../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  

- Подсистема ActiveX не поддерживается. Используйте вместо нее командную строку или скрипты PowerShell.

- Параметры запуска **-h** и **-g**. Дополнительные сведения см. в разделе [Параметры запуска службы Database Engine](/previous-versions/sql/2014/database-engine/configure-windows/database-engine-service-startup-options?view=sql-server-2014&preserve-view=true).

- Использование шифрования по протоколу SSL прекращено. Используйте вместо него протокол TLS. Дополнительные сведения см. в статье [Включение шифрования соединений в компоненте Database Engine](../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md).

- Параметр конфигурации SQL Server `precompute rank` не поддерживается, начиная с SQL Server 2008. Статья была удалена из документации.

## <a name="previous-versions"></a>Предыдущие версии

- [Неподдерживаемые функции ядра СУБД в SQL Server 2014](/previous-versions/sql/2014/database-engine/discontinued-database-engine-functionality-in-sql-server-2016?view=sql-server-2014&preserve-view=true)

### <a name="see-also"></a>См. также раздел

- [Нерекомендуемые функции ядра СУБД в SQL Server 2019](deprecated-database-engine-features-in-sql-server-version-15.md)
- [Нерекомендуемые функции ядра СУБД в SQL Server 2017](deprecated-database-engine-features-in-sql-server-2017.md)
- [Нерекомендуемые функции ядра СУБД в SQL Server 2016](../database-engine/deprecated-database-engine-features-in-sql-server-2016.md)
- [Критические изменения в функциях ядра СУБД в SQL Server 2019](breaking-changes-to-database-engine-features-in-sql-server-version-15.md)
- [Критические изменения в функциях ядра СУБД в SQL Server 2017](breaking-changes-to-database-engine-features-in-sql-server-2017.md)
- [Критические изменения в функциях ядра СУБД в SQL Server 2016](breaking-changes-to-database-engine-features-in-sql-server-2016.md)
- [Устаревшие функции репликации в SQL Server](../relational-databases/replication/deprecated-features-in-sql-server-replication.md)
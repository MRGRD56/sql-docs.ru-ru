---
title: Предоставление разрешений на выполнение скриптов Python и R
description: Узнайте, как предоставлять пользователям разрешения на выполнение внешних скриптов Python и R в Службах машинного обучения SQL Server и предоставление разрешений на чтение, запись или инструкции на языке описания данных (DDL) для баз данных.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/19/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019, contperf-fy20q4
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: bbfc55b4c0460d948d78d72e628033d195a1f027
ms.sourcegitcommit: 17f05be5c08cf9a503a72b739da5ad8be15baea5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2021
ms.locfileid: "105103721"
---
# <a name="grant-database-users-permission-to-execute-python-and-r-scripts-with-sql-server-machine-learning-services"></a>Предоставление пользователям базы данных разрешения на выполнение скриптов Python и R с помощью Служб машинного обучения SQL Server
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Узнайте, как предоставить [пользователю базы данных](../../relational-databases/security/authentication-access/create-a-database-user.md) разрешения на выполнение внешних скриптов Python и R в [Службах машинного обучения SQL Server](../sql-server-machine-learning-services.md), а также разрешения на чтение, запись или выполнение инструкций на языке описания данных (DDL) для баз данных.

Дополнительные сведения см. в подразделе "Разрешения" раздела [Общие сведения о безопасности для платформы расширяемости](../../machine-learning/concepts/security.md#permissions).

<a name="permissions-external-script"></a>

## <a name="permission-to-run-scripts"></a>Разрешение на выполнение скриптов

Каждому пользователю, запускающему скрипты Python или R со Службами машинного обучения SQL Server и не являющемуся администратором, необходимо предоставить разрешение на выполнение внешних скриптов в каждой базе данных, где используется этот язык.

Чтобы предоставить [пользователю базу данных](../../relational-databases/security/authentication-access/create-a-database-user.md) разрешение на выполнение внешнего скрипта, выполните следующий скрипт:

```sql
USE <database_name>
GO
GRANT EXECUTE ANY EXTERNAL SCRIPT TO [UserName]
```

> [!NOTE]
> Разрешения не относятся к конкретному поддерживаемому языку скриптов. Иными словами, не существует отдельных уровней разрешений для скриптов R и скриптов Python.

<a name="permissions-db"></a>

## <a name="grant-database-permissions"></a>Предоставление разрешений для базы данных

Когда пользователь базы данных выполняет скрипты, ему может потребоваться считывать данные из других баз данных. Пользователю базы данных также может потребоваться создавать новые таблицы для хранения результатов и записывать данные в таблицы.

Необходимо убедиться, что у каждой учетной записи пользователя базы данных или имени входа SQL, используемых для выполнения скриптов R или Python, имеются соответствующие разрешения для конкретной базы данных: 

+ `db_datareader` для чтения данных.
+ `db_datawriter` для сохранения объектов в базе данных.
+ `db_ddladmin` для создания таких объектов, как хранимые процедуры или таблицы, содержащие обученные и сериализованные данные.

Например, приведенная ниже инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)] предоставляет имени входа SQL *MySQLLogin* права на выполнение запросов T-SQL в базе данных *ML_Samples*. Для выполнения этой инструкции имя входа SQL уже должно существовать в контексте безопасности сервера. Дополнительные сведения см. в статье [Хранимая процедура sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md).

```sql
USE ML_Samples
GO
EXEC sp_addrolemember 'db_datareader', 'MySQLLogin'
```

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о разрешениях, включенных в каждую роль, см. в разделе [Роли уровня базы данных](../../relational-databases/security/authentication-access/database-level-roles.md).

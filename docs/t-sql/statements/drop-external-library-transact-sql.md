---
description: DROP EXTERNAL LIBRARY (Transact-SQL)
title: DROP EXTERNAL LIBRARY (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: language-reference
f1_keywords:
- DROP EXTERNAL LIBRARY
- DROP_EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DROP EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 3cbf3bfa6ad5cb6971d1cd7ab7d9d4d2ac490c2a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478485"
---
# <a name="drop-external-library-transact-sql"></a>DROP EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Удаляет существующую библиотеку пакета. Библиотеки пакетов используются поддерживаемыми внешними средами выполнения, например R, Python или Java.

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
> [!NOTE]
> В SQL Server 2017 поддерживаются язык R и платформа Windows. R, Python и Java на платформах Windows и Linux поддерживаются в SQL Server 2019 и более поздних версий.
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
> [!NOTE]
> В Управляемом экземпляре SQL Azure поддерживаются языки R и Python.
::: moniker-end

## <a name="syntax"></a>Синтаксис

```syntaxsql
DROP EXTERNAL LIBRARY library_name
[ AUTHORIZATION owner_name ];
```

### <a name="arguments"></a>Аргументы

**library_name**

Указывает имя существующей библиотеки пакетов.

Область действия библиотек ограничивается пользователем. Имена библиотек должны быть уникальными в контексте определенного пользователя или владельца.

**owner_name**

Указывает имя пользователя или роли, которой принадлежит внешняя библиотека.

Владельцы базы данных могут удалять библиотеки, созданные другими пользователями.

## <a name="permissions"></a>Разрешения

Для удаления библиотеки необходима привилегия ALTER ANY EXTERNAL LIBRARY. По умолчанию удалить внешнюю библиотеку может также любой владелец базы данных или владелец объекта.

### <a name="return-values"></a>Возвращаемые значения

Если инструкция была выполнена успешно, возвращается информационное сообщение.

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>Remarks

В отличие от других инструкций `DROP` в SQL Server, эта инструкция поддерживает указание необязательного предложения авторизации. Это позволяет **dbo** или пользователям роли **db_owner** удалять пакет библиотеки, отправленный обычным пользователем в базе данных.

Набор пакетов, называемых *системными пакетами*, устанавливается в экземпляре SQL предварительно. Пользователь не может добавлять, обновлять и удалять системные пакеты.

## <a name="examples"></a>Примеры

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
Добавление пользовательского пакета R `customPackage` в базу данных.

```sql
CREATE EXTERNAL LIBRARY customPackage 
FROM (CONTENT = 'C:\temp\customPackage_v1.1.zip')
WITH (LANGUAGE = 'R');
GO
```
::: moniker-end

Удалите библиотеку `customPackage`.

```sql
DROP EXTERNAL LIBRARY customPackage;
```

## <a name="see-also"></a>См. также раздел

[CREATE EXTERNAL LIBRARY (Transact-SQL)](create-external-library-transact-sql.md)  
[ALTER EXTERNAL LIBRARY (Transact-SQL)](alter-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)  

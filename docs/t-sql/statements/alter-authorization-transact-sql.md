---
description: ALTER AUTHORIZATION (Transact-SQL)
title: ALTER AUTHORIZATION (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 02/01/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_AUTHORIZATION_TSQL
- ALTER AUTHORIZATION
dev_langs:
- TSQL
helpviewer_keywords:
- owners [SQL Server], transferring
- modifying entity ownership
- full-text search [SQL Server], permissions
- owners [SQL Server], entities
- ALTER AUTHORIZATION statement
- entity ownership [SQL Server]
- transferring ownership
- search property lists [SQL Server], permissions
- TAKE OWNERSHIP
ms.assetid: 8c805ae2-91ed-4133-96f6-9835c908f373
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 66574b67a8a98c010cab144d3affceb2d822ee6c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104734385"
---
# <a name="alter-authorization-transact-sql"></a>ALTER AUTHORIZATION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Изменяет владельца защищаемой сущности.

 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Синтаксис

```syntaxsql
-- Syntax for SQL Server
ALTER AUTHORIZATION
    ON [ <class_type>:: ] entity_name
    TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::=
     {
      OBJECT | ASSEMBLY | ASYMMETRIC KEY | AVAILABILITY GROUP | CERTIFICATE
    | CONTRACT | TYPE | DATABASE | ENDPOINT | FULLTEXT CATALOG
    | FULLTEXT STOPLIST | MESSAGE TYPE | REMOTE SERVICE BINDING
    | ROLE | ROUTE | SCHEMA | SEARCH PROPERTY LIST | SERVER ROLE
    | SERVICE | SYMMETRIC KEY | XML SCHEMA COLLECTION
     }
```

```syntaxsql
-- Syntax for SQL Database

ALTER AUTHORIZATION
    ON [ <class_type>:: ] entity_name
    TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::=
     {
    OBJECT | ASSEMBLY | ASYMMETRIC KEY | CERTIFICATE
     | TYPE | DATABASE | FULLTEXT CATALOG
     | FULLTEXT STOPLIST
     | ROLE | SCHEMA | SEARCH PROPERTY LIST
     | SYMMETRIC KEY | XML SCHEMA COLLECTION
     }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics

ALTER AUTHORIZATION ON
     [ <class_type> :: ] <entity_name>
     TO { principal_name | SCHEMA OWNER }
    [;]

    <class_type> ::= {
    SCHEMA
     | OBJECT
    }

    <entity_name> ::=
    {
    schema_name
     | [ schema_name. ] object_name
    }
```

```syntaxsql
-- Syntax for Parallel Data Warehouse

ALTER AUTHORIZATION ON
     [ <class_type> :: ] <entity_name>
     TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::= {
    DATABASE
     | SCHEMA
     | OBJECT
    }

<entity_name> ::=
    {
    database_name
     | schema_name
     | [ schema_name. ] object_name
    }
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Аргументы

\<class_type>. Защищаемый класс сущности, для которой изменяется владелец. По умолчанию это класс OBJECT.

|Class|Продукт|
|-|-|
|OBJECT|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздние версии, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].|
|ASSEMBLY|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|ASYMMETRIC KEY|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|AVAILABILITY GROUP |**Область применения**: SQL Server 2012 и выше.|
|CERTIFICATE|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|CONTRACT|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше.|
|DATABASE|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Дополнительные сведения см. в статье об [ALTER AUTHORIZATION для баз данных](#alter-authorization-for-databases).|
|ENDPOINT|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше.|
|FULLTEXT CATALOG|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|FULLTEXT STOPLIST|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|MESSAGE TYPE|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше.|
|REMOTE SERVICE BINDING|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше.|
|ROLE|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|ROUTE|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше.|
|SCHEMA|**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздние версии, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].|
|SEARCH PROPERTY LIST|**Применимо к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|SERVER ROLE|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше.|
|SERVICE|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше.|
|SYMMETRIC KEY|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|TYPE|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|
|XML SCHEMA COLLECTION|**Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].|

*entity_name* — имя сущности.

*principal_name* | SCHEMA OWNER —имя субъекта безопасности, который будет являться владельцем сущности. Объекты базы данных должны принадлежать субъекту базы данных; пользователю базы данных или роли. Объекты сервера (такие как базы данных) должны принадлежать субъекту сервера (имя для входа). Определите **SCHEMA OWNER** в качестве *principal_name-, чтобы показать, что объект должен принадлежать участнику, который владеет схемой объекта.

## <a name="remarks"></a>Remarks

Инструкция ALTER AUTHORIZATION может использоваться для изменения владельца любой сущности, у которой он есть. Владение содержащимися в базе данных сущностями можно передать любому участнику уровня базы данных. Владение сущностями уровня сервера можно передать только участникам уровня сервера.

> [!IMPORTANT]
> Начиная с [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], пользователь может владеть объектом (OBJECT) или типом (TYPE), содержащимся в схеме, которая принадлежит другому пользователю базы данных. Это поведение было изменено по сравнению с предыдущими версиями [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в разделах [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md) и [TYPEPROPERTY (Transact-SQL)](../../t-sql/functions/typeproperty-transact-sql.md).

Владение можно передавать для следующих, содержащихся в схемах сущностей типа «объект»: таблиц, представлений, функций, процедур, очередей и синонимов.

Нельзя передавать владение для следующих сущностей: связанных серверов, статистики, ограничений, правил, значений по умолчанию, триггеров, очередей компонента [!INCLUDE[ssSB](../../includes/sssb-md.md)], учетных данных, функций секционирования, схем секционирования, главных ключей баз данных, главного ключа службы, а также для уведомлений о событиях.

Нельзя передавать владение элементами следующих защищаемых классов: сервером, именем входа, пользователем, ролью приложения и столбцом.

Аргумент SCHEMA OWNER допустим только в случае передачи владения сущностью, содержащейся в схеме. Аргумент SCHEMA OWNER позволяет передать владение сущностью владельцу схемы, в которой она находится. В схемах содержатся только сущности классов OBJECT, TYPE или XML SCHEMA COLLECTION.

Если целевая сущность не представляет собой базу данных, а передаваемая сущность передается новому владельцу, все разрешения на целевую сущность удаляются.

> [!CAUTION]
> В [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] поведение схем отличается от более ранних версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Код, предполагающий, что схемы эквивалентны пользователям базы данных, может возвращать неверные результаты. Старые представления каталога, содержащие таблицу sysobjects, не могут быть использованы в базе данных, в которой когда-либо выполнялась любая из следующих инструкций DDL: CREATE SCHEMA, ALTER SCHEMA, DROP SCHEMA, CREATE USER, ALTER USER, DROP USER, CREATE ROLE, ALTER ROLE, DROP ROLE, CREATE APPROLE, ALTER APPROLE, DROP APPROLE, ALTER AUTHORIZATION. В базе данных, в которой когда-либо выполнялась любая из этих инструкций, необходимо использовать новые представления каталога. В новых представлениях каталогов учитывается разделение участников и схем, введенное в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Дополнительные сведения о представлениях каталогов см. в статье [Представления каталогов (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md).

 Имейте в виду следующее:

> [!IMPORTANT]
> Единственный надежный способ найти владельца объекта — запросить представление каталога **sys.objects**. Единственный надежный способ найти владельца типа — использовать функцию TYPEPROPERTY.

## <a name="special-cases-and-conditions"></a>Особые случаи и условия

В следующей таблице перечислены особые случаи, исключения и условия, касающиеся изменения авторизации.

|Class|Условие|
|-----------|---------------|
|OBJECT|Нельзя изменить владельца триггеров, ограничений, правил, значений по умолчанию, статистик, системных объектов, очередей, индексированных представлений и таблиц с индексированными представлениями.|
|SCHEMA|При передаче владения разрешения на содержащиеся в схеме объекты, у которых нет явных владельцев, удаляются. Нельзя изменить владельца схем sys, dbo и information_schema.|
|TYPE|Нельзя изменить владельца сущности TYPE, принадлежащей схеме sys или information_schema.|
|CONTRACT, MESSAGE TYPE или SERVICE|Нельзя изменить владельца системных сущностей.|
|SYMMETRIC KEY|Нельзя изменить владельца глобальных временных ключей.|
|CERTIFICATE или ASYMMETRIC KEY|Нельзя передавать владение данными сущностями роли или группе.|
|ENDPOINT|Участник должен представлять собой имя входа в систему.|

## <a name="alter-authorization-for-databases"></a> ALTER AUTHORIZATION для баз данных

### <a name="for-sql-server"></a>Для SQL Server

**Требования к новому владельцу:** Новый участник-владелец должен быть одним из следующих:

- имя входа для проверки подлинности SQL Server;
- имя входа для проверки подлинности Windows, представляющее пользователя Windows (а не группу);
- пользователь Windows, проходящий проверку подлинности с использованием имени входа для проверки подлинности Windows, представляющего группу Windows.

**Требования к пользователю, выполняющему инструкцию ALTER AUTHORIZATION** Если вы не являетесь членом предопределенной роли сервера **sysadmin**, вам требуется как минимум разрешение TAKE OWNERSHIP для базы данных и разрешение IMPERSONATE для имени входа нового владельца.

### <a name="for-azure-sql-database"></a>Для Базы данных SQL Azure

**Требования к новому владельцу:** Новый участник-владелец должен быть одним из следующих:

- имя входа для проверки подлинности SQL Server;
- федеративный пользователь (не группа) в Azure AD;
- управляемый пользователь (не группа) или приложение в Azure AD.

Если новый владелец является пользователем Azure Active Directory, он не может существовать как пользователь в базе данных, где новый владелец станет новым DBO. Такого пользователя Azure AD необходимо удалить из базы данных перед выполнением инструкции ALTER AUTHORIZATION, указав в качестве владельца базы данных нового пользователя. Дополнительные сведения о настройке пользователей Azure Active Directory с базой данных SQL см. в статье [Подключение к базе данных SQL или [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] с использованием проверки подлинности Azure Active Directory](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/).

**Требования к пользователю, выполняющему инструкцию ALTER AUTHORIZATION** Необходимо подключиться к целевой базе данных, чтобы изменить ее владельца.

Изменить владельца базы данных могут следующие типы учетных записей.

- Имя входа участника уровня службы. (Администратор SQL Azure, подготовленный при создании сервера Базы данных SQL.)
- Администратор Active Directory для Azure SQL Server.
- Текущий владелец базы данных.

Следующая таблица содержит сводку требований.

Исполнитель  |Назначение  |Результат
---------|---------|---------
Имя входа для проверки подлинности SQL Server  |Имя входа для проверки подлинности SQL Server|Успешно
Имя входа для проверки подлинности SQL Server  |Пользователь Azure AD|Ошибка
Пользователь Azure AD  |Имя входа для проверки подлинности SQL Server|Успешно
Пользователь Azure AD  |Пользователь Azure AD|Успешно

Чтобы проверить владельца базы данных Azure AD, выполните следующую команду Transact-SQL в базе данных пользователя (в этом примере `testdb`).

```sql
SELECT CAST(owner_sid as uniqueidentifier) AS Owner_SID
FROM sys.databases
WHERE name = 'testdb';
```

Выходные данные будут идентификатором (например, 6D8B81F6-7C79-444C-8858-4AF896C03C67), соответствующим ObjectID Azure AD, назначенному `richel@cqclinic.onmicrosoft.com`. Если пользователь с именем для входа с проверкой подлинности SQL Server является владельцем базы данных, выполните следующую инструкцию в базе данных master, чтобы проверить сведения о владельце базы данных:

```sql
SELECT d.name, d.owner_sid, sl.name
FROM sys.databases AS d
JOIN sys.sql_logins AS sl
ON d.owner_sid = sl.sid;

```

### <a name="best-practice"></a>Рекомендации

Не используйте пользователей Azure AD в качестве отдельных владельцев базы данных — используйте группу Azure AD как член предопределенной роли базы данных **db_owner**. Далее приводятся действия по настройке имени входа в качестве владельца базы данных и назначению группы Azure Active Directory (`mydbogroup`) в качестве члена роли **db_owner**.

1. Войдите в SQL Server как администратор Azure AD и измените владельца базы данных на отключенное имя входа для проверки подлинности SQL Server. Например, в базе данных пользователя выполните следующую команду:

   ```sql
   ALTER AUTHORIZATION ON database::testdb TO DisabledLogin;
   ```

1. Создайте группу Azure AD, которая должна быть владельцем базы данных, и добавьте ее в качестве пользователя в базу данных пользователя. Пример:

   ```sql
   CREATE USER [mydbogroup] FROM EXTERNAL PROVIDER;
   ```

1. В базе данных добавьте пользователя, представляющего группу Azure AD, в предопределенную роль базы данных **db_owner**. Пример:

   ```sql
   ALTER ROLE db_owner ADD MEMBER mydbogroup;
   ```

Теперь члены `mydbogroup` могут централизованно управлять базой данных как члены роли **db_owner**.

- Когда члены этой группы удаляются из группы Azure AD, они автоматически теряют разрешения dbo на эту базу данных.
- Аналогичным образом новые члены, добавляемые в группу Azure AD `mydbogroup`, автоматически получают доступ dbo для этой базы данных.

Чтобы проверить наличие действующего разрешения dbo у конкретного пользователя, пользователь должен выполнить следующую инструкцию:

```sql
SELECT IS_MEMBER ('db_owner');
```

Возвращаемое значение 1 указывает, что пользователь является членом роли.

## <a name="permissions"></a>Разрешения

Требует разрешения TAKE OWNERSHIP для сущности. Если новый владелец не является пользователем, выполняющим данную инструкцию, также требуется одно из следующих условий: 1) разрешение IMPERSONATE для нового владельца, если это пользователь или имя входа; 2) если новый владелец представляет собой роль — членство в роли или разрешение ALTER для этой роли; 3) если новый владелец представляет собой роль приложения — разрешение ALTER для роли приложения.

## <a name="examples"></a>Примеры

### <a name="a-transfer-ownership-of-a-table"></a>A. Передача владения таблицей

В следующем примере владение таблицей `Sprockets` передается пользователю `MichikoOsada`. Эта таблица расположена в схеме `Parts`.

```sql
ALTER AUTHORIZATION ON OBJECT::Parts.Sprockets TO MichikoOsada;
GO
```

Запрос также может выглядеть следующим образом:

```sql
ALTER AUTHORIZATION ON Parts.Sprockets TO MichikoOsada;
GO
```

Если объекты схемы не включены в инструкцию, [!INCLUDE[ssDE](../../includes/ssde-md.md)] выполнит поиск объекта в схеме пользователей по умолчанию. Пример:

```sql
ALTER AUTHORIZATION ON Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::Sprockets TO MichikoOsada;
```

### <a name="b-transfer-ownership-of-a-view-to-the-schema-owner"></a>Б. Передача владения представлением владельцу схемы

В следующем примере передается владение представлением `ProductionView06` владельцу содержащей его схемы. Это представление расположено в схеме `Production`.

```sql
ALTER AUTHORIZATION ON OBJECT::Production.ProductionView06 TO SCHEMA OWNER;
GO
```

### <a name="c-transfer-ownership-of-a-schema-to-a-user"></a>В. Передача владения схемой пользователю

В следующем примере владение схемой `SeattleProduction11` передается пользователю `SandraAlayo`.

```sql
ALTER AUTHORIZATION ON SCHEMA::SeattleProduction11 TO SandraAlayo;
GO
```

### <a name="d-transfer-ownership-of-an-endpoint-to-a-sql-server-login"></a>Г. Передача владения конечной точкой имени входа в SQL Server

В следующем примере владение конечной точкой `CantabSalesServer1` передается `JaePak`. Так как конечная точка представляет собой защищаемую сущность уровня сервера, ее можно передать только участнику уровня сервера.

**Область применения**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и более поздних версий.

```sql
ALTER AUTHORIZATION ON ENDPOINT::CantabSalesServer1 TO JaePak;
GO
```

### <a name="e-changing-the-owner-of-a-table"></a>Д. Изменение владельца таблицы

В каждом из следующих примеров показано изменение владельца таблицы `Sprockets` в базе данных `Parts` на пользователя базы данных `MichikoOsada`.

```sql
ALTER AUTHORIZATION ON Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON dbo.Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::dbo.Sprockets TO MichikoOsada;
```

### <a name="f-changing-the-owner-of-a-database"></a>Е. Изменение владельца базы данных

 **Применимо к**: [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] и выше, [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)].

 В следующем примере показано, как изменить владельца базы данных `Parts` на имя входа `MichikoOsada`.

```sql
ALTER AUTHORIZATION ON DATABASE::Parts TO MichikoOsada;
```

### <a name="g-changing-the-owner-of-a-sql-database-to-an-azure-ad-user"></a>Ж. Изменение владельца базы данных SQL на пользователя Azure AD

В следующем примере администратор Azure Active Directory для SQL Server в организации с Active Directory `cqclinic.onmicrosoft.com` может изменить текущую принадлежность базы данных `targetDB` и сделать пользователя AAD `richel@cqclinic.onmicorsoft.com` новым владельцем базы данных с помощью следующей команды:

```sql
ALTER AUTHORIZATION ON database::targetDB TO [rachel@cqclinic.onmicrosoft.com];
```

В Azure AD имя пользователя должно быть заключено в скобки `[]`.

## <a name="see-also"></a>См. также:
 [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md) [TYPEPROPERTY (Transact-SQL)](../../t-sql/functions/typeproperty-transact-sql.md) [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)


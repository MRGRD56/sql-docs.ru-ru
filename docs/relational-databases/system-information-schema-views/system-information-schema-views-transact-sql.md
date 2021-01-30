---
description: Представления схемы системных сведений (Transact-SQL)
title: Представления схемы системных сведений (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 07/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- information schema views
- schemas [SQL Server], information schema views
- metadata [SQL Server], views
- views [SQL Server], information schema
- system views [SQL Server], information schema
ms.assetid: 7e9f1dfe-27e9-40e7-8fc7-bfc5cae6be10
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a39ec8c22b4267e1c6b6bc2f213775836b073a48
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185480"
---
# <a name="system-information-schema-views-transact-sql"></a>Представления схемы системных сведений (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Представление информационной схемы является одним из способов получения метаданных, которые предоставляет [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Представления информационной схемы обеспечивают внутренний, системный, не зависящий от таблиц обзор метаданных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Представления информационной схемы позволяют приложениям правильно работать, несмотря на значительные изменения, внесенные в базовые системные таблицы. Представления информационной схемы, включенные в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], соответствуют стандартному определению ISO для INFORMATION_SCHEMA.

> [!IMPORTANT]
> В представления информационной схемы были внесены определенные изменения, нарушающие обратную совместимость. Эти изменения описаны в разделах, посвященных конкретным представлениям.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает соглашения по трехкомпонентному именованию при ссылках на текущий сервер. Стандарт ISO также поддерживает соглашения по трехкомпонентному именованию. Однако имена, которые используются в обоих соглашениях, различаются. Представления информационной схемы определяются в специальной схеме с именем INFORMATION_SCHEMA. Эта схема содержится в любой базе данных. Каждое представление информационной схемы содержит метаданные для всех объектов, хранящихся в этой конкретной базе данных. В следующей таблице представлены связи между именами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и стандартными именами SQL.

|Имя SQL Server|Соответствует эквивалентному стандартному имени SQL|
|---------------------|-----------------------------------------------|
|База данных|Каталог|
|схема|схема|
|Объект|Объект|
|определяемый пользователем тип данных|Домен|

Настоящее соглашение по соответствию имен применяется к следующим представлениям [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], совместимым со стандартом ISO.

:::row:::
    :::column:::
        [CHECK_CONSTRAINTS](../../relational-databases/system-information-schema-views/check-constraints-transact-sql.md)

        [COLUMN_DOMAIN_USAGE](../../relational-databases/system-information-schema-views/column-domain-usage-transact-sql.md)

        [COLUMN_PRIVILEGES](../../relational-databases/system-information-schema-views/column-privileges-transact-sql.md)

        [COLUMNS](../../relational-databases/system-information-schema-views/columns-transact-sql.md)

        [CONSTRAINT_COLUMN_USAGE](../../relational-databases/system-information-schema-views/constraint-column-usage-transact-sql.md)

        [CONSTRAINT_TABLE_USAGE](../../relational-databases/system-information-schema-views/constraint-table-usage-transact-sql.md)

        [DOMAIN_CONSTRAINTS](../../relational-databases/system-information-schema-views/domain-constraints-transact-sql.md)

        [ДОМЕНЫ](../../relational-databases/system-information-schema-views/domains-transact-sql.md)

        [KEY_COLUMN_USAGE](../../relational-databases/system-information-schema-views/key-column-usage-transact-sql.md)

        [PARAMETERS](../../relational-databases/system-information-schema-views/parameters-transact-sql.md)
    :::column-end:::
    :::column:::
        [REFERENTIAL_CONSTRAINTS](../../relational-databases/system-information-schema-views/referential-constraints-transact-sql.md)

        [ROUTINES](../../relational-databases/system-information-schema-views/routines-transact-sql.md)

        [ROUTINE_COLUMNS](../../relational-databases/system-information-schema-views/routine-columns-transact-sql.md)

        [SCHEMATA](../../relational-databases/system-information-schema-views/schemata-transact-sql.md)

        [TABLE_CONSTRAINTS](../../relational-databases/system-information-schema-views/table-constraints-transact-sql.md)

        [TABLE_PRIVILEGES](../../relational-databases/system-information-schema-views/table-privileges-transact-sql.md)

        [TABLES](../../relational-databases/system-information-schema-views/tables-transact-sql.md)

        [VIEW_COLUMN_USAGE](../../relational-databases/system-information-schema-views/view-column-usage-transact-sql.md)

        [VIEW_TABLE_USAGE](../../relational-databases/system-information-schema-views/view-table-usage-transact-sql.md)

        [VIEWS](../../relational-databases/system-information-schema-views/views-transact-sql.md)
    :::column-end:::
:::row-end:::

Кроме того, некоторые представления содержат ссылки на различные классы данных, например символьные данные или двоичные данные.

При ссылке на представления информационной схемы необходимо использовать полное имя, включающее имя схемы `INFORMATION_SCHEMA`. Пример:

```sql
SELECT TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME, COLUMN_DEFAULT
FROM AdventureWorks2012.INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = N'Product';
```

## <a name="permissions"></a>Разрешения  
Видимость метаданных в представлениях информационной схемы ограничена защищаемыми объектами, которыми пользователь владеет или на которые пользователь предоставил какое-либо разрешение. Дополнительные сведения см. в разделе [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).

> [!NOTE]  
> Представления информационной схемы определяются на уровне сервера и поэтому не могут быть запрещены в контексте пользовательской базы данных. Чтобы отозвать или запретить доступ (SELECT), необходимо использовать базу данных master. По умолчанию открытая роль имеет разрешение SELECT на все представления информационной схемы, но содержимое ограничено правилами видимости метаданных.

## <a name="see-also"></a>См. также:

- [Системные представления &#40;&#41;Transact-SQL ](../../relational-databases/system-views/replication-views-transact-sql.md)
- [Типы данных (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)
- [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md) 

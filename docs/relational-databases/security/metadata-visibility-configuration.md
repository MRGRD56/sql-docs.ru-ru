---
title: Настройка видимости метаданных | Документация Майкрософт
description: Узнайте, как настроить видимость метаданных для защищаемых объектов, которыми владеет пользователь или которым предоставлено разрешение в SQL Server.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- subcomponents visibility [SQL Server]
- metadata [SQL Server], visibility
- permissions [SQL Server], metadata access
- viewing metadata
- objects [SQL Server], metadata
- displaying metadata
- database metadata [SQL Server]
- metadata [SQL Server], permissions
ms.assetid: 50d2e015-05ae-4014-a1cd-4de7866ad651
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 65eeb429fbef85f1e23a79a8f06299a64a5179d2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755864"
---
# <a name="metadata-visibility-configuration"></a>Настройка видимости метаданных
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Видимость метаданных ограничивается защищаемыми объектами, которыми пользователь владеет или на которые пользователю предоставлено разрешение. Например, следующий запрос возвращает строку, если пользователю было предоставлено разрешение SELECT или INSERT на таблицу `myTable`.  
  
```  
SELECT name, object_id  
FROM sys.tables  
WHERE name = N'myTable';  
GO  
```  
  
 Однако если пользователь не имеет никаких разрешений на `myTable`, запрос возвращает пустой результирующий набор.  
  
## <a name="scope-and-impact-of-metadata-visibility-configuration"></a>Область и влияние настроек видимости метаданных  
 Настройка видимости метаданных применяется только к следующим защищаемым объектам:  

:::row:::
    :::column:::
        Представления каталога

        Метаданные, предоставляющие встроенные функции

        представлений совместимости;
    :::column-end:::
    :::column:::
         Хранимые процедуры компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] **sp_help**

        Представления информационной схемы

        Расширенные свойства
    :::column-end:::
:::row-end:::
  
 Настройка видимости метаданных не применяется к следующим защищаемым объектам:  

:::row:::
    :::column:::
        Системные таблицы доставки журналов

        Системные таблицы плана обслуживания базы данных

        Системные таблицы репликации
    :::column-end:::
    :::column:::
         [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Системные таблицы агента

        Системные таблицы резервных копий

        Репликация и хранимые процедуры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sp_help **агента**
    :::column-end:::
:::row-end:::

 Ограниченная возможность доступа к метаданным означает следующее.  
  
-   Приложения, предполагающие **общий** доступ к метаданным, разорвутся.  
  
-   Запросы на системные представления смогут вернуть только подмножество строк или иногда пустой результирующий набор.  
  
-   Встроенные функции по формированию метаданных, например OBJECTPROPERTYEX, могут вернуть значение NULL.  
  
-   Хранимые процедуры [!INCLUDE[ssDE](../../includes/ssde-md.md)] **sp_help** могут возвращать только подмножество строк или значение NULL.  
  
 SQL модули, например хранимые процедуры и триггеры, выполняются в контексте безопасности участника и поэтому имеют ограниченный доступ к метаданным. Например, в следующем коде, когда хранимая процедура пытается получить доступ к метаданным для таблицы `myTable` , на который участник не имеет прав, возвращается пустой результирующий набор. В ранних версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]возвращается строка.  
  
```  
CREATE PROCEDURE assumes_caller_can_access_metadata  
BEGIN  
SELECT name, object_id   
FROM sys.objects   
WHERE name = N'myTable';  
END;  
GO  
```  
  
 Чтобы позволить участникам просмотреть метаданные, можно предоставить им разрешение VIEW DEFINITION на соответствующую область: уровень объекта, уровень базы данных или уровень сервера. Таким образом, если в предыдущем примере участник имеет разрешение VIEW DEFINITION на таблицу `myTable`, хранимая процедура возвращает строку. Дополнительные сведения см. в статьях [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md) и [GRANT Database Permissions (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md).  
  
 Также можно изменить хранимую процедуру таким образом, что она будет выполняться под учетными данными владельца. Если владелец процедуры и владелец таблицы один и тот же, применяются цепочки владения, и контекст безопасности владельца процедуры открывает доступ к метаданным таблицы `myTable`. Под таким сценарием следующий код возвращает строку метаданных участнику.  
  
> [!NOTE]  
>  В следующем примере использования представление каталога [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) применяется вместо представления совместимости [sys.sysobjects](../../relational-databases/system-compatibility-views/sys-sysobjects-transact-sql.md) .  
  
```  
CREATE PROCEDURE does_not_assume_caller_can_access_metadata  
WITH EXECUTE AS OWNER  
AS  
BEGIN  
SELECT name, object_id  
FROM sys.objects   
WHERE name = N'myTable'   
END;  
GO  
```  
  
> [!NOTE]  
>  Можно использовать EXECUTE AS, чтобы временно переключиться на контекст безопасности участника. Дополнительные сведения см. в разделе [EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-transact-sql.md).  
  
## <a name="benefits-and-limits-of-metadata-visibility-configuration"></a>Преимущества и ограничения настроек видимости метаданных  
 Настройка видимости метаданных может играть очень важную роль в общем плане безопасности. Однако иногда даже опытный пользователь может форсировать раскрытие некоторых метаданных. Рекомендуется развертывание разрешений метаданных как одной из многих глубоко эшелонированных защит.  
  
 Теоретически возможно принудительно вызвать выброс метаданных в сообщениях об ошибках, манипулируя порядком оценки предикатов в запросах. Возможность таких *атак методом проб и ошибок* относится не только к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это подразумевается ассоциативными и коммуникативными преобразованиями, разрешенными в реляционной алгебре. Можно снизить эту угрозу ограничением объема сведений, возвращаемых в сообщениях об ошибках. Также, чтобы ограничить видимость метаданных таким образом, можно запустить сервер с флагом трассировки 3625. Этот флаг трассировки ограничивает объем сведений, отображаемых в сообщениях об ошибках. В свою очередь, это помогает предотвратить форсированное раскрытие. Компромисс заключается в том, что сообщения об ошибках будут в сжатом виде и могут вызвать сложности при использовании для отладки. Дополнительные сведения см. в разделах [Параметры запуска службы Database Engine](../../database-engine/configure-windows/database-engine-service-startup-options.md) и [Флаги трассировки (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).  
  
 Следующие метаданные не подвергаются форсированному раскрытию.  
  
-   Значение, хранимое в столбце **provider_string** объекта **sys.servers**. Пользователь, не имеющий разрешения ALTER ANY LINKED SERVER, в данном столбце получит значение NULL.  
  
-   Определение источника пользовательского объекта, например хранимой процедуры или триггера. Код источника видим только в том случае, когда выполняется одно из следующих условий.  
  
    -   Пользователь имеет разрешение VIEW DEFINITION на объект.  
  
    -   Пользователю не было запрещено получение разрешения VIEW DEFINITION на объект и он имеет разрешения CONTROL, ALTER или TAKE OWNERSHIP. Все остальные пользователи получат значение NULL.  
  
-   Столбцы определений, найденные в следующих представлениях каталога:  

    - **sys.all_sql_modules**  
    - **sys.server_sql_modules**  
    - **sys.default_constraints**
    - **sys.numbered_procedures**
    - **sys.sql_modules**
    - **sys.check_constraints**
    - **sys.computed_columns**

-   Столбец **ctext** в представлении совместимости **syscomments** .  
  
-   Выходные данные процедуры **sp_helptext** .  
  
-   Следующие столбцы в представлениях информационной схемы:

    - INFORMATION_SCHEMA.CHECK_CONSTRAINTS.CHECK_CLAUSE
    - INFORMATION_SCHEMA.DOMAINS.DOMAIN_DEFAULT
    - INFORMATION_SCHEMA.ROUTINES.ROUTINE_DEFINITION
    - INFORMATION_SCHEMA.COLUMNS.COLUMN_DEFAULT
    - INFORMATION_SCHEMA.ROUTINE_COLUMNS.COLUMN_DEFAULT
    - INFORMATION_SCHEMA.VIEWS.VIEW_DEFINITION

-   Функция OBJECT_DEFINITION().  
  
-   Значение, хранимое в столбце password_hash объекта **sys.sql_logins**.  Пользователь, не имеющий разрешения CONTROL SERVER, в данном столбце получит значение NULL.  
  
> [!NOTE]  
>  Определения SQL встроенных системных процедур и функций видимы для всех пользователей через представление каталога **sys.system_sql_modules** , хранимую процедуру **sp_helptext** и функцию OBJECT_DEFINITION().  
  
## <a name="general-principles-of-metadata-visibility"></a>Общие принципы видимости метаданных  
 Ниже представлены некоторые общие принципы, относящиеся к видимости метаданных.  
  
-   Неявные разрешения предопределенных ролей.  
  
-   Область разрешений.  
  
-   Приоритет инструкции DENY  
  
-   Видимость метаданных вспомогательных компонентов.  
  
### <a name="fixed-roles-and-implicit-permissions"></a>Предопределенные роли и неявные разрешения  
 Доступ предопределенных ролей к метаданным зависит от соответствующих неявных разрешений.  
  
### <a name="scope-of-permissions"></a>Область разрешений.  
 Разрешения на одну область дают возможность видеть метаданные в этой области и на всех включенных областях. Например, разрешение SELECT на схему предполагает, что участник разрешения имеет разрешение SELECT на все защищаемые объекты, содержащиеся в этой схеме. Таким образом, предоставление разрешения SELECT на схему позволяет пользователю видеть метаданные схемы, а также все находящиеся в ней таблицы, представления, функции, процедуры, очереди, синонимы, типы и коллекции XML-схем. Дополнительные сведения об областях см. в разделе [Иерархия разрешений (компонент Database Engine)](../../relational-databases/security/permissions-hierarchy-database-engine.md).  
  
### <a name="precedence-of-deny"></a>Приоритет инструкции DENY  
 Инструкция DENY обычно является приоритетной по отношению к другим разрешениям. Например, если пользователю базы данных на схему было предоставлено разрешение EXECUTE, но это разрешение было запрещено на хранимую процедуру схемы, он не сможет просмотреть метаданные для этой хранимой процедуры.  
  
 К тому же, если пользователю было запрещено разрешение EXECUTE на схему, но была предоставлена инструкция EXECUTE на хранимую процедуру данной схемы, он также не сможет просмотреть метаданные для этой хранимой процедуры.  
  
 Другой пример: если пользователю разрешение EXECUTE на хранимую процедуру было и предоставлено, и запрещено, что возможно благодаря членству в различных ролях, то преимущество имеет инструкция DENY, и пользователь не сможет просмотреть метаданные хранимой процедуры.  
  
### <a name="visibility-of-subcomponent-metadata"></a>Видимость метаданных вспомогательных компонентов.  
 Видимость вспомогательных компонентов, таких как индексы, проверочные ограничения и триггеры, определяется разрешениями на родительский объект. Эти вспомогательные компоненты не имеют предоставляемых разрешений. Например, если пользователю предоставлено какое-то разрешение на таблицу, он может просмотреть метаданные для столбцов таблицы, индексов, проверочных ограничений, триггеров и других подобных вспомогательных компонентов.  
  
#### <a name="metadata-that-is-accessible-to-all-database-users"></a>Метаданные, доступные для всех пользователей базы данных  
 Некоторые метаданные должны быть доступны для всех пользователей в указанной базе данных. Например, файловые группы не имеют присвоенных разрешений; поэтому пользователю не может быть предоставлено разрешение на просмотр метаданных файловой группы. Однако любой пользователь, который может создать таблицу, должен иметь доступ к метаданным файловой группы, чтобы использовать предложения ON *filegroup* или TEXTIMAGE_ON *filegroup* инструкции CREATE TABLE.  
  
 Метаданные, возвращаемые функциями DB_ID() и DB_NAME(), видимы всем пользователям.  
  
 Это список представлений каталога, видимых для **общей** роли.  

:::row:::
    :::column:::
        **sys.partition_functions**

        **sys.partition_schemes**

        **sys.filegroups**

        **sys.database_files**

        **sys.partitions**

        **sys.schemas**

        **sys.sql_dependencies**

        **sys.parameter_type_usages**
    :::column-end:::
    :::column:::
        **sys.partition_range_values**

        **sys.data_spaces**

        **sys.destination_data_spaces**

        **sys.allocation_units**

        **sys.messages**

        **sys.configurations**

        **sys.type_assembly_usages**

        **sys.column_type_usages**
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>См. также:  
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [Предложение EXECUTE AS (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md)   
 [Представления каталога (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Представления совместимости (Transact-SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  

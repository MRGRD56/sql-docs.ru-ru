---
title: Собственная компиляция таблиц и хранимых процедур
description: Узнайте, как в выполняющейся в памяти OLTP можно компилировать оптимизированные для памяти таблицы и хранимые процедуры, которые обращаются к оптимизированным для памяти таблицам в машинный код для повышения производительности.
ms.custom: seo-dt-2019
ms.date: 04/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 5880fbd9-a23e-464a-8b44-09750eeb2dad
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 629f5fa5c8b3a6638ed7e18c58bb7073e03a6039
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107492520"
---
# <a name="native-compilation-of-tables-and-stored-procedures"></a>Собственная компиляция таблиц и хранимых процедур
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
В In-Memory OLTP введено понятие компиляции в собственном коде. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] имеет возможность компиляции в собственном коде хранимых процедур, которые обращаются к оптимизированным для памяти таблицам. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может также скомпилировать в собственном коде оптимизированные для памяти таблицы. Компиляция в собственном коде обеспечивает повышение скорости доступа к данным и более эффективное выполнение запросов по сравнению с интерпретируемыми (традиционными) командами [!INCLUDE[tsql](../../includes/tsql-md.md)]. Компиляция таблиц и хранимых процедур в собственном коде создает библиотеки DLL.

Кроме того, поддерживается компиляция в собственном коде оптимизированных для памяти табличных типов. Дополнительные сведения см. в разделе [Улучшение производительности временной таблицы и табличной переменной с помощью оптимизации памяти](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md).

Компиляцией в собственном коде называется процесс преобразования программных конструкций в машинный код, состоящий из процессорных инструкций, без необходимости их дальнейшей компиляции или интерпретации.

In-Memory OLTP компилирует оптимизированные для памяти таблицы при их создании и скомпилированные в собственном коде хранимые процедуры при их загрузке в собственные библиотеки DLL. Кроме того, библиотеки DLL компилируются повторно после перезапуска базы данных или сервера. Сведения, необходимые для воссоздания библиотек DLL хранятся в метаданных базы данных. Библиотеки DLL не являются частью базы данных, хотя они и связаны с ней. Например, DLL-библиотеки не входят в резервные копии базы данных.

> [!NOTE]
> Оптимизированные для памяти таблицы во время перезапуска сервера компилируются повторно. Чтобы ускорить восстановление данных, процедуры, скомпилированные в собственном коде, не компилируются повторно во время перезагрузки сервера, а компилируются во время первого выполнения. В результате этой отложенной компиляции процедуры, скомпилированные в собственном коде, отображаются только при вызове [sys.dm_os_loaded_modules (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-loaded-modules-transact-sql.md) после первого выполнения.

## <a name="maintenance-of-in-memory-oltp-dlls"></a>Обслуживание библиотек DLL базы данных In-Memory OLTP

Следующий запрос отображает все библиотеки DLL таблиц и хранимых процедур, загруженных на данный момент в память сервера:

```sql
SELECT
        mod1.name,
        mod1.description
    from
        sys.dm_os_loaded_modules  as mod1
    where
        mod1.description = 'XTP Native DLL';
```

Администраторам базы данных не нужно хранить файлы, созданные при компиляции в собственном коде. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически удаляет созданные файлы, которые больше не нужны. Например, созданные файлы будут стерты при удалении таблицы и хранимой процедуры или при удалении базы данных.

> [!NOTE]
> Если компиляция была прервана или завершилась ошибкой, то некоторые сформированные файлы не будут удалены. Эти файлы оставляются специально для целей поддержки и будут удалены при удалении базы данных.

> [!NOTE]
> SQL Server компилирует библиотеки DLL для всех таблиц, необходимых для восстановления базы данных. Если таблица была удалена непосредственно до перезапуска базы данных, остатки таблицы все еще могут оставаться в файлах контрольных точек или журнале транзакций для того, чтобы библиотеки DLL для таблицы можно было перекомпилировать во время запуска базы данных. После перезагрузки компьютера библиотека DLL будет выгружена и удалена в процессе обычной очистки.

## <a name="native-compilation-of-tables"></a>Компиляция таблиц в собственном коде

Создание оптимизированной для памяти таблицы с помощью инструкции **CREATE TABLE** приводит к тому, что табличные данные записываются в метаданные базы данных, а в памяти создаются структуры таблиц и индексов. Таблица также будет скомпилирована в библиотеке DLL.

Рассмотрим следующий пример скрипта, который создает базу данных и оптимизированную для памяти таблицу:

```sql
USE master;
GO

CREATE DATABASE DbMemopt3;
GO

ALTER DATABASE DbMemopt3
    add filegroup DbMemopt3_mod_memopt_1_fg
        contains memory_optimized_data
;
GO

-- You must edit the front portion of filename= path, to where your DATA\ subdirectory is,
-- keeping only the trailing portion '\DATA\DbMemopt3_mod_memopt_1_fn'!

ALTER DATABASE DbMemopt3
    add file
    (
        name     = 'DbMemopt3_mod_memopt_1_name',
        filename = 'C:\DATA\DbMemopt3_mod_memopt_1_fn'

        --filename = 'C:\Program Files\Microsoft SQL Server\MSSQL13.SQLSVR2016ID\MSSQL\DATA\DbMemopt3_mod_memopt_1_fn'
    )
        to filegroup DbMemopt3_mod_memopt_1_fg
;
GO

USE DbMemopt3;
GO

CREATE TABLE dbo.t1
(
    c1 int not null primary key nonclustered,
    c2 int
)
    with (memory_optimized = on)
;
GO



-- You can safely rerun from here to the end.

-- Retrieve the path of the DLL for table t1.


DECLARE @moduleName  nvarchar(256);

SET @moduleName =
    (
        '%xtp_t_' +
        cast(db_id() as nvarchar(16)) +
        '_' +
        cast(object_id('dbo.t1') as nvarchar(16)) +
        '%.dll'
    )
;


-- SEARCHED FOR NAME EXAMPLE:  mod1.name LIKE '%xtp_t_8_565577053%.dll'
PRINT @moduleName;


SELECT
        mod1.name,
        mod1.description
    from
        sys.dm_os_loaded_modules  as mod1
    where
        mod1.name LIKE @moduleName
    order by
        mod1.name
;
-- ACTUAL NAME EXAMPLE:  mod1.name = 'C:\Program Files\Microsoft SQL Server\MSSQL13.SQLSVR2016ID\MSSQL\DATA\xtp\8\xtp_t_8_565577053_184009305855461.dll'
GO

--   DROP DATABASE DbMemopt3;  -- Clean up.
GO
```

При создании таблицы также формируется и загружается в память библиотека DLL таблицы. Запрос динамического административного представления сразу после выполнения инструкции CREATE TABLE возвращает путь к библиотеке DLL таблицы.

Библиотека DLL таблицы понимает структуры индекса и формат строк таблицы. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует библиотеку DLL для просмотра индексов, получения строк, а также хранения содержимого строк.

## <a name="native-compilation-of-stored-procedures"></a>Компиляция хранимых процедур в собственном коде

Хранимые процедуры, которые отмечены как NATIVE_COMPILATION, компилируются в собственном коде. Это означает, что все инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] в процедуре компилируются в машинный код для эффективного выполнения бизнес-логики, критической с точки зрения производительности.

Дополнительные сведения о скомпилированных в собственном коде хранимых процедурах см. в разделе [Natively Compiled Stored Procedures](./a-guide-to-query-processing-for-memory-optimized-tables.md).

Рассмотрим следующий пример хранимой процедуры, которая вставляет строки в таблицу t1 из предыдущего примера:

```sql
CREATE PROCEDURE dbo.native_sp
    with native_compilation,
         schemabinding,
         execute as owner
as
begin atomic
    with (transaction isolation level = snapshot,
          language = N'us_english')

    DECLARE @i int = 1000000;

    WHILE @i > 0
    begin
        INSERT dbo.t1 values (@i, @i+1);
        SET @i -= 1;
    end
end;
GO

EXECUTE dbo.native_sp;
GO

-- Reset.

DELETE from dbo.t1;
GO
```

Библиотека DLL для native_sp может напрямую взаимодействовать с библиотекой DLL для t1, а также c подсистемой хранилища In-Memory OLTP, чтобы вставлять строки с максимально возможной скоростью.

Компилятор In-Memory OLTP использует оптимизатор запросов для создания эффективного плана выполнения для каждого из запросов в хранимой процедуре. Обратите внимание, что скомпилированные в собственном коде хранимые процедуры не перекомпилируются автоматически при изменении данных в таблице. Дополнительные сведения о сборе статистики и хранимых процедурах в In-Memory OLTP см. в разделе [Статистика для таблиц, оптимизированных для памяти](../../relational-databases/in-memory-oltp/statistics-for-memory-optimized-tables.md).

## <a name="security-considerations-for-native-compilation"></a>Вопросы безопасности для компиляции в собственном коде

Компиляция в собственном коде таблиц и хранимых процедур использует компилятор In-Memory OLTP. Этот компилятор создает файлы, которые записываются на диск и загружаются в память. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует следующие механизмы для ограничения доступа к ним.

### <a name="native-compiler"></a>Собственный компилятор

Исполняемый файл компилятора, а также двоичные файлы и файлы заголовков, необходимые для компиляции в собственном коде, устанавливаются как часть экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в папке MSSQL\Binn\Xtp. Таким образом, если экземпляр по умолчанию устанавливается в C:\Program Files, то файлы компилятора устанавливаются в папку C:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL13.MSSQLSERVER\MSSQL\Binn\Xtp.

Чтобы ограничить доступ к компилятору, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует списки управления доступом (ACL), ограничивающие доступ к двоичным файлам. Все двоичные файлы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] защищены от изменения и несанкционированного доступа через списки управления доступом. Списки управления доступом собственного компилятора также ограничивают использование компилятора; только учетная запись службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и администраторы имеют разрешения на чтение и выполнение для файлов собственного компилятора.

### <a name="files-generated-by-a-native-compilation"></a>Файлы, созданные компиляцией в собственном коде

Файлы, создаваемые при компиляции таблицы хранимой процедуры, включают DLL и промежуточные файлы, в том числе файлы со следующими расширениями: C, OBJ, XML и PDB. Созданные файлы сохраняются во вложенной папке папки данных по умолчанию. Вложенная папка называется Xtp. При установке экземпляра по умолчанию с папкой данных по умолчанию созданные файлы помещаются в папку C:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL13.MSSQLSERVER\MSSQL\DATA\Xtp.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предотвращает изменение создаваемых библиотек DLL тремя способами.

- Если таблица или хранимая процедура компилируется в DLL, эта DLL немедленно загружается в память и связывается с процессом sqlserver.exe. Библиотеку DLL нельзя изменить, если она связана с процессом.

- При перезапуске базы данных перекомпилируются все таблицы и хранимые процедуры (удаленные и воссозданные) на основе метаданных базы данных. При этом удаляются все изменения, внесенные в созданный файл вредоносным агентом.

- Созданные файлы считаются частью пользовательских данных и имеют те же ограничения безопасности через списки ACL, что и файлы базы данных: только учетная запись службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и системные администраторы имеют доступ к этим файлам.

Взаимодействие с пользователем для управления этими файлами не требуется. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создает и удаляет файлы по мере необходимости.

## <a name="see-also"></a>См. также:

[Таблицы, оптимизированные для памяти](./sample-database-for-in-memory-oltp.md)

[Скомпилированные в собственном коде хранимые процедуры](./a-guide-to-query-processing-for-memory-optimized-tables.md)
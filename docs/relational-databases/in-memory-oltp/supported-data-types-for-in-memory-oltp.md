---
title: Поддерживаемые типы данных для выполняющейся в памяти OLTP | Документация Майкрософт
description: Сведения о типах данных, которые не поддерживают выполняющиеся в памяти OLTP функции, оптимизированные для памяти таблицы и скомпилированные в собственном коде модули T-SQL.
ms.custom: ''
ms.date: 06/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: a7380ef0-c9d7-49e4-b6de-fad34752b9f3
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6cfce0e118f2ba924baa38adcbb84e294b5cb527
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107489294"
---
# <a name="supported-data-types-for-in-memory-oltp"></a>Поддерживаемые типы данных для выполняющейся в памяти OLTP
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  В этой статье перечислены типы данных, которые не поддерживаются для компонентов выполняющейся в памяти OLTP:  
  
-   Таблицы, оптимизированные для памяти  
  
-   Модули, скомпилированные в собственном коде T-SQL  
  
## <a name="unsupported-data-types"></a>Неподдерживаемые типы данных  
 Следующие типы данных не поддерживаются:  

:::row:::
    :::column:::
        [datetimeoffset (Transact-SQL)](../../t-sql/data-types/datetimeoffset-transact-sql.md)

        [hierarchyid (Transact-SQL)](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)

        [sql_variant (Transact-SQL)](../../t-sql/data-types/sql-variant-transact-sql.md)
    :::column-end:::
    :::column:::
        [geography (Transact-SQL)](../../t-sql/spatial-geography/spatial-types-geography.md)

        [rowversion (Transact-SQL)](../../t-sql/data-types/rowversion-transact-sql.md)

        Определяемые пользователем типы
    :::column-end:::
    :::column:::
        [geometry (Transact-SQL)](../../t-sql/spatial-geometry/spatial-types-geometry-transact-sql.md)

        [xml (Transact-SQL)](../../t-sql/xml/xml-transact-sql.md)
    :::column-end:::
:::row-end:::
  
## <a name="notable-supported-data-types"></a>Важные поддерживаемые типы данных  
 Большинство типов данных поддерживаются компонентами выполняющейся в памяти OLTP. Обратите внимание на следующие компоненты:  
  
|Строковые и двоичные типы|Дополнительные сведения|  
|-----------------------------|--------------------------|  
|binary и varbinary*|[binary и varbinary (Transact-SQL)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|  
|char и varchar*|[char и varchar (Transact-SQL)](../../t-sql/data-types/char-and-varchar-transact-sql.md)|  
|nchar и nvarchar*|[nchar и nvarchar (Transact-SQL)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)|  
  
Для предшествующих строковых и двоичных типов данных, начиная с SQL Server 2016:  
  
- Отдельная оптимизированная для памяти таблица может также содержать несколько столбцов типа long, например `nvarchar(4000)`, даже если их совокупная длина превысит размер физической строки 8060 байт.  
  
- Оптимизированная для памяти таблица может содержать строковые и двоичные столбцы с максимальной длиной таких типов данных, например `varchar(max)`.  


### <a name="identify-lobs-and-other-columns-that-are-off-row"></a>Определение LOB-столбцов и других столбцов вне строки

Начиная с SQL Server 2016, оптимизированные для памяти таблицы поддерживают [хранение столбцов вне строки](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md), что позволяет одной табличной строке иметь размер более 8060 байт. Следующая инструкция Transact-SQL SELECT возвращает все столбцы вне строки для таблиц, оптимизированных для памяти. Обратите внимание на следующее.

- Все ключевые столбцы индекса сохранены в строке.
  - Теперь ключи неуникальных индексов могут включать столбцы со значениями NULL в таблицах, оптимизированных для памяти.
  - Индексы могут быть объявлены как UNIQUE для таблицы, оптимизированной для памяти.
- Все столбцы LOB хранятся вне строки.
- max_lengthax_length со значением -1 означает столбец большого объекта (LOB).


```sql
SELECT
        OBJECT_NAME(m.object_id) as [table],
        c.name                   as [column],
        c.max_length
    FROM
             sys.memory_optimized_tables_internal_attributes AS m
        JOIN sys.columns                                     AS c
                ON  m.object_id = c.object_id
                AND m.minor_id  = c.column_id
    WHERE
        m.type = 5;
```


### <a name="other-data-types"></a>Прочие типы данных


|Другие типы|Дополнительные сведения|  
|-----------------|--------------------------|  
|табличные типы|[Основные сведения о табличных переменных, оптимизированных для памяти](../../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md)|  
  
## <a name="see-also"></a>См. также:  
 [Поддержка Transact-SQL для In-Memory OLTP](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)   
 [Реализация SQL_VARIANT в таблице, оптимизированной для памяти](../../relational-databases/in-memory-oltp/implementing-sql-variant-in-a-memory-optimized-table.md)  
 [Размер строки и таблицы для таблиц, оптимизированных для памяти](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md)  
  
  

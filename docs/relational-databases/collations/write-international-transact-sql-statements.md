---
description: Написание инструкций Transact-SQL, адаптированных к международному использованию
title: Написание инструкций Transact-SQL, адаптированных к международному использованию | Документация Майкрософт
ms.custom: ''
ms.date: 04/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- writing international statements
- Transact-SQL international considerations
- international considerations [SQL Server], Transact-SQL
- Database Engine international considerations [SQL Server], Transact-SQL
- statements [SQL Server], international
- database international considerations [SQL Server], Transact-SQL
- dates [SQL Server], international considerations
ms.assetid: f0b10fee-27f7-45fe-aece-ccc3f63bdcdb
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8872cf6e593279a282c34868a2f019ac82e5bf01
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2021
ms.locfileid: "100339490"
---
# <a name="write-international-transact-sql-statements"></a>Написание инструкций Transact-SQL, адаптированных к международному использованию
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  В базах данных и использующих их приложениях, в которых применяются инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)] , можно обеспечить большую степень языковой переносимости или поддержку нескольких языков при условии соблюдения следующих требований.  

-   Начиная с [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] и в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] следует использовать:
    -   типы данных **char**, **varchar** и **varchar(max)** с параметрами сортировки с поддержкой [символов UTF-8](../../relational-databases/collations/collation-and-unicode-support.md#utf8); данные кодируются с помощью UTF-8;
    -   типы данных **nchar**, **nvarchar** и **nvarchar(max)** с параметрами сортировки с поддержкой [дополнительных символов](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters); данные кодируются с помощью UTF-16. Использование параметров сортировки, не поддерживающих дополнительные символы, приводит к кодированию данных с использованием UCS-2.      

    Это позволяет избежать проблемы преобразования кодовых страниц. См. подробнее о [различиях в хранении для символов UTF-8 и UTF-16](../../relational-databases/collations/collation-and-unicode-support.md#storage_differences).  

-   Вплоть до [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] все элементы с типами данных **char**, **varchar** и **text** замените элементами с типами данных **nchar**, **nvarchar** и **nvarchar(max)**. При использовании параметров сортировки с поддержкой [дополнительных символов](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters) данные кодируются с помощью UTF-16. Использование параметров сортировки, не поддерживающих дополнительные символы, приводит к кодированию данных с использованием UCS-2. Это позволяет избежать проблемы преобразования кодовых страниц. Дополнительные сведения см. в статье [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md). 

    > [!IMPORTANT]
    > Тип данных **text** является устаревшим, и его не следует использовать в новых разработках. Запланируйте преобразование данных типа **text** в **varchar(max)**.
  
-   При выполнении сравнения и других операций со значениями месяцев и дней недели следует использовать числовые эквиваленты вместо строковых имен. При различных языковых настройках возвращаются различные названия месяцев и дней недели. Например, `DATENAME(MONTH,GETDATE())` возвращает `May` при языковой настройке "Английский (США)", возвращает `Mai` для немецкого и `mai` — для французского. Вместо нее следует использовать функцию наподобие [DATEPART](../../t-sql/functions/datepart-transact-sql.md), в которой вместо названия месяца используется его номер. При выводе результирующих наборов пользователю лучше использовать названия из функции DATEPART(), так как они зачастую более информативны, чем числовое представление даты. Однако не следует кодировать какую-либо логику, зависящую от отображаемых названий на том или ином языке.  
  
-   При указании дат для сравнения либо ввода с помощью инструкций INSERT или UPDATE следует использовать константы, интерпретируемые одним и тем же образом для всех языковых настроек.  
  
    -   В приложениях ADO, OLE DB и ODBC следует использовать принятые в ODBC форматы отметок времени, даты и времени:  
  
         **{ ts'** _yyyy_ **-** _mm_ **-** _dd_ _hh_ **:** _mm_ **:** _ss_ [ **.** _fff_] **'}** , например: **{ ts'1998-09-24 10:02:20'}**  
  
         **{ d'** _yyyy_ **-** _mm_ **-** _dd_ **'}**, например: **{ d'1998-09-24'}**
  
         **{ t'** _hh_ **:** _mm_ **:** _ss_ **'}** , например: **{ t'10:02:20'}**  
  
    -   В приложениях с использованием других прикладных программных API-интерфейсов, а также в скриптах языка [!INCLUDE[tsql](../../includes/tsql-md.md)] , хранимых процедурах и триггерах следует использовать числовые строки без разделителей. Например, *yyyymmdd* в виде 19980924.  
  
    -   В приложениях с использованием других программных интерфейсов API, а также в скриптах языка [!INCLUDE[tsql](../../includes/tsql-md.md)], хранимых процедурах и триггерах следует использовать инструкцию [CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) с явно заданным параметром стиля для всех преобразований между типами данных **time**, **date**, **smalldate**, **datetime**, **datetime2** и **datetimeoffset**, а также строковыми типами данных. Например, следующая инструкция интерпретируется одинаково для любых настроек языка и формата даты в соединении:  
  
        ```sql  
        SELECT *  
        FROM AdventureWorks2012.Sales.SalesOrderHeader  
        WHERE OrderDate = CONVERT(DATETIME, '20060719', 101)  
        ```  
  
## <a name="see-also"></a>См. также
[Функции CAST и CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)     
[DATEPART (Transact-SQL)](../../t-sql/functions/datepart-transact-sql.md)        
[Поддержка параметров сортировки и Юникода](../../relational-databases/collations/collation-and-unicode-support.md)      

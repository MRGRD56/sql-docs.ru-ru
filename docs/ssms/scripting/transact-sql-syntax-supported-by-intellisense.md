---
title: Синтаксис языка Transact-SQL, поддерживаемый технологией IntelliSense
description: Узнайте, какие инструкции и элементы синтаксиса Transact-SQL поддерживаются технологией IntelliSense SQL Server Management Studio в SQL Server 2019 (15.x).
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- Transact-SQL IntelliSense
- IntelliSense [SQL Server], Transact-SQL syntax
ms.assetid: 194e8f4f-fd7e-4f32-a169-f23531128004
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/16/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 38c0ce9079dddb67e274fccb071aabbb778fb050
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352916"
---
# <a name="transact-sql-syntax-supported-by-intellisense"></a>Синтаксис языка Transact-SQL, поддерживаемый технологией IntelliSense
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  В этом разделе описываются инструкции и элементы синтаксиса [!INCLUDE[tsql](../../includes/tsql-md.md)] , которые поддерживаются технологией IntelliSense в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
## <a name="statements-supported-by-intellisense"></a>Инструкции, поддерживаемые технологией IntelliSense  
 В [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]технология IntelliSense поддерживается только для наиболее часто используемых инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] . Некоторые общие условия редактора запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] могут блокировать работу технологии IntelliSense. Дополнительные сведения см. в разделе [Устранение сбоев в работе IntelliSense (среда SQL Server Management Studio)](./troubleshooting-intellisense.md).  
  
> [!NOTE]  
>  Технология IntelliSense недоступна для зашифрованных объектов баз данных, например зашифрованных хранимых процедур или определяемых пользователем функций. Справка и краткие сведения по параметрам недоступны для параметров расширенных хранимых процедур и определяемых пользователем типов при интеграции со средой CLR.  
  
### <a name="select-statement"></a>Инструкция SELECT  
 Редактор запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] обеспечивает поддержку технологии IntelliSense для следующих синтаксических элементов в инструкции SELECT:  
  
:::row:::
    :::column:::
        SELECT
    :::column-end:::
    :::column:::
        WHERE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        FROM
    :::column-end:::
    :::column:::
        ORDER BY
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        HAVING
    :::column-end:::
    :::column:::
        UNION
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        FOR
    :::column-end:::
    :::column:::
        GROUP BY
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        В начало
    :::column-end:::
    :::column:::
        OPTION (указание)
    :::column-end:::
:::row-end:::

### <a name="additional-transact-sql-statements-that-are-supported"></a>Дополнительные поддерживаемые инструкции Transact-SQL  
 Редактор запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] также обеспечивает поддержку технологии IntelliSense для инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] , приведенных в таблице ниже.  
  
|Инструкция Transact-SQL|Поддерживаемый синтаксис|Исключения|  
|-----------------------------|----------------------|----------------|  
|[INSERT](../../t-sql/statements/insert-transact-sql.md)|Все синтаксические конструкции, кроме предложения *execute_statement* .|None|  
|[UPDATE](../../t-sql/queries/update-transact-sql.md)|Все синтаксические конструкции.|None|  
|[DELETE](../../t-sql/statements/delete-transact-sql.md)|Все синтаксические конструкции.|None|  
|[DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)|Все синтаксические конструкции.|None|  
|[SET @local_variable](../../t-sql/language-elements/set-local-variable-transact-sql.md)|Все синтаксические конструкции.|None|  
|[EXECUTE](../../t-sql/language-elements/execute-transact-sql.md)|Выполнение определяемых пользователем хранимых процедур, системных хранимых процедур, определяемых пользователем функций и системных функций.|None|  
|[CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)|Все синтаксические конструкции.|None|  
|[CREATE VIEW](../../t-sql/statements/create-view-transact-sql.md)|Все синтаксические конструкции.|None|  
|[CREATE PROCEDURE](../../t-sql/statements/create-procedure-transact-sql.md)|Все синтаксические конструкции.|Для предложения EXTERNAL NAME технология IntelliSense не поддерживается.<br /><br /> В предложении AS технология IntelliSense поддерживается только для тех инструкций и синтаксических конструкций, которые перечислены в этом разделе.|  
|[ALTER PROCEDURE](../../t-sql/statements/alter-procedure-transact-sql.md)|Все синтаксические конструкции|Для предложения EXTERNAL NAME технология IntelliSense не поддерживается.<br /><br /> В предложении AS технология IntelliSense поддерживается только для тех инструкций и синтаксических конструкций, которые перечислены в этом разделе.|  
|[USE](../../t-sql/language-elements/use-transact-sql.md)|Все синтаксические конструкции.|None|  
  
## <a name="intellisense-in-supported-statements"></a>Технология IntelliSense в поддерживаемых инструкциях  
 В редакторе запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] технология IntelliSense поддерживается для следующих синтаксических элементов при их использовании в одной из поддерживаемых инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] :  
  
-   Все типы соединений, включая APPLY  
  
-   PIVOT и UNPIVOT  
  
-   Ссылки на следующие объекты базы данных.  
  
    -   Базы данных и схемы  
  
    -   Таблицы, представления, функции с табличным значением и табличные выражения  
  
    -   Столбцы  
  
    -   Процедуры и параметры процедур  
  
    -   Скалярные функции и скалярные выражения  
  
    -   Локальные переменные  
  
    -   Обобщенные табличные выражения  
  
-   Объекты базы данных, ссылки на которые имеются только в инструкциях CREATE или ALTER в скрипте или пакете, но которые не существуют в базе данных, поскольку скрипт или пакет еще не выполнялся. Ниже приведены эти объекты.  
  
    -   Таблицы и процедуры, указанные в инструкции CREATE TABLE или CREATE PROCEDURE в скрипте или пакете.  
  
    -   Изменения в таблицах и процедурах, указанных в инструкции ALTER TABLE или ALTER PROCEDURE в скрипте или пакете.  
  
    > [!NOTE]  
    >  Технология IntelliSense недоступна для столбцов инструкции CREATE VIEW, пока инструкция CREATE VIEW не будет исполнена.  
  
 Поддержка технологии IntelliSense для приведенных ранее элементов при их использовании в других инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)] не предоставляется. Например, поддержка технологии IntelliSense предоставляется для имен столбцов, которые используются в инструкции SELECT, но не для столбцов, используемых в инструкции CREATE FUNCTION.  
  
## <a name="examples"></a>Примеры  
 Внутри скрипта или пакета [!INCLUDE[tsql](../../includes/tsql-md.md)] технология IntelliSense в редакторе запросов компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] поддерживается только для тех инструкций и синтаксических конструкций, которые перечислены в этом разделе. В следующих примерах кода [!INCLUDE[tsql](../../includes/tsql-md.md)] показано, для каких инструкций и элементов синтаксиса поддерживается технология IntelliSense. Например, в приведенном ниже пакете технология IntelliSense доступна для инструкции `SELECT`, когда она используется в коде самостоятельно, а не `SELECT` содержится в инструкции `CREATE FUNCTION`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE Name LIKE N'Road-250%' and Color = N'Red';  
GO  
CREATE FUNCTION Production.ufn_Red250 ()  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT Name  
    FROM AdventureWorks2012.Production.Product  
    WHERE Name LIKE N'Road-250%'  
      AND Color = N'Red'  
);GO  
```  
  
 Эта функциональность также применяется к наборам инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] в предложении AS инструкций CREATE PROCEDURE или ALTER PROCEDURE.  
  
 Внутри скрипта или пакета [!INCLUDE[tsql](../../includes/tsql-md.md)] поддержка технологии IntelliSense обеспечивается для объектов, указанных в инструкции CREATE или ALTER. Однако эти объекты не существуют в базе данных, так как инструкции еще не выполнялись. Например, в редакторе запросов можно ввести следующий код:  
  
```  
USE MyTestDB;  
GO  
CREATE TABLE MyTable  
    (PrimaryKeyCol   INT PRIMARY KEY,  
    FirstNameCol      NVARCHAR(50),  
   LastNameCol       NVARCHAR(50));  
GO  
SELECT   
```  
  
 После ввода инструкции `SELECT`технология IntelliSense выдаст список, содержащий в качестве возможных вариантов выбора **PrimaryKeyCol**, **FirstNameCol** и **LastNameCol** даже в том случае, если скрипт еще не выполнялся и таблица `MyTable` пока не существует в базе данных `MyTestDB`.  
  

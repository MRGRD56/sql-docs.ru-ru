---
description: Копирование столбцов из одной таблицы в другую (компонент Database Engine)
title: Копирование столбцов из одной таблицы в другую (ядро СУБД) | Документация Майкрософт
ms.custom: ''
ms.date: 09/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- copying columns
- columns [SQL Server], copying
ms.assetid: 5f5e70dc-69f9-44b8-bc48-b5d51ac20d77
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ba2b4f4e106f1edb9d9f3fe57a0f42cdf20fec27
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748634"
---
# <a name="copy-columns-from-one-table-to-another-database-engine"></a>Копирование столбцов из одной таблицы в другую (компонент Database Engine)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  В этом разделе описывается копирование столбцов из одной таблицы в другую, когда копируется либо только определение столбца, либо определение и данные из [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Ограничения](#Restrictions)  
  
     [Безопасность](#Security)  
  
-   **Копирование столбцов с помощью:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
 При копировании из одной базы данных в другую столбца, имеющего псевдоним типа данных, исходный тип данных в целевой базе данных может оказаться недоступным. В этом случае столбцу будет назначен ближайший подходящий базовый тип данных, доступный в целевой базе данных.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Требуется разрешение ALTER на таблицу.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-copy-column-definitions-from-one-table-to-another"></a>Копирование определения столбца из одной таблицы в другую  
  
1.  Щелкнув правой кнопкой мыши, откройте таблицу со столбцами, предназначенными к копированию, и таблицу, в которую необходимо выполнить копирование, затем нажмите кнопку **Разработка**.  
  
2.  Выберите вкладку исходной таблицы и выделите нужные столбцы.  
  
3.  В меню **Правка** выберите **Копировать**.  
  
4.  Выберите вкладку таблицы, в которую требуется скопировать столбцы.  
  
5.  Выделите столбец, после которого нужно поместить вставляемые столбцы, и в меню **Правка** выберите **Вставить**.  

#### <a name="to-copy-data-from-one-table-to-another"></a>Копирование данных из одной таблицы в другую  
  
1.  Следуйте приведенным выше инструкциям для копирования определения столбцов.  
  
    > [!NOTE]  
    >  Прежде чем скопировать данные из одной таблицы в другую, убедитесь, что типы данных целевых столбцов совместимы с типами данных исходных.  
  
2.  Откройте новое окно редактора запросов. 

3.  Щелкните редактор запросов правой кнопкой мыши и выберите **Создать запрос в редакторе**. 

4.  В диалоговом окне **Добавление таблицы** выберите исходную и целевую таблицы, нажмите кнопку **Добавить**, а затем закройте диалоговое окно **Добавление таблицы** . 

5.  Щелкните правой кнопкой мыши пустую область редактора запросов, наведите указатель на пункт **Изменить тип** и выберите **Вставить результаты**.  

6.  В диалоговом окне **Выбор целевой таблицы для инструкции Insert Results** выберите целевую таблицу. 

7.  В верхней части конструктора запросов щелкните исходный столбец исходной таблицы.

8. Конструктор запросов создал запрос INSERT. Нажмите кнопку "ОК", чтобы поместить запрос в исходное окно редактора запросов.  

9.  Выполните запрос, чтобы вставить данные из исходной таблицы в целевую.

  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-copy-column-definitions-from-one-table-to-another"></a>Копирование определения столбца из одной таблицы в другую  
  
1.  Нельзя копировать отдельные столбцы из одной таблицы в другую (существующую) с использованием инструкций Transact-SQL. Однако можно создать новую таблицу в файловой группе по умолчанию и вставить в нее результирующие строки из запроса с помощью инструкции SELECT INTO. Дополнительные сведения см. в разделе [Предложение INTO (Transact-SQL)](../../t-sql/queries/select-into-clause-transact-sql.md).  
  
#### <a name="to-copy-data-from-one-table-to-another"></a>Копирование данных из одной таблицы в другую  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE TABLE dbo.EmployeeSales  
    ( BusinessEntityID   varchar(11) NOT NULL,  
      SalesYTD money NOT NULL  
    );  
    GO  
    INSERT INTO dbo.EmployeeSales  
        SELECT BusinessEntityID, SalesYTD   
        FROM Sales.SalesPerson;  
    GO  
    ```  
  
  

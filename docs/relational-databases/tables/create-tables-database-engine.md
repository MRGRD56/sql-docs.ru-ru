---
description: Создание таблиц (компонент Database Engine)
title: Создание таблиц (ядро СУБД) | Документация Майкрософт
ms.custom: ''
ms.date: 09/22/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- table creation [SQL Server], Visual Database Tools
ms.assetid: 6f7c6ac5-e6d3-4dca-831e-b28442ba535b
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4a9f1a24a634690951aac6053884cd25d41e63c0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748584"
---
# <a name="create-tables-database-engine"></a>Создание таблиц (компонент Database Engine)
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Предусмотрена возможность создавать таблицы, присваивать им имена и добавлять к существующим базам данных, используя [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  

  
##  <a name="check-your-permissions-first"></a><a name="Permissions"></a> Сначала проверьте разрешения!  
Эта задача требует разрешения CREATE TABLE в базе данных и разрешения ALTER на схему, в которой создается таблица.  
  
 Если какие-либо столбцы в инструкции CREATE TABLE определены как принадлежащие к определяемому пользователем типу данных CLR, необходимо быть владельцем данного типа либо иметь разрешение REFERENCES на него.  
  
 Если какие-либо столбцы в инструкции CREATE TABLE имеют связанную коллекцию схем XML, необходимо быть владельцем этого набора схем или иметь разрешение REFERENCES на него.  
  
 
## <a name="using-table-designer"></a>С помощью конструктора таблиц  
  
1.  В SSMS в **обозревателе объектов** подключитесь к экземпляру [!INCLUDE[ssDE](../../includes/ssde-md.md)] , который содержит изменяемую базу данных.  
  
2.  В **обозревателе объектов** разверните узел **Базы данных** , а затем базу данных, в которой будет размещена новая таблица.  
  
3.  В обозревателе объектов щелкните правой кнопкой мыши узел **Таблицы** базы данных и выберите **Создать таблицу**.  
  
4.  Введите имена столбцов, выберите типы данных и определите для каждого столбца, могут ли в нем присутствовать значения NULL, как показано на следующей иллюстрации:  
  
     ![Снимок экрана, показывающий параметр "Разрешить значения NULL", установленный для столбца ModifiedDate.](../../relational-databases/tables/media/addcolumnsintabledesigner.gif "AddColumnsinTableDesigner")  
  
5.  Вы также можете задать другие свойства столбца, например является ли этот столбец столбцом идентификаторов или вычисляемым столбцом. Для этого щелкните столбец на вкладке свойств столбцов. Дополнительные сведения о свойствах столбцов см. в разделе [Свойства столбца таблицы (среда SQL Server Management Studio)](../../relational-databases/tables/table-column-properties-sql-server-management-studio.md).  
  
6.  Чтобы указать, что столбец является столбцом первичного ключа, щелкните его правой кнопкой мыши и выберите **Задать первичный ключ**. Дополнительные сведения см. в статье [Create Primary Keys](../../relational-databases/tables/create-primary-keys.md).  
  
7.  Чтобы создать связи по внешнему ключу, проверочные ограничения или индексы, щелкните правой кнопкой мыши панель конструктора таблиц и выберите в списке объект, как показано на следующей иллюстрации:  
  
     ![Снимок экрана, показывающий параметр "Связи".](../../relational-databases/tables/media/addtableobjects.gif "AddTableObjects")  
  
     Дополнительные сведения об этих объектах см. в разделах [Create Foreign Key Relationships](../../relational-databases/tables/create-foreign-key-relationships.md), [Create Check Constraints](../../relational-databases/tables/create-check-constraints.md) и [Indexes](../../relational-databases/indexes/indexes.md).  
  
8.  По умолчанию таблица содержится в схеме **dbo** . Чтобы указать другую схему для таблицы, щелкните правой кнопкой мыши панель конструктора таблиц и выберите **Свойства** , как показано на следующей иллюстрации. Выберите нужную схему из раскрывающегося списка **Схема** .  
  
     ![Снимок экрана: панель "Свойства" с параметром параметр "Схема".](../../relational-databases/tables/media/specifyatableschema.gif "Specifyatableschema")  
  
     Дополнительные сведения о схемах см. в разделе [Create a Database Schema](../../relational-databases/security/authentication-access/create-a-database-schema.md).  
  
9. В меню **Файл** выберите команду **Сохранить** *имя_таблицы*.  
  
10. В диалоговом окне **Выбор имени** введите имя таблицы и нажмите кнопку **OK**.  
  
11. Чтобы просмотреть новую таблицу, в **обозревателе объектов** разверните узел **Таблицы** , а затем нажмите клавишу **F5** , чтобы обновить список объектов. Новая таблица будет отображена в списке таблиц.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
## <a name="using-query-editor"></a>С помощью редактора запросов  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    CREATE TABLE dbo.PurchaseOrderDetail  
    (  
        PurchaseOrderID int NOT NULL  
        ,LineNumber smallint NOT NULL  
        ,ProductID int NULL  
        ,UnitPrice money NULL  
        ,OrderQty smallint NULL  
        ,ReceivedQty float NULL  
        ,RejectedQty float NULL  
        ,DueDate datetime NULL  
    );  
    ```  
  
 Дополнительные сведения см. в разделе [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md).  
  
  

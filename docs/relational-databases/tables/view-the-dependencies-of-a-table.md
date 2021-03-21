---
description: Просмотр зависимостей таблицы
title: Просмотр зависимостей таблицы | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- table dependencies [SQL Server]
- dependencies [SQL Server], tables
- displaying dependences
- viewing dependencies
ms.assetid: c4351ef5-e7d0-46e7-8367-88695e9974f8
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4c98660deb9b7289a931decf8a0846ff18deb98d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748924"
---
# <a name="view-the-dependencies-of-a-table"></a>Просмотр зависимостей таблицы
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Зависимости таблицы в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] можно просмотреть в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или с помощью [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Безопасность](#Security)  
  
-   **Просмотр зависимостей таблицы с помощью следующих средств:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение VIEW DEFINITION в базе данных и разрешение SELECT на представление sys.sql_expression_dependencies в базе данных. По умолчанию разрешение SELECT предоставляется только членам предопределенной роли базы данных db_owner. Если разрешения SELECT и VIEW DEFINITION предоставлены другому пользователю, он может просматривать все зависимости в базе данных.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-view-the-objects-on-which-a-table-depends"></a>Просмотр объектов, от которых зависит таблица  
  
1.  В **Обозревателе объектов** разверните узел **Базы данных**, разверните саму базу данных, а затем разверните узел **Таблицы**.  
  
2.  Щелкните таблицу правой кнопкой мыши и выберите **Просмотр зависимостей**.  
  
3.  В диалоговом окне **Зависимости объектов** _\<object name\>_ выберите либо вариант **Объекты, которые зависят от** _\<object name\>_ , либо вариант **Объекты, от которых зависит** _\<object name\>_ .  
  
4.  Выберите объект в сетке **Зависимости** . Тип объекта (например, "Триггер" или "Хранимая процедура") появится в поле **Тип** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-view-the-objects-that-depend-on-a-table"></a>Просмотр объектов, зависящих от таблицы  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT * FROM sys.sql_expression_dependencies  
    WHERE referencing_id = OBJECT_ID(N'Production.vProductAndDescription');   
    GO  
  
    ```  
  
#### <a name="to-view-the-dependencies-of-a-table"></a>Просмотр зависимостей таблицы  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Следующий пример возвращает объекты, которые зависят от таблицы `Production.Product`. Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    SELECT * FROM sys.sql_expression_dependencies  
    WHERE referenced_id = OBJECT_ID(N'Production.Product');   
    GO  
  
    ```  
  
 Дополнительные сведения см. в разделе [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
  
  

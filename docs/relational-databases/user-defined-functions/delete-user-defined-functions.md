---
description: Удаление определенных пользователем функций
title: Удаление определяемых пользователем функций | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: db1d668a-23b7-4757-a9c5-1bd848ba7f6d
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9b532481a55f23116d0fd90760d1a940f4cd82f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184295"
---
# <a name="delete-user-defined-functions"></a>Удаление определенных пользователем функций
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Определяемые пользователем функции в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] можно удалить с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Ограничения](#Restrictions)  
  
     [Безопасность](#Security)  
  
-   **Для удаления определяемой пользователем функции используются**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Удалить функцию не удастся, если в базе данных имеются функции Transact-SQL или представления, которые ссылаются на эту функцию и были созданы с помощью SCHEMABINDING, или при наличии вычисляемых столбцов, ограничений CHECK либо DEFAULT, которые ссылаются на эту функцию.  
  
-   Удалить функцию не удастся, если имеются вычисляемые столбцы, которые ссылаются на эту функцию и были индексированы.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение ALTER на схему, которой принадлежит функция, или разрешение CONTROL на функцию.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-delete-a-user-defined-function"></a>Удаление определяемой пользователем функции  
  
1.  Щелкните значок плюса рядом с базой данных, содержащей функцию, которую надо изменить.  
  
2.  Щелкните значок плюса рядом с папкой **Программирование** .  
  
3.  Щелкните значок плюса рядом с папкой, содержащей функцию, которую надо изменить.  
  
    -   Table-valued Function  
  
    -   Скалярная функция  
  
    -   Агрегатная функция  
  
4.  Щелкните правой кнопкой мыши функцию, которую нужно удалить, и выберите пункт **Удалить**.  
  
5.  В диалоговом окне **Удаление объекта** нажмите кнопку **ОК**.  

    > [!IMPORTANT]  
    >  Щелкните **Показать зависимости** в диалоговом окне **Удаление объекта**, чтобы открыть диалоговое окно _Зависимости_**имя\_функции**. При этом будут отображены все объекты, зависящие от функции, и все объекты, от которых зависит функция.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-delete-a-user-defined-function"></a>Удаление определяемой пользователем функции  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    -- creates function called "Sales.ufn_SalesByStore"  
    USE AdventureWorks2012;  
    GO  
    CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)  
    RETURNS TABLE  
    AS  
    RETURN   
    (  
        SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'  
        FROM Production.Product AS P   
        JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID  
        JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID  
        JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID  
        WHERE C.StoreID = @storeid  
        GROUP BY P.ProductID, P.Name  
    );  
    GO  
    ```  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- determines if function exists in database  
    IF OBJECT_ID (N'Sales.fn_SalesByStore', N'IF') IS NOT NULL  
    -- deletes function  
        DROP FUNCTION Sales.fn_SalesByStore;  
    GO  
    ```  
  
 Дополнительные сведения см. в статье [DROP FUNCTION (Transact-SQL)](../../t-sql/statements/drop-function-transact-sql.md).  
  
  

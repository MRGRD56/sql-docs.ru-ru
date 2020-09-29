---
description: Установка и изменение параметров сортировки базы данных
title: Установка и изменение параметров сортировки базы данных | Документация Майкрософт
ms.custom: ''
ms.date: 10/11/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- collations [SQL Server], database
- database collations [SQL Server]
ms.assetid: 1379605c-1242-4ac8-ab1b-e2a2b5b1f895
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e2103871e039484a12234a7e26a2783125e9ffae
ms.sourcegitcommit: 63aef5a96905f0b026322abc9ccb862ee497eebe
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2020
ms.locfileid: "91364231"
---
# <a name="set-or-change-the-database-collation"></a>Установка и изменение параметров сортировки базы данных
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описано, как задать и изменить параметры сортировки базы данных с помощью [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)]. Если параметры сортировки не указаны, используются параметры сортировки сервера.  
  
> [!IMPORTANT]
> Инструкция `ALTER DATABASE COLLATE` в Базе данных SQL Azure не поддерживается.

 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Ограничения](#Restrictions)  
  
     [Рекомендации](#Recommendations)  
  
     [Безопасность](#Security)  
  
-   **Задание и изменение параметров сортировки базы данных с помощью:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Параметры сортировки Windows только для Юникода могут использоваться лишь с предложением COLLATE для применения параметров сортировки к данным типов **nchar**, **nvarchar**и **ntext** на уровне столбца и на уровне выражения. Их нельзя использовать с предложением COLLATE для изменения параметров сортировки базы данных или экземпляра сервера.  
  
-   Если указанные или используемые объектом по ссылке параметры сортировки используют кодовую страницу, не поддерживаемую Windows, то компонент [!INCLUDE[ssDE](../../includes/ssde-md.md)] выдаст ошибку.  

-   После создания базы данных в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] параметры сортировки невозможно изменить, используя [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Их можно изменить только с помощью [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
  
Имена поддерживаемых параметров сортировки вы можете найти в статьях [Имя параметров сортировки Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md) и [Имя параметров сортировки SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)или воспользоваться системной функцией [sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md) .  
  
Если изменяются параметры сортировки базы данных, то изменяется следующее:  
  
-   Все столбцы типа **char**, **varchar**, **text**, **nchar**, **nvarchar**или **ntext** в системных таблицах заменяются новым параметром сортировки.  
  
-   Все существующие параметры типа **char**, **varchar**, **text**, **nchar**, **nvarchar**или **ntext** и возвращаемые скалярные значения для хранимых процедур и определяемых пользователем функций заменяются новым параметром сортировки.  
  
-   Системные типы данных **char**, **varchar**, **text**, **nchar**, **nvarchar**или **ntext** и все определяемые пользователем типы данных, основанные на этих системных типах данных, заменяются новым параметром сортировки по умолчанию.  
  
Вы можете изменить параметры сортировки любых новых объектов, созданных в пользовательской базе данных, с помощью предложения `COLLATE` инструкции [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md). Эта инструкция **не изменяет** параметры сортировки столбцов в любых существующих пользовательских таблицах. Они могут быть изменены с помощью предложения `COLLATE` инструкции [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Чтобы создать базу данных, требуется разрешение `CREATE DATABASE` в базе данных **master**, разрешение `CREATE ANY DATABASE` либо `ALTER ANY DATABASE`.  
  
 Чтобы изменить параметры сортировки имеющейся базы данных, требуется разрешение `ALTER` в базе данных.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-set-or-change-the-database-collation"></a>Задание и изменение параметров сортировки базы данных  
  
1.  В **обозревателе объектов**подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], разверните его, а затем разверните узел **Базы данных**.  
  
2.  При создании новой базы данных щелкните правой кнопкой мыши **Базы данных** и выберите пункт **Создать базу данных**. Если использовать параметры сортировки по умолчанию не нужно, то перейдите на страницу **Параметры** и выберите нужный вариант в раскрывающемся списке **Параметры сортировки** .  
  
     Если база данных уже существует, щелкните правой кнопкой мыши нужную базу данных и выберите пункт **Свойства**. Перейдите на страницу **Параметры** , а затем выберите нужный вариант в раскрывающемся списке **Параметры сортировки** .  
  
3.  По завершении нажмите кнопку **ОК**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-set-the-database-collation"></a>Задание параметров сортировки базы данных  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере показано задание параметров сортировки с помощью предложения [COLLATE](~/t-sql/statements/collations.md) . В примере создается база данных `MyOptionsTest` , в которой используются параметры сортировки `Latin1_General_100_CS_AS_SC` . Чтобы проверить параметр, после создания базы данных выполните инструкцию `SELECT` .  
  
```sql  
USE master;  
GO  
IF DB_ID (N'MyOptionsTest') IS NOT NULL  
DROP DATABASE MyOptionsTest;  
GO  
CREATE DATABASE MyOptionsTest  
COLLATE Latin1_General_100_CS_AS_SC;  
GO  
  
--Verify the collation setting.  
SELECT name, collation_name  
FROM sys.databases  
WHERE name = N'MyOptionsTest';  
GO  
```  
  
#### <a name="to-change-the-database-collation"></a>Изменение параметров сортировки базы данных  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере показано изменение имени параметров сортировки с помощью предложения [COLLATE](~/t-sql/statements/collations.md) в инструкции [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) . Выполните инструкцию `SELECT` , чтобы проверить изменение.  
  
```sql  
USE master;  
GO  
ALTER DATABASE MyOptionsTest  
COLLATE French_CI_AS ;  
GO  
  
--Verify the collation setting.  
SELECT name, collation_name  
FROM sys.databases  
WHERE name = N'MyOptionsTest';  
GO  
```  
  
## <a name="see-also"></a>См. также:  
 [Поддержка параметров сортировки и Юникода](../../relational-databases/collations/collation-and-unicode-support.md)   
 [sys.fn_helpcollations (Transact-SQL)](../../relational-databases/system-functions/sys-fn-helpcollations-transact-sql.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [Имя параметров сортировки SQL Server (Transact-SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)   
 [Имя параметров сортировки Windows (Transact-SQL)](../../t-sql/statements/windows-collation-name-transact-sql.md)   
 [COLLATE (Transact-SQL)](~/t-sql/statements/collations.md)   
 [Очередность параметров сортировки (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)  
  
  

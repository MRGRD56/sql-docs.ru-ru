---
description: Хранимая процедура sp_control_plan_guide (Transact-SQL)
title: sp_control_plan_guide (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_control_plan_guide
- sp_control_plan_guide_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_control_plan_guide
ms.assetid: c96d43d5-6507-4d66-b3f5-f44c0617cb5c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0b4f5d1adbdadf9d8291257708a3098fb50ce175
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185788"
---
# <a name="sp_control_plan_guide-transact-sql"></a>Хранимая процедура sp_control_plan_guide (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Удаляет, включает или отключает структуру плана.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_control_plan_guide [ @operation = ] N'<control_option>'  
  [ , [ @name = ] N'plan_guide_name' ]  
  
<control_option>::=  
{   
    DROP   
  | DROP ALL  
  | DISABLE  
  | DISABLE ALL  
  | ENABLE   
  | ENABLE ALL  
}  
```  
  
## <a name="arguments"></a>Аргументы  
 **N "** _plan_guide_name_ **"**  
 Задает структуру плана, предназначенную к удалению, включению или отключению. *plan_guide_name* разрешается в текущую базу данных. Если не указано, *plan_guide_name* по умолчанию принимает значение null.  
  
 DROP  
 Удаляет структуру плана, указанную *plan_guide_name*. После удаления структуры плана будущее выполнение запроса, ранее соответствовавшего этой структуре плана, не затрагивается.  
  
 DROP ALL  
 Удаляет все структуры планов из текущей базы данных. **N '**_plan_guide_name_ не может быть указано, если указано Drop ALL.  
  
 DISABLE  
 Отключает структуру плана, заданную *plan_guide_name*. После отключения структуры плана будущее выполнение запроса, ранее соответствовавшего этой структуре плана, не затрагивается.  
  
 DISABLE ALL  
 Отключает все структуры планов в текущей базе данных. **N '**_plan_guide_name_ не может быть указано, если указан ключ отключения всех.  
  
 ENABLE  
 Включает структуру плана, заданную *plan_guide_name*. После включения структуры плана с ней может быть сопоставлен совпадающий запрос. По умолчанию структура плана включается во время создания.  
  
 ENABLE ALL  
 Включает все структуры планов в текущей базе данных. Невозможно указать **N "**_plan_guide_name_**"**, если задан параметр "включить все".  
  
## <a name="remarks"></a>Замечания  
 Попытка удаления или изменения функции, хранимой процедуры или триггера DML, на которые имеется ссылка в структуре плана (как включенных, так и отключенных), приводит к ошибке.  
  
 Отключение уже отключенной структуры плана или включение включенной не имеет силы и не вызывает ошибки.  
  
 Руководства планов доступны не во всех выпусках [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Сведения о функциях, поддерживаемых различными выпусками [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], см. в статье [Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md). Однако можно выполнить **sp_control_plan_guide** с параметром DROP или Drop ALL в любом выпуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="permissions"></a>Разрешения  
 Для выполнения **sp_control_plan_guide** в структуре плана типа Object (созданная с указанием **@type "** Object **"** ) требуется разрешение ALTER на объект, на который ссылается структура плана. Все остальные структуры планов требуют разрешения ALTER DATABASE.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-enabling-disabling-and-dropping-a-plan-guide"></a>A. Включение, отключение и удаление структур планов  
 В следующем примере структура плана создается, отключается, включается и удаляется.  
  
```  
--Create a procedure on which to define the plan guide.  
IF OBJECT_ID(N'Sales.GetSalesOrderByCountry', N'P') IS NOT NULL  
    DROP PROCEDURE Sales.GetSalesOrderByCountry;  
GO  
CREATE PROCEDURE Sales.GetSalesOrderByCountry   
    (@Country nvarchar(60))  
AS  
BEGIN  
    SELECT *  
    FROM Sales.SalesOrderHeader AS h   
    INNER JOIN Sales.Customer AS c ON h.CustomerID = c.CustomerID  
    INNER JOIN Sales.SalesTerritory AS t ON c.TerritoryID = t.TerritoryID  
    WHERE t.CountryRegionCode = @Country;  
END  
GO  
--Create the plan guide.  
EXEC sp_create_plan_guide N'Guide3',  
    N'SELECT *  
    FROM Sales.SalesOrderHeader AS h   
    INNER JOIN Sales.Customer AS c ON h.CustomerID = c.CustomerID  
    INNER JOIN Sales.SalesTerritory AS t ON c.TerritoryID = t.TerritoryID  
    WHERE t.CountryRegionCode = @Country',  
    N'OBJECT',  
    N'Sales.GetSalesOrderByCountry',  
    NULL,  
    N'OPTION (OPTIMIZE FOR (@Country = N''US''))';  
GO  
--Disable the plan guide.  
EXEC sp_control_plan_guide N'DISABLE', N'Guide3';  
GO  
--Enable the plan guide.  
EXEC sp_control_plan_guide N'ENABLE', N'Guide3';  
GO  
--Drop the plan guide.  
EXEC sp_control_plan_guide N'DROP', N'Guide3';  
```  
  
### <a name="b-disabling-all-plan-guides-in-the-current-database"></a>Б. Отключение всех структур планов в текущей базе данных  
 В следующем примере отключаются все структуры планов в базе данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_control_plan_guide N'DISABLE ALL';  
```  
  
## <a name="see-also"></a>См. также:  
 [Ядро СУБД хранимых процедур &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sp_create_plan_guide (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)   
 [sys.plan_guides (Transact-SQL)](../../relational-databases/system-catalog-views/sys-plan-guides-transact-sql.md)   
 [Руководства планов](../../relational-databases/performance/plan-guides.md)  
  

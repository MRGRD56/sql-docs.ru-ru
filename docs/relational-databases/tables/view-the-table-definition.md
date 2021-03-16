---
description: Просмотр определения таблицы
title: Просмотр определения таблицы
ms.custom: ''
ms.date: 03/09/2021
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- showing table properties
- displaying table properties
- tables [SQL Server], properties
- viewing table properties
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 49a0a836cb2e61bef9b4edb280685392f62544ec
ms.sourcegitcommit: 98acedd435aecfda1b3c4c23d3f0c3c1a12682a4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/10/2021
ms.locfileid: "102532340"
---
# <a name="view-the-table-definition"></a>Просмотр определения таблицы
[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  Отобразить свойства таблицы можно в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] при помощи [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Безопасность](#Security)  
  
-   **Отображение свойств таблицы с использованием:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Просматривать свойства таблицы может только владелец таблицы или лицо, получившее разрешения на таблицу.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-show-table-properties-in-the-properties-window"></a>Отображение свойств таблицы в окне «Свойства»  
  
1.  В обозревателе объектов выберите таблицу, для которой необходимо просмотреть свойства.  
  
2.  Щелкните таблицу правой кнопкой мыши и в контекстном меню выберите пункт **Свойства** . Дополнительные сведения см. в разделе [Свойства таблицы — SSMS](../../relational-databases/tables/table-properties-ssms.md).  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-show-table-properties"></a>Отображение свойств таблицы  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В примере выполняется системная хранимая процедура sp_help, чтобы получить все сведения о столбце для указанного объекта.  
  
```sql  
EXEC sp_help 'dbo.mytable';
```  
    
 Дополнительные сведения см. в статье [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md).

 Как вариант, можно отправлять запросы к представлениям системного каталога напрямую для получения метаданных таких объектов, как таблицы, схемы и столбцы. Пример:  
  
```sql
SELECT s.name, t.name, c.* FROM sys.columns AS c
INNER JOIN sys.tables AS t ON t.object_id = c.object_id
INNER JOIN sys.schemas AS s ON s.schema_id = t.schema_id
WHERE t.object_id = object_id('mytable') AND s.name = 'dbo';
```
    
 Дополнительные сведения см. в разделе: 

* [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)    
* [sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)    
* [sys.schemas (Transact-SQL)](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)     


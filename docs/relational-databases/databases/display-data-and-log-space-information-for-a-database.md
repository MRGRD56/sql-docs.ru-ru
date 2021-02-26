---
title: Отображение сведений о месте на диске, занимаемом данными и журналами базы данных
description: Узнайте, как на SQL Server отображать информацию об объеме данных и журнала в резервном наборе данных с помощью SQL Server Management Studio или Transact-SQL.
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], space
- status information [SQL Server], space
- displaying space information
- disk space [SQL Server], displaying
- databases [SQL Server], space used
- viewing space information
- space allocation [SQL Server], displaying
- data space [SQL Server]
ms.assetid: c7b99463-4bab-4e9b-9217-fcb0898dc757
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: 648e33d685e7c97a7e900fd752402ed135fb9ecb
ms.sourcegitcommit: c821c2bdc383a84e45bbdc95ff6fbabf4f54901c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/17/2021
ms.locfileid: "100560944"
---
# <a name="display-data-and-log-space-information-for-a-database"></a>Отображение данных и сведений о пространстве журнала для базы данных
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  В этом разделе описано, как отобразить данные и сведения о пространстве журнала для базы данных в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] при помощи среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Разрешение на выполнение процедуры **sp_spaceused** предоставлено роли **public** . Параметр **\@updateusage** могут указывать только члены предопределенной роли базы данных **db_owner**.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-display-data-and-log-space-information-for-a-database"></a>Отображение данных и сведений о пространстве для базы данных  
  
1. В обозревателе объектов подключитесь к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и разверните его.  
  
2. Разверните узел **Базы данных**.  
  
3. Щелкните правой кнопкой мыши базу данных, укажите **Отчеты**, **Стандартные отчеты**, а затем щелкните **Использование диска**.  

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL

#### <a name="to-display-data-and-log-space-information-for-a-database-by-using-sp_spaceused"></a>Отображение данных и сведений о пространстве журнала для базы данных при помощи процедуры sp_spaceused
  
1. Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2. На панели «Стандартная» нажмите **Создать запрос**.  
  
3. Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере используется системная хранимая процедура [sp_spaceused](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md), которая передает сведения о заполнении места на диске для всей базы данных — ее таблиц и индексов.  
  
```sql  
USE AdventureWorks2012;  
GO  
EXEC sp_spaceused;  
GO  
```  

#### <a name="to-display-data-space-used-by-object-and-allocation-unit-for-a-database"></a>Отображение пространства данных, используемого объектом и единицей распределения для базы данных
  
1. Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2. На панели «Стандартная» нажмите **Создать запрос**.  
  
3. Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере запрашиваются [представления каталога объектов](../system-catalog-views/object-catalog-views-transact-sql.md) для сообщения об использовании дискового пространства на таблицу и в каждой таблице на [единицу распределения](../pages-and-extents-architecture-guide.md#IAM).  
  
```sql  
SELECT
  t.object_id,
  OBJECT_NAME(t.object_id) ObjectName,
  sum(u.total_pages) * 8 Total_Reserved_kb,
  sum(u.used_pages) * 8 Used_Space_kb,
  u.type_desc,
  max(p.rows) RowsCount
FROM
  sys.allocation_units u
  join sys.partitions p on u.container_id = p.hobt_id
  join sys.tables t on p.object_id = t.object_id
GROUP BY
  t.object_id,
  OBJECT_NAME(t.object_id),
  u.type_desc
ORDER BY
  Used_Space_kb desc,
  ObjectName
```  

#### <a name="to-display-data-and-log-space-information-for-a-database-by-querying-sysdatabase_files"></a>Отображение данных и сведений о пространстве журнала для базы данных путем запроса к представлению каталога sys.database_files  
  
1. Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2. На панели «Стандартная» нажмите **Создать запрос**.  
  
3. Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере выполняется запрос к представлению каталога [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) , который возвращает определенные сведения о файлах данных и журнала из базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT file_id, name, type_desc, physical_name, size, max_size  
FROM sys.database_files ;  
GO  
  
```  
  
## <a name="see-also"></a>См. также:

 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [sp_spaceused (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md)   
 [Добавление файлов данных или журналов в базу данных](../../relational-databases/databases/add-data-or-log-files-to-a-database.md)   
 [Удаление файлов данных или журналов из базы данных](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)  

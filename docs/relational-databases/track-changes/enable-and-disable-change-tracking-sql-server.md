---
description: Включение и отключение отслеживания изменений (SQL Server)
title: Включение и отключение отслеживания изменений
ms.custom: seo-dt-2019
ms.date: 08/08/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- change tracking [SQL Server], disabling
- data changes [SQL Server]
- change tracking [SQL Server], enabling
- tracking data changes [SQL Server]
- change tracking [SQL Server], configuring
- data [SQL Server], changing
ms.assetid: 1c92ec7e-ae53-4498-8bfd-c66a42a24d54
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7dda1f0aaf7d5ca1470871752a78bdd04a99fb9f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472585"
---
# <a name="enable-and-disable-change-tracking-sql-server"></a>Включение и отключение отслеживания изменений (SQL Server)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  В этом разделе описано, как включить и отключить отслеживания изменений для базы данных и таблицы.  
  
## <a name="enable-change-tracking-for-a-database"></a>Включение отслеживания изменений для базы данных  
 Прежде чем начать отслеживание изменений, его надо включить на уровне базы данных. В следующем примере показано, как включить отслеживание изменений с помощью инструкции [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
```sql  
ALTER DATABASE AdventureWorks2012  
SET CHANGE_TRACKING = ON  
(CHANGE_RETENTION = 2 DAYS, AUTO_CLEANUP = ON)  
```  
  
 Включить отслеживание изменений можно также в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , в диалоговом окне [Свойства базы данных (страница "Отслеживание изменений")](../../relational-databases/databases/database-properties-changetracking-page.md) . Если база данных содержит оптимизированные для памяти таблицы, включить отслеживание изменений с помощью SQL Server Management Studio невозможно. Чтобы включить отслеживание изменений, используйте T-SQL.
  
 При включении отслеживания изменений, а также в любое время в дальнейшем можно указать и изменить значения параметров CHANGE_RETENTION и AUTO_CLEANUP.  
  
 Параметр срока хранения изменений определяет период времени, в течение которого сохраняются данные отслеживания изменений. Данные отслеживания изменений, срок хранения которых истек, периодически удаляются. При установке этого значения необходимо учитывать частоту синхронизации приложений с таблицами в базе данных. Указанный срок хранения должен быть не меньше максимального периода времени между синхронизациями. Если приложение получает сведения об изменениях через более длительные интервалы, возвращаемые результаты могут оказаться неверными, поскольку часть сведений об изменениях могла уже быть удалена. Чтобы избежать неверных результатов, приложение может определить, не является ли интервал между синхронизациями чрезмерно большим, с помощью системной функции CHANGE_TRACKING_MIN_VALID_VERSION.  
  
 Параметр AUTO_CLEANUP используется для включения и отключения задачи очистки, в процессе выполнения которой удаляются старые данные отслеживания изменений. Он может оказаться полезным при возникновении временной проблемы, которая мешает синхронизации приложений и вызывает необходимость приостановки процесса удаления устаревших данных отслеживания изменений на период своего разрешения.  
  
 При этом следует учесть следующие моменты.  
  
-   При отслеживании изменений уровень совместимости базы данных должен быть не ниже 90. Если уровень совместимости базы данных менее 90, то можно настроить отслеживание изменений. Однако функция CHANGETABLE, используемая для получения сведений об отслеживании изменений, возвратит ошибку.  
  
-   Простейший способ обеспечения согласованности всех данных отслеживания изменений — изоляция моментальных снимков. По этой причине настоятельно рекомендуется включить для базы данных изоляцию моментальных снимков. Дополнительные сведения см. в разделе [Работа с отслеживанием изменений (SQL Server)](../../relational-databases/track-changes/work-with-change-tracking-sql-server.md).  
  
## <a name="enable-change-tracking-for-a-table"></a>Включение отслеживания изменений для таблицы  
 Отслеживание изменений должно быть включено для каждой отслеживаемой таблицы. Если отслеживание изменений включено, ведется сбор сведений об отслеживании для всех строк в таблице, на которую влияет операция DML.  
  
 В следующем примере показано, как настроить отслеживание изменений для таблицы с помощью инструкции [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
```sql  
ALTER TABLE Person.Contact  
ENABLE CHANGE_TRACKING  
WITH (TRACK_COLUMNS_UPDATED = ON)  
```  
  
 Включить отслеживание изменений можно также в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , в диалоговом окне [Свойства базы данных (страница "Отслеживание изменений")](../../relational-databases/databases/database-properties-changetracking-page.md) .  
  
 Если параметр TRACK_COLUMNS_UPDATED установлен в значение ON, компонент [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] сохраняет во внутренней таблице отслеживания дополнительные сведения о столбцах, которые были обновлены. Отслеживание столбцов позволяет приложению синхронизировать только те столбцы, которые были обновлены. Это может повысить эффективность и производительность. Но поскольку отслеживание столбцов требует дополнительного места на диске, по умолчанию этот параметр имеет значение OFF.  
  
## <a name="disable-change-tracking-for-a-database-or-table"></a>Отключение отслеживания изменений для базы данных или таблицы  
 Перед отключением отслеживания изменений для базы данных необходимо отключить его для всех таблиц в этой базе. Чтобы определить, для каких таблиц было включено отслеживание изменений, воспользуйтесь представлением каталога [sys.change_tracking_tables](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md) .  
  
 Если ни для одной из таблиц базы данных отслеживание изменений не настроено, то оно может быть отключено и на уровне базы данных. В следующем примере показано, как отключить отслеживание изменений для базы данных с помощью инструкции [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
```sql  
ALTER DATABASE AdventureWorks2012  
SET CHANGE_TRACKING = OFF  
```  
  
 В следующем примере показано, как отключить отслеживание изменений для таблицы с помощью инструкции [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md).  
  
```sql  
ALTER TABLE Person.Contact  
DISABLE CHANGE_TRACKING;  
```  
  
## <a name="see-also"></a>См. также:  
 [Свойства базы данных (страница "Отслеживание изменений")](../../relational-databases/databases/database-properties-changetracking-page.md)   
 [Параметры ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.change_tracking_databases (Transact-SQL)](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-databases.md)   
 [sys.change_tracking_tables (Transact-SQL)](../../relational-databases/system-catalog-views/change-tracking-catalog-views-sys-change-tracking-tables.md)   
 [Отслеживание измененных данных (SQL Server)](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [Об отслеживании изменений (SQL Server)](../../relational-databases/track-changes/about-change-tracking-sql-server.md)   
 [Работа с информацией об изменениях (SQL Server)](../../relational-databases/track-changes/work-with-change-data-sql-server.md)   
 [Управление отслеживанием изменений (SQL Server)](../../relational-databases/track-changes/manage-change-tracking-sql-server.md)  
  
  

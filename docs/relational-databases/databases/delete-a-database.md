---
title: Удаление базы данных | Документация Майкрософт
description: Узнайте, как на SQL Server удалить пользовательскую базу данных в SQL Server Management Studio или Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- database removal [SQL Server], SQL Server Management Studio
- removing databases
- deleting databases
- dropping databases
- databases [SQL Server], dropping
- database removal [SQL Server]
ms.assetid: 1fd8c0f5-03e1-449a-af45-b8cacb479d9c
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e6835865e78147a11878b12ae8ce9c465b66f0e7
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489428"
---
# <a name="delete-a-database"></a>Удаление базы данных
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В этом разделе описывается, как удалить пользовательскую базу данных в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Ограничения](#Restrictions)  
  
     [Предварительные требования](#Prerequisites)  
  
     [Рекомендации](#Recommendations)  
  
     [Безопасность](#Security)  
  
-   **Удаление базы данных с использованием следующих средств:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Дальнейшие действия.**  [После удаления базы данных](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Системные базы данных не могут быть удалены.  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Предварительные требования  
  
-   Удалите все моментальные снимки базы данных, которые существуют для базы. Дополнительные сведения см. в разделе [Удаление моментального снимка базы данных (Transact-SQL)](../../relational-databases/databases/drop-a-database-snapshot-transact-sql.md).  
  
-   Если база данных участвует в доставке журналов, удалите доставку журналов.  
  
-   Если база данных публикуется для репликации транзакций, опубликована или подписана на репликацию слиянием, удалите репликацию из базы данных.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
  
-   Учтите возможность полного резервного копирования базы данных. Удаленную базу данных можно восстановить только с помощью резервной копии.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Для выполнения инструкции DROP DATABASE пользователь должен, как минимум, иметь разрешение CONTROL на базу данных.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-delete-a-database"></a>Удаление базы данных  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]и разверните его.  
  
2.  Разверните **Базы данных**, щелкните правой кнопкой мыши удаляемую базу данных и затем нажмите кнопку **Удалить**.  
  
3.  Подтвердите, что выбрана верная база данных, а затем нажмите кнопку **ОК**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-delete-a-database"></a>Удаление базы данных  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В следующем примере удаляются базы данных `Sales` и `NewSales` .  
  
```sql  
USE master ;  
GO  
DROP DATABASE Sales, NewSales ;  
GO  
```  
  
##  <a name="follow-up-after-deleting-a-database"></a><a name="FollowUp"></a> Дальнейшие действия. После удаления базы данных  
 Создание резервной копии базы данных **master** . Если необходимо восстановить базу данных **master** , любая база данных, удаленная с момента создания последней резервной копии базы данных **master** , останется в представлениях системного каталога и может вызвать сообщения об ошибках.  
  
## <a name="see-also"></a>См. также:  
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)  
  

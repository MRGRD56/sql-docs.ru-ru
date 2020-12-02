---
title: Удаление несуществующих файловых групп (SQL Server) | Документация Майкрософт
description: В этой статье показано, как удалить уничтоженные файловые группы в SQL Server с помощью SQL Server Management Studio или Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- piecemeal restores [SQL Server], defunct filegroups
- defunct filegroups
- restoring filegroups [SQL Server]
- deferred transactions
- filegroups [SQL Server], defunct
- unrestored filegroups
ms.assetid: 055f9c6a-5c18-4942-98e7-ec918f0ff975
author: cawrites
ms.author: chadam
ms.openlocfilehash: 37c305f6901819ade1c30c2bc71c9d4cb11f6ce5
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130311"
---
# <a name="remove-defunct-filegroups-sql-server"></a>Удаление уничтоженных файловых групп (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом разделе описывается удаление уничтоженных файловых групп в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Ограничения](#Restrictions)  
  
-   [Рекомендации](#Recommendations)  
  
     [Безопасность](#Security)  
  
-   **Удаление уничтоженных файловых групп с использованием следующих средств:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Сведения в этом разделе относятся к базам данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , содержащим несколько файлов или файловых групп (а для простой модели восстановления — к файловым группам, доступным только для чтения).  
  
-   При удалении файловой группы вне сети все файлы группы помечаются удаленными.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
  
-   Если восстановление невосстановленной файловой группы не предполагается, необходимо сделать ее *уничтоженной* , удалив из базы данных. Такая файловая группа не может быть восстановлена в данной базе данных, однако при этом сохраняются ее метаданные. После того как файловая группа стала уничтоженной, базу данных можно перезапустить, и в процессе восстановления будет восстановлена согласованность базы данных между восстановленными файловыми группами.  
  
     Например, объявление файловой группы как нефункционирующей позволяет разрешить отложенные транзакции, возникшие из-за файловой группы вне сети, которая больше не нужна в базе данных. Транзакции, отложенные из-за того, что файловая группа находилась в режиме «вне сети», выходят из отложенного состояния после того, как эта файловая группа перестанет функционировать. Дополнительные сведения см. в разделе [Отложенные транзакции (SQL Server)](../../relational-databases/backup-restore/deferred-transactions-sql-server.md).  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение ALTER на базу данных.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-remove-defunct-filegroups"></a>Удаление уничтоженных файловых групп  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] и разверните его.  
  
2.  Раскройте список **Базы данных**, щелкните правой кнопкой мыши базу данных, из которой удаляется файл, а затем выберите пункт **Свойства**.  
  
3.  Выберите страницу **Файлы** .  
  
4.  В списке **Файлы базы данных** выберите файлы для удаления, нажмите кнопку **Удалить**, а затем кнопку **ОК**.  
  
5.  Выберите страницу **Файловые группы** .  
  
6.  В списке **Строки** выберите файловую группу для удаления, нажмите кнопку **Удалить**, а затем кнопку **ОК**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-remove-defunct-filegroups"></a>Удаление уничтоженных файловых групп  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. (**Примечание**. В этом примере предполагается, что файлы и файловая группа уже существуют. Для создания этих объектов см. пример Б в разделе [Параметры инструкции ALTER DATABASE для файлов и файловых групп](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).) В первом примере удаляются файлы `test1dat3` и `test1dat4` из уничтоженной файловой группы с помощью инструкции `ALTER DATABASE` с предложением `REMOVE FILE`. Во втором примере удаляется уничтоженная файловая группа `Test1FG1`с помощью предложения `REMOVE FILEGROUP` .  
  
```sql  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE test1dat3 ;  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE test1dat4 ;  
GO  
  
```  
  
```sql  
USE master;  
GO  
ALTER DATABASE AdventureWorks2012  
REMOVE FILEGROUP Test1FG1 ;  
GO  
  
```  
  
## <a name="see-also"></a>См. также:  
 [Параметры инструкции ALTER DATABASE для файлов и файловых групп (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)   
 [Отложенные транзакции (SQL Server)](../../relational-databases/backup-restore/deferred-transactions-sql-server.md)   
 [Восстановления файлов (модель полного восстановления)](../../relational-databases/backup-restore/file-restores-full-recovery-model.md)   
 [Восстановление файлов (простая модель восстановления)](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [Восстановление в сети (SQL Server)](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [Восстановление страниц (SQL Server)](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [Поэтапное восстановление (SQL Server)](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)  
  
  

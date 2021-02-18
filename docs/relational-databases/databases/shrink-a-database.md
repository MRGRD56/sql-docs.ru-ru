---
title: Сжатие базы данных | Документация Майкрософт
description: Узнайте, как с помощью Object в SQL Server уменьшить базу данных с помощью SQL Server Management Studio или Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.shrinkdatabase.f1
helpviewer_keywords:
- shrinking databases
- databases [SQL Server], shrinking
- decreasing database size
- database shrinking [SQL Server]
- reducing database size
ms.assetid: 83afbf74-fd50-4c39-831c-b1f473a50620
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9e2eced4a7ca9a8e8f443c709d65cc6e2906082a
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489498"
---
# <a name="shrink-a-database"></a>Сжатие базы данных
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  В этом разделе содержатся инструкции по сжатию базы данных в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью объектов [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Сжатие файлов данных позволяет освободить неиспользуемое пространство путем перемещения страниц данных с конца файла в незанятое пространство ближе к началу файла. Когда в конце файла образуется достаточно свободного места, страницы данных в конце файла могут быть освобождены и возвращены в файловую систему.  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Ограничения](#Restrictions)  
  
     [Рекомендации](#Recommendations)  
  
     [Безопасность](#Security)  
  
-   **Сжатие базы данных с помощью:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Дальнейшие действия.**  [Сжатие базы данных](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Размер базы данных нельзя сделать меньше минимального размера базы данных. Минимальный размер — это первоначальный размер, заданный при создании базы данных, или последний размер, явно установленный операцией изменения размера файла (например, DBCC SHRINKFILE). Если, допустим, база данных была создана с размером 10 МБ и затем увеличилась до 100 МБ, ее можно сжать только до 10 МБ, даже если удалить из нее все данные.  
  
-   Невозможно сжать базу данных во время создания ее резервной копии. И наоборот, нельзя создать резервную копию базы данных во время операции сжатия.
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
  
-   Просмотр количества свободного (нераспределенного) пространства в базе данных. Дополнительные сведения см. в разделе [Отображение данных и сведений о пространстве журнала для базы данных](../../relational-databases/databases/display-data-and-log-space-information-for-a-database.md).  
  
-   Обратите внимание на следующие сведения при планировании сжатия базы данных.  
  
    -   Наибольший эффект от операции сжатия достигается при ее применении после операции, создающей много неиспользуемого пространства, например после усечения таблицы или удаления таблицы.  
  
    -   Большинству баз данных требуется некоторое свободное пространство для выполнения обычных ежедневных операций. Если сжатие базы данных производится регулярно, но она снова увеличивается в размерах, это означает, что место, освобожденное при сжатии, необходимо для нормальной работы. В таких случаях повторное сжатие базы данных бессмысленно.  
  
    -   Операция сжатия не избавляет от фрагментации индексов в базе данных и обычно приводит к еще более сильной фрагментации. Это еще одна причина, по которой не стоит выполнять регулярное сжатие базы данных.  
  
    -   Не следует устанавливать параметр базы данных AUTO_SHRINK в значение ON без достаточных на то оснований.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо быть членом предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-shrink-a-database"></a>Сжатие базы данных  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]и разверните его.  
  
2.  Разверните узел **Базы данных** и щелкните правой кнопкой мыши базу данных, которую нужно сжать.  
  
3.  В меню наведите указатель на пункт **Задачи**, **Сжать** и выберите команду **База данных**.  
  
     **База данных**  
     Отображает имя выбранной базы данных.  
  
     **Выделенное в данный момент место**  
     Отображает суммарное используемое и неиспользуемое пространство для выбранной базы данных.  
  
     **Доступное свободное место**  
     Отображает суммарное свободное место для файлов журналов и данных в выбранной базе данных.  
  
     **Реорганизовать файлы перед освобождением неиспользованного места**  
     Установка данного флажка эквивалентна выполнению инструкции DBCC SHRINKDATABASE с заданием целевого процентного параметра. Снятие этого флажка равнозначно выполнению процедуры DBCC SHRINKDATABASE с параметром TRUNCATEONLY. По умолчанию при открытии диалогового окна этот флажок не установлен. Если этот флажок установлен, то пользователь должен задать целевое процентное значение.  
  
     **Максимальное свободное пространство в файлах после сжатия**  
     Введите максимальный процент свободного пространства, которое должно остаться в базе данных после ее сжатия. Допустимы значения от 0 до 99.  
  
4.  Нажмите кнопку **ОК**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-shrink-a-database"></a>Сжатие базы данных  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере инструкция [DBCC SHRINKDATABASE](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md) используется для уменьшения размера данных и файлов журнала в базе данных `UserDB` и для выделения `10` процентов свободного пространства в этой базе данных.  
  
 [!code-sql[DBCC#DBCC_SHRINKDB1](../../relational-databases/databases/codesnippet/tsql/shrink-a-database_1.sql)]  
  
##  <a name="follow-up-after-you-shrink-a-database"></a><a name="FollowUp"></a> Дальнейшие действия. После сжатия базы данных  
 Данные, перемещаемые в процессе сжатия файла, могут быть разбросаны по любым доступным местам в файле. Это вызывает фрагментацию индекса и может увеличить время выполнения запросов, выполняющих поиск в диапазоне индекса. Чтобы устранить фрагментацию, предусмотрите возможность перестроения индексов файла после сжатия.  
  
## <a name="see-also"></a>См. также:  
 [Сжатие файла](../../relational-databases/databases/shrink-a-file.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [sys.database_files (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)   
 [DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)   
 [DBCC SHRINKFILE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-shrinkfile-transact-sql.md)   
 [Файлы и файловые группы базы данных](../../relational-databases/databases/database-files-and-filegroups.md)  
  
  

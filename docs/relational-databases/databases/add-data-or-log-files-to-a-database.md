---
title: Добавление файлов данных или журналов в базу данных | Документация Майкрософт
description: Узнайте, как в SQL Server 2019 добавить данные или файлы журнала в базу данных с помощью SQL Server Management Studio или Transact-SQL.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], files
- adding data files
- adding files
- adding log files
- file additions [SQL Server], steps
- files [SQL Server], adding
- data additions [SQL Server]
ms.assetid: 8ead516a-1334-4f40-84b2-509d0a8ffa45
author: stevestein
ms.author: sstein
ms.openlocfilehash: e0e411e82a6492fda48cc8d3541dbd272075f994
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813133"
---
# <a name="add-data-or-log-files-to-a-database"></a>Добавление файлов данных или журналов в базу данных
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В этом подразделе содержатся инструкции по добавлению файлов данных или журналов в базу данных на сервере [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] при помощи [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Ограничения](#Restrictions)  
  
     [Безопасность](#Security)  
  
-   **Добавление файлов данных или журналов в базу данных при помощи следующих средств.**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Добавить или удалить файл во время выполнения инструкции BACKUP невозможно.  
  
-   Для каждой базы данных может указываться не более 32 767 файлов и 32 767 файловых групп.  
  
###  <a name="security"></a><a name="Security"></a> безопасность  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permissions  
 Необходимо разрешение ALTER на базу данных.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-add-data-or-log-files-to-a-database"></a>Добавление файлов данных или журналов в базу данных  
  
1.  В **обозревателе объектов** подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] и разверните его.  
  
2.  Разверните узел **Базы данных**, щелкните правой кнопкой мыши базу данных, в которую необходимо добавить файлы, и выберите пункт **Свойства**.  
  
3.  В диалоговом окне **Свойства базы данных** перейдите на вкладку **Файлы** .  
  
4.  Чтобы добавить данные или файл журнала транзакций, нажмите кнопку **Добавить**.  
  
5.  В сетке **Файлы базы данных** введите логическое имя файла. Имя файла должно быть уникальным в пределах базы данных.  
  
6.  Выберите тип файла, данные или журнал.  
  
7.  Выберите файловую группу, в которую следует добавить файл данных, или выберите **\<new filegroup>** , чтобы создать новую. Журналы транзакций не могут быть помещены в файловые группы.  
  
8.  Укажите исходный размер файла. Файл данных следует делать как можно большего размера, в соответствии с максимальным предполагаемым объемом данных в базе данных.  
  
9. Укажите, как должен расширяться файл, нажав кнопку ( **...** ) в столбце **Авторасширение**. Выберите один из следующих параметров.  
  
    1.  Чтобы разрешить выбранному файлу расти по мере необходимости, установите флажок **Разрешить авторасширение** и выберите один из следующих параметров.  
  
    2.  Чтобы файл увеличивался с фиксированным приращением, выберите параметр **В мегабайтах** и укажите значение.  
  
    3.  Чтобы файл увеличивался на определенный процент от текущего размера, выберите параметр **В процентах** и укажите значение.  
  
10. Укажите максимальный размер файла, выбрав один из следующих параметров.  
  
    1.  Чтобы указать максимальный размер, до которого может увеличиваться файл, выберите параметр **Ограничение размера файла (МБ)** и укажите нужное значение.  
  
    2.  Чтобы разрешить файлу увеличиваться по мере необходимости, выберите параметр **Неограниченный рост размера файлов**.  
  
    3.  Чтобы предотвратить рост файла, снимите флажок **Разрешить авторасширение** . При этом файл не превысит размер, указанный в столбце **Начальный размер (МБ)** .  
  
    > [!NOTE]  
    >  Максимальный размер базы данных зависит от доступного пространства на диске и от ограничений лицензии, устанавливаемых используемой версией [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
11. Укажите путь к файлу. Указанный путь к добавляемому файлу должен существовать.  
  
    > [!NOTE]  
    >  Данные и журналы транзакций по умолчанию помещаются на один и тот же диск и в один и тот же каталог. Это сделано в соответствии с требованиями, предъявляемыми системами с одним диском, но для рабочей среды это может оказаться неоптимальным. Дополнительные сведения см. в статье [Файлы и группы файлов базы данных](../../relational-databases/databases/database-files-and-filegroups.md).  
  
12. Нажмите кнопку **ОК**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-add-data-or-log-files-to-a-database"></a>Добавление файлов данных или журналов в базу данных  
  
1.  Установите соединение с компонентом [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На панели «Стандартная» нажмите **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**. В этом примере выполняется добавление в базу данных группы из двух файлов. В примере в базе данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]создается файловая группа `Test1FG1` и добавляются два файла по 5 МБ в эту файловую группу.  
  
 [!code-sql[DatabaseDDL#AlterDatabase2](../../relational-databases/databases/codesnippet/tsql/add-data-or-log-files-to_1.sql)]  
  
 Дополнительные сведения см. в разделе [Параметры инструкции ALTER DATABASE для файлов и файловых групп (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).  
  
## <a name="see-also"></a>См. также:  
 [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md)   
 [Удаление файлов данных или журнала из базы данных](../../relational-databases/databases/delete-data-or-log-files-from-a-database.md)   
 [Увеличение размера базы данных](../../relational-databases/databases/increase-the-size-of-a-database.md)  
  
  

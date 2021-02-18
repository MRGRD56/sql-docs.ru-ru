---
title: Удаление существующего экземпляра
description: Эта статья описывает удаление изолированного экземпляра SQL Server, который также подготавливает систему для переустановки SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- removing instances of SQL Server
- uninstalling instances of SQL Server
- removing SQL Server
- instances of SQL Server, uninstalling
- uninstalling SQL Server
ms.assetid: 3c64b29d-61d7-4b86-961c-0de62261c6a1
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6a5b77c8e67d6512e691f748265e7901319a5e4f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336085"
---
# <a name="uninstall-an-existing-instance-of-sql-server-setup"></a>Удаление существующего экземпляра SQL Server (программа установки)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  В данной статье описан процесс удаления изолированного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Шаги, перечисленные в этой статье, помогут подготовить систему для повторной установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 > [!NOTE]
 > Для удаления отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] используется функция удаления узла, предоставляемая программой установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которая удаляет каждый узел по отдельности. Дополнительные сведения см. на странице [Добавление и удаление узлов в отказоустойчивом кластере SQL Server (настройка)](../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md).  

## <a name="considerations"></a>Рекомендации

- Удаление экземпляра SQL Server должен производить локальный администратор, имеющий разрешения на вход в систему в качестве службы. 
- Если на компьютере установлен *минимальный* требуемый объем физической памяти, увеличьте размер файла подкачки вдвое больше объема физической памяти. Нехватка виртуальной памяти может привести к неполному удалению SQL Server. 
- В системе с несколькими экземплярами SQL Server служба браузера SQL Server удаляется только после удаления последнего экземпляра SQL Server. Службу браузера SQL Server можно удалить вручную через **Программы и компоненты** на **панели управления**. 
- При удалении [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] удаляются файлы данных tempdb, добавленные во время процесса установки. Файлы с именем, удовлетворяющим шаблону tempdb_mssql_*.ndf, удаляются, если они существуют в каталоге системной базы данных. 
  

  
## <a name="prepare"></a>Подготовка.  
  
1.  **Резервное копирование данных.** Либо создайте [полные](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md) резервные копии всех баз данных, включая системные базы данных, либо вручную скопируйте MDF- и LDF-файлы в отдельное место. База данных **master** содержит все сведения на уровне системы для сервера, такие как имена входа и схемы. База данных **msdb** содержит сведения о заданиях, такие как задания агента SQL Server, журнал резервного копирования и планы обслуживания. Дополнительные сведения о системных базах данных см. в разделе [Системные базы данных](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md). 
  
    Необходимо сохранить следующие файлы баз данных.  

    * master.mdf
    * msdbdata.mdf
    * Tempdb.mdf
    * mastlog.ldf
    * msdblog.ldf
    * Templog.ldf
    * model.mdf
    * Mssqlsystemresource.mdf
    * ReportServer[$InstanceName]
    * modellog.ldf
    * Mssqlsustemresource.ldf
    * ReportServer[$InstanceName]TempDB

    > [!NOTE]
    > Базы данных ReportServer включены в службы SQL Server Reporting Services.   

 
1.  **Остановите все** **службы** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Перед удалением компонентов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] рекомендуется остановить все службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Наличие активных соединений может помешать удалению компонентов.  
  
1.  **Выбор учетной записи с необходимыми разрешениями.** Выполните вход на сервер с учетной записью службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или с учетной записью, обладающей аналогичным набором разрешений. Например, можно войти на сервер с учетной записью, входящей в локальную группу администраторов.  
  
## <a name="uninstall"></a>Удаление 

# <a name="windows-10--2016-"></a>[Windows 10 и 2016 +](#tab/Windows10)

Чтобы удалить SQL Server из Windows 10, Windows Server 2016, Windows Server 2019 и более поздних версий, выполните следующие действия. 

1. Чтобы начать процесс удаления, перейдите к **Параметры** в меню "Пуск" и выберите **Приложения**. 
1. Введите `sql` в поле поиска. 
1. Выберите **Microsoft SQL Server (версия) (разрядность)** . Например, `Microsoft SQL Server 2017 (64-bit)`.
1. Выберите **Удалить**.
 
    ![Удаление SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/uninstall-sql-server-windows-10.png)

1. Выберите **Удалить** во всплывающем диалоговом окне SQL Server, чтобы запустить мастер установки Microsoft SQL Server. 

    ![Удаление SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/remove-sql-2017.png)
  
1.  На странице **Выбор экземпляра** воспользуйтесь раскрывающимся списком, чтобы указать удаляемый экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], или укажите параметр для удаления только общих компонентов и средств управления [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Чтобы продолжить работу, щелкните **Далее**.  
  
1.  На странице **Выбор компонентов** укажите компоненты, которые нужно удалить из указанного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
1.  На странице **Все готово** для удаления просмотрите список компонентов и функций, подлежащих удалению. Нажмите кнопку **Удалить** , чтобы начать удаление  
 
1. Обновите окно **Приложения и компоненты**, чтобы убедиться, что экземпляр SQL Server был успешно удален, и определите, какие компоненты SQL Server все еще остались. При необходимости удалите эти компоненты из этого окна. 

# <a name="windows-2008---2012-r2"></a>[Windows 2008 - 2012 R2](#tab/windows2012)

Чтобы удалить SQL Server из Windows Server 2008, Windows Server 2012 и Windows 2012 R2, выполните следующие действия. 

1. Чтобы начать процесс удаления, перейдите в **панель управления**, а затем выберите **Программы и компоненты**.
1. Щелкните правой кнопкой мыши **Microsoft SQL Server (версия) (разрядность)**  и выберите **Удалить**. Например, `Microsoft SQL Server 2012 (64-bit)`.  
  
    ![Удаление SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/uninstall-sql-server-windows-2012.png)

1. Выберите **Удалить** во всплывающем диалоговом окне SQL Server, чтобы запустить мастер установки Microsoft SQL Server. 

    ![Удаление SQL Server](media/uninstall-an-existing-instance-of-sql-server-setup/remove-sql-2012.png)
  
1.  На странице **Выбор экземпляра** воспользуйтесь раскрывающимся списком, чтобы указать удаляемый экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], или укажите параметр для удаления только общих компонентов и средств управления [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Чтобы продолжить работу, щелкните **Далее**.  
  
1.  На странице **Выбор компонентов** укажите компоненты, которые нужно удалить из указанного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
1.  На странице **Все готово** для удаления просмотрите список компонентов и функций, подлежащих удалению. Нажмите кнопку **Удалить** , чтобы начать удаление  
 
1. Обновите окно **Программы и компоненты**, чтобы убедиться, что экземпляр SQL Server был успешно удален, и определите, какие компоненты SQL Server все еще остались. При необходимости удалите эти компоненты из этого окна. 

---

  
## <a name="in-the-event-of-failure"></a>В случае сбоя  

В случае сбоя процесса удаления изучите [файлы журнала установки SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md), чтобы определить основную причину. 

Статья базы знаний [Обнаружение проблем установки SQL Server в файлах журнала установки](https://support.microsoft.com/kb/955396/en-us) может помочь в расследовании. Хотя она предназначена для SQL Server 2008, описываемая методология применима к каждой версии SQL Server. 

  
## <a name="see-also"></a>См. также:  
 [Просмотр и чтение файлов журналов программы установки SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
  

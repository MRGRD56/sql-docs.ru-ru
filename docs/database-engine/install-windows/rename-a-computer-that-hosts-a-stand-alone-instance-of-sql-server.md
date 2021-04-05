---
title: Переименование компьютера, на котором размещен экземпляр
description: При переименовании компьютера, на котором размещен экземпляр SQL Server, следует обновить системные метаданные, хранящиеся в каталоге sys.servers.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- remote login errors [SQL Server]
- standalone computer names [SQL Server]
- names [SQL Server], standalone instances of SQL Server
- renaming standalone instances of SQL Server
- sysservers system table
- removing remote logins
- deleting remote logins
- dropping remote logins
ms.assetid: bbaf1445-b8a2-4ebf-babe-17d8cf20b037
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: debdeffa2409642c01f9abdc502a897d6ea3dac3
ms.sourcegitcommit: 2f971c85d87623c0aed1612406130d840e7bdb2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2021
ms.locfileid: "105744463"
---
# <a name="rename-a-computer-that-hosts-a-stand-alone-instance-of-sql-server"></a>Переименование компьютера, на который установлен изолированный экземпляр SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Если изменить имя компьютера, на котором работает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], новое имя будет распознано в момент следующего запуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Не нужно заново запускать программу установки, чтобы изменить имя компьютера. Вместо этого следует выполнить следующие шаги для обновления системных метаданных, хранимых в представлении каталога sys.servers и возвращаемых системной функцией @@SERVERNAME . Обновите системные метаданные таким образом, чтобы отразить в них изменения в именах компьютеров для удаленных соединений и приложений, в которых используется системная функция @@SERVERNAME или которые запрашивают имя сервера в представлении каталога sys.servers.  
  
Следующие действия нельзя использовать для переименования экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ими можно воспользоваться только для изменения части имени экземпляра, соответствующей имени компьютера. Например, можно изменить имя компьютера MB1, на котором расположен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с именем Instance1, на другое имя, например MB2. Однако часть имени, представляющая собой имя экземпляра (Instance1), останется неизменной. В данном примере \\\\*ИмяКомпьютера*\\*ИмяЭкземпляра* изменится с \\\MB1\Instance1 на \\\MB2\Instance1.  
  
 **Before you begin**  
  
 Прежде чем приступить к процессу переименования, обратите внимание на следующее:  
  
-   Если экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] является частью отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , переименование компьютера выполняется не так, как для изолированного экземпляра. Дополнительные сведения см. в статье [Переименование экземпляра отказоустойчивого кластера SQL Server](../../sql-server/failover-clusters/install/rename-a-sql-server-failover-cluster-instance.md).
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не поддерживает переименование компьютеров, участвующих в репликации, за исключением репликации с доставкой журналов. Компьютер-получатель в доставке журнала может быть переименован, если компьютер-источник окончательно потерян. Дополнительные сведения см. в статье [Репликация и доставка журналов (SQL Server)](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md).  
  
-   После переименования компьютера, настроенного для использования служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] могут оказаться недоступными. Дополнительные сведения см. в разделе [Переименование компьютера, на котором установлен сервер отчетов](../../reporting-services/report-server/rename-a-report-server-computer.md).  
  
-   Если компьютер настроен для использования зеркального отображения базы данных, перед его переименованием оно должно быть отключено. После этого зеркальное отображение необходимо вновь установить для нового имени компьютера. Метаданные для зеркального отображения базы данных не будут обновлены автоматически для отражения нового имени компьютера. Выполните следующие шаги, чтобы обновить системные метаданные.  
  
-   Пользователи, которые подключаются к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] через группу Windows, в которой имя компьютера задано жестко, могут лишиться возможности подключения к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это может произойти после переименования, если в группе Windows останется прежнее имя компьютера. Чтобы убедиться в том, что возможно соединение с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с использованием таких групп Windows после операции переименования, обновите группу Windows для указания нового имени компьютера.  
  
 Подключение к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью нового имени компьютера станет возможно после перезапуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Чтобы убедиться в том, что системная функция @@SERVERNAME возвращает новое имя локального экземпляра сервера, необходимо вручную выполнить следующую процедуру, применяющуюся в сценарии пользователя. Какая именно процедура должна быть выполнена, зависит от того, установлен ли на компьютере именованный экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]или экземпляр по умолчанию.  
  
## <a name="rename-a-computer-that-hosts-a-stand-alone-instance-of-ssnoversion"></a>Переименование компьютера, на котором расположен изолированный экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Для компьютера с измененным именем, на котором установлен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]по умолчанию, следует выполнить следующие процедуры.  
  
    ```sql
    EXEC sp_dropserver '<old_name>';  
    GO  
    EXEC sp_addserver '<new_name>', local;  
    GO  
    ```  
  
     Повторно запустите экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Для компьютера с измененным именем, на котором установлен именованный экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], следует выполнить следующие процедуры.  
  
    ```sql
    EXEC sp_dropserver '<old_name\instancename>';  
    GO  
    EXEC sp_addserver '<new_name\instancename>', local;  
    GO  
    ```  
  
     Повторно запустите экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="after-the-renaming-operation"></a>После операции переименования  
 После переименования компьютера все соединения, которые используют прежнее имя, должны подключаться с помощью нового имени.  
  
## <a name="verify-renaming-operation"></a>Проверка операции переименования  
  
-   Выберите данные из @@SERVERNAME или sys.servers. Функция @@SERVERNAME возвращает новое имя, а в таблице sys.servers отображается новое имя. В следующем примере показано использование @@SERVERNAME .  
  
    ```  
    SELECT @@SERVERNAME AS 'Server Name';  
    ```  
  
## <a name="additional-considerations"></a>Дополнительные сведения  
 **Удаленные имена входа** — если на компьютере имеются удаленные имена входа, при запуске хранимой процедуры **sp_dropserver** может возникнуть ошибка, аналогичная следующей:  
  
 `Server: Msg 15190, Level 16, State 1, Procedure sp_dropserver, Line 44 There are still remote logins for the server 'SERVER1'.`  
  
 Чтобы исправить ошибку, необходимо удалить имена для удаленного входа в систему для этого сервера.  
  
### <a name="drop-remote-logins"></a>Сброс удаленных входов в систему  
  
-   В случае с экземпляром по умолчанию, выполните следующие действия:  
  
    ```sql
    EXEC sp_dropremotelogin old_name;  
    GO  
    ```  
  
-   В случае с именованным экземпляром, выполните следующие действия:  
  
    ```sql
    EXEC sp_dropremotelogin old_name\instancename;  
    GO  
    ```  
  
 **Конфигурации связанных серверов** . Операция переименования компьютера повлияет на конфигурации связанных серверов. Для обновления ссылок на имена компьютеров используйте хранимые процедуры **sp_addlinkedserver** или **sp_setnetname**. Дополнительные сведения см. в статье [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) или [sp_setnetname (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-setnetname-transact-sql.md).  
  
 **Имена-псевдонимы клиентов**. Операция переименования компьютера повлияет на псевдонимы клиентов, в которых используются именованные каналы. Например, если псевдоним «PROD_SRVR» указывает на SRVR1 и в нем используется протокол именованных каналов, то имя канала будет выглядеть следующим образом: `\\SRVR1\pipe\sql\query`. После переименования компьютера путь именованного канала станет недействительным. Дополнительные сведения об именованных каналах см. в разделе [Создание допустимой строки подключения, использующей протокол именованных каналов](/previous-versions/sql/sql-server-2008/ms189307(v=sql.100)).  
  
## <a name="see-also"></a>См. также раздел  
 [Установка SQL Server](../../database-engine/install-windows/install-sql-server.md)  
  

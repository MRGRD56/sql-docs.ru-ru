---
title: Справка диспетчера конфигурации SQL Server
description: Ознакомьтесь с диспетчером конфигурации SQL Server. Узнайте, как использовать его для управления службами SQL Server и настройки сетевого подключения.
ms.custom: seo-lt-2019
ms.date: 02/01/2018
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Configuration Manager, help
ms.assetid: 6e909911-39a6-469b-b22a-3afdfd08a30b
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 0e690630174bb9e8f92575f69b586b44c4dabbfc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345173"
---
# <a name="sql-server-configuration-manager-help"></a>Справка диспетчера конфигурации SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Используйте диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , чтобы настроить службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и сетевые соединения. Для создания и управления объектами базы данных, настройки безопасности и создания запросов [!INCLUDE[tsql](../../includes/tsql-md.md)] используйте среду [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Дополнительные сведения о среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]см. в электронной документации по [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

 > [!TIP]
 > Если вам нужно настроить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в Linux, используйте средство **mssql-conf**. См. сведения о [настройке SQL Server на Linux с помощью средства mssql-conf](../../linux/sql-server-linux-configure-mssql-conf.md).

 В этом разделе содержатся разделы справки F1 диалоговых окон в диспетчере конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] С помощью Configuration Manager нельзя настроить версию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], предшествующую [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
## <a name="services"></a>Службы  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] управляет службами, относящимися к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Большинство этих задач могут быть выполнены при помощи диалогового окна «Службы [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows», но необходимо отметить, что диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняет дополнительные операции с управляемыми им службами, такие как применение правильных разрешений при изменении учетной записи службы. Использование обычного диалогового окна служб Windows для настройки любых служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может привести к неправильной работе службы.  
  
 Используйте диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для выполнения следующих задач со службами:  
  
-   запуск, прекращение и временная остановка служб;  
  
-   настройка служб на автоматический или ручной запуск, отключение служб или изменение других настроек служб;  
  
-   изменение паролей для учетных записей, используемых службами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ;  
  
-   запуск [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при помощи флагов трассировки (параметры командной строки);  
  
-   просмотр свойств служб.  
  
## <a name="sql-server-network-configuration"></a>Сетевая конфигурация SQL Server  
 Используйте диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для выполнения следующих задач, относящихся к службам [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на этом компьютере:  
  
-   включение или отключение сетевого протокола [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ;  
  
-   настройка сетевого протокола [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Краткие инструкции по настройке протоколов и подключению к компоненту [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] см. в руководстве [по началу работы с ядром СУБД](../../relational-databases/tutorial-getting-started-with-the-database-engine.md).  
  
## <a name="sql-server-native-client-configuration"></a>Конфигурация собственного клиента SQL Server  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] подключаются к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при помощи сетевой библиотеки собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Используйте диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для следующих задач, относящихся к клиентским приложениям на этом компьютере:  
  
-   при работе с клиентскими приложениями [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на этом компьютере указание порядка протоколов при соединении с экземплярами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)];  
  
-   настройка клиентских протоколов соединения;  
  
-   при работе с клиентскими приложениями [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создание псевдонимов экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]с целью предоставить клиентам возможность подключения при помощи пользовательской строки соединения.  
  
 Дополнительные сведения о каждой из этих задач см. в справке F1 соответствующей задачи.  
  
#### <a name="to-open-sql-server-configuration-manager"></a>Запуск диспетчера конфигурации SQL Server  
  
-   В меню **Пуск** укажите **Все программы**, **Microsoft SQL Server** (версия), **Средства настройки** и выберите пункт **Диспетчер конфигурации SQL Server**.  
  
  
 **Запуск диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в [!INCLUDE[win8](../../includes/win8-md.md)]**  
  
 Так как диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] является оснасткой консоли управления [!INCLUDE[msCoName](../../includes/msconame-md.md)] , а не отдельной программой, при работе в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] диспетчер конфигурации [!INCLUDE[win8](../../includes/win8-md.md)]не отображается как приложение. Чтобы открыть диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], нажмите чудо-кнопку **Поиск**, выберите **Приложения**, введите **SQLServerManager12.msc** (для [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) или **SQLServerManager11.msc** (для [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) и нажмите клавишу **ВВОД**.  
  

## <a name="see-also"></a>См. также:  
 [Службы SQL Server](../../tools/configuration-manager/sql-server-services.md)   
 [Сетевая конфигурация SQL Server](../../tools/configuration-manager/sql-server-network-configuration.md)   
 [Конфигурация SQL Native Client 11.0](../../tools/configuration-manager/sql-native-client-11-0-configuration.md)   
 [Выбор сетевого протокола](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))  
  

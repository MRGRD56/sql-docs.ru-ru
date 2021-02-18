---
title: служба «SQL Server, браузер»
description: Узнайте, как использовать обозреватель SQL Server — службу, которая прослушивает запросы на ресурсы SQL Server и предоставляет сведения об установленных экземплярах SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
f1_keywords:
- sql13.swb.browseservers.local.f1
- sql13.swb.browseservers.network.f1
helpviewer_keywords:
- services [SQL Server], security
- SQL Browser service (See SQL Server Browser Service)
- Browser Service
- SQL Server Browser service
ms.assetid: 3cc00d3a-487c-4cd9-a155-655f02485fa0
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a4c58894434c503f92c8bcf4df4f74cf9fa2c688
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345190"
---
# <a name="sql-server-browser-service"></a>служба «SQL Server, браузер»
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  Браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выполняется как служба Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] прослушивает входящие запросы к ресурсам [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и предоставляет сведения об экземплярах [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], установленных на компьютере. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] предназначен для выполнения трех задач:  
  
-   просмотра списка доступных серверов;  
  
-   соединения с нужным экземпляром сервера;  
  
-   соединения с конечными точками через выделенное административное соединение (DAC).  
  
 [!INCLUDE[ssDE](../../includes/ssde-md.md)] и [!INCLUDE[ssAS](../../includes/ssas-md.md)] получают от службы "Браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]" (sqlbrowser) имя и номер версии для каждого экземпляра. Браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] устанавливается вместе с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] настраивается в ходе установки или с помощью диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. По умолчанию служба "Браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]" запускается автоматически:  
  
-   при обновлении установки;  
  
-   при установке в кластере;  
  
-   при установке именованного экземпляра компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)], включая все экземпляры SQL Server Express;  
  
-   при установке именованного экземпляра служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
## <a name="background"></a>Историческая справка  
 До версии [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]на компьютер мог быть установлен только один экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] прослушивал входящие запросы через порт 1433, назначенный для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] комитетом IANA. Порт может использоваться только одним экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , поэтому после появления в [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] поддержки нескольких экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]был разработан протокол разрешения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SSRP) для прослушивания UDP-порта 1434. Эта служба прослушивания отвечала на клиентские запросы, передавая им имена установленных экземпляров с указанием портов или именованных каналов, используемых экземпляром. Чтобы избавиться от ограничений протокола SSRP, в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] она была заменена службой браузера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="how-sql-server-browser-works"></a>Как работает служба «Обозреватель SQL Server»  
 Если [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] настроен на использование протокола TCP/IP, то при запуске экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]серверу назначается порт TCP/IP. Если включен протокол именованных каналов, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] прослушивает указанный именованный канал. Этот порт или «канал», используется конкретным экземпляром для обмена данными с клиентскими приложениями. Экземпляру по умолчанию при установке назначается TCP-порт 1433 и канал `\sql\query` , но затем эти значения могут быть изменены администратором сервера при помощи диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Поскольку порт или канал может использоваться только одним экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , именованным экземплярам, включая [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], назначаются другие номера портов и имена каналов. По умолчанию, если и именованные экземпляры и [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] настроены для работы с динамическими портами, это означает, что доступный порт назначается при запуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . При необходимости экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]может быть назначен конкретный порт, и при соединении клиенты смогут указать именно его. Но если порт назначается динамически, то он может измениться в любой момент после перезапуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , поэтому клиент может и не знать правильного номера порта.  
  
 После запуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] запускается браузер и пытается занять UDP-порт 1434. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] читает реестр, находит все экземпляры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на данном компьютере и помечает используемые ими порты и именованные каналы. Если сервер имеет несколько сетевых плат, браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает первый допустимый порт, который найден для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает протоколы ipv6 и ipv4.  
  
 При запросе клиентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ресурсов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] клиентская сетевая библиотека передает на сервер UDP-сообщение через порт 1434. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Браузер в ответ сообщает TCP/IP-порт или именованный канал запрошенного экземпляра. Затем сетевая библиотека клиентского приложения завершает соединение, отправляя запрос на сервер с указанием номера порта или имени канала, относящегося к нужному экземпляру.  
  
 Дополнительные сведения о запуске и остановке службы браузера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в электронной документации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="using-sql-server-browser"></a>Применение обозревателя SQL Server  
 Если служба « [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , браузер» не запущена, то возможность соединения с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] остается только при указании верного номера порта или именованного канала. Например, к экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по умолчанию можно подключиться по порту TCP/IP, если он прослушивает порт 1433.  
  
 Однако если служба « [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , браузер» не запущена, следующие соединения невозможны.  
  
-   Если какой-либо компонент пытается подключиться к именованному экземпляру без полного указания всех параметров (номера порта TCP/IP или именованного канала).  
  
-   Если компонент формирует или сохраняет сведения о сервере и экземпляре, которые затем используются другими компонентами для повторного соединения.  
  
-   При соединении с именованным экземпляром без указания номера порта или канала.  
  
-   При использовании выделенного административного соединения с именованным экземпляром или экземпляром по умолчанию без использования порта TCP/IP 1433.  
  
-   При использовании службы перенаправителя OLAP.  
  
-   При перечислении серверов в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], программе Enterprise Manager или Query Analizer.  
  
 В клиент-серверном режиме работы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (например, если приложения обращаются к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по сети) при остановке или отключении службы « [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , браузер» необходимо назначить каждому экземпляру определенные номера портов и указывать их в коде клиентских приложений. Такой подход приводит к следующим проблемам.  
  
-   Необходимо обновлять и поддерживать код клиентских приложений, чтобы они соединялись по соответствующим номерам портов.  
  
-   Порт, указанный для экземпляра, может быть уже занят другой службой или приложением, работающим на сервере, что может привести к недоступности экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="clustering"></a>Кластеризация  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не является кластеризованным ресурсом и не поддерживает отработку отказа с одного узла кластера на другой. Следовательно, при использовании кластера браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] необходимо устанавливать и включать для каждого узла. При работе на кластерах браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] прослушивает порт IP_ANY.  
  
> [!NOTE]  
>  Если указан порт IP_ANY, при включении прослушивания на определенных IP-адресах пользователь должен настроить тот же TCP-порт на каждом из IP-адресов, поскольку браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] возвращает каждую найденную пару «адрес-порт».  
  
## <a name="installing-uninstalling-and-running-from-the-command-line"></a>Установка, удаление и запуск из командной строки  
 По умолчанию браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] устанавливается в C:\Program Files (x86)\Microsoft SQL Server\90\Shared\sqlbrowser.exe.  
  
 Служба браузера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] удаляется при удалении последнего экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] В целях диагностики браузер можно запустить из командной строки с параметром **-c** :  
  
```  
<drive>\<path>\sqlbrowser.exe -c  
```  
  
## <a name="security"></a>Безопасность  
  
### <a name="account-privileges"></a>Права доступа учетной записи  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Браузер прослушивает UDP-порт и принимает запросы без проверки подлинности с использованием протокола разрешения [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SSRP). [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должен запускаться в контексте безопасности непривилегированного пользователя, чтобы минимизировать ущерб при возможном проникновении злоумышленника. Учетную запись входа можно изменить при помощи диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Права, которые необходимо назначить браузеру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Запретить сетевой доступ к этому компьютеру.  
  
-   Запретить локальный вход в систему.  
  
-   Запретить вход в систему в качестве пакетного задания.  
  
-   Запретить вход в систему через службы терминалов.  
  
-   Вход в систему в качестве службы.  
  
-   Разрешить чтение и запись разделов реестра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , связанных с сетью (порты и каналы).  
  
### <a name="default-account"></a>Учетная запись по умолчанию  
 Программа установки настраивает браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для использования учетной записи, выбранной для служб при установке. Можно указать другую учетную запись:  
  
-   Любая учетная запись **домен\локальная** .  
  
-   Учетная запись **локальной службы**  
  
-   Учетная запись **локальной системы** (не рекомендуется за избыточностью прав доступа).  
  
### <a name="hiding-sql-server"></a>Скрытие экземпляра SQL Server  
 Скрытые экземпляры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] — это экземпляры, которые поддерживают только соединения через общую память. В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]установите флаг `HideInstance` , чтобы браузер [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не выдавал сведения об этом экземпляре сервера.  
  
### <a name="using-a-firewall"></a>Применение брандмауэра  
 Для связи со службой браузера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] на сервере, защищенном брандмауэром, в дополнение к TCP-порту [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (например 1433) откройте UDP-порт 1434. Сведения о работе с брандмауэром см. в разделе "Практическое руководство. Настройка брандмауэра для доступа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]" в документации по [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>См. также:  
 [Сетевые протоколы и библиотеки](../../sql-server/install/network-protocols-and-network-libraries.md)  
  
  

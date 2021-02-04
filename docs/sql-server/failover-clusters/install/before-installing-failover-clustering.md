---
title: Подготовка к установке отказоустойчивого кластера | Документация Майкрософт
description: В этой статье рассматриваются вопросы планирования при подготовке к установке отказоустойчивого кластера SQL Server, включая оборудование, операционную систему и конфигурацию.
ms.custom: ''
ms.date: 08/24/2016
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], preinstallation checklist
- installing failover clusters
- failover clustering [SQL Server], preinstallation checklist
ms.assetid: a655225d-8c54-4b30-95fd-31f588167899
author: cawrites
ms.author: chadam
ms.openlocfilehash: 87294a8d20bf1c297c1629509a85cde7493b6ef5
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076612"
---
# <a name="before-installing-failover-clustering"></a>Подготовка к установке отказоустойчивого кластера
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Перед тем как установить отказоустойчивый кластер SQL Server, необходимо выбрать оборудование и операционную систему, на которых SQL Server будет работать. Кроме того, необходимо настроить отказоустойчивую кластеризацию Windows Server (WSFC) и проверить настройки сети, безопасности и другого программного обеспечения, которое будет запускаться на отказоустойчивом кластере.  
  
 Если в кластере Windows есть локальный диск и при этом диск с такой же буквой используется на одном или нескольких узлах кластера в качестве общего диска, то установить [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на этот диск невозможно. Это ограничение относится как к экземплярам отказоустойчивого кластера SQL Server, так и к автономным экземплярам на сервере, который входит в состав экземпляра отказоустойчивого кластера Windows.
  
 Дополнительные сведения об основных понятиях, функциях и задачах отказоустойчивых кластеров [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] приведены в следующих разделах.  
  
|Описание раздела|Раздел|  
|-----------------------|-----------|  
|Содержит описание основных понятий отказоустойчивых кластеров [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , а также ссылки на связанное содержимое и задачи.|[Экземпляры отказоустойчивого кластера (режим AlwaysOn) (SQL Server)](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)|  
|Содержит описание основных понятий политик отработки отказов [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , а также ссылки на сведения о настройке политики отработки отказов для обеспечения потребностей организации.|[Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)|  
|Содержит описание обеспечения работоспособности существующего отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Администрирование и обслуживание экземпляров отказоустойчивого кластера](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)|  
|Описание установки служб [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] в отказоустойчивый кластер Windows Server (WSFC).|[Кластеризация служб SQL Server Analysis Services](/previous-versions/sql/sql-server-2012/dn736073(v=msdn.10))|  
  
 
  
##  <a name="best-practices"></a><a name="BestPractices"></a> Рекомендации  
  
-   Просмотрите [заметки о выпуске](/previous-versions/sql/sql-server-15/dn169381(v=sql.15)) [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)].  
  
-   Обязательное программное обеспечение для установки. Перед запуском программы установки для установки или обновления [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]установите следующие компоненты, чтобы сократить время установки. Можно установить обязательное программное обеспечение на каждом узле отказоустойчивого кластера, а затем один раз перезапустить узлы перед началом работы программы установки.  
  
    -   Windows PowerShell больше не устанавливается программой установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Оболочка Windows PowerShell является обязательной для установки компонентов [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)][!INCLUDE[ssDE](../../../includes/ssde-md.md)] и [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Если оболочка Windows PowerShell отсутствует на компьютере, ее можно включить, следуя указаниям на странице [Windows Management Framework](/powershell/scripting/windows-powershell/wmf/overview) .  
  
    -   Платформа .NET Framework 3.5 с пакетом обновления 1 (SP1) больше не устанавливается программой установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , но может потребоваться при установке [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в старых операционных системах Windows. Дополнительные сведения см. в статье [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)][Заметки о выпуске](/previous-versions/sql/sql-server-15/dn169381(v=sql.15)).  
  
    -   **Пакет обновления [!INCLUDE[msCoName](../../../includes/msconame-md.md)]:** Чтобы избежать перезагрузки компьютера из-за установки .NET Framework 4 во время установки, программа установки [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] требует установки на компьютере центра обновления [!INCLUDE[msCoName](../../../includes/msconame-md.md)].  Это обновление включено в установку [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] в Windows 7 с пакетом обновления 1 (SP1) или [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] с пакетом обновления 2 (SP2). При установке в более старой операционной системе Windows загрузите его по ссылке: [Центр обновления Майкрософт для .NET Framework 4.0 в Windows Vista и Windows Server 2008](https://go.microsoft.com/fwlink/?LinkId=198093).  
  
    -   .NET Framework 4. Программа установки устанавливает платформу .NET Framework 4 в кластеризованной операционной системе. Чтобы сократить время установки, перед запуском программы установки рекомендуется установить .NET Framework 4.  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Эти файлы можно установить, запустив файл SqlSupport.msi, который находится на установочном носителе [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] .  
  
-   Убедитесь, что в кластере сервера WSFC не установлены антивирусные программы. Дополнительные сведения см. в статье [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Базы знаний Майкрософт, [Antivirus software may cause problems with cluster services](/troubleshoot/windows-server/high-availability/not-cluster-aware-antivirus-software-cause-issue).  
  
-   В имени кластерной группы при установке отказоустойчивого кластера нельзя использовать следующие символы:  
  
    -   Оператор "меньше" (\<)  
  
    -   Оператор "больше" (>)  
  
    -   двойная кавычка (");  
  
    -   одинарная кавычка (');  
  
    -   Амперсанд (&)  
  
     Убедитесь также, что имена существующих кластерных групп не содержат недопустимых символов.  
  
-   Необходимо, чтобы все узлы кластера имели одинаковую конфигурацию, в т.ч. COM+, буквы разделов диска и пользователей в группе администраторов.  
  
-   Проверьте, чтобы все системные журналы на всех узлах были очищены, и просмотрите их снова. Прежде чем продолжить, убедитесь в том, что в журналах нет сообщений об ошибках.  
  
-   Перед установкой или обновлением отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] отключите все приложения и все службы, которые могут использовать компоненты [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в ходе установки. Дисковые ресурсы необходимо оставить в режиме «в сети».  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] автоматически задает зависимости между кластерной группой [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и дисками, которые будут находиться в отказоустойчивом кластере. Не задавайте зависимости перед запуском программы установки.  
  
    -   При установке отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] создается объект компьютера (учетные записи Active Directory) для имени сетевого ресурса [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . В кластере [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] учетная запись имени кластера (учетная запись компьютера для самого кластера) должна иметь разрешение на создание объектов компьютера. Дополнительные сведения см. в статье [Настройка учетных записей в Active Directory](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx).  
  
    -   Если в качестве файлового хранилища используется общая папка SMB, учетная запись программы установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Setup должна иметь права доступа SeSecurityPrivilege на этом файловом сервере. Для этого с помощью консоли локальной политики безопасности на файловом сервере назначьте учетной записи программы установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] права **Управление журналом аудита и безопасности** .  
  
##  <a name="verify-your-hardware-solution"></a><a name="Hardware"></a> Проверка оборудования  
  
-   Если кластерное решение включает в себя географически распределенные узлы кластеров, необходимо проанализировать также дополнительные элементы, такие как задержка сети и поддержка использования общих дисков.  
  
    -   Дополнительные сведения о [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] и [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]см. в разделах [Проверка аппаратного обеспечения отказоустойчивого кластера](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10)) и [Политика поддержки для отказоустойчивых кластеров Windows](https://go.microsoft.com/fwlink/?LinkId=196818).  
  
-   Убедитесь, что диск, на который будет установлен [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , не является сжатым или зашифрованным диском. При попытке установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на сжатый или зашифрованный диск программа установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] завершится с ошибкой.  
  
-   Конфигурации SAN поддерживаются также в операционных системах [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] и [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] выпусков Advanced Server и Datacenter Edition. В категории «Кластеры и многокластерные устройства» каталога Windows и перечня совместимого оборудования дан перечень устройств хранения, поддерживающих сети SAN, прошедших испытания и поддерживаемых в качестве элементов хранения сетей SAN с подключением нескольких кластеров WSFC. После нахождения необходимых сертифицированных компонентов запустите проверку кластеров.  
  
-   При установке файлов данных также поддерживается файловый ресурс общего доступа SMB. Дополнительные сведения см. в разделе [Storage Types for Data Files](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes).  
  
    > [!WARNING]  
    >  Если в качестве общего хранилища файлового сервера SMB используется файловый сервер Windows, то учетная запись программы установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] должна иметь права доступа SeSecurityPrivilege на этом файловом сервере. Для этого с помощью консоли локальной политики безопасности на файловом сервере назначьте учетной записи программы установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] права **Управление журналом аудита и безопасности** .  
    >   
    >  Если в качестве общего хранилища файлового сервера SMB используется не файловый сервер Windows, то свяжитесь с поставщиком ПО хранилища данных и выясните у него эквивалентные настройки на стороне файлового сервера.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] поддерживает точки подключения.  
  
     Подключенные тома или точки подключения позволяют использовать одну букву диска для ссылки на множество дисков или томов. Если имеется обычный диск или том, обозначенный буквой D:, можно подключить или «смонтировать» дополнительные диски или тома в качестве каталогов этого диска D: при этом присваивать им собственные буквенные обозначения диска не потребуется.  
  
     Ниже приведены дополнительные замечания о точках подключения для отказоустойчивых кластеров [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] требует наличия буквы диска у базового раздела подключенного диска. При установке отказоустойчивых кластеров такой базовый раздел должен быть кластеризованным диском. В этой версии не поддерживаются идентификаторы GUID томов.  
  
    -   Базовый раздел, которому назначена буква диска, не может совместно использоваться экземплярами отказоустойчивого кластера. Это обычное для отказоустойчивых кластеров ограничение не касается, однако, изолированных серверов, на которых размещено несколько экземпляров.  
  
    -   При установке число кластеров [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ограничено количеством имеющихся букв дисков. Если на один раздел диска устанавливается операционная система, а все остальные разделы диска могут использоваться в качестве обычных дисков кластера или дисков кластера, содержащих точки подключения, максимальное число экземпляров [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на один отказоустойчивый кластер составляет 25.  
  
        > [!TIP]  
        >  Ограничение в 25 экземпляров можно обойти за счет использования общей папки SMB. Если в качестве хранилища используется общая папка SMB, то можно установить до 50 экземпляров кластера отработки отказа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    -   Форматирование диска после установки дополнительных дисков не поддерживается.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] поддерживает локальные диски только для установки файлов tempdb. Проверьте правильность пути, указанного для файлов tempdb и файлов журнала на всех узлах кластера. Если во время отработки отказа каталоги tempdb недоступны на целевом узле отработки отказа, то при переводе ресурсов [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в режим «в сети» произойдет ошибка. Дополнительные сведения см. в разделах [Типы хранилищ для файлов данных](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes) и [Настройка компонента Database Engine — каталоги данных](../../../database-engine/install-windows/install-sql-server.md).  
  
-   При развертывании отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на компонентах технологии iSCSI рекомендуется предпринимать соответствующие меры предосторожности. Дополнительные сведения см. в статье [Support for SQL Server on iSCSI technology components](/troubleshoot/sql/admin/support-iscsi-technology-components).  
  
-   Дополнительные сведения см. в разделе [SQL Server support policy for Microsoft Clustering](/troubleshoot/sql/failover-clusters/support-policy-clustered-configurations)(на английском языке).  
  
-   Дополнительные сведения о настройке кворумного диска см. в статье [Quorum Drive Configuration Information](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770620(v=ws.10))(на английском языке).  
  
-   Чтобы установить отказоустойчивый кластер [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] при размещении исходных файлов установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в домене, отличном от самого кластера, скопируйте файлы установки на текущий домен, доступный для отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
##  <a name="review-security-considerations"></a><a name="Security"></a> Ознакомьтесь с соображениями по безопасности  
  
-   Чтобы использовать шифрование, на всех узлах отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] необходимо установить сертификат сервера с полным именем DNS кластера WSFC. Например, при наличии кластера из двух узлов с именами Test1.DomainName.com и Test2.DomainName.com, а также экземпляра отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с именем Virtsql необходимо получить сертификат Virtsql.DomainName.com и установить его на узлы test1 и test2. Затем для настройки шифрования в отказоустойчивом кластере установите в диспетчере конфигурации **флажок** Принудительное шифрование протокола [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    > [!IMPORTANT]  
    >  Не устанавливайте флажок **Принудительное шифрование протокола** до того, как на всех узлах, входящих в экземпляр отказоустойчивого кластера, будут установлены сертификаты.  
  
-   В случае установок [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в конфигурациях параллельно с предыдущими версиями службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] должны использовать только учетные записи, входящие в глобальную группу доменов. Кроме того, учетные записи, которые используют службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , не должны присутствовать в локальной группе администраторов. При несоблюдении этой рекомендации система безопасности может повести себя непредвиденным образом.  
  
-   Чтобы создать отказоустойчивый кластер, необходимо иметь разрешения локального администратора с правом входа в качестве службы и выполнять действия от имени компонента операционной системы на всех экземплярах отказоустойчивого кластера.  
  
-   В [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)]идентификаторы безопасности служб формируются автоматически для использования со службами [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] . Для экземпляров кластеров отработки отказа [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] , обновленных с предыдущих версий [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], сохраняются имеющиеся конфигурации групп доменов и списков управления доступом.  
  
-   Группы домена должны находиться в том же домене, что и учетные записи компьютера. Например, если компьютер, на который устанавливается [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , находится в домене SQLSVR, а его родителем является MYDOMAIN, то необходимо указать группу в домене SQLSVR. Домен SQLSVR может содержать учетные записи пользователей MYDOMAIN.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не устанавливаются в конфигурациях, в которых узлы кластера являются контроллерами домена.  
  
-   Ознакомьтесь с разделом [Security Considerations for a SQL Server Installation](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
-   О включении проверки подлинности Kerberos для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], см. в статье базы [!INCLUDE[msCoName](../../../includes/msconame-md.md)] знаний Майкрософт [ Как использовать проверку подлинности Kerberos в SQL Server](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/319723).  

-   Для экземпляра отказоустойчивого кластера SQL Server требуется, чтобы узлы кластера были присоединены к домену. Следующие конфигурации **не поддерживаются**: 
    *   Экземпляр отказоустойчивого кластера SQL в кластерах рабочей группы. 
    *   Экземпляр отказоустойчивого кластера SQL в кластере с несколькими доменами.   
    *   Экземпляр отказоустойчивого кластера SQL с кластерами рабочей группы. 

  
##  <a name="review-network-port-and-firewall-considerations"></a><a name="Network"></a> Ознакомьтесь с вопросами, связанными с сетями, портами и брандмауэром  
  
-   Перед запуском программы установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] отключите протокол NetBIOS для всех адаптеров частной сети.  
  
-   Сетевое имя и IP-адрес [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не должны использоваться для других целей, например, для совместного использования файлов. Если требуется создать ресурс общей папки, используйте для этого ресурса другое уникальное сетевое имя и IP-адрес.  
  
    > [!IMPORTANT]  
    >  Корпорация Майкрософт рекомендует не размещать общие папки на дисках с данными, поскольку это негативно влияет на поведение и производительность [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Хотя [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] поддерживает в кластерах как именованные каналы, так и сокеты TCP/IP, корпорация Майкрософт рекомендует в кластеризованных конфигурациях использовать сокеты TCP/IP.  
  
-   Обратите внимание, что ISA Server не поддерживается службой кластеров Windows и, следовательно, не поддерживается в отказоустойчивых кластерах [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Служба удаленного реестра должна быть запущена.  
  
-   Удаленное администрирование должно быть разрешено.  
  
- Для экземпляров SQL Server, использующих порт, отличный от порта по умолчанию, используйте конфигурацию сети в диспетчере конфигурации SQL Server, чтобы определить порт, используемый экземпляром SQL Server, который нужно разблокировать. Включите TCP-порт для IPALL в брандмауэре, если хотите подключиться к экземпляру SQL Server с помощью [службы обозревателя SQL Server](../../../tools/configuration-manager/sql-server-browser-service.md), которая использует IP-адрес, отличный от того, на котором установлен кластеризованный экземпляр, и порт UDP 1434. 
  
-   К числу операций по установке отказоустойчивого кластера относится правило, которое проверяет порядок привязки к сети. Даже в случаях, когда порядок привязки выглядит правильным, в системе могут оказаться отключенные или фантомные конфигурации сетевых адаптеров. Фантомные конфигурации сетевых адаптеров могут повлиять на порядок привязки, и в результате правило порядка привязки выдаст предупреждение. Чтобы избежать такой ситуации, выполните следующие действия для обнаружения и удаления отключенных сетевых адаптеров.  
  
    1.  В командной строке введите: set devmgr_Show_Nonpersistent_Devices=1.  
  
    2.  Введите и запустите команду: start Devmgmt.msc.  
  
    3.  Разверните список сетевых адаптеров. В список должны входить только физические адаптеры. Если присутствует отключенный сетевой адаптер, программа установки сообщит об ошибке в правиле порядка привязки к сети. В окне «Сетевые подключения» на панели управления также будет показано, что адаптер отключен. Убедитесь, что в окне «Сетевые подключения» на панели управления выводится тот же список включенных физических адаптеров, что и в средстве devmgmt.msc.  
  
    4.  Удалите отключенные сетевые адаптеры перед запуском программы установки SQL Server.  
  
    5.  После завершения установки вернитесь к окну «Сетевые подключения» на панели управления и отключите неиспользуемые сетевые адаптеры.  
  
##  <a name="verify-your-operating-system"></a><a name="OS_Support"></a> Проверьте операционную систему  
 Убедитесь в том, что операционная система установлена должным образом и поддерживает отказоустойчивые кластеры. В следующей таблице приведен список выпусков [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и операционных систем, которые их поддерживают.  
  
|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] edition|[!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] Enterprise|[!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] Datacenter Server|[!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] Enterprise|[!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] Datacenter Server|  
|---------------------------------------|------------------------------------------------|-------------------------------------------------------|----------------------------------------------|-----------------------------------------------------|  
|[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Enterprise x64 (64-разрядная)|Да|Да|Да**|Да**|  
|[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Enterprise (32-разрядная)|Да|Да|||  
|[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]-разрядная) Developer (64|Да|Да|Да**|Да**|  
|[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] Developer (32-разрядная)|Да|Да|||  
|[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] Standard (64-разрядная)|Да|Да|Да|Да|  
|[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] Standard (32-разрядная)|Да|Да|||  
  
 *[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не поддерживаются в режиме WOW. Это также относится к обновлению с предыдущих версий отказоустойчивых кластеров [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , первоначально установленных в режиме WOW. В этих случаях единственная возможность обновления состоит в установке в той же среде новой версии и проведении миграции.  
  
 **Поддерживается при отказоустойчивой кластеризации с использованием нескольких подсетей [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
##  <a name="additional-considerations-for-multi-subnet-configurations"></a><a name="MultiSubnet"></a> Дополнительные соображения по конфигурациям отказоустойчивого кластера с несколькими подсетями  
 В приведенных далее разделах описываются требования, которые необходимо учитывать при установке отказоустойчивого кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с несколькими подсетями. При реализации конфигурации с несколькими подсетями кластеризация охватывает несколько подсетей, в связи с чем используются несколько IP-адресов и происходит изменение зависимостей ресурсов IP-адресов.  
  
### <a name="ssnoversion-edition-and-operating-system-considerations"></a>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и операционным системам  
  
-   Сведения о выпусках [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , которые поддерживают отказоустойчивый кластер [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с несколькими подсетями, см. в разделе [Функции, поддерживаемые различными выпусками SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
-   Чтобы создать кластер отработки отказа [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с несколькими подсетями, необходимо сначала создать отказоустойчивый кластер [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] на нескольких объектах и в нескольких подсетях.  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] использует отказоустойчивый кластер Windows Server для обеспечения допустимости условий зависимости IP-адресов при отработке отказа.  
  
-   [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] требует, чтобы все серверы кластера находились в одном домене Active Directory. Поэтому отказоустойчивый кластер [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] с несколькими подсетями требует, чтобы все узлы отказоустойчивого кластера находились в одном и том же домене Active Directory, даже если они все находятся в разных подсетях.  
  
#### <a name="ip-address-and-ip-address-resource-dependencies"></a>IP-адреса и зависимости ресурсов IP-адресов  
  
1.  В конфигурации с несколькими подсетями для зависимости ресурса "IP-адрес" задается значение OR. Дополнительные сведения см. на странице [Создание нового отказоустойчивого кластера SQL Server (программа установки)](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md).  
  
2.  Смешанные конфигурации зависимостей IP-адресов со значением AND-OR не поддерживаются. Например, \<IP1> И \<IP2> ИЛИ \<IP3> не поддерживается.  
  
3.  Также не поддерживается несколько IP-адресов в одной в подсети.  
  
     При использовании нескольких IP-адресов в одной подсети во время запуска [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] клиенты могут сталкиваться со сбоями соединений.  
  
#### <a name="related-content"></a>См. также  
 Дополнительные сведения об отказоустойчивых кластерах на платформе [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] на нескольких сайтах см. в статьях [Сайт кластеризации отработки отказа на платформе Windows Server 2008 R2](https://technet.microsoft.com/library/ff182338\(v=WS.10\).aspx) и [Проектирование службы, поддерживающей работу в кластере, или приложения в пределах кластера отработки отказа, размещенного на нескольких сайтах](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197430(v=ws.10)).  
  
##  <a name="configure-windows-server-failover-cluster"></a><a name="WSFC"></a> Настройка отказоустойчивого кластера Windows Server  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Служба кластеров (Майкрософт) (WSFC) должна быть настроена как минимум на одном узле серверного кластера. Кроме того, совместно со службой WSFC должен работать выпуск [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Enterprise, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Business Intelligence или [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Standard. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Enterprise поддерживает кластеры отработки отказа с числом узлов до 16. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Выпуски Business Intelligence и [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Standard поддерживают отказоустойчивые кластеры, состоящие из двух узлов.  
  
-   Библиотека ресурсов DLL для службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] экспортирует две функции, которые используются диспетчером кластеров WSFC для проверки доступности ресурса [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в разделе [Политика отработки отказа для экземпляров отказоустойчивого кластера](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md).  
  
-   Служба WSFC должна иметь возможность проверять состояние экземпляра отказоустойчивого кластера с помощью проверки IsAlive. Для этого необходимо установить доверительное соединение с сервером. По умолчанию учетная запись, с которой работает служба кластеров, не является учетной записью администратора на всех узлах в кластере, кроме того, группа BUILTIN\Администраторы не имеет разрешение для входа на [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Эти параметры изменяются только в случае изменения разрешений на узлах кластера.  
  
-   Настройте службы DNS или WINS. В среде, где будет устанавливаться отказоустойчивый кластер [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , должен быть запущен DNS-сервер или WINS-сервер. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] для виртуальной ссылки на IP-интерфейс [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] необходима зарегистрированная служба динамических доменных имен. Конфигурация DNS-сервера должна позволять узлам кластера динамически регистрировать привязку оперативного IP-адреса к сетевому имени. Если динамическая регистрация не может быть завершена, программа установки выдает сообщение об ошибке и выполняет откат установки. Дополнительные сведения см. в [этой статье базы знаний Майкрософт](https://support.microsoft.com/kb/947048).  
  
##  <a name="install-msconame-distributed-transaction-coordinator"></a><a name="MSDTC"></a> Установить координатор распределенных транзакций [!INCLUDE[msCoName](../../../includes/msconame-md.md)]  
 Перед установкой [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в отказоустойчивом кластере определите, есть ли необходимость создания кластерного ресурса координатора распределенных транзакций ( [!INCLUDE[msCoName](../../../includes/msconame-md.md)] ) (MSDTC). Если устанавливается только компонент [!INCLUDE[ssDE](../../../includes/ssde-md.md)], кластерный ресурс MSDTC не требуется. Если устанавливается компонент [!INCLUDE[ssDE](../../../includes/ssde-md.md)] и службы SSIS, компоненты рабочей станции или если планируется использовать распределенные транзакции, необходимо установить MSDTC. Обратите внимание, что MSDTC не требуется для экземпляров только со службами [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
 На платформах [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] и [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]в одном кластере отработки отказа можно установить несколько экземпляров MSDTC. Первым установленным экземпляром MSDTC будет экземпляр MSDTC по умолчанию для кластера. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] будет автоматически использовать экземпляр MSDTC, установленный в локальной группе ресурсов кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Однако отдельные приложения можно сопоставить с любым экземпляром MSDTC в кластере.  
  
 Для экземпляра MSDTC, выбираемого [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], применяются следующие правила.  
  
-   Использовать MSDTC, установленный в локальной группе, или  
  
-   Использовать сопоставленный экземпляр MSDTC или  
  
-   Использовать экземпляр MSDTC по умолчанию для данного кластера или  
  
-   Использовать экземпляр MSDTC, установленный на локальном компьютере  
  
> [!IMPORTANT]  
>  В случае сбоя экземпляра MSDTC, установленного в локальной группе кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не будет автоматически пытаться использовать экземпляр MSDTC по умолчанию для кластера или экземпляр MSDTC, установленный на локальном компьютере. Чтобы начать использовать другой экземпляр MSDTC, потребуется полностью удалить сбойный экземпляр MSDTC из группы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Точно так же, как при создании сопоставления для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и сбоя сопоставленного экземпляра MSDTC, в распределенных транзакциях произойдет сбой. Если потребуется, чтобы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] использовал другой экземпляр MSDTC, необходимо либо добавить другой экземпляр MSDTC в локальную группу кластера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] или удалить сопоставление.  
  
### <a name="configure-msconame-distributed-transaction-coordinator"></a>Настройте координатор распределенных транзакций ( [!INCLUDE[msCoName](../../../includes/msconame-md.md)] )  
 После установки операционной системы и настройки кластера необходимо настроить координатор MSDTC для работы в кластере с помощью администратора кластера. Если MSDTC не будет настроен для работы в кластере, это не помешает установке [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , но может ограничить функциональные возможности приложений [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>См. также:  
 [Требования к оборудованию и программному обеспечению для установки SQL Server 2016](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [Параметры для средства проверки конфигурации системы](../../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)   
 [Администрирование и обслуживание экземпляров отказоустойчивого кластера](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)  
  

---
title: Партнеры в области высокой доступности и аварийного восстановления SQL Server
description: Список сторонних партнеров, предлагающих решения для обеспечения высокого уровня доступности и аварийного восстановления служб SQL Server.
ms.topic: conceptual
ms.custom: seo-dt-2019
ms.date: 09/17/2017
ms.prod: sql
ms.technology: release-landing
ms.author: mikeray
author: MikeRayMSFT
ms.openlocfilehash: 7c6a58d336de824aaad46b9e67d4f491fe96fc37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339360"
---
# <a name="sql-server-high-availability-and-disaster-recovery-partners"></a>Партнеры в области высокой доступности и аварийного восстановления SQL Server
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
В отрасли доступно множество передовых средств, позволяющих обеспечить высокую доступность и аварийное восстановление для служб SQL Server.  В этой статье указаны партнеры корпорации Майкрософт, которые предлагают решения по обеспечению высокой доступности и аварийного восстановления, поддерживающие Microsoft SQL Server.

## <a name="high-availability-and-disaster-recovery-partners"></a>Партнеры в области высокой доступности и аварийного восстановления

| Партнер | Описание | Ссылки | 
| --- | --- | --- |
|![Azure][5] |**Azure Site Recovery**<br>Site Recovery реплицирует рабочие нагрузки, выполняемые на виртуальных машинах или физических серверах, чтобы они оставались доступными во вторичном расположении, когда первичный сайт недоступен. Вы можете выполнять репликацию и переход на другой ресурс для виртуальных машин SQL Server из локального центра обработки данных в Azure или другой локальный центр обработки данных, а также из одного центра обработки данных Azure в другой.<br><br> Выпуски Enterprise и Standard продуктов SQL Server 2008 R2–SQL Server 2016|[Веб-сайт][azure_website]<br>[Marketplace][azure_marketplace]<br>[Таблица данных][azure_datasheet]<br>[Twitter][azure_twitter]<br>[Видео][azure_youtube]|
|![DH2i][2] |**DH2i**<br>DxEnterprise — это ПО для обеспечения смарт-доступности для Windows, Linux и Docker, которое помогает добиться почти нулевого запланированного и незапланированного простоя, позволяет реализовать огромный потенциал экономии затрат, значительно упрощает управление и обеспечивает как физическую, так и логическую консолидацию.<br><br>SQL Server 2005+, Windows Server 2008R2+, Ubuntu 16+, RHEL 7+, CentOS 7+|[Веб-сайт][dh2i_website]<br>[Таблица данных][dh2i_datasheet]<br>[Twitter][dh2i_twitter]<br>[Видео][dh2i_youtube]|
|![Логотип HPE][4] |**HPE Serviceguard**<br>HPE Serviceguard for Linux (SGLX) помогает вам защитить критические рабочие нагрузки SQL Server 2017 на базе Linux&reg; от незапланированных и запланированных простоев, вызванных множеством ошибок инфраструктуры и приложений в физических и виртуальных средах любой удаленности. HPE SGLX A.12.20.00 и более поздних версий предлагает варианты контекстного мониторинга и восстановления для экземпляра отказоустойчивого кластера и рабочих нагрузок групп доступности Always On SQL Server. С помощью HPE SGLX вы можете обеспечить максимальное время доступности, не жертвуя целостностью данных и производительностью.<br><br>SQL Server 2017 на базе Linux — RedHat 7.3, 7.4, SUSE 12 с пакетами обновлений 2 (SP2) и 3 (SP3)|[Веб-сайт][hpe_website]<br>[Таблица данных][hpe]<br>[Скачать ознакомительную версию][hpe_download]<br>[Блог][hpe_download]<br>[Twitter][hpe_twitter]
|![IDERA][3]|**IDERA**<br>SQL Safe Backup — это высокопроизводительное решение резервного копирования и восстановления для SQL Server, которое экономит средства за счет уменьшения времени резервного копирования базы данных и размера получаемых при этом файлов, а также предоставления мгновенного доступа на чтение и запись к базам данных в резервных копиях.<br><br>Microsoft SQL Server: 2005 с пакетом обновления 1 SP1) или более поздней версии, 2008, 2008 R2, 2012, 2014, 2016; все выпуски |[Веб-сайт][idera_website]|
|![NEC][7]|**NEC**<br>ExpressCluster — это полнофункциональное и полностью автоматизированное решение для обеспечения высокого уровня доступности и аварийного восстановления. Решение справляется со всеми основными типами сбоев, включая сбои оборудования, программного обеспечения, сети и сайта и предназначено для SQL Server и сопутствующих приложений, работающих на компьютерах или виртуальных машинах в локальных или облачных средах.<br><br>Microsoft SQL Server: 2005 или более поздней версии; все выпуски |[Веб-сайт][necec_website]<br>[Таблица данных][necec_datasheet]<br>[Видео][necec_youtube]<br>[Загрузить][necec_download]|
|![Portworx][6] |**Portworx**<br>Portworx — это решение для использования контейнеров с отслеживанием состояния в рабочей среде. С помощью Portworx пользователи могут управлять любой базой данных или службой с отслеживанием состояния в любой инфраструктуре, используя любой планировщик контейнеров, включая Kubernetes, Mesosphere DC/OS и Docker Swarm. Portworx решает пять основных проблем, с которыми команды DevOps сталкиваются при использовании контейнерных баз данных и других служб с отслеживанием состояния в рабочей среде: сохраняемость, высокая доступность, автоматизация данных, поддержка нескольких хранилищ данных и соответствующей инфраструктуры, а также безопасность.<br><br>SQL Server 2017 с Docker |[Веб-сайт][portworx_website]<br>[Документация][portworx_docs]<br>[Видео][portworx_youtube]|
|![SIOS][8] |**SIOS**<br>SIOS Technology предлагает экономичные решения для обеспечения высокой доступности и аварийного восстановления SQL Server в ОС Windows и Linux. Работающие без сети SAN кластеры SIOS не требуют наличия общедоступного хранилища в сети SAN, тем самым предоставляя максимальную гибкость в защите наиболее важных приложений в физических, виртуальных, облачных и гибридно-облачных конфигурациях в средах с одной и несколькими сетями.<br><br>Добавив SIOS DataKeeper в среду отказоустойчивой кластеризации Windows Server, вы создадите ресурс тома без сети SAN, который заменяет традиционное общедоступное хранилище и позволяет легко развернуть отказоустойчивый кластер Windows Server в Azure.<br><br>Комплект защиты SIOS — это максимально гибкое решение для кластеризации, которое защищает критически важные приложения Linux, в частности SQL Server, SAP, HANA, Oracle и многие другие.|[Веб-сайт][sios_website]<br>[Таблица данных][sios_datasheet]<br>[Twitter][sios_twitter]<br>[Marketplace][sios_marketplace]<br>[Видео][sios_youtube]|
|![Veeam][1] |**Veeam**<br>Veeam Backup & Replication — это производительное, недорогое и удобное решение для резервного копирования и обеспечения доступности. Оно предлагает быстрое, гибкое и надежное восстановление виртуализованных приложений и данных, объединяя функции резервного копирования и репликации виртуальных машин. Решение обладает отмеченной наградами поддержкой виртуальных сред VMware vSphere и Microsoft Hyper-V.<br><br>SQL Server 2005 с пакетом обновления 4 (SP4) — SQL Server 2016 на Windows |[Веб-сайт][veeam_website]<br>[Таблица данных][veeam_datasheet]<br>[Twitter][veeam_twitter]<br>[Видео][veeam_youtube]|

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о других партнерах см. в статьях, посвященных [мониторингу][mon_partners], [партнерам по управлению][management_partners] и [партнерам по разработке][dev_partners].

<!--Image references-->
[1]: ./media/partner-hadr-sql-server/Veeam-green-logo.png
[2]: ./media/partner-hadr-sql-server/dh2i-logo.png
[3]: ./media/partner-hadr-sql-server/idera-logo.png
[4]: ./media/partner-hadr-sql-server/hpe.png
[5]: ./media/partner-hadr-sql-server/azure-logo.png
[6]: ./media/partner-hadr-sql-server/portworx-logo.png
[7]: ./media/partner-hadr-sql-server/nec-logo.png
[8]: ./media/partner-hadr-sql-server/sios-logo.png

<!--Article links-->
[mon_partners]: ./partner-monitor-sql-server.md
[management_partners]: ./partner-management-sql-server.md
[dev_partners]: ./partner-dev-sql-server.md

<!--Website links -->
[veeam_website]:https://www.veeam.com/
[dh2i_website]:https://dh2i.com
[idera_website]:https://www.idera.com/productssolutions/sqlserver
[hpe_website]: https://www.hpe.com/us/en/product-catalog/detail/pip.376220.html
[azure_website]: /azure/site-recovery/site-recovery-sql
[necec_website]: https://www.necam.com/ExpressCluster/
[portworx_website]: https://portworx.com/
[sios_website]: https://us.sios.com/

<!--Get Started Links-->

<!--Datasheet Links-->
[veeam_datasheet]:https://www.veeam.com/veeam_backup_9_5_datasheet_en_ds.pdf
[dh2i_datasheet]:https://dh2i.com/wp-content/uploads/DxE-Win-QuickFacts.pdf
[hpe]:https://www.hpe.com/h20195/v2/default.aspx?cc=us&lc=en&oid=376220
[necec_datasheet]: https://www.necam.com/docs/?id=0d9ef7a7-f935-4909-b6bb-20a47b3
[azure_datasheet]: /azure/site-recovery/vmware-physical-azure-support-matrix
[sios_datasheet]: https://us.sios.com/solutions/high-availability-cluster-software-cloud/

<!--Marketplace Links -->
[azure_marketplace]: https://azuremarketplace.microsoft.com/marketplace/apps?search=site%20recovery&page=1
[sios_marketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/sios_datakeeper.sios-datakeeper-8
<!--Press links-->
<!--[veeam_press]:-->

<!--YouTube links-->
[veeam_youtube]:https://www.youtube.com/user/YouVeeam
[dh2i_youtube]:https://www.youtube.com/user/dh2icompany 
[idera_youtube]:https://www.idera.com/resourcecentral/videos/sql-safe-overview
[azure_youtube]: https://mva.microsoft.com/en-US/training-courses/is-your-lack-of-a-disaster-recovery-site-keeping-you-up-at-night-8680?l=oF7YrFH1_7504984382
[necec_youtube]: https://www.youtube.com/watch?v=9La3Cw1Q1Jk
[portworx_youtube]: https://www.youtube.com/channel/UCSexpvQ9esSRgiS_Q9_3mLQ
[sios_youtube]: https://www.youtube.com/watch?v=U3M44gJNWQE

<!--Twitter links-->
[veeam_twitter]:https://twitter.com/veeam
[dh2i_twitter]:https://twitter.com/dh2i
[hpe_twitter]:https://twitter.com/hpe
[azure_twitter]:https://twitter.com/hashtag/azuresiterecovery
[sios_twitter]:https://www.twitter.com/SIOSTech

<!--Docs links>-->
[portworx_docs]: https://docs.portworx.com/

<!--Download links-->
[hpe_download]: https://h20392.www2.hpe.com/portal/swdepot/displayProductInfo.do?productNumber=SGLX-DEMO
[necec_download]: https://www.necam.com/ExpressCluster/30daytrial/
<!--Blog links-->
[hpe_blog]: https://community.hpe.com/t5/Servers-The-Right-Compute/SQL-Server-for-Linux-Is-Here-and-A-New-Chapter-for-Mission/ba-p/6977571#.WiHWW0xFwUE
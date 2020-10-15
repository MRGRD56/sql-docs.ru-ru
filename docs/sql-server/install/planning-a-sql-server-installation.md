---
title: Планирование установки SQL Server | Документация Майкрософт
description: Эта статья поможет спланировать установку SQL Server. Она содержит ссылки на ресурсы, необходимые для установки SQL Server.
ms.custom: ''
ms.date: 08/23/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: quickstart
helpviewer_keywords:
- installing SQL Server, planning
ms.assetid: b1d56f2f-603f-48f2-b902-c715f14a6db9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0197c453f2eaeda872eac7ec639fdb56eb7545ce
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2020
ms.locfileid: "91987110"
---
# <a name="planning-a-sql-server-installation"></a>Планирование установки SQL Server
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Чтобы установить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], выполните описанные далее действия.  
  
-   Ознакомьтесь с требованиями по установке, сведениями о проверках конфигурации системы и вопросами безопасности для установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Запуск программы установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для установки или обновления до последней версии. Перед обновлением ознакомьтесь со статьей [Обновление SQL Server](../../database-engine/install-windows/upgrade-sql-server.md).  
  
-   Настройка [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при помощи программ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Независимо от метода установки, необходимо подтвердить свое согласие с условиями лицензии на использование пакета программ как физического лица или от имени организации, если на используемое программное обеспечение не распространяется отдельное соглашение [!INCLUDE[msCoName](../../includes/msconame-md.md)] , такое как соглашение о корпоративном лицензировании Майкрософт или отдельное соглашение с независимым поставщиком программного обеспечения или изготовителем оборудования (OEM).  
  
 Условия лицензионного соглашения отображаются для ознакомления и принятия в пользовательском интерфейсе программы установки. Автоматические установки (с использованием параметров `/Q` или `/QS`) должны включать параметр `/IAcceptSQLServerLicenseTerms`. Скачайте лицензию и ознакомьтесь с ее условиями в разделе [Условия лицензии и информация по Microsoft SQL Server](https://www.microsoft.com/Licensing/product-licensing/sql-server.aspx). Условия корпоративного лицензирования см. в разделе [Условия лицензии и документация](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=53). Для более старых версий SQL Server см. раздел [Условия лицензионного соглашения на использование программного обеспечения корпорации Майкрософт](https://go.microsoft.com/fwlink/?LinkID=148209).  
  
> [!NOTE]  
>  В зависимости от способа получения ПО (например, по [!INCLUDE[msCoName](../../includes/msconame-md.md)] ), на его использование могут распространяться дополнительные условия.  
  
## <a name="in-this-section"></a>в этом разделе  
 [Новые возможности установки SQL Server](../../sql-server/install/what-s-new-in-sql-server-installation.md)  
 В этой статье подробно рассматриваются новые и усовершенствованные возможности установки в данной версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Требования к оборудованию и программному обеспечению для установки [SQL Server 2016 и 2017](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md), [SQL Server 2019](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) или [SQL Server на Linux](../../linux/sql-server-linux-setup.md) В этой статье перечислены минимальные требования к оборудованию и программному обеспечению для установки и запуска экземпляра SQL Server. .  
  
 [Вопросы безопасности при установке SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
 В этой статье приведены некоторые рекомендации по безопасности, которых следует придерживаться как до установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], так и после установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Настройка учетных записей службы Windows и разрешений](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  
 В этой статье описана конфигурация по умолчанию служб данного выпуска [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а также параметры конфигурации служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которые можно настроить во время и после установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Сетевые протоколы и библиотеки](../../sql-server/install/network-protocols-and-network-libraries.md)  
 В этой статье описана конфигурация сетевых протоколов, устанавливаемая по умолчанию для служб данной версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], а также доступные для настройки параметры.  
  
 [Работа с несколькими версиями и экземплярами SQL Server](../../sql-server/install/work-with-multiple-versions-and-instances-of-sql-server.md)  
 В этой статье описаны рекомендации по установке нескольких версий и экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Версии SQL Server на местных языках](../../sql-server/install/local-language-versions-in-sql-server.md)  
 В этой статье рассматриваются локализованные версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="related-sections"></a>См. также  
 [Установка SQL Server](../../database-engine/install-windows/install-sql-server.md)  
 В этом разделе представлены общие сведения о разных параметрах установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Установка компонентов бизнес-аналитики SQL Server](../../sql-server/install/install-sql-server-business-intelligence-features.md)  
 В этом разделе документации по программе установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] описывается процедура установки функций [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которые входят в состав платформы бизнес-аналитики Майкрософт.  
  
 [Обновление SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
 В данном разделе представлена общая информация по обновлению экземпляров предыдущих версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 [Удаление SQL Server](../../sql-server/install/uninstall-sql-server.md)  
 В этом разделе описано, как полностью удалить существующий экземпляр [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] и подготовить систему к повторной установке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [Установка отказоустойчивого кластера SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)  
 В этом разделе документации по программе установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] описывается процедура установки и настройки кластера отработки отказа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="see-also"></a>См. также:  
 [Установка SQL Server из командной строки](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)   
 [Решения высокого уровня доступности (SQL Server)](../../database-engine/sql-server-business-continuity-dr.md)   
 [Подготовка к установке отказоустойчивого кластера](../../sql-server/failover-clusters/install/before-installing-failover-clustering.md)   
 [Обновление SQL Server с помощью мастера установки (программа установки)](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
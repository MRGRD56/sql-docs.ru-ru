---
title: Настройка контактной зоны | Документация Майкрософт
description: Узнайте, как изменить параметры по умолчанию для установки SQL Server и выборочно включать или отключать функции работающего экземпляра SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- reducing attackable surface area
- upgrading SQL Server, security
- surface area configuration [SQL Server]
- surface area configuration [SQL Server], about surface area configuration
- attackable surface area [SQL Server]
- installing SQL Server, security
ms.assetid: f741169c-1453-4ad2-830b-bf2be27d712f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f439f07e8bdc374e5457c946e3b57120b0c6b0c8
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91869155"
---
# <a name="surface-area-configuration"></a>Настройка контактной зоны
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  В конфигурации по умолчанию для новых установок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]многие из функций отключены. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] выборочно устанавливает и запускает только ключевые службы и функции, чтобы свести к минимуму количество функций, которые могут подвергнуться атаке злоумышленника. Системный администратор может изменить эти значения по умолчанию в ходе установки, а также включать или отключать функции работающего экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]по своему выбору. Кроме того, при подключении с других компьютеров определенные компоненты могут быть недоступны до настройки протоколов.  
  
> [!NOTE]  
>  В отличие от новых установок, во время обновления существующие службы или функции не отключаются, но после его завершения могут быть применены дополнительные параметры конфигурации контактной зоны.  
  
## <a name="protocols-connection-and-startup-options"></a>Протоколы, соединение и параметры запуска  
 С помощью диспетчера конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно запускать и останавливать службы, настраивать параметры запуска и включать протоколы и другие параметры соединения.  
  
#### <a name="to-start-sql-server-configuration-manager"></a>Запуск диспетчера конфигурации SQL Server  
  
1.  В меню **Пуск** последовательно выберите пункты **Все программы**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Средства настройки**и щелкните **Диспетчер конфигурации SQL Server**.  
  
    -   Для запуска компонентов и настройки автоматического запуска используйте область **Службы SQL Server** диспетчера конфигурации.  
  
    -   Используйте область **Сетевая конфигурация SQL Server** для включения протоколов и параметров соединения, таких как фиксированные порты TCP/IP или принудительное шифрование.  
  
 Дополнительные сведения см. в разделе [SQL Server Configuration Manager](../../relational-databases/sql-server-configuration-manager.md). Возможность удаленного подключения также зависит от правильной настройки брандмауэра. Дополнительные сведения см. в статье [Настройка брандмауэра Windows для разрешения доступа к SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
## <a name="enabling-and-disabling-features"></a>Включение и отключение функций  
 Включение и отключение функций [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно настроить с помощью аспектов в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
#### <a name="to-configure-surface-area-using-facets"></a>Настройка контактной зоны с помощью аспектов  
  
1.  В [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] выполните соединение с компонентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  В обозревателе объектов щелкните правой кнопкой мыши сервер и выберите пункт **Аспекты**.  
  
3.  В диалоговом окне **Просмотр аспектов** разверните список **Аспекты** , выберите соответствующий аспект **Настройка контактной зоны** (**Настройка контактной зоны**, **Настройка контактной зоны для служб Analysis Services**или **Настройка контактной зоны для служб Reporting Services**).  
  
4.  В области **Свойства аспектов** выберите необходимые значения для всех свойств.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 Для периодической проверки конфигурации аспекта используйте управление на основе политик. Дополнительные сведения об управлении на основе политик см. в статье [Администрирование серверов с помощью управления на основе политик](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md).  
  
 Также можно задать параметры компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] при помощи хранимой процедуры **sp_configure** . Дополнительные сведения см. в разделе [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
 Чтобы изменить свойство **Включить встроенную безопасность**[!INCLUDE[ssRS](../../includes/ssrs.md)], используйте настройки свойств в службах [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Чтобы изменить свойство **Планирование событий и доставка отчетов** и свойство **Доступ к веб-службам и HTTP** , измените файл конфигурации **RSReportServer.config** .  
  
## <a name="command-prompt-options"></a>Параметры командной строки  
 Используйте командлет PowerShell **Invoke-PolicyEvaluation**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для вызова политик конфигурации контактной зоны. Дополнительные сведения см. в разделе [Использование командлетов компонента Database Engine](../../powershell/sql-server-powershell.md).  
  
## <a name="soap-and-service-broker-endpoints"></a>Конечные точки SOAP и Service Broker  
 Для отключения конечных точек используйте управление на основе политик. Для создания и изменения свойств конечных точек используйте инструкции [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md) и [ALTER ENDPOINT (Transact-SQL)](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
## <a name="related-content"></a>См. также  
 [Центр безопасности для ядра СУБД SQL Server и Базы данных Azure SQL](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
  
 [sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  

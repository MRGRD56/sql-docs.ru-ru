---
title: Варианты окончания поддержки
description: Узнайте о различных опциях, доступных для продуктов SQL Server, поддержка которых подошла к концу, таких как SQL Server 2005, SQL Server 2008 и SQL Server 2008 R2.
ms.date: 08/12/2020
ms.prod: sql
ms.technology: install
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.reviewer: pmasl
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c7560fbda04ef5a14c73ead8a7bbc8337e3e38ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352900"
---
# <a name="sql-server-end-of-support-options"></a>Варианты окончания поддержки SQL Server 
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

В этой статье объясняются варианты решения проблем с продуктами SQL Server, которые достигли конца поддержки.

## <a name="understanding-the-sql-server-lifecycle"></a>Основные сведения о жизненном цикле SQL Server

Каждая версия [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживается не менее 10 лет, это — пять лет основной поддержки и пять лет расширенной поддержки.
-  **Основная фаза поддержки** включает в себя обновления функций, производительности, масштабируемости и безопасности. 
-  **Расширенная поддержка** включает только обновления для системы безопасности. 

**Окончание поддержки** (также иногда называемое окончанием срока службы) указывает на то, что продукт достиг конца своего жизненного цикла, а его обслуживание и поддержка больше недоступны. Дополнительные сведения о жизненном цикле Майкрософт см. в разделе [Политика жизненного цикла поддержки Майкрософт](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy).



## <a name="options"></a>Параметры

Как только ваш [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] достигнет конца этапа поддержки, вы можете выбирать один из приведенных ниже вариантов.
- Обновление до текущей версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Приобретение [подписки на дополнительные обновления для системы безопасности](https://www.microsoft.com/cloud-platform/extended-security-updates). 
- Перенос рабочей нагрузки на виртуальную машину Azure для получения [дополнительных обновлений для системы безопасности](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support).
- Перенос рабочей нагрузки на службу [Базы данных SQL Azure](/azure/sql-database/sql-database-paas-vs-sql-server-iaas). 

Дополнительные сведения, инструкции и средства для планирования и автоматизации обновления или миграции см. в разделах [Прекращение поддержки SQL Server 2005](https://www.microsoft.com/sql-server/sql-server-2005) и [Прекращение поддержки SQL Server 2008](https://www.microsoft.com/cloud-platform/windows-sql-server-2008).  

![Варианты окончания поддержки](media/sql-server-end-of-life-overview/sql-server-upgrade-paths.png)

В этой статье описываются преимущества и рекомендации для каждого подхода, а также дополнительные ресурсы, которые могут помочь в принятии решения.

## <a name="upgrade-sql-server"></a>Обновление SQL Server

После окончания этапа поддержки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно перейти на более новую и поддерживаемую версию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это обеспечивает стабильность среды, позволяет использовать новейший набор функций и принимает новый жизненный цикл поддержки версии.

### <a name="benefits"></a>Преимущества
- **Новейшие технологии**. В новых версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] введены инновации, включающие лучшую производительность, масштабируемость и высокую доступность, а также улучшенную безопасность. 
- **Управление**. У вас есть полный контроль над возможностями и масштабируемостью, так как вы управляете и оборудованием, и программным обеспечением.
- **Знакомая среда**. Эта среда наиболее близка к среде более старой версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- **Широкая область применения**. Применимо для приложений баз данных любого типа, включая OLTP-системы и хранилища данных.
- **Низкая степень риска для приложений баз данных**. Поддерживая совместимость базы данных на том же уровне, что и старая система, существующие приложения базы данных защищены от функциональных изменений и изменений производительности, которые могут иметь негативные последствия. Приложение следует повторно сертифицировать, только если необходимо использовать функции, которые являются производными от более новых параметров совместимости базы данных. Дополнительные сведения см. в статье [Сертификация на совместимость](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Рекомендации

- **Затраты** — этот подход требует наибольшего уровня инвестирования и наиболее актуального управления. Необходимо покупать и обслуживать собственное оборудование и программное обеспечение, а также управлять им.
- **Простой** — в зависимости от стратегии обновления возможны простои. Существует также риск возникновения проблем во время процесса обновления на месте.
- **Сложность** — если вы используете Windows Server 2008 или [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)], вам также потребуется обновить операционную систему, так как новые версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] могут не поддерживаться в этих версиях Windows. В процессе обновления ОС добавляется риск, поэтому выполнение параллельной миграции может быть более разумным, но более дорогостоящим подходом. Обновления ОС на месте не поддерживаются в экземплярах отказоустойчивого кластера для Windows Server 2008 или [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)]. 

  > [!NOTE]
  > Последовательные обновления ОС кластера доступны начиная с Windows Server версии 2016.

### <a name="resources"></a>Ресурсы

[Установочный носитель](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-rtm)   
[Обновление SQL Server с помощью мастера установки](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)

Новые возможности в:
- [SQL Server 2016](../what-s-new-in-sql-server-2016.md)
- [SQL Server 2017](../what-s-new-in-sql-server-2017.md) 
- [SQL Server 2019](../what-s-new-in-sql-server-ver15.md)   

Требования к оборудованию:
- [SQL Server 2017 и более ранние версии](../install/hardware-and-software-requirements-for-installing-sql-server.md)  
- [SQL Server 2019](../install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)    

Поддерживаемые обновления версий и выпусков:
- [SQL Server 2016](../../database-engine/install-windows/supported-version-and-edition-upgrades.md?view=sql-server-2016&preserve-view=true) 
- [SQL Server 2017](../../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md)
- [SQL Server 2019](../../database-engine/install-windows/supported-version-and-edition-upgrades-version-15.md)

Инструменты:
-  [Database Experimentation Assistant](../../dea/database-experimentation-assistant-overview.md) может помочь оценить целевую версию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для конкретной рабочей нагрузки. 
-  [Помощник по миграции данных](../../dma/dma-overview.md) может помочь обнаружить проблемы совместимости, которые могут повлиять на функциональность базы данных в новой версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
-  [Помощник по настройке запросов](../../relational-databases/performance/upgrade-dbcompat-using-qta.md) может помочь в настройке рабочих нагрузок, которые могут испытывать негативные последствия при обновлении совместимости базы данных.

На следующем рисунке приведен пример инноваций для различных версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в течение нескольких лет: 

![25 лет инноваций SQL Server](media/sql-server-end-of-life-overview/sql-server-version-improvements.png)

## <a name="extend-support"></a>Расширенная поддержка 

Если вы не готовы к обновлению и переходу в облако, у вас есть возможность приобрести расширенную подписку на обновления безопасности, чтобы получить **критические** обновления для системы безопасности до трех лет после окончания срока поддержки.  

### <a name="benefits"></a>Преимущества 

- **Поддержка приложений**. Это лучший вариант, если приложению требуется повторная сертификация в более новой версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это распространено для приложений, которые не используют [Сертификацию на совместимость](../../database-engine/install-windows/compatibility-certification.md). 
- **Последовательная инфраструктура**. Вам не нужно изменять инфраструктуру каким бы то ни было образом. 
- **Техническая поддержка**. Если у вас есть Software Assurance или другой план поддержки, вы можете продолжить получать техническую поддержку из [!INCLUDE[msCoName](../../includes/msconame-md.md)] уже после окончания срока поддержки продукта [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это единственный способ получить поддержку для [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] и [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]. 
- **Time**: этот вариант доступен в течение трех лет, что дает вам дополнительные возможности для сертификации приложений. 

### <a name="considerations"></a>Рекомендации 

- **Ограниченные доступности**. Этот параметр доступен только клиентам с лицензиями подписок или Software Assurance 
- **Затраты** — Этот вариант может оказаться дорогостоящим, так как расширенные обновления безопасности составляют примерно 75 % от стоимости лицензии на месте ежегодно.
- **Ограниченные временные рамки**. Этот параметр доступен только в течение трех лет, поэтому вам по-прежнему потребуется выполнить обновление или миграцию в конце 3-летнего периода, если требуется обеспечить безопасность и соответствие требованиям.
- **Без исправления ошибок**. Если при работе с продуктом возникла ошибка, не относящаяся к безопасности, [!INCLUDE[msCoName](../../includes/msconame-md.md)] не выпустит исправление для нее. 
- **Ограниченная поддержка**. Расширенные обновления безопасности не включают в себя новые функции, функциональные улучшения или исправления, запрашиваемые клиентом. Исправления безопасности ограничиваются тем, что они оцениваются как критические [Центром Microsoft Security Response Center (MSRC)](https://portal.msrc.microsoft.com/).

### <a name="resources"></a>Ресурсы

[What are Extended Security Updates for SQL Server?](sql-server-extended-security-updates.md)      (Что такое расширенные обновления для системы безопасности?)  
[Вопросы и ответы о расширенных обновлениях безопасности](https://www.microsoft.com/cloud-platform/extended-security-updates)       
[Extend support for SQL Server 2008 and SQL Server 2008 R2 with Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)      (Расширение поддержки SQL Server 2008 и SQL Server 2008 R2 с помощью Azure)  
[Обзор программы Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default)       

## <a name="azure-virtual-machine"></a>Виртуальная машина Azure

Еще одним вариантом является миграция вашей рабочей нагрузки на [виртуальную машину Azure под управлением SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview). Вы можете перенести вашу систему как есть и сохранить завершение поддержки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], или вы можете обновить ее до более новой версии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Это оптимальное решение для миграции и для приложений, требующих доступа на уровне ОС. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Виртуальные машины поддерживают lift-and-shift для существующих приложений, которым требуется быстрая миграция в облако с минимальными изменениями или без них. 

### <a name="benefits"></a>Преимущества

- **Бесплатные расширенные обновления безопасности**: Если вы решите сохранить ваши [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] как есть, используя [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] или [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)], вы можете получить бесплатные расширенные обновления безопасности на три года после окончания срока действия поддержки, даже не имея Software Assurance. 
- **Экономичность**. Вы экономите на аппаратном и серверном программном обеспечении, оплачивая только почасовое использование. 
- **Перенос по методу lift-and-shift**. Вы можете переместить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и инфраструктуру приложений в облако с минимальными изменениями или без них. 
- **Среда размещения**. Вы получите преимущества размещенной среды, например для разгрузки оборудования и обслуживания программного обеспечения. 
- **Автоматизация.** Если вы используете [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)] и более поздних версий, вы получите преимущества автоматической установки исправлений и автоматического резервного копирования. 
- **Управление OS**. Вы можете управлять средой операционной системы, но с помощью знакомого набора функций [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Быстрое развертывание**. Можно быстро выполнить развертывание из библиотеки образов виртуальных машин. 
- **Перемещение лицензий**. Вы можете использовать лицензию, что позволит снизить эксплуатационные расходы. 
- **Высокий уровень доступности**. Вы получаете не только преимущества встроенной доступности виртуальных машин благодаря инфраструктуре Azure, которая может достигать 99.99 % доступности, но вы также можете воспользоваться преимуществами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] опций высокой доступности, таких как экземпляры отказоустойчивого кластера и группы доступности Always On. 
- **Низкая степень риска для приложений баз данных**. Поддерживая совместимость базы данных на том же уровне, что и старая база данных, существующие приложения базы данных защищены от функциональных изменений и изменений производительности, которые могут иметь негативные последствия. Приложение следует повторно сертифицировать, только если необходимо использовать функции, которые являются производными от более новых параметров совместимости базы данных. Дополнительные сведения см. в статье [Сертификация на совместимость](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Рекомендации

- **Управляемость.** Вам по-прежнему необходимо управлять программным обеспечением [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и операционной системы. 
- **Сеть**. Необходимо настроить виртуальную машину для интеграции с сетью и инфраструктурой Active Directory, что является дополнительным уровнем сложности. 
- **Экземпляр отказоустойчивого кластера общего хранилища**. Виртуальные машины Azure поддерживают только экземпляры отказоустойчивого кластера с использованием локального дискового пространства или общих файловых ресурсов уровня "Премиум" и не поддерживают экземпляр отказоустойчивого кластера с использованием общего хранилища. Таким образом, виртуальные машины Azure поддерживают только экземпляры отказоустойчивого кластера при использовании Windows Server 2012 или более поздней версии.
- **Простой масштабирования**. При изменении ресурсов ЦП и хранилища возникают простои. 
- **Ограничение размера**. Хотя экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может поддерживать столько баз данных, сколько необходимо, совокупное количество всех баз данных для одного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] составляет 256 ТБ, а не 524 ПБ для локальной [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

### <a name="resources"></a>Ресурсы

[What is SQL Server on Azure Virtual Machines? (Windows)](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)      (Что собой представляет SQL Server на виртуальных машинах Azure (Windows))  
[Choose the right deployment option in Azure SQL](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)      (Выбор правильного варианта развертывания в SQL Azure)  
[Migrate a SQL Server database to SQL Server in an Azure VM](/azure/virtual-machines/windows/sql/virtual-machines-windows-migrate-sql)      (Перенос базы данных SQL Server на SQL Server в ВМ Azure)  
[Extend support for SQL Server 2008 and SQL Server 2008 R2 with Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)      (Расширение поддержки SQL Server 2008 и SQL Server 2008 R2 с помощью Azure)  
[What are Extended Security Updates for SQL Server?](sql-server-extended-security-updates.md)      (Что такое расширенные обновления для системы безопасности?)  
[Вопросы и ответы о расширенных обновлениях безопасности](https://www.microsoft.com/cloud-platform/extended-security-updates)       
[Automated Patching for SQL Server in Azure Virtual Machines (Resource Manager)](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)      (Автоматическая установка исправлений SQL Server на виртуальных машинах Azure (Resource Manager))  
[Automated Backup v2 for Azure Virtual Machines (Resource Manager)](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2)      (Автоматическая архивация исправлений для виртуальных машин SQL)  
[High availability and disaster recovery for SQL Server in Azure Virtual Machines](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr)      (Высокий уровень доступности и аварийное восстановление для SQL Server на виртуальных машинах Azure)  
[Frequently asked questions for SQL Server running on Windows virtual machines in Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-faq) (Часто задаваемые вопросы об SQL Server, работающем на виртуальных машинах Windows в Azure)       

## <a name="azure-sql-database-single-database"></a>Отдельная база данных Базы данных SQL Azure

Если вы хотите разгрузить обслуживание, сократить расходы и устранить необходимость обновления в будущем, можно переместить рабочую нагрузку в [базы данных SQL Azure](/azure/sql-database/sql-database-single-database). Этот вариант лучше подходит для современных облачных приложений, которые используют новейшие стабильные функции [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] и имеют ограничения по времени в разработке и маркетинге. 

### <a name="benefits"></a>Преимущества

- **Затраты** — Отдельная база данных может быть экономичной, так как оборудование, программное обеспечение и обслуживание разгружаются, и вы можете платить за использование на почасовой или посекундной основе. 
- **Гибкость**. Отдельная база данных отлично подходит для облачных приложений, чувствительных к скорости разработки и оперативности при выводе решений на рынок, а также облачных приложений, для которых требуется запросить внешний доступ.  
- **Общие функции**. Здесь доступны самые популярные функции [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], но не в таком количестве, как для Управляемого экземпляра SQL.  
- **Быстрое развертывание**. Можно быстро развернуть отдельную базу данных. 
- **Масштабируемость.** Вы можете быстро и легко увеличивать и уменьшать масштаб, что необходимо Вашему бизнесу, обеспечивая дополнительные преимущества с точки зрения экономии средств. 
- **Доступность.** Стоимость службы включает в себя как хранение, так и высокий уровень доступности с гарантией доступности 99,995 %.  
- **Автоматизация.** Исправление и резервное копирование происходит автоматически, экономя время на обслуживание.  
- **Intelligent Insights**. Получите представление о производительности базы данных с помощью встроенной аналитики средства искусственного интеллекта.  
- **Без версии**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не имеет версий, то есть всегда находится в последней версии и никогда не нужно беспокоиться об обновлении или простоях. Кроме того, вы всегда в курсе последних событий, и наши новейшие стабильные функции будут выпущены в первую очередь в "облако".
- **Низкая степень риска для приложений баз данных**. Поддерживая совместимость базы данных на том же уровне, что и локальная база данных, существующие приложения защищены от функциональных изменений и изменений производительности, которые могут иметь негативные последствия. Приложение следует повторно сертифицировать, только если необходимо использовать функции, которые являются производными от более новых параметров совместимости базы данных. Дополнительные сведения см. в статье [Сертификация на совместимость](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Рекомендации

- **Ограниченные варианты миграции**:  Можно перенести только одну базу данных за раз, а не весь экземпляр.   
- **Ограничения возможностей**.  Несмотря на то что наиболее часто используемые функции Базы данных SQL Azure доступны, набор функций для одной базы данных не является полным, как для Управляемого экземпляра SQL Azure или [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Отличия Transact-SQL**.  Существует несколько [!INCLUDE[tsql](../../includes/tsql-md.md)] различий (T-SQL) между отдельной базой данных и локальной[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Ограничения размера**.  Для отдельной базы данных максимальный размер составляет 100 ТБ по сравнению с размером 524 ПБ для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Время обслуживания**. Точное время обслуживания не гарантируется, хотя оно — почти прозрачно. 

### <a name="resources"></a>Ресурсы

[What is the Azure SQL Database service?](/azure/sql-database/sql-database-technical-overview)      (Общие сведения о базе данных SQL Azure)  
[Choose the right deployment option in Azure SQL](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)      (Выбор правильного варианта развертывания в SQL Azure)  
[Azure SQL Database Features](/azure/sql-database/sql-database-features)      (Функции Базы данных SQL Azure)  
[SQL Server database migration to Azure SQL Database](/azure/sql-database/sql-database-single-database-migrate)      (Перенос базы данных SQL Server в Базу данных Azure SQL)  
[Accelerate migration by migrating multiple databases or entire SQL Servers](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)      (Ускорение миграции путем миграции нескольких баз данных или целых серверов SQL Server)  
[Resolving Transact-SQL differences during migration to SQL Database](/azure/sql-database/sql-database-transact-sql-information)      (Устранение различий Transact-SQL во время миграции в базу данных SQL)  
Ограничения ресурсов [виртуального ядра](/azure/sql-database/sql-database-vcore-resource-limits-single-databases) и [DTU](/azure/sql-database/sql-database-dtu-resource-limits-single-databases)       
[Intelligent Insights](/azure/sql-database/sql-database-intelligent-insights)       

Инструменты:
- [Помощник по миграции данных](../../dma/dma-overview.md)
- [Database Migration Service](/azure/dms/dms-overview)

## <a name="sql-managed-instance"></a>Управляемый экземпляр SQL

Если вы хотите воспользоваться преимуществами разгрузки обслуживания и затрат, но считаете, что набор функций отдельной базы данных [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] слишком ограничен, можно перейти на [Управляемый экземпляр SQL](/azure/sql-database/sql-database-managed-instance). Управляемый экземпляр похож на локальную [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], не беспокоясь о сбоях оборудования или установке исправлений. Управляемый экземпляр — это коллекция системных и пользовательских баз данных с общим набором ресурсов, которые можно использовать для выполнения большинства операций миграции в облако. Этот вариант подходит для новых приложений или существующих локальных приложений, которые используют последние стабильные возможности [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] и переносятся в облако с минимальными изменениями. 

### <a name="benefits"></a>Преимущества

- **Затраты** — Затраты можно сэкономить путем разгрузки обслуживания программного обеспечения и оборудования.  
- **Методика Lift-and-Shift**. Вы можете переместить весь локальный экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в управляемый экземпляр, включая все базы данных с минимальными изменениями базы данных. 
- **Компоненты**. Набор функций управляемого экземпляра точно соответствует аналогичным функциям локального экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], таким как запросы между базами данных, публикация и распространение репликации транзакций, планирование заданий SQL и поддержка CLR. 
- **Масштабируемость.** Все базы данных в управляемом экземпляре совместно используют ресурсы, и в любой момент времени можно увеличивать и уменьшать его работу без простоев.   
- **Автоматизация.** Исправление и резервное копирование происходит автоматически, экономя время на обслуживание.  
- **Доступность.** Стоимость службы включает в себя как хранение, так и высокий уровень доступности с гарантией доступности 99,99 %.  
- **Intelligent Insights**. Получите представление о производительности баз данных с помощью встроенной аналитики средства искусственного интеллекта.  
- **Без версии**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] не имеет версий, то есть всегда находится в последней версии и никогда не нужно беспокоиться об обновлении или простоях. Кроме того, вы всегда в курсе последних событий, и наши новейшие стабильные функции будут выпущены в первую очередь в "облако".
- **Низкая степень риска для приложений баз данных**. Поддерживая совместимость базы данных на том же уровне, что и локальные базы данных, существующие приложения базы данных защищены от функциональных изменений и изменений производительности, которые могут иметь негативные последствия. Приложение следует повторно сертифицировать, только если необходимо использовать функции, которые являются производными от более новых параметров совместимости базы данных. Дополнительные сведения см. в статье [Сертификация на совместимость](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Рекомендации

- **Затраты** — Параметр управляемого экземпляра может быть более дорогостоящим, чем параметр отдельной базы данных.  
- **Отличия Transact-SQL**. Существует несколько [!INCLUDE[tsql](../../includes/tsql-md.md)] различий (T-SQL) между отдельной базой данных и локальной[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
- **Развертывание.**  Развертывание управляемого экземпляра может занять больше времени, чем отдельная база данных.  
- **Ограничения возможностей**. Несмотря на то, что управляемый экземпляр использует большинство функций с помощью [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], некоторые функции все еще не поддерживаются. 
- **Ограничение размера**. Общий размер хранилища для всех баз данных в управляемом экземпляре ограничен до 8 ТБ по сравнению с 524 ПБ для локальной [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
- **Сеть**. Требования к сети для управляемого экземпляра добавляют дополнительный уровень сложности к инфраструктуре и требуют использования Azure ExpressRoute или VPN-шлюза.
- **Время обслуживания**. Точное время обслуживания не гарантируется, хотя оно — почти прозрачно. 

### <a name="resources"></a>Ресурсы

[Обзор Управляемого экземпляра SQL](/azure/sql-database/sql-database-managed-instance)       
[Choose the right deployment option in Azure SQL](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)      (Выбор правильного варианта развертывания в SQL Azure)  
[Azure SQL Database Features](/azure/sql-database/sql-database-features)      (Функции Базы данных SQL Azure)  
[Миграция SQL Server в Управляемый экземпляр SQL Azure](/azure/sql-database/sql-database-managed-instance-migrate)       
[Accelerate migration by migrating multiple databases or entire SQL Servers](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration) (Ускорение миграции путем миграции нескольких баз данных или целых серверов SQL Server)       

Инструменты:
- [Помощник по миграции данных](../../dma/dma-overview.md)
- [Database Migration Service](/azure/dms/dms-overview)

## <a name="non-sql-options"></a>Параметры, не относящееся к SQL

Для некоторых типов приложений также может потребоваться рассмотреть нереляционное или NoSQL решение, например Azure Cosmos DB или хранилище таблиц Azure.

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Рекомендуем использовать Azure Cosmos DB для современных, масштабируемых мобильных и веб-приложений, которые используют данные JSON и которым необходимо надежное выполнение запросов и обработка транзакционных данных. Дополнительные сведения см. в разделе [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Сведения об импорте данных см. в статье [Импорт данных в Cosmos DB](/azure/cosmos-db/import-data/).

Преимущества Azure Cosmos DB:
- Документы индексируются, и для запросов к ним можно использовать знакомый синтаксис SQL.
- База данных является бессхемной.
- Можно добавлять свойства в документы без необходимости перестроения индексов.
- Поддержка JSON и JavaScript осуществляется прямо внутри ядра СУБД.
- Вы получаете встроенную поддержку геопространственных данных и интеграции с другими службами Azure, включая поиск Azure, HDInsight и фабрику данных.
- Вы получаете высокопроизводительное хранилище с низкими задержками, имеющее зарезервированные уровни пропускной способности.

### <a name="azure-table-storage"></a>Хранилище таблиц Azure

Рекомендуется использовать хранилище таблиц Azure для хранения петабайт частично структурированных данных в экономичном решении. Дополнительные сведения см. в разделе [Табличное хранилище](https://azure.microsoft.com/services/storage/tables/).

Преимущества хранилища таблиц Azure:
- Можно развивать приложения и табличную схему без перевода данных в автономный режим.
- Можно увеличивать масштаб без сегментирования набора данных.
- Вы получаете географически избыточное хранилище, которое реплицирует данные по нескольким регионам.

## <a name="lifecycle-dates"></a>Даты жизненного цикла

В следующей таблице приведена приблизительная информация о датах жизненного цикла продуктов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. на странице [Microsoft Lifecycle Policy](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy) (Политика жизненного цикла Майкрософт). 

| **Версия**     | **Год выпуска** | **Год окончания основной фазы поддержки** | **Год окончания расширенной поддержки** |
| :---------------| :--------------- | :------------------------------ | :---------------------------- |
| [SQL Server 2019](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202019) | 2019 | 2025 | 2030 |
| [SQL Server 2017](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202017) | 2017 | 2022 | 2027 |
| [SQL Server 2016](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202016) | 2016 | 2021 | 2026 |
| [SQL Server 2014](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202014) | 2014 | 2019 | 2024 |
| [SQL Server 2012](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202012) | 2012 | 2017 | 2022 |
| [SQL Server 2008 R2](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202008%20R2) | 2010 | 2012 | [2019](https://www.microsoft.com/sql-server/sql-server-2008) |
| [SQL Server 2008](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202008) | 2008 | 2012 | [2019](https://www.microsoft.com/sql-server/sql-server-2008) |
| [SQL Server 2005](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202005) | 2006 | 2011 | [2016](https://www.microsoft.com/sql-server/sql-server-2005) |
| [SQL Server 2000](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202000) | 2000 | 2005 | [2013](/archive/blogs/cdnitmanagers/sql-server-2000-end-of-support-april-2013) |

> [!IMPORTANT]
> Если существует какое-либо расхождение между этой таблицей и страницей жизненного цикла [!INCLUDE[msCoName](../../includes/msconame-md.md)], то Жизненный цикл [!INCLUDE[msCoName](../../includes/msconame-md.md)] заменяет эту таблицу, так как эта таблица предназначена для использования в качестве ориентировочной ссылки.  

## <a name="next-steps"></a>Next Steps  
[SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019)   
[Прекращение поддержки SQL Server 2005](https://www.microsoft.com/sql-server/sql-server-2005)   
[Прекращение поддержки SQL Server 2008](https://www.microsoft.com/cloud-platform/windows-sql-server-2008)   
[What are Extended Security Updates for SQL Server?](sql-server-extended-security-updates.md)  (Что такое расширенные обновления для системы безопасности?)  
[Extend support for SQL Server 2008 and SQL Server 2008 R2 with Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)  (Расширение поддержки SQL Server 2008 и SQL Server 2008 R2 с помощью Azure)  
[What is SQL Server on Azure Virtual Machines? (Windows)](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)  (Что собой представляет SQL Server на виртуальных машинах Azure (Windows))  
[What is the Azure SQL Database service?](/azure/sql-database/sql-database-technical-overview) (Общие сведения о базе данных SQL Azure)
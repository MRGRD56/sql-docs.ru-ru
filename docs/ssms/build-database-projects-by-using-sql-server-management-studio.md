---
description: Создание проектов баз данных с использованием среды SQL Server Management Studio
title: Создание проектов баз данных
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- scripts [SQL Server], database projects
- database projects [SQL Server]
- projects [SQL Server Management Studio], about projects
- projects [SQL Server Management Studio]
ms.assetid: c2e80045-894d-44cf-b65c-e547ed738947
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.openlocfilehash: 1c017a50c2c5806012547d87fd67786e8121510a
ms.sourcegitcommit: 22dacedeb6e8721e7cdb6279a946d4002cfb5da3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92038148"
---
# <a name="build-database-projects-by-using-sql-server-management-studio"></a>Создание проектов баз данных с использованием среды SQL Server Management Studio

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Проект скрипта базы данных — это организованный набор скриптов, сведений о соединении и шаблонов, связанных с базой данных или одной из частей базы данных. [!INCLUDE[msCoName](../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] предоставляет [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] для администрирования и проектирования баз данных [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] в контексте проекта скрипта. [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] включает конструкторы, редакторы, руководства и мастера, помогающие пользователям в разработке, развертывании и обслуживании баз данных.  
  
## <a name="sql-server-management-studio"></a>SQL Server Management Studio.  
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] — это набор административных средств для управления компонентами, относящимися к [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Эта интегрированная среда позволяет пользователям выполнять разнообразные задачи, например резервное копирование данных, редактирование запросов и автоматизацию общих функций в одном интерфейсе.  
  
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] включает в себя следующие средства:  
  
-   Редактор кода — богатый возможностями редактор скриптов для написания и редактирования скриптов. [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] предоставляет четыре версии редактора кода: редактор запросов [!INCLUDE[ssDE](../includes/ssde_md.md)] для скриптов [!INCLUDE[tsql](../includes/tsql-md.md)] , редактор запросов многомерных выражений, редактор запросов расширения интеллектуального анализа данных и редактор запросов XML/A.  
  
-   Обозреватель объектов для размещения, изменения, создания скрипта или выполнения объектов, принадлежащих экземплярам [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
-   Обозреватель шаблонов для размещения и написания сценариев шаблонов.  
  
-   Обозреватель решений для организации и хранения связанных скриптов как части проекта.  
  
-   Окно свойств для отображения текущих свойств выбранных объектов.  
  
[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] обеспечивает эффективность рабочих процессов, предоставляя:  
  
-   Отключенный доступ. Можно писать и изменять скрипты, не соединяясь с экземпляром [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
-   Создание сценариев из любого диалогового окна. Можно создать скрипт из любого диалогового окна, а также читать, изменять, сохранять и многократно использовать скрипты после создания.  
  
-   Немодальные диалоговые окна. При обращении к диалоговому окну интерфейса можно просмотреть другие ресурсы в среде [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] , не закрывая диалоговое окно.  
  
## <a name="solutions-and-script-projects"></a>Решения и проекты скриптов  
Обозреватель решений — это программа для хранения и повторного открытия решений для базы данных. Решения организовывают связанные проекты скрипта и файлы. Проекты скрипта хранят файлы скрипта [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , шаблоны SQL, сведения о соединении и другие разные файлы. После сохранения скрипта в проекте скриптов у пользователей появляются следующие возможности.  
  
-   Сопровождать управление версиями в скриптах.  
  
-   Хранить параметры результатов со скриптом.  
  
-   Организовывать связанные скрипты в один проект скриптов.  
  
-   Сохранять сведения о соединении со скриптами.  
  
Обозреватель решений — инструмент для разработчиков, создающих и многократно использующих скрипты, связанные с одним и тем же проектом. Если подобная задача потребуется позже, можно использовать группу скриптов, которые были сохранены в проекте. Те, кто уже имеет опыт разработки приложений с помощью [!INCLUDE[msCoName](../includes/msconame_md.md)] [!INCLUDE[vsprvs](../includes/vsprvs-md.md)], сориентируются в обозревателе решений довольно легко.  
  
Решение состоит из одного или более проектов скриптов. Проект состоит из одного или более скриптов или соединений. Проект может содержать не только файлы сценариев.  
  
## <a name="see-also"></a>См. также:  
[Использование среды SQL Server Management Studio](./sql-server-management-studio-ssms.md)  
[Решения (среда SQL Server Management Studio)](../ssms/solution/solutions-sql-server-management-studio.md)  

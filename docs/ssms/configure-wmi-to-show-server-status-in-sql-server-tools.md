---
description: Настройка инструментария WMI для отображения состояния сервера в инструментальных средствах SQL Server
title: Настройка инструментария WMI для отображения состояния сервера в инструментальных средствах SQL Server
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- WMI Provider for Server Events, setting permissions
- WMI permissions [SQL Server]
ms.assetid: 7e97197b-ed4d-40d1-9a52-9ab1d92401d7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 307211456c8b82aec5bad63d6b1d0023b9de6ae3
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251271"
---
# <a name="configure-wmi-to-show-server-status-in-sql-server-tools"></a>Настройка инструментария WMI для отображения состояния сервера в инструментальных средствах SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
В этом разделе описывается настройка инструментария WMI для отображения состояния сервера в средствах SQL Server в [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]. При соединении с сервером компоненты «Зарегистрированные серверы» и «Обозреватель объектов» среды [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], как и диспетчер конфигурации [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , используют инструментарий WMI для получения сведений о состоянии служб [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (MSSQLSERVER) и агента [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (MSSQLSERVER). Для отображения состояния службы пользователь должен иметь права удаленного доступа к объекту инструментария WMI. Для настройки этого разрешения на сервере должен быть установлен инструментарий WMI.  
  
## <a name="to-configure-wmi-permission"></a><a name="SSMSProcedure"></a>Настройка разрешения WMI  
  
1.  В меню **Пуск** на удаленном сервере выберите **Выполнить**.  
  
2.  В окне **Открыть** введите **wmimgmt.msc** и нажмите кнопку **ОК**.  
  
3.  В программе **инфраструктуры управления Windows** щелкните правой кнопкой мыши **Элемент управления WMI (локальный)** и выберите **Свойства**.  
  
4.  В диалоговом окне **Свойства элемента управления WMI (локальный)** на вкладке **Безопасность** разверните **Корень** и щелкните **CIMV2**.  
  
5.  Щелкните **Безопасность** , чтобы открыть диалоговое окно **Защита для ROOT\CIMV2** .  
  
6.  Добавьте группу или пользователя в поле **Имена групп или пользователей** и выберите их.  
  
7.  В поле **Разрешение для** _<group or user>_ выберите столбец **Разрешить** для разрешения **Включить удаленно** у пользователей, которым требуется удаленно определить состояние службы.  
  
## <a name="see-also"></a>См. также:  
[Запуск, остановка или приостановка службы агента SQL Server](../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
  

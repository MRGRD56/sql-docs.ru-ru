---
title: Конфигурация собственного клиента SQL 11.0
description: Узнайте о параметрах, настроенных в диалоговых окнах настройки SQL Server Native Client в Microsoft SQL Server Configuration Manager.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- client configuration [SQL Server], SQL Server Native Client
ms.assetid: e73143e9-5e7b-4d0a-8827-ab900efdcb35
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: aa0eac24445ec24c4b86c54bad6fc1c94c7072d4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463765"
---
# <a name="sql-native-client-110-configuration"></a>Конфигурация собственного клиента SQL 11.0
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  В этом разделе содержатся разделы справки F1 диалоговых окон модуля **Конфигурация SQL Server Native Client** в диспетчере конфигурации [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client является сетевой библиотекой, при помощи которой клиентские компьютеры подключаются к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], начиная с [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Настройки, установленные в конфигурации собственного клиента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , используются на компьютере, на котором запущена программа клиента. Если настройки выполняются на компьютере, где запущен [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], они затрагивают только те клиентские программы, которые работают на сервере.  
  
 Эти настройки не затрагивают клиентов, подключающихся к предыдущим версиям [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], за исключением случая, когда клиенты используют клиентские средства [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]и последующих версий, такие как [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="in-this-section"></a>В этом разделе  
  
-   [Свойства конфигурации собственного клиента SQL Server (вкладка "Флаги")](../../tools/configuration-manager/sql-server-native-client-configuration-properties-flags-tab.md)  
  
-   [Клиентские протоколы (диспетчер конфигурации SQL Server)](../../tools/configuration-manager/client-protocols-sql-server-configuration-manager.md)  
  
    -   [Свойства клиентских протоколов (вкладка "Порядок")](../../tools/configuration-manager/client-protocols-properties-order-tab.md)  
  
    -   [Клиентские протоколы — свойства общей памяти (вкладка "Протокол")](../../tools/configuration-manager/client-protocols-shared-memory-properties-protocol-tab.md)  
  
    -   [Клиентские протоколы — свойства TCP/IP (вкладка "Протокол")](../../tools/configuration-manager/client-protocols-tcp-ip-properties-protocol-tab.md)  
  
    -   [Клиентские протоколы — свойства именованных каналов (вкладка "Протокол")](../../tools/configuration-manager/client-protocols-named-pipes-properties-protocol-tab.md)  
  
-   [Псевдонимы (диспетчер конфигурации SQL Server)](../../tools/configuration-manager/aliases-sql-server-configuration-manager.md)  
  
    -   [Создание псевдонима (вкладка "Псевдоним")](../../tools/configuration-manager/new-alias-alias-tab.md)  
  
    -   [Свойства &#60;Псевдоним&#62; (вкладка "Псевдоним")](../../tools/configuration-manager/alias-properties-alias-tab.md)  
  
    -   [Создание допустимой строки соединения с использованием протокола общей памяти](../../tools/configuration-manager/creating-a-valid-connection-string-using-shared-memory-protocol.md)  
  
    -   [Создание допустимой строки подключения с использованием протокола TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)  
  
    -   [Создание допустимой строки подключения, использующей протокол именованных каналов](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))  
  

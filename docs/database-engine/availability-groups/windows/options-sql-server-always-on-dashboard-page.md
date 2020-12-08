---
title: Панель мониторинга группы доступности в SSMS
description: Сведения о странице "Параметры" на панели мониторинга Always On в SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Alwayson.Dashboard
ms.assetid: 4369b588-e982-4b57-80a1-beb2e879ce0b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1b34fcb55625bf2283389abe129fe33e0aa5c6ef
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96503705"
---
# <a name="options-sql-server-always-on-dashboard-page"></a>Параметры (страница панели мониторинга SQL Server AlwaysOn)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Для настройки панели мониторинга AlwaysOn используйте страницу **SQL Server AlwaysOn Dashboard** (Панель мониторинга SQL Server) диалогового окна [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]Параметры **в среде**.  
  
 **Открытие этой страницы**  
  
 В меню **Сервис** выберите пункт **Параметры**, откройте папку **SQL Server AlwaysOn** и выберите **Панель мониторинга**.  
  
## <a name="on-this-page"></a>На этой странице  
 **Включить автоматическое обновление.**  
 Выберите, чтобы включить автоматическое обновление. Доступны следующие возможности:  
  
-   В поле **Интервал обновления (в секундах)** отображается количество секунд, через которое панель будет обновлена. Значение по умолчанию — 30. Если автоматическое обновление включено, значение в этом поле можно изменить, чтобы указать новый интервал обновления.  
  
-   В поле **Количество повторных попыток подключения** отображается, сколько раз панель мониторинга будет пытаться установить соединение с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , на котором расположена реплика доступности группы доступности, мониторинг которой выполняет панель. Значение по умолчанию — 65535. Если автоматическое обновление включено, значение в этом поле вы можете изменить, чтобы указать другое количество попыток.  
  
 **Включить собственную, определяемую пользователем политику AlwaysOn.**  
 Если вы определили собственную политику AlwaysOn, выберите этот параметр, чтобы включить ее.  
  
## <a name="see-also"></a>См. также:  
 [Использование панели мониторинга AlwaysOn (среда SQL Server Management Studio)](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  

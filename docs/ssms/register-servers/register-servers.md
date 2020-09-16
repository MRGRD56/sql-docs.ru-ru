---
description: Регистрация серверов
title: Регистрация серверов
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.sqlserverregisteredserver.dhelp
helpviewer_keywords:
- connections [SQL Server], registered servers
- registering servers
- servers [SQL Server], registering
- server management [SQL Server], registering servers
- server registration [SQL Server]
ms.assetid: c2a2513e-fa09-419c-99e7-a12d57c5a0db
author: markingmyname
ms.author: maghan
ms.manageR: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: dd50ab7a771dd0cd479716c94fa26110198822b0
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88468554"
---
# <a name="register-servers"></a>Регистрация серверов

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Регистрация сервера в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] позволяет сохранить информацию о подключении сервера для будущих подключений. Есть три способа зарегистрировать сервер в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
1.  Локальные экземпляры [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] автоматически регистрируются во время первого запуска среды [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] после ее установки.  
  
2.  Можно также запустить процесс автоматической регистрации в любое время, чтобы восстановить регистрацию локальных экземпляров сервера.  
  
3.  Наконец, можно зарегистрировать сервер с помощью средства «Зарегистрированные серверы» в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="benefits-of-registered-servers"></a>Преимущества зарегистрированных серверов  
 С помощью списка «Зарегистрированные серверы» можно:  
  
-   регистрировать серверы для сохранения данных о соединениях;  
  
-   определять, запущен ли зарегистрированный сервер;  
  
-   легко подключать обозреватель объектов и редактор запросов к зарегистрированному серверу;  
  
-   редактировать и удалять регистрационные данные зарегистрированного сервера;  
  
-   создавать группы серверов;  
  
-   назначать зарегистрированным серверам понятные имена путем задания в поле **Имя зарегистрированного сервера** значения, отличного от имеющегося в списке **Имя сервера** ;  
  
-   создавать подробные описания зарегистрированных серверов;  
  
-   указывать подробные описания групп зарегистрированных серверов;  
  
-   экспортировать группы зарегистрированных серверов;  
  
-   импортировать группы зарегистрированных серверов;  
  
-   просматривать файлы журналов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для экземпляров [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], находящихся в сети и вне сети.  
  
## <a name="related-tasks"></a>Связанные задачи  
 Используйте следующие разделы для начала работы с зарегистрированными серверами.  
  
|**Описание**|**Раздел**|  
|---------------------|---------------|  
|Регистрация экземпляров локального сервера|[Регистрация подключенного сервера (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/register-a-connected-server-sql-server-management-studio.md)|  
|Регистрация сервера|[Создание нового зарегистрированного сервера (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/create-a-new-registered-server-sql-server-management-studio.md)|  
|Просмотр зарегистрированных серверов|[Просмотр зарегистрированных серверов в среде SQL Server Management Studio](../../tools/sql-server-management-studio/view-registered-servers-in-sql-server-management-studio.md)|  
|Удаление зарегистрированного сервера|[Удаление зарегистрированного сервера (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/remove-a-registered-server-sql-server-management-studio.md)|  
|Изменение регистрации сервера|[Изменение регистрационных данных сервера (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/change-a-server-s-registration-sql-server-management-studio.md)|  
|Подключение к зарегистрированному серверу|[Подключение к зарегистрированному серверу (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/connect-to-a-registered-server-sql-server-management-studio.md)|  
|Отключение от зарегистрированного сервера|[Отключение от зарегистрированного сервера (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/disconnect-from-a-registered-server-sql-server-management-studio.md)|  
|Перемещение зарегистрированного сервера или группы серверов|[Перемещение зарегистрированного сервера или зарегистрированной группы серверов (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/move-a-registered-server-or-registered-server-group.md)|  
|Изменение имени зарегистрированного сервера или группы серверов|[Изменение имени зарегистрированного сервера или зарегистрированной группы серверов (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/change-the-name-of-registered-server-or-registered-server-group.md)|  
|Создание или изменение группы серверов|[Создание или изменение группы серверов (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/create-or-edit-a-server-group-sql-server-management-studio.md)|  
|Удалить группу серверов|[Удаление группы серверов (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/remove-a-server-group-sql-server-management-studio.md)|  
|Экспорт сведений о зарегистрированном сервере|[Выполнение экспорта сведений компонента "Зарегистрированные серверы" (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/export-registered-server-information-sql-server-management-studio.md)|  
|Удаление сведений о зарегистрированном сервере|[Импорт сведений компонента "Зарегистрированные серверы" (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/import-registered-server-information-sql-server-management-studio.md)|  
|Создание центрального сервера управления и группы серверов|[Создание центрального сервера управления и группы сервера (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/create-a-central-management-server-and-server-group.md)|  
|Выполнение инструкций на нескольких серверах одновременно|[Выполнение инструкции на нескольких серверах одновременно (среда SQL Server Management Studio)](../../tools/sql-server-management-studio/execute-statements-against-multiple-servers-simultaneously.md)|  
  
## <a name="see-also"></a>См. также:  
 [Удаленные серверы](../../database-engine/configure-windows/remote-servers.md)  
  
  

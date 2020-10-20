---
title: Настройка и администрирование сервера отчетов (собственный режим) | Документация Майкрософт
description: Сведения о подходах, которые можно использовать для настройки Reporting Services, а также о поиске статей по настройке компонентов, функций или возможностей сервера.
ms.date: 05/15/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, components
- deploying [Reporting Services], component options
- report servers [Reporting Services], component options
- configuration options [Reporting Services]
- administering Reporting Services
- components [Reporting Services], configuring
- configuring servers [Reporting Services]
ms.assetid: cfec012b-56f1-4346-9814-247acf22351c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3f9d31706fd0ba46c565d4c9049a981dd06fb54b
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933798"
---
# <a name="configure-and-administer-a-report-server-ssrs-native-mode"></a>Настройка и администрирование сервера отчетов (службы Reporting Services в собственном режиме)
  В этой статье описаны разные подходы к настройке [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Он также включает список разделов, в которых объясняется, как настраивать конкретные компоненты, функции и возможности сервера. Для настройки служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]можно.  
  
-   Используйте диспетчер конфигурации сервера отчетов. Многие подразделы этого раздела содержат сведения о том, как настраивать конкретные возможности с помощью этого средства.  
  
-   Используйте среду [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , чтобы включить папку "Мои отчеты", включить журналы трассировки и установить значения по умолчанию для уровня сайта. Дополнительные сведения о настройках сайта см. в разделе [Сервер отчетов служб Reporting Services (основной режим)](../../reporting-services/report-server/reporting-services-report-server-native-mode.md) для среды Management Studio. Обратите внимание, что можно создать и запустить скрипт, который программно задает свойства сервера. Дополнительные сведения см. в статьях [Написание скриптов для задач развертывания и администрирования](../../reporting-services/tools/script-deployment-and-administrative-tasks.md) и [Системные свойства сервера отчетов](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md).  
  
-   Используйте веб-портал, чтобы предоставить разрешения на доступ к серверу отчетов. Разрешения предоставляются с помощью назначения ролей для каждой учетной записи пользователя или группы пользователей. Дополнительные сведения см. в статье [Роли и разрешения (службы Reporting Services)](../../reporting-services/security/roles-and-permissions-reporting-services.md).  
  
-   Дополнительно можно изменить файлы конфигурации для изменения настроек приложений. Дополнительные сведения о каждом файле и рекомендации по их изменению см. в разделе [Reporting Services Configuration Files](../../reporting-services/report-server/reporting-services-configuration-files.md).  
  
## <a name="in-this-section"></a>В этом разделе  
 [Настройка URL-адресов сервера отчетов (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)  
 Описывает способ определения URL-адресов, используемых для доступа к серверу отчетов и веб-порталу.  
  
 [Настройка учетной записи службы сервера отчетов (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)  
 Содержит рекомендации и описывает шаги по изменению учетной записи службы и пароля.  
  
 [Создание базы данных сервера отчетов (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
 Описывает, как создать базу данных сервера отчетов, необходимую для хранения метаданных и объектов сервера.  
  
 [Настройка подключения к базе данных сервера отчетов (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)  
 Описывает, как изменить строку, используемую сервером отчетов для подключения к базе данных сервера отчетов.  
  
 [Доставка по электронной почте в службах Reporting Services](../install-windows/e-mail-settings-reporting-services-native-mode-configuration-manager.md)  
 Описывает, как настроить сервер отчетов для поддержки рассылки отчетов по электронной почте.  
  
 [Настройка учетной записи автоматического выполнения (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)  
 Описывает, как настроить пользовательскую учетную запись для обработки отчетов в автоматическом режиме.  
  
## <a name="see-also"></a>См. также раздел  
 [Файлы конфигурации служб Reporting Services](../../reporting-services/report-server/reporting-services-configuration-files.md)   
 [Диспетчер конфигурации сервера отчетов (собственный режим)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Защита и обеспечение безопасности служб Reporting Services](../../reporting-services/security/reporting-services-security-and-protection.md)   
 [Сервер отчетов служб Reporting Services (собственный режим)](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)  
  
  

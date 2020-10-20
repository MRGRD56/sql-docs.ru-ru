---
title: Программы командной строки сервера отчетов | Документация Майкрософт
description: Сведения о служебных программах командной строки SQL Server Reporting Services, которые используются для администрирования сервера отчетов.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- rsconfig utility
- components [Reporting Services], command line utilities
- rs utility
- command prompt utilities [Reporting Services]
- rskeymgmt utility
ms.assetid: 68f2f9f4-f894-40ff-a71c-f9756bf4b68c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e08d96d7a03cb1449c725672e698496f48c29ae2
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935510"
---
# <a name="report-server-command-prompt-utilities-ssrs"></a>Программы командной строки сервера отчетов (SSRS)
  В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] предусмотрено несколько программ командной строки, с помощью которых можно администрировать сервер отчетов. Эти программы автоматически устанавливаются при установке сервера отчетов.  
  
|Имя|Командный файл|Поддерживаемый режим развертывания|Описание|  
|----------|------------------|-------------------------------|-----------------|  
|Программа RSS|rs.exe|Собственный режим и режим интеграции с SharePoint. В выпуске [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] появилась поддержка режима с SharePoint.|Программа [rs](../../reporting-services/tools/rs-exe-utility-ssrs.md) является сервером скриптов, который можно применять для выполнения операций, выраженных в форме скриптов. Используйте эту программу для выполнения скриптов [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] , предназначенных для копирования данных из одной базы данных сервера отчетов в другую, публикации отчетов, создания объектов в базе данных сервера отчетов и т. д. Использование скриптов для администрирования сервера описывается в разделе [Написание скриптов для задач развертывания и администрирования](../../reporting-services/tools/script-deployment-and-administrative-tasks.md).|  
|Командлеты PowerShell||Только режим интеграции с SharePoint|Список командлетов Powershell см. в статье [Командлеты PowerShell для служб Reporting Services в режиме SharePoint](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).|  
|rsconfig, программа|rsconfig.exe|Только собственный режим|Программа [rsconfig](../../reporting-services/tools/rsconfig-utility-ssrs.md) служит для настройки и управления соединением сервера отчетов с базой данных сервера отчетов. Кроме того, с ее помощью можно указать учетную запись пользователя, которую следует использовать для автоматической обработки отчетов. Дополнительные сведения см. в разделе [Сервер отчетов служб Reporting Services (основной режим)](../../reporting-services/report-server/reporting-services-report-server-native-mode.md). Дополнительные сведения о конфигурации подключения см. в статье [Настройка подключения к базе данных сервера отчетов (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).|  
|Программа rskeymgmt|rskeymgmt.exe|Только собственный режим|Программа [rskeymgmt](../../reporting-services/tools/rskeymgmt-utility-ssrs.md) представляет собой средство управления ключами шифрования. Ее можно использовать для создания резервной копии, применения, повторного создания и удаления симметричных ключей. Кроме того, она позволяет подключить экземпляр сервера отчетов к общей базе данных сервера отчетов. Программу Rskeymgmt можно использовать при восстановлении базы данных. Существующую базу данных можно повторно использовать на вновь установленном экземпляре, применив резервную копию симметричного ключа. Если ключи восстановить невозможно, программа rskeymgmt позволяет удалить больше не используемое зашифрованное содержимое. Хранению конфиденциальных данных и управлению ключами посвящены разделы [Хранение зашифрованных данных сервера отчетов (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/ssrs-encryption-keys-store-encrypted-report-server-data.md) и [Настройка ключей шифрования и управление ими (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md).|  
  
> [!NOTE]  
>  Если предпочтительным является средство с графическим пользовательским интерфейсом, то вместо программ **rsconfig** и **rskeymgmt** можно использовать диспетчер конфигурации сервера отчетов.  
  
## <a name="see-also"></a>См. также:  
 [Диспетчер конфигурации сервера отчетов (собственный режим)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Инструментальные средства служб Reporting Services](../../reporting-services/tools/reporting-services-tools.md)   
 [Сервер отчетов служб Reporting Services (собственный режим)](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)  
  
  

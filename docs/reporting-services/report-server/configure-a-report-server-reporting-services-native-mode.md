---
title: Настройка сервера отчетов (службы Reporting Services в собственном режиме) | Документы Майкрософт
description: Сведения о дополнительной конфигурации сервера отчетов SQL Server, которая зависит от параметров, выбранных во время установки.
ms.date: 06/18/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- report server configuration
- report servers [Reporting Services], configuring
ms.assetid: ef84ce9d-9156-48e9-8073-7e0535476932
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: cf028fa4de6457b5ddfe520bae1d2348c52362bf
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91935015"
---
# <a name="configure-a-report-server-reporting-services-native-mode"></a>Настройка сервера отчетов (службы Reporting Services в собственном режиме)
  В зависимости от параметров, выбранных во время установки, сервер отчетов может потребовать дополнительной настройки перед началом его использования. Как минимум настройка сервера отчетов включает следующие действия.  
  
-   Учетная запись службы сервера отчетов (настраивается во время установки).  
  
-   URL-адрес веб-службы, обеспечивающей доступ к серверу отчетов.  
  
-   База данных сервера отчетов, в которой хранятся данные приложений, отчеты и другие элементы.  
  
 Программа установки производит минимальную настройку, если выбран один из следующих параметров установки: настройка по умолчанию в собственном режиме или настройка по умолчанию в режиме интеграции с SharePoint. Если сервер отчетов устанавливается в режиме "только файлы" (параметр **Установить, но не настраивать сервер** в мастере установки), то настраивается только учетная запись службы. URL-адрес веб-службы и база данных сервера отчетов должны быть настроены после завершения программы установки.  
  
Мы рекомендуем настраивать диспетчер отчетов таким образом, чтобы пользователь имел доступ к серверу отчетов и мог управлять содержимым на нем. При развертывании сервера отчетов в режиме интеграции с SharePoint используйте для предоставления доступа клиентский веб-интерфейс сервера SharePoint.  
  
 Дополнительные компоненты (например, электронная почта сервера отчетов и учетная запись автоматического выполнения) можно настроить по необходимости. Дополнительные сведения см. в разделе [Управление сервером отчетов служб Reporting Services в собственном режиме](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md).  
  
 Настройка сервера отчетов производится при помощи программы настройки служб Reporting Services.  
  
## <a name="to-minimally-configure-a-report-server-installation"></a>Минимальная настройка установки сервера отчетов  
  
1.  Запустите программу настройки служб Reporting Services и подключитесь к экземпляру сервера отчетов. Дополнительные сведения см. в разделе [Использование диспетчера конфигурации сервера отчетов (собственный режим)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).  
  
2.  Щелкните **URL-адрес веб-службы** , чтобы открыть страницу для настройки URL-адреса для сервера отчетов. Инструкции по определению URL-адресов см. в разделе [Настройка URL-адреса (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md).  
  
3.  Чтобы создать базу данных сервера отчетов, нажмите кнопку **База данных** . Инструкции см. в статье [Создание базы данных сервера отчетов, работающего в собственном режиме (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  
  
4.  Вернитесь на страницу **URL-адрес веб-службы** и щелкните URL-адрес, чтобы проверить, как он работает.  
  
5.  Для завершения развертывания следуйте инструкциям в разделе «Следующие шаги».  
  
## <a name="next-steps"></a>Next Steps  
 Чтобы завершить развертывание, необходимо настроить веб-портал или интеграцию с SharePoint. Дополнительные сведения см. в статье [о настройке веб-портала](../../reporting-services/report-server/configure-web-portal.md).  
  
 Если включен брандмауэр Windows, то порт, на который настроен сервер отчетов, скорее всего, закрыт. Одним из индикаторов того, что порт закрыт, является пустая страница при попытке открыть веб-портал на удаленном клиентском компьютере. Сведения о настройке брандмауэра см. в разделе [Configure a Firewall for Report Server Access](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).  
  
 Если используется Windows Vista или Windows Server 2008, то перед локальным открытием веб-портала необходимо выполнить дополнительные шаги. Дополнительные сведения см. в разделе [Настройка сервера отчетов, работающего в основном режиме, для локального администрирования (службы SSRS)](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  
  
 Проверьте правильность установки, выполнив создание папки, передачу элементов и запуск отчетов. Следуйте инструкциям в разделе [Проверка установки служб Reporting Services](../../reporting-services/install-windows/verify-a-reporting-services-installation.md) для проверки установки.  
  
## <a name="see-also"></a>См. также раздел  
 [Управление сервером отчетов служб Reporting Services в собственном режиме](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
 [Настройка брандмауэра для доступа к серверу отчетов](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)   
 [Настройка сервера отчетов, работающего в собственном режиме, для локального администрирования (службы SSRS)](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md)   
 [Настройка сервера отчетов для удаленного администрирования](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md)   
 [Диспетчер конфигурации сервера отчетов (собственный режим)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  
  

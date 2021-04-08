---
title: Запуск и остановка службы сервера отчетов | Документы Майкрософт
description: Узнайте, как запускать и останавливать службу Windows, которая включает в себя веб-службу сервера отчетов, веб-портал и приложение фоновой обработки.
ms.date: 03/22/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- stopping Report Server service
- Report Server Windows service, starting
- Report Server service, starting
- starting Report Server service
ms.assetid: 6ec69ac3-27b0-472d-91e1-733af9078ed2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d56cf652fab81403c50a0de122c1b5ad004de1a8
ms.sourcegitcommit: ab0c654d924eeb5647e47444abb59d934345b205
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2021
ms.locfileid: "106450145"
---
# <a name="start-and-stop-the-report-server-service"></a>Запуск и остановка службы сервера отчетов

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

  Сервер отчетов реализован как служба Windows, которая включает веб-службу сервера отчетов, веб-портал и приложение фоновой обработки. Чтобы использовать какие-либо функции сервера отчетов, эта служба должна работать. Остановка службы приводит к прекращению всех операций сервера отчетов.  
  
 Пока служба остановлена, запросы для запланированных отчетов и обработки подписок, которые были бы удовлетворены, если бы служба работала, добавляются в очередь. Это связано с тем, что задания, выполняемые агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , создают события. Если требуется предотвратить создание незавершенных операций во время простоя службы, рассмотрите возможность остановить работу агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Чтобы запустить или остановить службу сервера отчетов, можно применить ряд средств, включая программу настройки служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и оснастку «Службы», существующую в [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
> [!NOTE]
> Configuration Manager [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] — это вариант только для SQL Server Reporting Services 2016 и более ранних версий. Сюда не входят Reporting Services 2017 и более поздних версий или Сервер отчетов Power BI.
  
 Чтобы выполнить какие-либо другие операции, кроме запуска и остановки службы, например для смены учетной записи службы, необходимо пользоваться программой настройки служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Использование для смены учетной записи службы других средств может привести к неработоспособности установки [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и ключа шифрования. Дополнительные сведения см. в разделе [Настройка учетной записи службы сервера отчетов (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  
  
 Эту службу нельзя приостанавливать и возобновлять, и она не имеет параметров запуска. Хотя явные зависимости отсутствуют, для поддержки подписок и проводимых по расписанию операций с отчетами на сервере отчетов должен работать агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="use-the-reporting-services-configuration-tool"></a>Использование программы настройки служб Reporting Services  
  
1. Запустите программу настройки служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и подключитесь к серверу отчетов.  
  
2. На странице состояния сервера отчетов нажмите **Остановить** или **Запустить**.  
  
## <a name="use-administrative-tools"></a>Использование средств администрирования  

1. В разделе средств администрирования выберите **Службы**.
2. Щелкните правой кнопкой мыши **SQL Server Reporting Services (MSSQLSERVER)** , **SQL Server Reporting Services** или **Сервер отчетов Power BI**.
3. Выберите **Остановить** или **Перезапустить**.

  
Если при использовании Reporting Services 2016 несколько экземпляров выполняется параллельно или сервер отчетов выполняется в качестве именованного экземпляра, убедитесь, что имя экземпляра в круглых скобках соответствует экземпляру сервера отчетов, который нужно остановить или перезапустить.  

  
## <a name="see-also"></a>См. также раздел  
 [Диспетчер конфигурации сервера отчетов (собственный режим)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)   
 [Запуск, остановка или приостановка службы агента SQL Server](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
  

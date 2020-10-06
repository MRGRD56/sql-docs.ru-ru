---
description: SMO, диспетчер соединений
title: Диспетчер подключений SMO | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.smoconnection.f1
helpviewer_keywords:
- connections [Integration Services], SMO
- SMO connection manager
- connection managers [Integration Services], SMO
ms.assetid: d273f1fb-a6a8-4f2f-a5ff-55c2e3de4723
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 23a54cf3d07772276dbdf21dd7bf752a35121097
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91719559"
---
# <a name="smo-connection-manager"></a>SMO, диспетчер соединений

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Диспетчер соединений SMO позволяет пакету подключиться к серверу SQL Management Object (SMO). Задачи пересылки, включенные в службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], используют диспетчер соединений SMO. Например, задача «Передача имен входа», пересылающая данные для входа в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , использует диспетчер соединений SMO.  
  
 При добавлении к пакету диспетчера подключений SMO [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] создает диспетчер подключений, который будет разрешен в соединение SMO во время выполнения, устанавливает свойства диспетчера подключений и добавляет его к коллекции **Подключения** пакета. Свойству **ConnectionManagerType** диспетчера соединений присваивается значение **SMOServer**.  
  
 Можно настроить диспетчер соединений SMO следующим образом:  
  
-   Указать имя сервера, на котором установлен [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Выберите режим проверки подлинности для соединения с сервером.  
  
## <a name="configuration-of-the-smo-connection-manager"></a>Настройка диспетчера соединений SMO  
 Значения свойств можно задавать с помощью конструктора [!INCLUDE[ssIS](../../includes/ssis-md.md)] или программными средствами.  
  
 Дополнительные сведения о свойствах, которые можно задавать в конструкторе служб [!INCLUDE[ssIS](../../includes/ssis-md.md)] , см. в статье [Редактор диспетчера соединений SMO]().  
  
 Дополнительные сведения о программной настройке диспетчера подключений см. в разделах <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> и [Добавление соединений программным образом](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="smo-connection-manager-editor"></a>редактор диспетчера соединений SMO
  Используйте **Редактор диспетчера соединений SMO** для настройки соединения сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которое может использоваться различными задачами, переносящими объекты [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Дополнительные сведения о диспетчере соединений объектов SMO см. в разделе [SMO Connection Manager](../../integration-services/connection-manager/smo-connection-manager.md).  
  
### <a name="options"></a>Параметры  
 **Имя сервера**  
 Введите имя экземпляра сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или выберите имя сервера из списка.  
  
 **Обновить**  
 Обновите список доступных экземпляров сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , которые могут быть обнаружены в сети.  
  
 **Использовать проверку подлинности Windows**  
 Используйте проверку подлинности Windows для подключения к выбранному экземпляру сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Использовать проверку подлинности SQL Server**  
 Используйте проверку подлинности сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для подключения к выбранному экземпляру сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **User name**  
 При выборе проверки подлинности сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] введите имя пользователя сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Пароль**  
 При выборе проверки подлинности сервера [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] введите пароль.  
  
 **Проверка соединения**  
 Проверка соединения согласно настройке.  
  
## <a name="see-also"></a>См. также  
 [Соединения в службах Integration Services (SSIS)](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  

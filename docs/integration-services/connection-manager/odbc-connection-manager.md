---
description: диспетчер соединений ODBC
title: Диспетчер соединений ODBC | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.odbcconnection.f1
helpviewer_keywords:
- connections [Integration Services], ODBC
- ODBC connection manager
- data sources [Integration Services], connections
- connection managers [Integration Services], ODBC
ms.assetid: e8c77aa7-6772-485e-918e-cef9eeb18c58
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7fe63ad503b49a760823635902b3744a1ae2d85a
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/05/2020
ms.locfileid: "91719644"
---
# <a name="odbc-connection-manager"></a>диспетчер соединений ODBC

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Диспетчер соединений ODBC позволяет пакету подключаться к разнообразным системам управления базами данных, используя открытый интерфейс взаимодействия с базами данных (ODBC).  
  
 При добавлении соединения ODBC к пакету и задании свойств диспетчера соединений службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] создают диспетчер соединений и добавляют его к коллекции **Connections** пакета. Во время выполнения диспетчер соединений рассматривается как физическое соединение с ODBC.  
  
 Свойству **ConnectionManagerType** диспетчера соединений присваивается значение **ODBC**.  
  
 Настроить диспетчер соединений ODBC можно следующими способами:  
  
-   Предоставить строку соединения, которая ссылается на пользовательское или системное имя источника данных.  
  
-   Указать сервер, к которому нужно подключиться.  
  
-   Указать, сохранять ли соединение во время выполнения.  
  
## <a name="configuration-of-the-odbc-connection-manager"></a>Настройка диспетчера соединений ODBC  
 Значения свойств можно задавать с помощью конструктора [!INCLUDE[ssIS](../../includes/ssis-md.md)] или программными средствами.  
  
 Дополнительные сведения о свойствах, которые можно задать в конструкторе служб [!INCLUDE[ssIS](../../includes/ssis-md.md)] , см. в одном из последующих разделов.  
  
-   [Справочник по пользовательскому интерфейсу диспетчера подключений ODBC]()  
  
 Дополнительные сведения о программной настройке диспетчера подключений см. в разделах <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> и [Добавление соединений программным образом](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="odbc-connection-manager-ui-reference"></a>Справочник по пользовательскому интерфейсу диспетчера соединений ODBC
  Диалоговое окно **Настройка диспетчера соединений ODBC** используется для добавления соединения с источником данных ODBC.  
  
 Дополнительные сведения о диспетчере соединений ODBC см. в разделе [ODBC Connection Manager](../../integration-services/connection-manager/odbc-connection-manager.md).  
  
### <a name="options"></a>Параметры  
 **Подключения к данным**  
 Выберите из списка существующий диспетчер соединений ODBC.  
  
 **Свойства подключения к данным**  
 Просмотрите свойства и значения выбранного диспетчера соединений ODBC.  
  
 **Новые**  
 Создайте новый диспетчер соединений ODBC с помощью диалогового окна **Диспетчер соединений** . При необходимости это диалоговое окно также позволяет создать новый источник данных ODBC.  
  
 **Удаление**  
 Выберите соединение и затем удалите его, используя кнопку **Удалить** .  
## <a name="see-also"></a>См. также  
 [Соединения в службах Integration Services (SSIS)](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  

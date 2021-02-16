---
description: Диспетчер соединений OData
title: Диспетчер подключений OData | Документы Майкрософт
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.custom: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 3caa4372-aff3-4c0f-9ecd-97870948b8d0
f1_keywords:
- sql13.dts.designer.odatasource.connectionmanager.f1
- sql13.dts.designer.odataconnectionmanager.f1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7d02fbd8a7029a794b933053bec5f676646eced1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338426"
---
# <a name="odata-connection-manager"></a>Диспетчер соединений OData

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


 Диспетчер подключений OData позволяет подключаться к источнику данных OData. Компонент источника OData подключается к источнику OData с помощью диспетчера подключений OData и использует данные из этой службы. Дополнительные сведения см. в разделе [OData Source](../../integration-services/data-flow/odata-source.md).  
  
## <a name="adding-an-odata-connection-manager-to-an-ssis-package"></a>Добавление диспетчера соединений OData в пакет SSIS  
 Новый диспетчер подключений OData можно добавить в пакет служб SSIS тремя способами:  
  
-   Нажмите кнопку **Создать…** в **редакторе источника OData**.  
  
-   Щелкните правой кнопкой мыши папку **Диспетчеры соединений** в **обозревателе решений** и выберите команду **Новый диспетчер соединений**. Для параметра **Тип диспетчера соединений** выберите **ODATA**.  
  
-   Щелкните правой кнопкой мыши панель **Диспетчеры соединений** в нижней части конструктора пакетов и выберите **Создать соединение**. Для параметра **Тип диспетчера соединений** выберите **ODATA**.  
  
## <a name="connection-manager-authentication"></a>Проверка подлинности диспетчера соединений  
 Диспетчер подключений OData поддерживает пять режимов проверки подлинности.  
  
-   Проверка подлинности Windows  
  
-   Обычная проверка подлинности (с использованием имени пользователя и пароля)  

-   Microsoft Dynamics AX Online (с помощью имени пользователя и пароля)
  
-   Microsoft Dynamics CRM Online (с помощью имени пользователя и пароля)
  
-   Microsoft Online Services (с помощью имени пользователя и пароля)  
  
Для анонимного доступа выберите режим проверки подлинности Windows.  

Для подключения к Microsoft Dynamics AX Online или Microsoft Dynamics CRM Online запрещено использовать способ проверки подлинности **Microsoft Online Services**. Кроме того, запрещено использовать любой способ, рассчитанный на многофакторную проверку подлинности. Современная проверка подлинности в настоящее время не поддерживается. 
  
### <a name="specifying-and-securing-credentials"></a>Указание и защита учетных данных  
 Если служба OData использует обычную проверку подлинности, то можно указать имя пользователя и пароль в окне [Редактор диспетчера соединений OData](). Значения, указываемые в редакторе, сохраняются в пакете. Значение пароля шифруется в соответствии с уровнем защиты пакета.  
  
 Существует несколько способов параметризации значений имени пользователя и пароля или их хранения за пределами пакета. Например, можно использовать параметры или непосредственно настроить свойства диспетчера подключений при запуске пакета из SQL Server Management Studio.  
  
## <a name="odata-connection-manager-properties"></a>Свойства диспетчера соединений OData  
 В следующем списке описаны свойства диспетчера подключений OData.  
  
|Свойство|Описание|  
|-|-|  
|Url|URL-адрес сервисного документа.|  
|UserName|Имя пользователя для проверки подлинности, если это необходимо.|  
|Пароль|Пароль для проверки подлинности, если это необходимо.|  
|ConnectionString|Включает другие свойства диспетчера подключений.|  
  
## <a name="odata-connection-manager-editor"></a>Редактор диспетчера соединений c OData
  Диалоговое окно **Редактор диспетчера соединений c OData** служит для добавления нового или изменения существующего соединения с источником данных OData.  
  
### <a name="options"></a>Параметры  
 **Имя диспетчера подключений**  
 Имя диспетчера соединений.  
  
 **Расположение сервисного документа**  
 URL-адрес службы OData. Например: https://services.odata.org/V3/Northwind/Northwind.svc/.  
  
 **Аутентификация**  
Выберите один из следующих параметров:
-   **Проверка подлинности Windows**. Выберите этот режим для анонимного доступа.
-   **Обычная проверка подлинности** 
-   **Microsoft Dynamics AX Online** для Dynamics AX Online
-   **Microsoft Dynamics CRM Online** для Dynamics CRM Online
-   **Microsoft Online Services** для Microsoft Online Services

Если выбран параметр, отличный от "Проверка подлинности Windows", введите **имя пользователя** и **пароль**. 

Для подключения к Microsoft Dynamics AX Online или Microsoft Dynamics CRM Online запрещено использовать способ проверки подлинности **Microsoft Online Services**. Кроме того, запрещено использовать любой способ, рассчитанный на многофакторную проверку подлинности.

 **Проверка соединения**  
 Нажмите эту кнопку для проверки подключения к источнику OData.
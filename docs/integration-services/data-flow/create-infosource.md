---
description: Создание InfoSource
title: Создание InfoSource | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: e7db233b-5464-43de-9d26-6dd24c7ac1b7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c4672ce48890af7202445d283d15c2c167550f5a
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127248"
---
# <a name="create-infosource"></a>Создание InfoSource

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Используйте диалоговое окно **Создание InfoSource** для создания нового InfoSource. Для создания нового InfoSource используется либо диалоговое окно **Создание InfoSource для данных транзакции** , либо диалоговое окно **Создание InfoSource для основных данных** .  
  
 Диалоговое окно **Создание InfoSource** можно открыть на странице **Диспетчер соединений****Редактора назначений SAP BW**. Дополнительные сведения о назначении SAP BW см. в статье [SAP BW Destination](../../integration-services/data-flow/sap-bw-destination.md).  
  
> [!IMPORTANT]  
>  Документация по Microsoft Connector 1.1 для SAP BW предполагает, что читатель знаком со средой SAP Netweaver BW. Дополнительные сведения о SAP Netweaver BW или сведения о настройке объектов и процессов SAP Netweaver BW см. в документации SAP.  
  
 **Открытие диалогового окна «Создание InfoSource»**  
  
1.  В [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]откройте пакет [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , содержащий назначение SAP BW.  
  
2.  На вкладке **Поток данных** дважды щелкните назначение SAP BW.  
  
3.  В **Редакторе назначений SAP BW** щелкните **Диспетчер соединений** , чтобы открыть страницу редактора **Диспетчер соединений** .  
  
4.  На вкладке **Диспетчер соединений** в поле группы **Создание объектов SAP BW** выберите **InfoSource** и щелкните **Создать**.  
  
## <a name="options"></a>Параметры  
 **Данные транзакций**  
 Создание нового InfoSource для данных транзакции.  
  
 Если выбран этот параметр, то откроется диалоговое окно **Создание InfoSource для данных транзакции** . Используйте диалоговое окно **Создание InfoSource для данных транзакции** для создания нового InfoSource. Дополнительные сведения об этом диалоговом окне см. в разделе [Create InfoSource for Transaction Data](../../integration-services/data-flow/create-infosource-for-transaction-data.md).  
  
 **Основные данные**  
 Создание нового InfoSource для основных данных.  
  
 Если выбран этот параметр, то откроется диалоговое окно **Создание InfoSource для основных данных** . Используйте диалоговое окно **Создание InfoSource для основных данных** для создания нового InfoSource. Дополнительные сведения об этом диалоговом окне см. в разделе [Create InfoSource for Master Data](../../integration-services/data-flow/create-infosource-for-master-data.md).  
  
## <a name="see-also"></a>См. также:  
 [Справка F1 по Microsoft Connector для SAP BW](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  

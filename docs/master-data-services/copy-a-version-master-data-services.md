---
description: Копирование версии (службы Master Data Services)
title: Копирование версии
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- versions [Master Data Services], copying
- copying versions [Master Data Services]
ms.assetid: f4678a02-bbe9-4f21-9e32-627eae053fe7
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: f7871bb7e0f5912e2a1039355d2b5508120a97f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272745"
---
# <a name="copy-a-version-master-data-services"></a>Копирование версии (службы Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  В среде [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]копирование версии модели позволяет создать из нее новую версию.  
  
## <a name="prerequisites"></a>Предварительные требования  
 Для выполнения этой процедуры:  
  
-   необходимо иметь разрешение на доступ к функциональной области **Управление версиями** ;  
  
-   необходимо быть администратором модели. Дополнительные сведения см. в разделе [administrators &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Необходимо иметь разрешение на доступ к функциональной области "Управление версиями". Дополнительные сведения см. в разделе [разрешения функциональной области &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
### <a name="to-copy-a-version"></a>Копирование версии  
  
1.  В среде [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]щелкните область **Управление версиями**.  
  
2.  На странице **Управление версиями** выберите строку версии, которую необходимо скопировать.  
  
    > [!NOTE]  
    >  В зависимости от параметров [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)]копирование может быть разрешено только для версий с состоянием **Зафиксирована** . Дополнительные сведения см. в разделе [Системные параметры (службы Master Data Services)](../master-data-services/system-settings-master-data-services.md).  
  
3.  Нажмите кнопку **Копировать**.  
  
4.  В диалоговом окне подтверждения нажмите кнопку **ОК**.  
  
## <a name="next-steps"></a>Next Steps  
  
-   [Изменение имени версии (службы Master Data Services)](../master-data-services/change-a-version-name-master-data-services.md)  
  
## <a name="see-also"></a>См. также:  
 [Версии (службы Master Data Services)](../master-data-services/versions-master-data-services.md)  
  
  

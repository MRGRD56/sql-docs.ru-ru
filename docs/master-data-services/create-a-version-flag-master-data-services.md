---
description: Создание флага версии (службы Master Data Services)
title: Создание флага версии
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- creating version flags [Master Data Services]
- version flags [Master Data Services], creating
- versions [Master Data Services], creating flags
ms.assetid: 3067e1e3-05b7-4f11-9206-c612ef4e7e42
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 1e64ae6bb0ee7d31d871cbb4d843831b19cf6139
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272585"
---
# <a name="create-a-version-flag-master-data-services"></a>Создание флага версии (службы Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  В среде [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]флаг версии создается, чтобы назначить версию. Флаг может указывать на то, какую версию следует использовать пользователям или системам-подписчикам.  
  
## <a name="prerequisites"></a>Предварительные требования  
 Для выполнения этой процедуры:  
  
-   необходимо иметь разрешение на доступ к функциональной области **Управление версиями** ;  
  
-   необходимо быть администратором модели. Дополнительные сведения см. в разделе [administrators &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Необходимо иметь разрешение на доступ к функциональной области "Управление версиями". Дополнительные сведения см. в разделе [разрешения функциональной области &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
### <a name="to-create-a-version-flag"></a>Создание флага версии  
  
1.  В среде [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]щелкните область **Управление версиями**.  
  
2.  На панели меню страницы **Управление версиями** наведите курсор на **Управление** и выберите **Флаги**.  
  
3.  На странице **Управление флагами версий** выберите модель в поле **Модель** , для которой нужно создать флаг.  
  
4.  Нажмите кнопку **Добавить**.  
  
5.  В поле **Имя** введите имя.  
  
6.  В поле **Описание** введите описание.  
  
7.  В поле **Только зафиксированные версии** выберите **True** , чтобы указать, что флаг может назначаться только версиям со статусом **Зафиксированная** . Выберите **False** , чтобы указать, что флаг может назначаться версиям с любым состоянием.  
  
8.  Выберите команду **Сохранить**.  
  
## <a name="next-steps"></a>Next Steps  
  
-   [Назначение флага версии (службы Master Data Services)](../master-data-services/assign-a-flag-to-a-version-master-data-services.md)  
  
## <a name="see-also"></a>См. также:  
 [Версии &#40;Master Data Services&#41;](../master-data-services/versions-master-data-services.md)   
 [Изменение имени флага версии (службы Master Data Services)](../master-data-services/change-a-version-flag-name-master-data-services.md)  
  
  

---
description: Создание администратора модели (службы Master Data Services)
title: Создание администратора модели
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- administrators [Master Data Services], creating
ms.assetid: dae17afc-3b39-490e-b51f-2d8da26d429e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 11aada5002d7f35a66d64040016e9cf6e614fdf0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272655"
---
# <a name="create-a-model-administrator-master-data-services"></a>Создание администратора модели (службы Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  В среде [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]администратор модели создается, когда группе или пользователю необходимо предоставить все разрешения на все объекты из одной или нескольких моделей.  
  
> [!TIP]  
>  Чтобы упростить администрирование, создайте локальную группу Windows или локальной группы и настройте ее в качестве администратора модели. Впоследствии добавлять пользователей в группу и удалять их из нее можно без обращения к [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)].  
  
## <a name="prerequisites"></a>Предварительные требования  
 Для выполнения этой процедуры:  
  
-   необходимо разрешение на доступ к функциональной области **Разрешения пользователей и групп** ;  
  
-   необходимо быть администратором модели. Дополнительные сведения см. в разделе [administrators &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-create-a-model-administrator"></a>Создание администратора модели  
  
1.  В среде [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]щелкните **Разрешения пользователей и групп**.  
  
2.  На странице **Пользователи** или **Группы** выберите строку пользователя или группы, которые необходимо изменить.  
  
3.  Нажмите **Изменить выделенного пользователя**.  
  
4.  Перейдите на вкладку **Модели** .  
  
5.  По желанию выберите модель из списка **Модель** .  
  
6.  Нажмите кнопку **Изменить**.  
  
7.  Щелкните модель, разрешение на которую необходимо предоставить.  
  
8.  В меню выберите пункт **Администратор**.  
  
9. Выполните шаги 7 и 8 для каждой модели, для которой необходимо назначить администратором пользователя или группу.  
  
10. Выберите команду **Сохранить**.  
  
## <a name="see-also"></a>См. также:  
 [Администраторы &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)   
 [Назначение разрешений объекта модели &#40;Master Data Services&#41;](../master-data-services/assign-model-object-permissions-master-data-services.md)   
 [Назначение разрешений элемента иерархии &#40;Master Data Services&#41;](../master-data-services/assign-hierarchy-member-permissions-master-data-services.md)   
 [Разрешения объекта модели &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Разрешения на элементы иерархии (службы Master Data Services)](../master-data-services/hierarchy-member-permissions-master-data-services.md)  
  
  

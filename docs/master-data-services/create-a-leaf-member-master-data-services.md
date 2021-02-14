---
description: Создание конечного элемента (службы Master Data Services)
title: Создание конечного элемента
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- leaf members [Master Data Services], creating
- creating leaf members [Master Data Services]
- members [Master Data Services], creating leaf members
ms.assetid: 0499d3b3-d508-4d43-a740-19cf53ade9f1
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ffffd1fa28597a41ba1b8f7631e52f6264bb8203
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272675"
---
# <a name="create-a-leaf-member-master-data-services"></a>Создание конечного элемента (службы Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  В [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]создайте конечный элемент, если вам нужно добавить в систему основные данные. Если нужно добавить данные в массовом режиме, следует использовать промежуточные таблицы. Дополнительные сведения см. в разделе  [Импорт данных из таблиц &#40;Master Data Services&#41;](../master-data-services/import-data-from-tables-master-data-services.md)  
  
 Также для импорта данных можно использовать [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../includes/ssmdsxls-md.md)] .  
  
## <a name="prerequisites"></a>Предварительные требования  
 Для выполнения этой процедуры:  
  
-   Необходимо иметь разрешение на доступ к функциональной области **Обозреватель** .  
  
-   Необходимо иметь как минимум разрешение **Создание** или **Обновление** для конечного объекта модели для сущности, в которую нужно добавить элемент. Разрешение "Создание" позволяет создать элемент и изменить только атрибут Code. Разрешение "Обновление" позволяет обновлять другие атрибуты.  
  
     Дополнительные сведения см. в разделе [Безопасность (службы Master Data Services)](../master-data-services/security-master-data-services.md).  
  
### <a name="to-create-a-leaf-member"></a>Создание конечного элемента  
  
1.  На домашней странице [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] выберите модель в списке **Модель** .  
  
2.  Если вы пользователь, выберите открытую версию из списка **Версия** . Если вы являетесь администратором, выберите версию в открытом или блокированном состоянии из списка **Версия** .  
  
3.  Нажмите кнопку **Браузер**.  
  
4.  В строке меню наведите указатель на пункт **Сущности** и щелкните имя сущности, к которой нужно добавить элемент.  
  
5.  Нажмите кнопку **Добавить участника**.  
  
6.  Заполните поля в области **Сведения** .  
  
     Если фильтр применен к атрибуту, который основан на домене, список значений атрибута будет ограничен родительским атрибутом фильтра.  
  
     Дополнительные сведения о родительских атрибутах фильтров и атрибутах на основе домена см. в разделе [Создание атрибута на основе домена (службы Master Data Services)](../master-data-services/create-a-domain-based-attribute-master-data-services.md).  
  
7.  Необязательный элемент. В поле **Заметки** введите комментарий о том, для чего добавлен элемент. Заметку могут просматривать все пользователи, которые имеют доступ к элементу.  
  
8.  Нажмите кнопку **ОК**.  
  
## <a name="see-also"></a>См. также:  
 [Создание &#40;объединенного элемента Master Data Services&#41;](../master-data-services/create-a-consolidated-member-master-data-services.md)   
 [Элементы (службы Master Data Services)](../master-data-services/members-master-data-services.md)  
  
  

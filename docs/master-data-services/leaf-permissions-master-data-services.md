---
description: Разрешения конечного элемента (службы основных данных)
title: Разрешения конечного элемента
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- attribute groups [Master Data Services], permissions
- members [Master Data Services], leaf member permissions
- permissions [Master Data Services], leaf members
- leaf members [Master Data Services], attribute permissions
- attributes [Master Data Services], leaf member attribute permissions
ms.assetid: bde16e8c-bcd4-4041-8130-55c5450e5f72
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 1924b33891b36d68a7b79c9046e0375b92f1d940
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338214"
---
# <a name="leaf-permissions-master-data-services"></a>Разрешения конечного элемента (службы основных данных)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Разрешения конечного элемента применяются к значениям атрибутов для всех конечных элементов сущности.  
  
 Для сущностей, у которых нет явных иерархий, назначение разрешения **Конечный** равнозначно назначению разрешения сущности.  
  
 **Примечания.**  
  
-   Разрешения конечного элемента применяются только к функциональной области **Обозреватель** пользовательского интерфейса.  
  
-   Разрешения, назначенные для атрибутов **Имя** и **Код** , не применяются.  
  
|Разрешение|Описание|  
|----------------|-----------------|  
|**Чтение**|Пользователь может просматривать конечные элементы и атрибуты.|  
|**Создание**|Пользователь может создавать конечные элементы и назначать значения атрибутов во время создания.|  
|**Обновление**|Пользователь может обновлять конечные элементы и атрибуты.|  
|**Удаление**|Пользователь может удалять конечные элементы.|  
|**Запретить**|Запрет любого доступа к конечным элементам.|  
  
 Разрешения на чтение, создание, обновление и удаление можно использовать в различных сочетаниях. Когда назначается разрешение на создание, обновление или удаление, автоматически добавляется и разрешение на чтение.  
  
## <a name="attribute-permissions"></a>Разрешения атрибута  
 Разрешения атрибута применимы только к значениям атрибута указанной сущности. Пользователи с разрешениями только для атрибутов не могут добавлять или удалять элементы.  
  
|Разрешение|Описание|  
|----------------|-----------------|  
|**Чтение**|Пользователь может просматривать атрибуты.|  
|**Создание**|Пользователь может назначать значения при создании элементов.|  
|**Обновление**|Пользователь может обновлять атрибуты.|  
|**Удаление**|Не влияет.|  
|**Запретить**|Атрибут не отображается.<br /><br /> Примечание. Нельзя явно запретить доступ к атрибутам "Имя" и "Код".|  
  
### <a name="example"></a>Пример  
 Сущности "Продукт" назначьте разрешение **Обновление** для атрибута Subcategory. Отмените разрешения для всех остальных атрибутов.  
  
|Имя|Код|Подкатегория (Обновление)|  
|----------|----------|----------------------------|  
|Mountain-100|BK-M101|{5} Mountain Bikes|  
|Mountain-100|BK-M201|{5} Mountain Bikes|  
  
 В **обозревателе** можно обновить любое значение атрибута в столбце "Subcategory". Если у пользователя нет разрешения для атрибута, атрибут не отображается.  
  
> [!NOTE]  
>  В этом примере «Подкатегория» является доменным атрибутом на основе сущности SubcategoryList. Можно выбрать другую подкатегорию для Mountain-100, но нельзя добавлять элементы или удалять элементы из сущности SubcategoryList.  
  
## <a name="see-also"></a>См. также:  
 [Назначение разрешения для объекта модели (службы Master Data Services)](../master-data-services/assign-model-object-permissions-master-data-services.md)   
    
 [Разрешения объекта модели &#40;Master Data Services&#41;](../master-data-services/model-object-permissions-master-data-services.md)   
 [Master Data Services &#40;членов&#41;](../master-data-services/members-master-data-services.md)   
 [Атрибуты (службы Master Data Services)](../master-data-services/attributes-master-data-services.md)  
  
  

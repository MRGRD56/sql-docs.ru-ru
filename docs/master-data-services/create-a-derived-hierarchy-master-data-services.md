---
description: Создание производной иерархии (службы Master Data Services)
title: Создание производной иерархии
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- derived hierarchies, creating
- creating derived hierarchies [Master Data Services]
ms.assetid: fec653c4-11cc-46a2-8dd8-b605341ebb40
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 3b69064a3adb5b9e13bd37e9d46bf6a41f6df992
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272705"
---
# <a name="create-a-derived-hierarchy-master-data-services"></a>Создание производной иерархии (службы Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  В среде [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]производные иерархии создаются, если необходима многоуровневая иерархия, гарантирующая, что элементы располагаются на правильном уровне. Производные иерархии опираются на имеющиеся в модели связи атрибутов на основе домена.  
  
> [!NOTE]  
>  Если значение атрибута на основе домена отсутствует для какого-либо элемента, он не включается в производную иерархию. Сведения о том, как сделать значение атрибута на основе домена обязательным для всех элементов, см. в разделе [Запрос значений атрибута (службы Master Data Services)](../master-data-services/require-attribute-values-master-data-services.md).  
  
## <a name="prerequisites"></a>Предварительные требования  
 Для выполнения этой процедуры:  
  
-   Необходимо иметь разрешение на доступ к функциональной области **Администрирование системы** .  
  
-   необходимо быть администратором модели. Дополнительные сведения см. в разделе [administrators &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-create-a-derived-hierarchy"></a>Создание производной иерархии  
  
1.  В [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]щелкните область **Администрирование системы**.  
  
2.  В строке меню наведите указатель на пункт **Управление** и выберите команду **Производные иерархии**.  
  
3.  На странице **Обслуживание производной иерархии** выберите модель из списка **Модель** .  
  
4.  Нажмите кнопку **Добавить**.  
  
5.  В поле **Имя производной иерархии** на странице **Добавление производной иерархии** введите имя иерархии.  
  
    > [!TIP]  
    >   Используйте имя, описывающее уровни иерархии, например **Продукт: от подкатегории к категории**.  
  
6.  Нажмите кнопку **Сохранить производную иерархию**.  
  
7.  На панели **Доступные сущности и иерархии** страницы **Изменить производную иерархию** щелкните сущность или иерархию и перетащите ее в область **Drop Parent Here** (Перетащите сюда родительский элемент) на панели **Текущие уровни** .  
  
8.  Перетаскивайте сущности или иерархии, пока не завершите создание иерархии.  
  
9. Нажмите кнопку **Назад**.  
  
## <a name="see-also"></a>См. также:  
 [Производные иерархии &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)   
 [Производные иерархии с явно заданными ограничениями &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-with-explicit-caps-master-data-services.md)   
 [Атрибуты на основе домена (службы Master Data Services)](../master-data-services/domain-based-attributes-master-data-services.md)  
  
  

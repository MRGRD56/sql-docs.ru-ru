---
description: Изменение и удаление индекса (Master Data Services)
title: Изменение и удаление индекса
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: f8fb2a63-f9ae-4b9d-b26f-2024d9af15c5
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: c6fa616af0fcc16b4898194bcdbb82e9382ba98d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341761"
---
# <a name="edit-and-delete-an-index-master-data-services"></a>Изменение и удаление индекса (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Индекс, созданный для атрибутов, можно изменить или удалить.  
  
## <a name="prerequisites"></a>Предварительные требования  
 Для выполнения этой процедуры:  
  
-   Иметь разрешение на доступ к функциональной области "Администрирование системы". Дополнительные сведения см. в разделе [разрешения функциональной области &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
-   необходимо быть администратором модели. Дополнительные сведения см. в разделе [administrators &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
 **Изменение индекса**  
  
1.  В диспетчере основных данных щелкните **Системное администрирование**.  
  
2.  На странице **Manage Model** (Управление моделью) выберите в сетке модель и щелкните **Сущности**.  
  
3.  На странице **Manage Entity** (Управление сущностью) в сетке выберите сущность, содержащую индекс, который нужно изменить.  
  
4.  Щелкните **Индексы**.  
  
5.  На странице **Manage Index** (Управление индексом) выберите индекс, который нужно изменить, и щелкните **Изменить**.  
  
6.  Введите новое имя индекса в поле **Имя** .  
  
7.  Установите или снимите флажок **IsUnique** .  
  
8.  Измените список назначенных атрибутов, добавив атрибуты в список или удалив их оттуда.  
  
9. Нажмите кнопку **Сохранить**.  
  
 **Удаление индекса**  
  
1.  На странице **Управление моделью** выберите модель в сетке и щелкните **сущности**.  
  
2.  На странице **Manage Entity** (Управление сущностью) в сетке выберите сущность, содержащую индекс, который нужно удалить.  
  
3.  Щелкните **Индексы**.  
  
4.  На странице **Manage Index** (Управление индексом) выберите индекс, который нужно удалить, и щелкните **Удалить**.  
  
5.  В окнах подтверждения нажимайте кнопку **ОК**.  
  
## <a name="see-also"></a>См. также:  
 [Создание индекса &#40;Master Data Services&#41;](../master-data-services/create-an-index-master-data-services.md)   
 [Пользовательский индекс (Master Data Services)](../master-data-services/custom-index-master-data-services.md)  
  
  

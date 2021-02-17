---
description: Создание индекса (Master Data Services)
title: Создание индекса
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: d694a105-69b1-4ff6-99d3-1f408b916b81
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: cad316eb2c1bd4925e72839ede2e29bccb825118
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351893"
---
# <a name="create-an-index-master-data-services"></a>Создание индекса (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Создайте пользовательский индекс списка часто запрашиваемых атрибутов, чтобы повысить производительность запросов.  
  
## <a name="prerequisites"></a>Предварительные требования  
 Для выполнения этой процедуры:  
  
-   Иметь разрешение на доступ к функциональной области "Администрирование системы". Дополнительные сведения см. в разделе [разрешения функциональной области &#40;Master Data Services&#41;](../master-data-services/functional-area-permissions-master-data-services.md).  
  
-   необходимо быть администратором модели. Дополнительные сведения см. в разделе [administrators &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
 **Создание индекса**  
  
1.  В диспетчере основных данных щелкните **Системное администрирование**.  
  
2.  На странице **Manage Model** (Управление моделью) выберите в сетке модель и щелкните **Сущности**.  
  
3.  На странице **Manage Entity** (Управление сущностью) выберите в **сетке** строку сущности, для которой необходимо создать индекс.  
  
4.  Щелкните **Индексы**.  
  
5.  В поле **Имя** введите имя индекса.  
  
6.  Выберите параметр **Уникальный** , если хотите создать уникальный индекс. Дополнительные сведения о типах индекса см. в разделе [Пользовательский индекс (Master Data Services)](../master-data-services/custom-index-master-data-services.md).  
  
7.  Выберите атрибуты в поле **Доступные атрибуты** и щелкните стрелку **Добавить** . Чтобы добавить все атрибуты, щелкните стрелку **Добавить все** .  
  
8.  Выберите команду **Сохранить**.  
  
 Для каждого созданного индекса в сетке создается строка с четырьмя столбцами. В следующей таблице приводятся описания этих столбцов.  
  
|Имя столбца|Описание|  
|-----------------|-----------------|  
|Состояние|Состояние индекса.<br /><br /> При нажатии кнопки **сохранить** отображается ![значок обновления состояния](../master-data-services/media/mds-statusicon-updating.png "Значок обновления состояния") , указывающий, что индекс обновляется.<br /><br /> При возникновении ошибок при создании или изменении индекса отображается изображение ![состояния ошибки значок](../master-data-services/media/mds-statusicon-error.png "Значок состояния ошибки") .<br /><br /> В противном случае отображается состояние ОК, а ![для значка состояния ОК](../master-data-services/media/mds-statusicon-ok.png "Значок состояния "ОК"") — изображение.|  
|Имя|Имя индекса.|  
|Уникальный|Указывает, является ли индекс уникальным.|  
|On Attributes|Показывает отображаемые имена атрибутов, для которых определен индекс.|  
  
 Если щелкнуть индекс, отображается следующая информация.  
  
-   **Кем создан**— имя пользователя, создавшего индекс.  
  
-   **Когда создан**— дата и время создания индекса.  
  
-   **Кем обновлено**— имя пользователя, выполнившего последнее обновление индекса.  
  
-   **Когда обновлено**— дата и время последнего обновления индекса.  
  
## <a name="next-steps"></a>Next Steps  
 [Изменение и удаление индекса (Master Data Services)](../master-data-services/edit-and-delete-an-index-master-data-services.md)  
  
## <a name="see-also"></a>См. также:  
 [Пользовательский индекс (Master Data Services)](../master-data-services/custom-index-master-data-services.md)  
  
  

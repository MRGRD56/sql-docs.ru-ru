---
title: База данных служб Master Data Services
description: База данных Master Data Services содержит все сведения о системе Master Data Services и является центральной частью развертывания Master Data Services.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- database [Master Data Services], about the database
- database [Master Data Services]
ms.assetid: 5f590cc1-6ec2-4b8c-a598-03de0f6051a0
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 59d430ea44a83d3795d7d3e3473917ed1c14345f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338204"
---
# <a name="master-data-services-database"></a>База данных служб Master Data Services

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  База данных содержит все сведения, необходимые для системы [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Она является основой для развертывания [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . База данных [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
-   В ней хранятся параметры, объекты базы данных и данные, необходимые системе [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
-   Содержит промежуточные таблицы, используемые для обработки данных из исходных систем.  
  
-   Предоставляет схему и объекты базы данных для хранения основных данных из исходных систем.  
  
-   Поддерживает управление версиями, в том числе проверку бизнес-правил и уведомления по электронной почте.  
  
-   Предоставляет представления для систем-подписчиков, которым надо извлекать данные из базы данных.  
  
## <a name="in-this-section"></a>В этом разделе  
  
-   [Конечный элемент таблицы элементов (службы Master Data Services)](../master-data-services/leaf-member-staging-table-master-data-services.md)  
  
-   [Промежуточная таблица консолидированных элементов (службы Master Data Services)](../master-data-services/consolidated-member-staging-table-master-data-services.md)  
  
-   [Промежуточная таблица связей (службы Master Data Services)](../master-data-services/relationship-staging-table-master-data-services.md)  
  
-   [Ошибки промежуточного процесса (службы Master Data Services)](../master-data-services/staging-process-errors-master-data-services.md)  
  
## <a name="see-also"></a>См. также:  
 [Создание базы данных Master Data Services](../master-data-services/install-windows/create-a-master-data-services-database.md)   
 [&#40;Master Data Services безопасности объектов базы данных&#41;](../master-data-services/database-object-security-master-data-services.md)   
 [Имена входа, пользователи и роли базы данных (службы Master Data Services)](../master-data-services/database-logins-users-and-roles-master-data-services.md)  
  
  

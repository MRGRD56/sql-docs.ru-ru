---
title: Добавление пользователя
description: Узнайте, как добавить пользователя в список пользователей в диспетчер основных данных. Необходимо добавить пользователя, чтобы начать процесс назначения разрешения для веб-приложения.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- users [Master Data Services], adding
- adding users [Master Data Services]
ms.assetid: 44262bdd-430c-4337-ac92-9333f54c7039
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: b3e42d4e173dddf2042a16fa1b32decb0cb3013a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347773"
---
# <a name="add-a-user-master-data-services"></a>Добавление пользователя (службы Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Чтобы начать назначение разрешений для веб-приложения, нужно добавить пользователя в список **Пользователи** в [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . Чтобы пользователь из этого списка мог воспользоваться средой [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)], ему нужно предоставить разрешение на одну или более функциональных областей и объектов модели.  
  
## <a name="prerequisites"></a>Предварительные требования  
 Для выполнения этой процедуры:  
  
-   необходимо иметь разрешение на доступ к функциональной области **Разрешения пользователей и групп** ;  
  
### <a name="to-add-a-user"></a>Добавление пользователя  
  
1.  В среде [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]щелкните **Разрешения пользователей и групп**.  
  
2.  На странице **Пользователи** нажмите кнопку **Добавить пользователя**.  
  
3.  Введите имя пользователя, которому предшествует Active Directory доменное имя, или имя компьютера сервера, например *домен* \\ *user_name* или *компьютер \ user_name*.  
  
4.  По желанию нажмите кнопку **Проверить имена**.  
  
5.  Нажмите кнопку **ОК**.  
  
## <a name="next-steps"></a>Next Steps  
  
-   [Назначение разрешений для функциональной области (службы Master Data Services)](../master-data-services/assign-functional-area-permissions-master-data-services.md)  
  
## <a name="see-also"></a>См. также:  
 [Безопасность (службы Master Data Services)](../master-data-services/security-master-data-services.md)  
  
  

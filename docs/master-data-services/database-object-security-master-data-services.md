---
description: Защита объектов базы данных (службы Master Data Services)
title: Защита объектов базы данных
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- database [Master Data Services], object security
- security [Master Data Services], database objects
ms.assetid: dd5ba503-7607-45d9-ad0d-909faaade179
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: dd693c0f1bdbb7651375f6cba1e68a60b889db04
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339064"
---
# <a name="database-object-security-master-data-services"></a>Защита объектов базы данных (службы Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  В базе данных [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] данные хранятся во многих таблицах базы данных, а также отображаются в представлениях. Информация, которая может быть защищена в веб-приложении [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] , видна пользователям, у которых есть доступ к базе данных [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .  
  
 В частности, в модели Employee могут содержаться сведения о заработной плате сотрудников, а в модели Account может присутствовать финансовая информация компании. В пользовательском интерфейсе [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] можно запретить пользователям доступ к этим моделям, но пользователи с доступом к базе данных все равно смогут просматривать такие данные.  
  
 Пользователям можно предоставлять разрешения на объекты базы данных, чтобы они могли работать с определенными частями данных. Дополнительные сведения о предоставлении разрешений см. в разделе [GRANT, предоставление разрешений на объект (Transact-SQL)](../t-sql/statements/grant-object-permissions-transact-sql.md). Дополнительные сведения о настройке безопасности сервера SQL см. в разделе [Securing SQL Server](../relational-databases/security/securing-sql-server.md).  
  
 Для следующих задач необходим доступ к базе данных [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] :  
  
-   [Промежуточные данные](#Staging)  
  
-   [Проверка данных на соответствие бизнес-правилам](#rules)  
  
-   [Удаление версий](#Versions)  
  
-   [Немедленное применение разрешений для элементов иерархии](#Hierarchy)  
  
-   [Настройка параметров системы](#SysSettings)  
  
##  <a name="staging-data"></a><a name="Staging"></a> Промежуточное сохранение данных  
 В следующей таблице каждый защищаемый объект имеет строку name в составе имени. Это указывает на имя промежуточной таблицы, которая определена при создании сущности. Дополнительные сведения см. в разделе [Обзор: импорт данных из таблиц (службы Master Data Services)](../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
|Действие|Защищаемые объекты|Разрешения|  
|------------|----------------|-----------------|  
|Создание, обновление и удаление конечных элементов и их атрибутов.|stg.name_Leaf|Требуется: INSERT<br /><br /> Необязательно: SELECT и UPDATE|  
|Загрузить данные из конечной промежуточной таблицы в соответствующие таблицы базы данных MDS.|stg.udp_name_Leaf|EXECUTE|  
|Создание, обновление и удаление консолидированных элементов и их атрибутов.|stg.name_Consolidated|Требуется: INSERT<br /><br /> Необязательно: SELECT и UPDATE|  
|Загрузить данные из объединенной промежуточной таблицы в соответствующие таблицы базы данных MDS.|stg.udp_name_Consolidated|EXECUTE|  
|Перемещение элементов в явную иерархию.|stg.name_Relationship|Требуется: INSERT<br /><br /> Необязательно: SELECT и UPDATE|  
|Загрузить данные из промежуточной таблицы связей в соответствующие таблицы базы данных MDS.|stg.udp_name_Relationship|EXECUTE|  
|Просмотреть ошибки, которые возникли при вставке данных из промежуточных таблиц в таблицы базы данных MDS.|stg.udp_name_Relationship|SELECT|  
  
 Дополнительные сведения см. в разделе [Обзор: импорт данных из таблиц &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md).  
  
##  <a name="validating-data-against-business-rules"></a><a name="rules"></a> Проверка данных на соответствие бизнес-правилам  
  
|Действие|Защищаемый объект|Разрешения|  
|------------|---------------|-----------------|  
|Проверка версии данных на соответствие бизнес-правилам|MDM.udpValidateModel|EXECUTE|  
  
 Дополнительные сведения см. в разделе [Проверка хранимых процедур (службы Master Data Services)](../master-data-services/validation-stored-procedure-master-data-services.md).  
  
##  <a name="deleting-versions"></a><a name="Versions"></a> Удаление версий  
  
|Действие|Защищаемые объекты|Разрешения|  
|------------|----------------|-----------------|  
|Определение идентификатора версии, которую необходимо удалить|mdm.viw_SYSTEM_SCHEMA_VERSION|SELECT|  
|Удаление версии модели|mdm.udpVersionDelete|EXECUTE|  
  
 Дополнительные сведения см. в разделе [Удаление версии (службы Master Data Services)](../master-data-services/delete-a-version-master-data-services.md).  
  
##  <a name="immediately-applying-hierarchy-member-permissions"></a><a name="Hierarchy"></a> Немедленное применение разрешений для элементов иерархии  
  
|Действие|Защищаемые объекты|Разрешения|  
|------------|----------------|-----------------|  
|Срочное применение разрешений для элемента|mdm.udpSecurityMemberProcessRebuildModel|EXECUTE|  
  
 Дополнительные сведения см. в разделе [Срочное применение разрешений для элемента (службы Master Data Services)](../master-data-services/immediately-apply-member-permissions-master-data-services.md).  
  
##  <a name="configuring-system-settings"></a><a name="SysSettings"></a> Настройка параметров системы  
 Эти параметры системы можно изменять, настраивая поведение в [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. Их можно настроить в [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] или при наличии доступа на выполнение команды UPDATE изменять непосредственно в таблице базы данных mdm.tblSystemSetting. Дополнительные сведения см. в разделе [Системные параметры (службы Master Data Services)](../master-data-services/system-settings-master-data-services.md).  
  
## <a name="see-also"></a>См. также:  
 [Безопасность (службы Master Data Services)](../master-data-services/security-master-data-services.md)  
  
  

---
title: Имена входа, пользователи и роли базы данных
description: Master Data Services включает имена входа, пользователей и роли, установленные на экземпляре ядро СУБД SQL Server, где размещена база данных Master Data Services.
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- security [Master Data Services], database roles
- database [Master Data Services], users
- security [Master Data Services], database users
- database [Master Data Services], roles
- database [Master Data Services], logins
- security [Master Data Services], database logins
ms.assetid: 72ee383e-a619-461b-9f9d-1cac162ab0c5
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e909388212b30942be2d11d53991f7e8d8756b35
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339077"
---
# <a name="database-logins-users-and-roles-master-data-services"></a>Имена входа, пользователи и роли базы данных (службы Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] включает имена входа, пользователей и роли, которые автоматически устанавливаются на экземпляр [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] , где размещена база данных [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Изменять этих пользователей, имена входа и роли не рекомендуется.  
  
## <a name="logins"></a>Имена входа  
  
|Имя для входа|Описание|  
|-----------|-----------------|  
|**mds_dlp_login**|Разрешает создание сборок UNSAFE. Дополнительные сведения см. в статье [Creating an Assembly](../relational-databases/clr-integration/assemblies/creating-an-assembly.md).<br /><br /> — Отключенное имя входа со случайно созданным паролем.<br /><br /> — Сопоставляет со схемой dbo для базы данных [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .<br /><br /> — Для msdb это имя входа сопоставляется с mds_clr_user.|  
|**mds_email_login**|Включенное имя входа, используемое для уведомлений.<br /><br /> Для msdb и базы данных [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] это имя входа сопоставляется с mds_email_user.|  
  
## <a name="msdb-users"></a>Пользователи msdb  
  
|User (Пользователь)|Описание|  
|----------|-----------------|  
|**mds_clr_user**|Не используется. Сопоставляется с mds_dlp_login.|  
|**mds_email_user**|Используется для уведомлений.<br /><br /> — Сопоставляется с mds_email_login.<br /><br /> — Является членом роли DatabaseMailUserRole.|  
  
## <a name="master-data-services-database-users"></a>Пользователи базы данных Master Data Services  
  
|User (Пользователь)|Описание|  
|----------|-----------------|  
|**mds_email_user**|Используется для уведомлений.<br /><br /> — Имеет разрешение SELECT для схемы mdm.<br /><br /> — Имеет разрешение EXECUTE для определяемого пользователем табличного типа mdm.MemberGetCriteria.<br /><br /> — Имеет разрешение EXECUTE для хранимой процедуры mdm.udpNotificationQueueActivate.|  
|**mds_schema_user**|Владеет схемами mdm и mdq. Схема по умолчанию — mdm.<br /><br /> Не имеет сопоставленного имени входа.|  
|**mds_ssb_user**|Используется для выполнения задач компонента Service Broker.<br /><br /> — Имеет разрешения DELETE, INSERT, REFERENCES, SELECT и UPDATE для всех схем.<br /><br /> — Не имеет сопоставленного имени входа.|  
  
## <a name="master-data-services-database-role"></a>Роль базы данных служб Master Data Services  
  
|Роль|Описание|Разрешения|  
|----------|-----------------|-----------------|  
|**mds_exec**|Эта роль содержит учетную запись, создаваемую в службах [!INCLUDE[ssMDScfgmgr](../includes/ssmdscfgmgr-md.md)] при создании веб-приложения [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] и учетной записи для пула приложений.|Разрешение EXECUTE для всех схем.<br /><br /> <br /><br /> Разрешения ALTER, INSERT и SELECT для следующих таблиц:<br /><br /> mdm.tblStgMember<br /><br /> mdm.tblStgMemberAttribute<br /><br /> mdm.tbleStgRelationship<br /><br /> <br /><br /> Разрешение SELECT для следующих таблиц:<br /><br /> mdm.tblUser<br /><br /> mdm.tblUserGroup<br /><br /> mdm.tblUserPreference<br /><br /> <br /><br /> Разрешение SELECT для следующих представлений:<br /><br /> mdm.viw_SYSTEM_SECURITY_NAVIGATION<br /><br /> mdm.viw_SYSTEM_SECURITY_ROLE_ACCCESSCONTROL<br /><br /> mdm.viw_SYSTEM_SECURITY_ROLE_ACCCESSCONTROL_MEMBER<br /><br /> mdm.viw_SYSTEM_SECURITY_USER_MODEL|  
  
## <a name="schemas"></a>Схемы  
  
|Роль|Описание|  
|----------|-----------------|  
|**Корпоратив**|Содержит все объекты базы данных [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] и компонента Service Broker кроме функций, содержащихся в схеме mdq.|  
|**mdq**|Содержит функции базы данных [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , относящиеся к фильтрации результирующих элементов на основе регулярных выражений или подобия, а также для форматирования уведомлений по электронной почте.|  
|**stg**|Содержит таблицы базы данных [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] , хранимые процедуры и представления, связанные с промежуточным процессом. Запрещается удалять любые из этих объектов. Дополнительные сведения см. в разделе [Обзор: импорт данных из таблиц (службы Master Data Services)](../master-data-services/overview-importing-data-from-tables-master-data-services.md).|  
  
## <a name="see-also"></a>См. также:  
 [Защита объектов базы данных (службы Master Data Services)](../master-data-services/database-object-security-master-data-services.md)  
  
  

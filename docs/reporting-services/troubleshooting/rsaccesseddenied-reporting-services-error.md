---
title: rsAccessedDenied — ошибка служб Reporting Services | Документы Майкрософт
description: "В этом разделе об ошибке содержатся дополнительные сведения об ошибке rsAccessedDenied: Предоставленные пользователю 'mydomain\\myAccount' разрешения недостаточны для выполнения данной операции."
ms.date: 05/22/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: troubleshooting
ms.topic: conceptual
helpviewer_keywords:
- rsAccessDenied error
ms.assetid: 2f76b1bf-96a2-4755-b76b-84e933220efc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 20d213cac8f3d3413266dc410b0d66879e771619
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054187"
---
# <a name="rsaccesseddenied---reporting-services-error"></a>rsAccessedDenied — ошибка служб Reporting Services
  Ошибка службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]**rsAccessedDenied** происходит, когда у пользователя нет разрешения на выполнение действия. Например, у пользователя нет назначения роли, позволяющей открывать отчет, или браузер был открыт без необходимых разрешений.  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]** [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в собственном режиме | в режиме интеграции с SharePoint|  
  
- Если ошибка произошла во время доступа к серверу отчетов напрямую при помощи URL-адреса, то это исключение соответствует ошибке HTTP 401.  
  
- Если произошла ошибка при использовании веб-портала, исключение обычно сопоставляется с ошибкой HTTP 401 или с другой определенной страницей ошибки HTML.  
  
- Если ошибка произошла во время запланированной операции, подписки или доставки, то сведения о ней появятся только в файле журнала сервера отчетов.  
  
## <a name="details"></a>Сведения  
  
|Подробный сведения|Значение|  
|-|-|  
|**Название продукта**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|**Идентификатор события**|rsAccessedDenied|  
|**Источник события**|Microsoft.ReportingServices.Diagnostics.Utilities.ErrorStrings|  
|**Компонент**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|  
|**Текст сообщения**|Предоставленные пользователю 'mydomain\myAccount' разрешения недостаточны для выполнения данной операции. (rsAccessDenied) (ReportingServicesLibrary)|  
  
## <a name="user-action"></a>Рекомендуемые действия  
 Разрешения на доступ к содержимому и операциям сервера отчетов назначаются при помощи ролей. В новой установке только у локальных администраторов есть доступ к серверу отчетов. Чтобы предоставить доступ другим пользователям, локальному администратору необходимо создать назначение ролей, описывающее учетную запись пользователя или группы домена, одну или несколько ролей, определяющих, какие задачи пользователь сможет выполнять, и область видимости (обычно корневую папку или корневой узел в иерархии папок сервера отчетов). На веб-портале можно создать назначения ролей. Подробные сведения см. в статье [Role Assignments](../../reporting-services/security/role-assignments.md) (Назначения ролей).  
  
 Данная ошибка вызывается также локальным администрирование сервера отчетов. Дополнительные сведения см. в разделе [Настройка сервера отчетов, работающего в основном режиме, для локального администрирования (службы SSRS)](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  
  
## <a name="see-also"></a>См. также раздел  
 [Назначения ролей](../../reporting-services/security/role-assignments.md)  
 [Предоставление разрешений на сервер отчетов в собственном режиме](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
 [Роли и разрешения (службы Reporting Services)](../../reporting-services/security/roles-and-permissions-reporting-services.md)  
  
---
title: Активация функций интеграции с сервером отчетов и Power View в SharePoint | Документы Майкрософт
description: Надстройка SQL Server Reporting Services для функций SharePoint обычно активируется автоматически. Используйте эти инструкции, если их необходимо активировать вручную.
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 2f79af39e2ba871a0ee4acf9f33c23f54f2bfd3b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461465"
---
# <a name="activate-the-report-server-and-power-view-integration-features-in-sharepoint"></a>Активация функций интеграции с сервером отчетов и Power View в SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

  Компоненты семейства веб-сайтов служб Reporting Services обычно активируются по умолчанию после установки надстройки [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] для продуктов SharePoint. В некоторых ситуациях необходимо активировать эти компоненты вручную.  

> [!NOTE]
> Интеграция служб Reporting Services с SharePoint больше не доступна после выхода SQL Server 2016.

 При установке надстройки служб Reporting Services для продуктов SharePoint 2010 после установки SharePoint функции интеграции с сервером отчетов и Power View будут активированы только для корневых семейств веб-сайтов. Для других семейств веб-сайтов необходимо активировать эти функции вручную. Например, если имеется семейство веб-сайтов **https://[имя_сервера]/sites/[имя_семейства_веб-сайтов]**, то необходимо вручную активировать функции семейства веб-сайтов служб Reporting Services.  
  
 Когда корневого семейства веб-сайтов нет, надстройка Reporting Services занесет в журнал сообщение, подобное приведенному ниже.  
  
 «Веб-приложение SharePoint 80 не содержит корневого семейства веб-сайтов»  
  
 Сообщение сохраняется в журнале установки надстройки с именем RS_SP_#.log, где # ― это номер с приращением. Файл журнала сохраняется в папку Temp текущего пользователя, например C:\Users\\[имя_пользователя]\AppData\Local\Temp. Дополнительные сведения о параметрах ведения журнала надстройки см. в разделе [Установка или удаление надстройки служб Reporting Services для SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md).  

## <a name="activate-the-report-server-and-power-view-integration-site-collection-features"></a>Активация функций семейства веб-сайтов для интеграции с сервером отчетов и Power View
  
1.  Откройте в браузере сайт, где нужно активировать функции служб Reporting Services.  
  
2.  Щелкните **Действия сайта**.  
  
3.  Щелкните элемент **Настройки сайта**.  
  
4.  Щелкните **Функции семейства веб-сайтов** в группе администраторов семейства веб-сайтов.  
  
5.  Найдите в списке **Функцию интеграции сервера отчетов** или **Функцию интеграции средства Power View** .  
  
6.  Нажмите кнопку **Активировать**.  
  
 Деактивация компонентов выполняется схожим образом, только вместо кнопки **Активировать** нужно нажать кнопку **Деактивировать**.  
  
## <a name="activate-or-deactivate-reporting-services-central-administration-site-collection-feature"></a>Активация и деактивация функции семейства веб-сайтов для центра администрирования служб Reporting Services
  
1.  Откройте в браузере центр администрирования SharePoint.  
  
2.  Щелкните **Действия сайта**.  
  
3.  Щелкните элемент **Настройки сайта**.  
  
4.  Щелкните **Функции семейства веб-сайтов** в группе администраторов семейства веб-сайтов.  
  
5.  Найдите в списке пункт **Функция центра администрирования сервера отчетов** .  
  
6.  Нажмите кнопку **Активировать**.  
  
 Деактивация компонента выполняется схожим образом, только вместо кнопки **Активировать** нужно нажать кнопку **Деактивировать**.  
  
## <a name="next-steps"></a>Дальнейшие действия

После активации компонента можно продолжить интеграцию сервера.

Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).

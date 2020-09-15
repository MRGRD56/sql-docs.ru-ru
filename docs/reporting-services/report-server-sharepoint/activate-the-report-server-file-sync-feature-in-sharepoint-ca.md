---
title: Активация функции синхронизации файлов на сервере отчетов в SharePoint | Документы Майкрософт
description: Функция синхронизации файлов сервера отчетов использует обработчики событий SharePoint для синхронизации каталога сервера отчетов с элементами в библиотеках документов.
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: e05dfa8cf6b519468ec631c5b6aa0eb49c040141
ms.sourcegitcommit: e8f6c51d4702c0046aec1394109bc0503ca182f0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2020
ms.locfileid: "87934269"
---
# <a name="activate-the-report-server-file-sync-feature-in-sharepoint"></a>Активация функции синхронизации файлов на сервере отчетов в SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

Функция синхронизации файлов сервера отчетов служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] использует обработчики событий SharePoint для синхронизации каталога сервера отчетов с элементами в библиотеках документов. Эта функция может оказаться полезной в том случае, если пользователи часто передают элементы опубликованных отчетов напрямую в библиотеки документов SharePoint. Если функция синхронизации файлов не включена, содержимое будет синхронизироваться, но не так часто.

> [!NOTE]
> Интеграция служб Reporting Services с SharePoint больше не доступна после выхода SQL Server 2016.
  
 Функция синхронизации файлов активируется в центре администрирования сайта SharePoint после установки надстройки служб [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] для продуктов SharePoint.  
  
 Эту функцию можно активировать и деактивировать вручную не на уровне семейства веб-сайтов, а раздельно для каждого сайта.  
  
## <a name="prerequisites"></a>Предварительные требования

 Надстройка служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] для SharePoint должна быть установлена. Если надстройка не установлена, функция синхронизации файлов не будет отображаться в списке функций сайта.  
  
 Чтобы проверить установку, просмотрите список установленных приложений на [!INCLUDE[msCoName](../../includes/msconame-md.md)]**панели управления** Windows. Если надстройка служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] установлена, следуйте инструкциям в этом разделе для активации функции синхронизации файлов сервера отчетов.  
  
### <a name="to-activate-or-deactivate-the-reporting-services-file-sync-feature-on-a-site"></a>Активация и деактивация функции синхронизации файлов служб Reporting Services на сайте
  
1.  На главной странице сайта откройте меню **Действия сайта** и выберите пункт **Настройки веб-сайта**.  
  
2.  В меню **Действия сайта** выберите пункт **Управление функциями сайта**.  
  
3.  Найдите в списке **Синхронизация файлов сервера отчетов** .  
  
4.  Нажмите кнопку **Активировать**.  

> [!NOTE]
> Деактивировать функцию синхронизации файлов сервера отчетов можно с помощью той же процедуры, но выбрав пункт **Деактивировать**.

## <a name="see-also"></a>См. также раздел

 [Активация функций интеграции семейства веб-сайтов с сервером отчетов и Power View в SharePoint](../../reporting-services/report-server-sharepoint/site-collection-features-report-server-and-power-view.md)   
 [Установка и удаление надстройки служб Reporting Services для SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)   
 [Установка и удаление надстройки служб Reporting Services для SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)  

Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).

---
title: Руководство по Поиск и запуск средств служб Reporting Services | Документация Майкрософт
description: Узнайте о средствах, используемых для настройки сервера отчетов, управления содержимым и операциями сервера отчетов, а также создания и публикации отчетов с разбиением на страницы и мобильных отчетов Reporting Services.
ms.date: 12/09/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.custom: seodec18
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, tutorials
- Reporting Services, tools
- Reporting Services Configuration tool
- Business Intelligence Development Studio, locating and starting tool
- Model Designer [Reporting Services], locating and starting tool
- Report Designer [Reporting Services], tutorials
- tools [Reporting Services]
- tutorials [Reporting Services]
- Report Builder
ms.assetid: 51ad69d8-fe92-4662-a7cd-d235692f0c03
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 60d621e9bb833615aaed5e6f622afb9591916037
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597066"
---
# <a name="tutorial-how-to-locate-and-start-reporting-services-tools-ssrs"></a>Руководство по Инструкции по поиску и запуску средств служб Reporting Services (SSRS)

В этом руководстве рассказывается о средствах, используемых для настройки сервера отчетов, управления содержимым и операциями сервера отчетов, а также создания и публикации отчетов с разбиением на страницы и мобильных отчетов [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Если вы уже знакомы с этими средствами, то можете перейти к другим руководствам, которые помогут научиться правильно использовать службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Другие учебные материалы см. в разделе [Учебники по службам Reporting Services (SSRS)](../../reporting-services/reporting-services-tutorials-ssrs.md).

## <a name="report-server-configuration-manager-native-mode"></a><a name="bkmk_configuration_manager"></a> Диспетчер конфигурации сервера отчетов (собственный режим)
Используйте диспетчер конфигурации для работы в основном режиме, чтобы сделать следующее.

- Укажите учетную запись службы.
- Создайте или обновите базу данных сервера отчетов.
- Измените свойства соединения.
- Укажите URL-адреса.
- Настройте ключи шифрования.
- Настройте автоматическую обработку отчетов и доставку отчетов по электронной почте.

**Установка**. Диспетчер конфигурации [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] устанавливается при установке служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в собственном режиме. Дополнительные сведения см. в разделе [Установка сервера отчетов служб Reporting Services в собственном режиме](../../reporting-services/install-windows/install-reporting-services-native-mode-report-server.md).

### <a name="to-start-the-report-server-configuration-manager"></a>Запуск диспетчера конфигурации сервера отчетов

1. На начальном экране Windows введите **отчет** и в результатах поиска **Приложения** выберите **Диспетчер конфигурации сервера отчетов**.

    ![Диспетчер конфигурации сервера отчетов на начальном экране](../../reporting-services/tools/media/bi-ssrs-configmanager-win8-startscreen.gif "Диспетчер конфигурации сервера отчетов на начальном экране")

    **Or**

    Нажмите кнопку **Пуск**, выберите пункт **Программы**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Средства конфигурации**, а затем **Диспетчер конфигурации сервера отчетов**.

    Появится диалоговое окно **Выбор экземпляра установки сервера отчетов** , в котором можно выбрать настраиваемый экземпляр сервера отчетов.

2. В поле **Имя сервера** укажите имя компьютера, на котором установлен экземпляр сервера отчетов. По умолчанию указывается имя локального компьютера, но можно ввести имя удаленного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

    Если указан удаленный компьютер, для установления соединения нажмите кнопку **Найти** . Сервер отчетов должен быть заранее настроен для удаленного администрирования. Дополнительные сведения о подготовке сервера отчетов для удаленного администрирования см. в разделе [Настройка сервера отчетов для удаленного администрирования](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md).

3. В поле **Имя экземпляра** выберите экземпляр служб [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], который вам нужно настроить. В списке отображаются только экземпляры сервера отчетов SQL Server 2008 и более поздней версии. Более ранние версии служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]настраивать нельзя.

4. Нажмите кнопку **Соединить**.

5. Убедиться, что средство запущено, можно, сравнив полученные результаты со следующим изображением:

    ![Средство конфигурации служб Reporting Services](../../reporting-services/tools/media/rs-ui-reportserverconfigkatmai.png "средство настройки служб Reporting Services")

 **Следующие шаги**. [Настройка и администрирование сервера отчетов (собственный режим SSRS)](../../reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode.md) и [Использование диспетчера конфигурации сервера отчетов (собственный режим)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).

## <a name="web-portal-native-mode"></a>Веб-портал (основной режим служб SSRS)

Используйте [Web portal (SSRS Native Mode)](../../reporting-services/web-portal-ssrs-native-mode.md) для задания разрешений, управления подписками и расписаниями, а также для работы с отчетами. Можно также использовать веб-портал для просмотра отчетов.

**Установка**. Веб-портал устанавливается при установке служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в собственном режиме: [Установка сервера отчетов служб Reporting Services в собственном режиме](../../reporting-services/install-windows/install-reporting-services-native-mode-report-server.md)

Для работы с веб-порталом необходимо иметь соответствующие разрешения (изначально разрешения на доступ к функциональным возможностям веб-порталом имеют только члены группы локальных администраторов). Веб-портал предоставляет различные страницы и параметры, в зависимости от назначений роли текущего пользователя. Пользователи, у которых нет разрешения, получат пустую страницу. Пользователи, которые имеют разрешения на просмотр отчетов, получат ссылки для открытия отчетов. Дополнительные сведения о разрешениях см. в разделе [Роли и разрешения &#40;службы Reporting Services&#41;](../../reporting-services/security/roles-and-permissions-reporting-services.md).

### <a name="to-start-the-web-portal"></a>Запуск веб-портала

1. Откройте браузер. Сведения о поддерживаемых браузерах и версиях см. в разделе [Поддержка браузера для служб Reporting Services и Power View](../../reporting-services/browser-support-for-reporting-services-and-power-view.md).

2. В адресной строке браузера введите URL-адрес веб-портала. URL-адрес по умолчанию — `https://<serverName>/reports`. Можно использовать программу настройки служб Reporting Services для подтверждения имени сервера и URL-адреса. Дополнительные сведения об URL-адресах, используемых в [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], см. в разделе [Настройка URL-адресов сервера отчетов (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md).

3. Веб-портал откроется в окне браузера. Стартовой страницей является корневая папка. В зависимости от разрешений на стартовой странице могут быть видны дополнительные папки, гиперссылки на отчеты и файлы ресурсов. На панели инструментов расположены дополнительные кнопки и команды.

4. Если веб-портал запускается на локальном сервере отчетов, см. раздел [Настройка сервера отчетов, работающего в собственном режиме, для локального администрирования (SSRS)](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).

## <a name="management-studio"></a><a name="bkmk_managements_studio"></a> Среда Management Studio

Администраторы сервера отчетов могут использовать среду [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] для управления сервером отчетов наряду с другими серверными компонентами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в учебнике по [SQL Server Management Studio](../../ssms/quickstarts/ssms-connect-query-sql-server.md).

### <a name="to-start-sql-server-management-studio"></a>Начало работы в среде SQL Server Management Studio

1. На начальном экране Windows введите **sql server** и в результатах поиска **Приложения** выберите **SQL Server Management Studio**.

    ![Management Studio на начальном экране Windows](../../reporting-services/tools/media/bi-ssms-win8-startscreen.gif "Management Studio на начальном экране Windows")

    **Or**

    Нажмите кнопку **Пуск**, а затем последовательно выберите **Все программы**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]и **SQL Server Management Studio**. Откроется диалоговое окно **Соединение с сервером** .

2. Если диалоговое окно **Соединение с сервером** отсутствует, то в **обозревателе объектов** нажмите **Подключиться** и выберите **службы Reporting Services**.

3. В раскрывающемся списке **Тип сервера** выберите **Службы Reporting Services**. Если службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] не находятся на списке, значит, они не установлены.

4. В раскрывающемся списке **Имя сервера** выберите экземпляр сервера отчетов. В этом списке появятся локальные экземпляры. Можно также ввести имя удаленного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

5. Нажмите кнопку **Соединить**. Предусмотрена возможность развернуть корневой узел, чтобы задать свойства сервера, изменить определения ролей или отключить те или иные компоненты сервера отчетов.

## <a name="sql-server-data-tools-with-report-designer-and-report-wizard"></a><a name="bkmk_ssdt"></a> SQL Server Data Tools с конструктором и мастером отчетов

Отчеты [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с разбиением на страницы можно создать с помощью двух разных средств: конструктора отчетов и [построителя отчетов](#bkmk_report_builder).

Конструктор отчетов доступен в [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] в среде Visual Studio. Рабочая область конструирования в конструкторе отчетов содержит окна, мастера и меню на вкладках, используемые для доступа к функциям создания отчетов. Средство конструктора отчетов становится доступным при выборе шаблона проекта сервера отчетов или мастера сервера отчетов в средствах [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Дополнительные сведения см. в разделе [Службы Reporting Services в SQL Server Data Tools (SSDT)](../../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md).

Скачать [SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md).

### <a name="to-start-report-designer"></a>Запуск конструктора отчетов

1. Откройте **SQL Server Data Tools**.

2. В меню **Файл** укажите **Создать**, затем нажмите **Проект**.

3. В списке **Типы проектов** выберите значение **Проекты бизнес-аналитики**.

4. В списке **Шаблоны** выберите значение **Проект сервера отчетов**. На следующей диаграмме показано, как шаблоны проекта выглядят в диалоговом окне.

    ![Диалоговое окно "Создание шаблона проекта"](../../reporting-services/tools/media/rs-ui-newrsproject.gif "Диалоговое окно "Создание шаблона проекта"")

5. Введите имя и местоположение проекта или нажмите кнопку **Обзор** и выберите местоположение.

6. Откроется [!INCLUDE[clickOK](../../includes/clickok-md.md)] [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] и отобразит начальную страницу [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. В обозревателе решений содержатся категории для создания отчетов и источников данных. Эти категории можно использовать для создания новых отчетов и источников данных. При создании определения отчета появляются окна со вкладками. К окнам со вкладками относятся такие окна, как «Данные», «Макет» и «Предварительный просмотр».

Чтобы приступить к работе над первым отчетом, см. раздел [Создание простого табличного отчета &#40;учебник по службам SSRS&#41;](../../reporting-services/create-a-basic-table-report-ssrs-tutorial.md). Дополнительные сведения о конструкторах запросов, которые можно использовать в конструкторе отчетов, см. в разделе [Средства проектирования запросов (SSRS)](../../reporting-services/report-data/query-design-tools-ssrs.md).

## <a name="ssrbnoversion"></a><a name="bkmk_report_builder"></a> [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]

[Построитель отчетов в SQL Server](../../reporting-services/report-builder/report-builder-in-sql-server-2016.md) — это автономное приложение, с помощью которого можно создавать отчеты с разбиением на страницы вне [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Существующие отчеты можно настраивать и обновлять независимо от того, были ли они созданы в конструкторе отчетов или в предыдущих версиях [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]. Его можно установить с веб-портала [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или из Центра загрузки Майкрософт.

Когда отчет с разбивкой на страницы будет готов, опубликуйте его на сервере отчетов или [сохраните его в службе Power BI](/power-bi/paginated-reports-save-to-power-bi-service).
[Построитель отчетов можно скачать](https://go.microsoft.com/fwlink/?LinkID=219138) из Центра загрузки Майкрософт.

### <a name="to-start-ssrbnoversion"></a>Запуск [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]

1. На веб-портале [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в меню **Создание** выберите пункт **Отчет с разбиением на страницы**.

    ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png "PBI_SSMRP_NewMenu")

2. Если [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] еще не установлен на этом компьютере, выберите пункт **Get [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]** .

    либо

    [Построитель отчетов можно скачать](https://go.microsoft.com/fwlink/?LinkID=219138) из Центра загрузки Майкрософт.

3. [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] , и можно будет создать или открыть отчет с разбиением на страницы.

## <a name="ss_mobilereptpub_long"></a><a name="bkmk_mobile_report_pub"></a> [!INCLUDE[SS_MobileReptPub_Long](../../includes/ss-mobilereptpub-long.md)]

[Издатель мобильных отчетов SQL Server](../mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md) позволяет создавать мобильные отчеты, которые можно просмотреть на веб-портале [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и на мобильных устройствах, например iPad и iPhone.
Его можно установить с веб-портала [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или из Центра загрузки Майкрософт.

[Издатель мобильных отчетов для SQL Server можно скачать](https://go.microsoft.com/fwlink/?LinkID=733527) из Центра загрузки Майкрософт.

### <a name="to-start-ss_mobilereptpub_short"></a>Запуск [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)]

1. На веб-портале [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в меню **Создание** выберите пункт **Мобильный отчет**.

     ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png "PBI_SSMRP_NewMenu")

2. Если [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] еще не установлен на этом компьютере, выберите пункт **Get [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)]** .

    либо

    [Издатель мобильных отчетов для SQL Server можно скачать](https://go.microsoft.com/fwlink/?LinkID=733527) из Центра загрузки Майкрософт.

3. [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] , и можно будет создать или открыть мобильный отчет.

## <a name="next-steps"></a>Дальнейшие действия

[Издатель мобильных отчетов для SQL Server можно скачать](https://go.microsoft.com/fwlink/?LinkID=733527)  
[Построитель отчетов можно скачать](https://go.microsoft.com/fwlink/?LinkID=219138)  
[Скачивание SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)  
[Установка режима интеграции с SharePoint для служб Reporting Services](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)  
[Сервер отчетов служб Reporting Services](../../reporting-services/report-server-sharepoint/reporting-services-report-server.md)  
[Средства проектирования запросов](../../reporting-services/report-data/query-design-tools-ssrs.md)  
[Учебники по службам Reporting Services](../../reporting-services/reporting-services-tutorials-ssrs.md)  

Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
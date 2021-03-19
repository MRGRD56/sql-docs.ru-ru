---
description: Обновление Power Pivot для SharePoint
title: Обновление Power Pivot для SharePoint | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 80ba9e43-f3f0-4730-9fb1-2afd2dd3e6fc
author: Minewiskan
ms.author: owend
monikerRange: '>=sql-server-2016'
manager: erikre
ms.openlocfilehash: 971c23cfe9b49ad052c3808deca6e6221acb38e5
ms.sourcegitcommit: bf7577b3448b7cb0e336808f1112c44fa18c6f33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2021
ms.locfileid: "104610931"
---
# <a name="upgrade-power-pivot-for-sharepoint"></a>Обновление Power Pivot для SharePoint

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  В этой статье описаны шаги, необходимые для обновления развертывания [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] до [!INCLUDE[ssGeminiLong](../../includes/ssgeminilong-md.md)]. Конкретные действия зависят от версии SharePoint, в которой в настоящее время выполняется среда, и включают [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для надстройки SharePoint (**spPowerPivot.msi**).  
  
 **[!INCLUDE[applies](../../includes/applies-md.md)]**  SharePoint 2010 | SharePoint 2013  
  
 Сведения о выпуске см. в статье [SQL Server 2016 Release Notes](../../sql-server/sql-server-2016-release-notes.md)(Заметки о выпуске SQL Server 2016).  
  
 **В этой статье:**  
  
 [Предварительные требования](#bkmk_prereq)  
  
 [Обновление существующей фермы SharePoint 2013](#bkmk_uprgade_sharepoint2013)  
  
 [Обновление существующей фермы SharePoint 2010](#bkmk_uprgade_sharepoint2010)  
  
 [книги](#bkmk_workbooks)  
  
 [Обновление данных](#bkmk_datarefresh)  
  
 [Проверка версий компонентов и служб Power Pivot](#bkmk_verify_versions)  
  
 [Обновление нескольких серверов Power Pivot для SharePoint в ферме SharePoint](#geminifarm)  
  
 [Применение QFE к экземпляру Power Pivot в ферме](#qfe)  
  
 [Задачи проверки после обновления](#verify)  
  
## <a name="background"></a>Историческая справка  
  
-   При обновлении многосерверной фермы SharePoint 2010 с двумя или более экземпляров [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] необходимо полностью обновить каждый сервер, **прежде** чем переходить к следующему серверу. Полное обновление включает запуск программы установки SQL Server для обновления программных файлов [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] , вслед за чем выполняются действия по обновлению SharePoint, которые состоят в настройке обновленных служб. Доступность сервера будет ограничена, пока не выполнены действия по обновлению в подходящем средстве настройки [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] или Windows PowerShell.  
  
-   Все экземпляры системной службы [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] и служб Analysis Services в ферме SharePoint 2010 должны иметь одинаковые версии. Сведения о том, как проверить версию, см. в разделе [Проверка версий компонентов и служб Power Pivot](#bkmk_verify_versions) этой статьи.  
  
-   Средства настройки [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] — это одни из общих компонентов SQL Server. Все общие компоненты обновляются одновременно. Если во время обновления будут выбраны другие экземпляры SQL Server или функции, для которых требуется обновление общих компонентов, то средство настройки [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] также будет обновлено. Если средство настройки [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] обновлено, а экземпляр [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] — нет, могут возникнуть проблемы. Дополнительные сведения об общих функциях SQL Server см. в разделе [Обновление до SQL Server 2016 с использованием мастера установки (Setup)](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md).  
  
-   Надстройка [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint (**spPowerPivot.msi**) устанавливается параллельно предыдущим версиям. Например, надстройка устанавливается в папку `c:\Program Files\Microsoft SQL Server\nnn\Tools\PowerPivotTools`. Сведения о файлах установки SQL Server см. в разделе о [расположениях файлов](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md#shared-files-for-all-instances-of-).
  
##  <a name="prerequisites"></a><a name="bkmk_prereq"></a> Предварительные требования  
 **Разрешения**  
  
-   Обновить [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint может лишь администратор фермы. Для запуска программы установки SQL Server необходимо быть локальным администратором.  
  
-   Необходимо иметь разрешения **db_owner** в базе данных конфигураций фермы.  
  
 **SQL Server:**  
  
-   Если существующая установка [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] имеет вид [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] с пакетом обновления 2 (SP2) требуется для обновления до версии [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)].  
  
-   Если существующая установка [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] имеет вид [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] с пакетом обновления 1 (SP1) требуется для обновления до версии [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)].  
  
 **SharePoint 2010.**  
  
-   Если существующая установка выполняет SharePoint 2010, установите пакет обновления 2 (SP2) для SharePoint 2010 перед обновлением до [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)][!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]. Дополнительные сведения см. в разделе [Пакет обновления 2 (SP2) для Microsoft SharePoint 2010](https://www.microsoft.com/download/details.aspx?id=39672). Используйте команду PowerShell `(Get-SPfarm).BuildVersion.ToString()` , чтобы проверить версию. Для получения информации о дате выпуска по версии сборки см. раздел [Номера сборок в SharePoint 2010](https://www.toddklindt.com/blog/Lists/Posts/Post.aspx?ID=224).  
  
##  <a name="upgrade-an-existing-sharepoint-2013-farm"></a><a name="bkmk_uprgade_sharepoint2013"></a> Обновление существующей фермы SharePoint 2013  
 Для обновления [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] , развернутых в SharePoint 2013, выполните следующие действия.  
  
 ![обновление powerpivot для sharepoint 2013](../../database-engine/install-windows/media/as-powepivot-upgrade-flow-sharepoint2013.png "обновление powerpivot для sharepoint 2013")  
  
1.  Запустите программу установки [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] на внутренних серверах, выполняющих [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] в режиме интеграции с SharePoint. Если сервер содержит несколько экземпляров [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], обновите по крайней мере экземпляр **POWERPIVOT** . В следующем списке приведена сводка шагов мастера установки, относящихся к обновлению [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] :  
  
    1.  В мастере установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] нажмите кнопку **Установка**.  
  
    2.  Выберите **Обновление с SQL Server...** .  
  
    3.  На странице **Выбор экземпляра** выберите экземпляр с именем **POWERPIVOT** и нажмите кнопку **Далее**.  
  
    4.  Дополнительные сведения см. в разделе [Обновление до SQL Server 2016 с использованием мастера установки (программа настройки)](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  
  
2.  Перезапустите сервер.  
  
3.  Запустите надстройку [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint (**spPowerPivot.msi**) на каждом сервере в ферме SharePoint 2013, чтобы установить поставщики данных. Исключением будут серверы, где вы запустили мастер установки SQL Server, который также обновляет поставщики данных. Дополнительные сведения см. в разделах [Загрузка Microsoft SQL Server 2014 Power Pivot для Microsoft SharePoint 2013](https://www.microsoft.com/download/details.aspx?id=42300) и [Установка или удаление надстройки Power Pivot для SharePoint (SharePoint 2013)](/analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013).  
  
4.  **Запустите надстройку [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint 2013** на одном из серверов приложений SharePoint, чтобы настроить ферму SharePoint, используя обновленные файлы решений, которые были установлены надстройкой. На этом шаге нельзя использовать центр администрирования SharePoint. Дополнительные сведения см. в следующих разделах:  
  
    1.  На начальной странице в Windows введите **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** и в результатах поиска щелкните **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint 2013**. Обратите внимание, что в результатах поиска могут быть возвращены обе версии средства настройки.  
  
         ![два средства настройки powerpivot](/analysis-services/analysis-services/instances/install-windows/media/as-powerpivot-configtools-bothicons.gif "два средства настройки powerpivot")  
  
         либо  
  
         В меню **Пуск** последовательно выберите пункты **Все программы**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Средства настройки**, **[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint 2013**. Обратите внимание, что это средство присутствует в списке вариантов, только если на локальном сервере установлен компонент [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] .  
  
    2.  Во время запуска средство настройки проверяет состояние обновления решения фермы [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] и решений веб-приложений [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . При обнаружении более старых версий решений появится сообщение: "**Обнаружены более новые версии файлов решений [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]. Выберите параметр обновления для обновления фермы**". Нажмите кнопку **ОК** , чтобы закрыть сообщение проверки системы.  
  
    3.  Выберите **Удаление компонентов, служб, приложений и решений** и нажмите кнопку **ОК**.  
  
    4.  Просмотрите действия в списке задач левой панели и исключите те, которые не хотите применять. По умолчанию в список включены все действия. Чтобы удалить действие, выберите его в левом списке задач и снимите флажок напротив параметра **Включить это действие в список задач** на странице **Параметры** .  
  
    5.  При необходимости просмотрите подробную информацию на вкладке **Скрипт** или **Вывод** .  
  
         Вкладка «Вывод» содержит краткое описание действий, которые будут выполнены средством. Эти сведения сохраняются в файлах журналов, расположенных в папке `C:\Program Files\Microsoft SQL Server\130\Tools\PowerPivotTools\SPAddinConfiguration\Log`.  
  
         Вкладка «Скрипт» содержит командлеты PowerShell или ссылки на файлы скриптов PowerShell, которые будут выполнены средством.  
  
    6.  Нажмите кнопку **Проверка** , чтобы убедиться, что каждое действие является допустимым. Если кнопка **Проверка** недоступна, значит, все действия являются допустимыми. Если кнопка **Проверка** доступна, возможно, вы изменили входное значение (например, имя приложения службы Excel), либо средство определило, что определенное действие выполнить невозможно. Если действие не может быть выполнено, следует исключить его или исправить базовые условия, из-за которых действие было отмечено как недопустимое.  
  
        > [!IMPORTANT]  
        >  Первое действие, **Обновление решения фермы**, должно всегда обрабатываться в первую очередь. Это действие регистрирует командлеты PowerShell, которые используются для настройки сервера. При получении ошибки, связанной с этим действием, не продолжайте операцию. Вместо этого используйте сведения из сообщения об ошибке для диагностики и разрешения проблемы, прежде чем обрабатывать другие действия в списке задач.  
  
    7.  Нажмите кнопку **Запуск** , чтобы выполнить все действия, которые являются допустимыми для данной задачи. Кнопка **Запуск** становится доступной только после прохождения проверки. При нажатии кнопки **Запуск** появится предупреждение о том, что действия обрабатываются в пакетном режиме: "**Все параметры конфигурации, отмеченные в средстве как действительные, будут применены к ферме SharePoint. Продолжить?** ".  
  
    8.  Чтобы продолжить, нажмите кнопку **Да** .  
  
    9. Обновление решений и компонентов в ферме может занять несколько минут. В это время запросы на подключение для данных [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]**будут завершаться сбоем** с ошибками типа "**Не удается обновить данные**" или "**При попытке выполнить запрошенное действие произошла ошибка. Повторите попытку**". После завершения обновления сервер станет доступным, и эти ошибки больше не будут возникать.  
  
     Дополнительные сведения см. в следующих разделах:  
  
    -   [Средства настройки PowerPivot](/analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools)  
  
    -   [Настройка или восстановление Power Pivot для SharePoint 2013 (Средство настройки Power Pivot)](/analysis-services/power-pivot-sharepoint/configure-or-repair-power-pivot-for-sharepoint-2013)  
  
    -   [Настройка PowerPivot с помощью Windows PowerShell](/analysis-services/power-pivot-sharepoint/power-pivot-configuration-using-windows-powershell)  
  
    -   [Справочник по PowerShell для Power Pivot для SharePoint](/analysis-services/powershell/powershell-reference-for-power-pivot-for-sharepoint)  
  
5.  Убедитесь, что обновление выполнено успешно, выполнив рекомендуемые действия после обновления и проверив версию сервера [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] в ферме. Дополнительные сведения см. в разделе [Задачи проверки после обновления](#verify) и следующем разделе этой статьи.  
  
##  <a name="upgrade-an-existing-sharepoint-2010-farm"></a><a name="bkmk_uprgade_sharepoint2010"></a> Обновление существующей фермы SharePoint 2010  
 Для обновления [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] , развернутых в SharePoint 2010, выполните следующие действия.  
  
 ![обновление powerpivot для sharepoint 2010](../../database-engine/install-windows/media/as-powepivot-upgrade-flow-sharepoint2010.png "обновление powerpivot для sharepoint 2010")  
  
1.  Загрузите [Пакет обновления 2 (SP2) для Microsoft SharePoint 2010](https://www.microsoft.com/download/details.aspx?id=39672) и примените его на всех серверах в ферме. Проверьте успешность установки SharePoint с пакетом обновления 2 (SP2). В центре администрирования на странице «Обновление и миграция» откройте страницу состояния «Проверка установки продукта и обновлений», чтобы просмотреть сообщения о состоянии, относящиеся к пакету обновления 2 (SP2).  
  
2.  Убедитесь, что служба администрирования Windows SharePoint 2010 запущена.  
  
    ```  
    Get-Service | where {$_.displayname -like "*SharePoint*"}  
    ```  
  
3.  Убедитесь, что службы **SQL Server Analysis Services** и **SQL Server [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] System Service** служб **SharePoint** установлены в центре администрирования SharePoint или воспользуйтесь следующей командой PowerShell:  
  
    ```  
    get-SPserviceinstance | where {$_.typename -like "*sql*"}  
    ```  
  
4.  Убедитесь, что службы **SQL Server Analysis Services службы **Windows** ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)])** запущены.  
  
    ```  
    Get-Service | where {$_.displayname -like "*powerpivot*"}  
    ```  
  
5.  **Запустите установку [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]** на первом сервере приложений SharePoint, где выполняются службы Windows **SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)])** , чтобы обновить экземпляр POWERPIVOT. На странице «Установка» мастера установки SQL Server выберите вариант обновления. Дополнительные сведения см. в разделе [Обновление до SQL Server 2016 с использованием мастера установки (программа установки)](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md).  
  
6.  **Перезапустите сервер** перед запуском средства настройки. В этом случае любые обновления или необходимые компоненты, установленные программой установки SQL Server, будут полностью настроены в системе.  
  
7.  **Запустите средство настройки [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** на первом сервере приложений SharePoint, где выполняется служба SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]), чтобы обновить решения и веб-службы в SharePoint. На этом шаге нельзя использовать центр администрирования.  
  
    1.  В меню **Пуск** укажите на пункт **Все программы**, щелкните [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Средства настройки** и **Средство настройки [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]** . Обратите внимание, что это средство присутствует в списке вариантов, только если на локальном сервере установлен компонент [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] .  
  
    2.  Во время запуска средство настройки проверяет состояние обновления решения фермы [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] и решений веб-приложений [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . При обнаружении более старых версий решений появится сообщение: "Обнаружены более новые версии файлов решений [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]. Выберите параметр обновления для обновления фермы". Нажмите кнопку **OК** , чтобы закрыть окно сообщения.  
  
    3.  Для продолжения выберите **Обновление компонентов, служб, приложений и решений** и нажмите кнопку **ОК** .  
  
    4.  Появится следующее предупреждение: "Книги на панели управления [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] будут обновлены до последней версии. Все настройки, внесенные в существующие книги, будут утеряны. Продолжить?".  
  
         Это предупреждение относится к книгам на панели мониторинга для управления [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , связанным с обновлением данных. Если эти книги были настроены, изменения, внесенные в книги, будут утеряны при замене существующих файлов файлами новой версии.  
  
         Нажмите кнопку **Да** для перезаписи книг книгами более новой версии. В противном случае нажмите кнопку **Нет** , чтобы вернуться на главную страницу. Сохраните книги в другой папке, чтобы иметь их копии, и затем вернитесь к этому шагу для продолжения.  
  
         Дополнительные сведения о настройке книг, используемых на панели управления, см. в статье [Customizing the Power Pivot Management Dashboard](/previous-versions/sql/sql-server-2008-r2/ff718155(v=sql.105))(Настройка панели управления PowerPivot).  
  
    5.  Просмотрите действия в списке задач и исключите те, которые средство настройки не должно выполнять. По умолчанию в список включены все действия. Чтобы удалить действие, выберите его в списке задач и снимите флажок напротив параметра **Включить это действие в список задач** на странице «Параметры».  
  
    6.  При необходимости просмотрите подробную информацию на вкладке **Вывод** или **Скрипт** .  
  
         Вкладка «Вывод» содержит краткое описание действий, которые будут выполнены средством. Эти сведения сохраняются в файлах журналов, расположенных в папке `c:\Program Files\Microsoft SQL Server\130\Tools\PowerPivotTools\ConfigurationTool\Log`.  
  
         Вкладка «Скрипт» содержит командлеты PowerShell или ссылки на файлы скриптов PowerShell, которые будут выполнены средством.  
  
    7.  Нажмите кнопку **Проверка** , чтобы убедиться, что каждое действие является допустимым. Если кнопка **Проверка** недоступна, значит, все действия являются допустимыми. Если кнопка **Проверка** доступна, возможно, вы изменили входное значение (например, имя приложения службы Excel), либо средство определило, что определенное действие выполнить невозможно. Если действие не может быть выполнено, следует исключить его или исправить базовые условия, из-за которых действие было отмечено как недопустимое.  
  
        > [!IMPORTANT]  
        >  Первое действие, **Обновление решения фермы**, должно всегда обрабатываться в первую очередь. Это действие регистрирует командлеты PowerShell, которые используются для настройки сервера. При получении ошибки, связанной с этим действием, не продолжайте операцию. Вместо этого используйте сведения из сообщения об ошибке для диагностики и разрешения проблемы, прежде чем обрабатывать другие действия в списке задач.  
  
    8.  Нажмите кнопку **Запуск** , чтобы выполнить все действия, которые являются допустимыми для данной задачи. Кнопка **Запуск** становится доступной только после прохождения проверки. При нажатии кнопки **Запуск** появится предупреждение о том, что действия обрабатываются в пакетном режиме: "Все параметры конфигурации, отмеченные в средстве как действительные, будут применены к ферме SharePoint. Продолжить?".  
  
    9. Чтобы продолжить, нажмите кнопку **Да** .  
  
    10. Обновление решений и компонентов в ферме может занять несколько минут. В это время запросы на подключение для данных [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] будут завершаться с ошибками "Не удалось обновить данные" или "При выполнении запрошенного действия возникла ошибка. Повторите попытку". После завершения обновления сервер станет доступным, и эти ошибки больше не будут возникать.  
  
8.  **Повторите эту процедуру** для каждой службы SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]) в ферме: 1) запустите настройку SQL Server; 2) запустите средство конфигурации [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)].  
  
9. Убедитесь, что обновление выполнено успешно, выполнив рекомендуемые действия после обновления и проверив версию сервера [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] в ферме. Дополнительные сведения см. в разделе [Задачи проверки после обновления](#verify) и следующем разделе этой статьи.  
  
10. **Устранение ошибок**  
  
     На панели «Параметры» для каждого действия можно просмотреть сведения об ошибке.  
  
     Для решения проблем, связанных с развертыванием или откатом решения, убедитесь, что служба администрирования SharePoint 2010 запущена. Эта служба запускает задания таймера, которые вызывают изменения конфигурации фермы. Если служба не запущена, развертывание или откат решения будет невозможен. Повторяющиеся ошибки указывают на то, что существующее задание развертывания или отката уже поставлено в очередь и происходит блокирование дальнейших действий средства настройки.  
  
    1.  Запустите консоль управления SharePoint 2010 с учетной записью администратора и выполните следующую команду для просмотра заданий, поставленных в очередь:  
  
        ```  
        Stsadm -o enumdeployments  
        ```  
  
    2.  Просмотрите существующие развертывания для уточнения следующих сведений: **Тип** — откат или развертывание, **Файл** — powerpivotwebapp.wsp или powerpivotfarm.wsp.  
  
    3.  Для развертываний или откатов, связанных с решениями [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)], скопируйте значение GUID для **JobId** и вставьте его в следующую команду (воспользуйтесь командами "Пометить", "Копировать" и "Вставить" в меню правки Shell, чтобы копировать GUID):  
  
        ```  
        Stsadm -o canceldeployment -id "<GUID>"  
        ```  
  
    4.  Повторите задачу в средстве настройки, нажав кнопку **Проверка** , а затем кнопку **Запуск**.  
  
     Сведения о других ошибках см. в журналах ULS. Дополнительные сведения см. в разделе [Настройка и просмотр файлов журнала SharePoint и журнала диагностики (Power Pivot для SharePoint)](/analysis-services/power-pivot-sharepoint/configure-and-view-sharepoint-and-diagnostic-logging).  
  
##  <a name="workbooks"></a><a name="bkmk_workbooks"></a> книги  
 При обновлении сервера книги [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , расположенные на сервере, необязательно обновляются, но старые книги, созданные в предыдущей версии [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для Excel, будут работать как и прежде, используя функции, доступные в старом выпуске. Работоспособность книг сохраняется, поскольку обновленный сервер имеет версию поставщика OLE DB служб Analysis Services, который был частью предыдущей установки.  
  
##  <a name="data-refresh"></a><a name="bkmk_datarefresh"></a> Обновление данных  
 Обновление повлияет на операции обновления данных. Плановое обновление данных на сервере доступно только для книг, соответствующих версии сервера. Если имеются книги предыдущей версии, то обновление данных для этих книг может перестать работать. Чтобы снова обеспечить возможность обновления данных, необходимо выполнить обновление этих книг. Можно вручную обновить каждую книгу в [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для Excel или включить автоматическое обновление для функции обновления данных в SharePoint 2010. Автоматическое обновление обновит книгу до текущей версии перед тем, как выполнять обновление данных, что позволяет выполнять операции обновления данных по расписанию.  
  
##  <a name="verify-the-versions-of-power-pivot-components-and-services"></a><a name="bkmk_verify_versions"></a> Проверка версий компонентов и служб Power Pivot  
 Все экземпляры служб [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] PowerPivot System Service и Analysis Services должны иметь одну и ту же версию. Чтобы убедиться, что все компоненты сервера имеют одну и ту же версию, проверьте сведения о версии следующих объектов.  
  
### <a name="verify-the-version-of-power-pivot-solutions-and-the-power-pivot-system-service"></a>Проверка версий решений Power Pivot и системной службы Power Pivot  
 Выполните следующую команду PowerShell:  
  
```  
Get-PowerPivotSystemService  
```  
  
 Проверьте **CurrentSolutionVersion**. [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] имеет версию 13.0.\<major build>.\<minor build>  
  
### <a name="verify-the-version-of-the-analysis-services-windows-service"></a>Проверьте версию службы Windows Analysis Services  
 Если обновлены лишь некоторые серверы [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] в ферме SharePoint 2010, экземпляр [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] на необновленных серверах будет более старым, чем версия, ожидаемая в ферме. Чтобы использовать все серверы, потребуется обновить их до одной и той же версии. Используйте один из следующих методов для проверки службы Windows служб SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]) на каждом компьютере.  
  
 **Обозреватель файлов Windows**:  
  
1.  Перейдите к папке **Bin** для экземпляра [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Например, `C:\Program Files\Microsoft SQL Server\MSAS13.POWERPIVOT\OLAP\bin`.  
  
2.  Щелкните `msmdsrv.exe`правой кнопкой мыши и выберите **Свойства**.  
  
3.  Щелкните **Сведения**.  
  
4.  Версия файла [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] должна быть 13.00.\<major build>.\<minor build>.  
  
5.  Проверьте, что этот номер такой же, как у версий решения и системной службы [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 **Сведения о запуске службы.**  
  
 При запуске служба [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] записывает сведения о версии в журнал событий Windows.  
  
1.  Запустите Windows `eventvwr`  
  
2.  Создайте фильтр для источника `MSOLAP$POWERPIVOT`.  
  
3.  Найдите информационное событие следующего вида:  
  
     Служба   запущена. Оценочный 64-разрядный RTM-выпуск Microsoft SQL Server Analysis Services (x64) **13.0.2000.8**.  
  
 **Использование PowerShell для проверки версии файла.**  
  
 Для проверки версии продукта можно использовать PowerShell. Использование PowerShell будет хорошим вариантом, если вы хотите запрограммировать в скрипте или автоматизировать проверку версии.  
  
```  
(get-childitem "C:\Program Files\Microsoft SQL Server\MSAS13.POWERPIVOT2000\OLAP\bin\msmdsrv.exe").VersionInfo  
```  
  
 Вышеуказанная команда PowerShell возвращает информацию, аналогичную следующей.  
  
 ProductVersion   FileVersion           FileName  
  
 **13.0.2000.8** 2016.0130.200    C:\Program Files\Microsoft SQL Server\MSAS13.POWERPIVOT2000\OLAP\bin\msmdsrv.exe  
  
### <a name="verify-the-msolap-data-provider-version-on-sharepoint"></a>Проверка версии поставщика данных MSOLAP в SharePoint  
 Чтобы проверить, какие версии поставщиков OLE DB служб Analysis Services являются надежными для служб Excel, используйте следующие инструкции. Проверить настройки надежного поставщика данных служб Excel может только администратор фермы или приложения службы.  
  
1.  В разделе «Управление приложениями» центра администрирования выберите пункт **Управление приложениями служб**.  
  
2.  Щелкните имя приложения служб Excel, например **ExcelServiceApp1**.  
  
3.  Щелкните **Надежные поставщики данных**. Должна отобразиться MSOLAP.5 (поставщик Microsoft OLE DB для служб OLAP 11.0). Если вы обновили установку [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] , также будет показано MSOLAP.4 из предыдущей версии.  
  
4.  Дополнительные сведения см. в разделе [Добавление MSOLAP.5 в качестве надежного поставщика данных в службах Excel Services](/analysis-services/power-pivot-sharepoint/add-msolap-5-as-a-trusted-data-provider-in-excel-services).  
  
 MSOLAP.4 описывается как поставщик OLE DB (Майкрософт) для служб OLAP 10.0. Эта версия может являться версией по умолчанию для [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] , которая устанавливается со службами Excel, или это может быть версия [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] . Версия по умолчанию, устанавливаемая SharePoint, не поддерживает доступ к данным [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Чтобы подключиться к книгам [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] на сервере SharePoint, требуется версия [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] или более поздняя. Чтобы убедиться в наличии версии [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] , используйте инструкции в предыдущем разделе, в котором описывается проверка версий путем просмотра свойств файлов.  
  
### <a name="verify-the-adomdnet-data-provider-version"></a>Проверка версии поставщика данных ADOMD.NET  
 Для определения установленной версии ADOMD.NET действуйте следующим образом. Проверить настройки надежного поставщика данных служб Excel может только администратор фермы или приложения службы.  
  
1.  На сервере приложений SharePoint перейдите к `c:\Windows\Assembly`.  
  
2.  Отсортируйте результаты по имени сборки и найдите **Microsoft.Analysis Services.Adomd.Client**.  
  
3.  Убедитесь, что используется версия 13.0.\<build number>.  
  
##  <a name="upgrading-multiple-power-pivot-for-sharepoint-servers-in-a-sharepoint-farm"></a><a name="geminifarm"></a> Обновление нескольких серверов Power Pivot для SharePoint в ферме SharePoint  
 В многосерверной топологии, включающей более одного сервера [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] , все экземпляры серверов и компонентов должны иметь одинаковую версию. Сервер, на котором работает более высокая версия ПО, задает уровень поддержки для всех серверов фермы. Если обновить лишь некоторые из серверов, то все оставшиеся, на которых работают предыдущие версии ПО, станут недоступными до тех пор, пока тоже не будут обновлены.  
  
 После обновления первого сервера дополнительные серверы, которые еще не обновлены, **станут недоступными**. Доступность восстанавливается после того, как все серверы начнут работать на одном и том же уровне.  
  
 Программа установки SQL Server обновляет файлы решения [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] на месте на физическом компьютере, но для обновления решений, которые используются в ферме, необходимо воспользоваться средством настройки [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)], описанном в предыдущем разделе этой статьи.  
  
##  <a name="applying-a-qfe-to-a-power-pivot-instance-in-the-farm"></a><a name="qfe"></a> Применение QFE к экземпляру Power Pivot в ферме  
 При обновлении сервера [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint существующие программные файлы будут обновлены до новых версий, включая исправления некоторых проблем. При применении QFE к многосерверной топологии отсутствует основной сервер, с которого нужно начинать. Поэтому обновление можно начать с любого сервера, при условии, что к остальным серверам [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] в ферме будет применяться тот же QFE.  
  
 При применении QFE также необходимо выполнить шаг настройки, в котором информация о версии сервера обновляется в базе данных конфигурации фермы. Версия обновленного сервера становится новой ожидаемой версией фермы. До применения и настройки QFE на всех компьютерах экземпляры [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint, к которым не применен QFE, будут недоступны для обработки запросов данных [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Следуйте данным инструкциям, чтобы обеспечить применения и правильную настройку QFE.  
  
1.  Установите обновление, следуя инструкциям, прилагаемым к QFE.  
  
2.  Запустите средство настройки [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
3.  Выберите **Удаление компонентов, служб, приложений и решений** и нажмите кнопку **ОК**.  
  
4.  Ознакомьтесь с действиями, которые включены в обновления задач и затем щелкните **Проверить**.  
  
5.  Нажмите кнопку **Выполнить** для предварительного просмотра отчета.  
  
6.  Повторите для дополнительных экземпляров [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint в ферме.  
  
    > [!IMPORTANT]  
    >  В многосерверной среде развертывания убедитесь, что обновление применено и настроено на каждом экземпляре. Затем переходите к следующему компьютеру. Прежде чем перейти к следующему экземпляру, необходимо обновить текущий экземпляр с помощью средства настройки [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Проверить сведения о версии для служб в ферме можно на странице **Проверка состояния установки продукта и обновлений** в разделе «Управление обновлением и исправлениями» центра администрирования.  
  
##  <a name="post-upgrade-verification-tasks"></a><a name="verify"></a> Задачи проверки после обновления  
 После завершения обновления выполните следующие действия, чтобы проверить работоспособность сервера.  
  
|Задача|Ссылка|  
|----------|----------|  
|Проверьте, запущена ли служба на всех компьютерах, где работает [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] для SharePoint.|[Запуск и остановка службы PowerPivot для SharePoint Server](/analysis-services/power-pivot-sharepoint/start-or-stop-a-power-pivot-for-sharepoint-server)|  
|Проверьте активацию компонентов на уровне семейства веб-сайтов.|[Включение интеграции функций PowerPivot для семейств веб-сайтов в центре администрирования](/analysis-services/power-pivot-sharepoint/activate-power-pivot-integration-for-site-collections-in-ca)|  
|Проверьте, правильно ли загружаются книги [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , открыв книгу и выбрав фильтры и срезы, чтобы выполнить запрос.|Проверьте наличие кэшированных файлов на жестком диске. Если файл присутствует в кэше, значит этот файл данных загрузился на данном физическом сервере. Кэшированные файлы находятся в расположении C:\Program Files\Microsoft SQL Server\MSAS13.POWERPIVOT\OLAP\Backup.|  
|Проверьте функцию обновления данных в выбранных книгах, настроенных для поддержки обновления данных.|Самый простой способ проверки обновления данных заключается в изменении расписания обновления данных с помощью установки флажка **Обновлять при первой возможности** , чтобы операции обновления данных выполнялись немедленно. При помощи этого шага удастся определить успешность обновления данных для текущей книги. Повторите эти шаги для других часто используемых книг, чтобы убедиться, что обновление данных работает. Дополнительные сведения о планировании обновления данных см. в разделе [Планирование обновления данных (Power Pivot для SharePoint)](/sharepoint/administration/data-refresh-using-the-unattended-data-refresh-account).|  
|Следите за отчетами об обновлении данных на панели управления для мониторинга [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , чтобы убедиться в отсутствии ошибок обновления данных.|[Информационная панель управления PowerPivot и данные об использовании](/analysis-services/power-pivot-sharepoint/power-pivot-management-dashboard-and-usage-data)|  
  
 Дополнительные сведения о настройке параметров и функций [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] см. в разделе [Администрирование и конфигурация сервера Power Pivot в центре администрирования](/analysis-services/power-pivot-sharepoint/power-pivot-server-administration-and-configuration-in-central-administration).  
  
 Пошаговые инструкции по выполнению всех задач по настройке системы после установки см. в разделе [Первоначальная настройка (Power Pivot для SharePoint)](/sharepoint/administration/configure-power-pivot-for-sharepoint-2013).  
  
## <a name="see-also"></a>См. также:  
 [Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)   
 [Установка Power Pivot для SharePoint 2010](https://sharepointgeorge.com/2012/installing-sql-server-powerpivot-sharepointstep-step-guide/)  
  

---
description: Перенос установки служб Reporting Services (собственный режим)
title: Перенос установки служб Reporting Services (собственный режим) | Документация Майкрософт
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.date: 05/01/2020
ms.openlocfilehash: d45e00b7d99f87ec3edc9bdd123d5392412dcf73
ms.sourcegitcommit: fe59f8dc27fd633f5dfce54519d6f5dcea577f56
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91934842"
---
# <a name="migrate-a-reporting-services-installation-native-mode"></a>Перенос установки служб Reporting Services (собственный режим)

В этом разделе приводятся пошаговые инструкции, позволяющие выполнить миграцию одной из следующих поддерживаемых версий [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], развернутых в собственном режиме, в новый экземпляр SQL Server Reporting Services.  
  
::: moniker range=">=sql-server-2017||=sqlallproducts-allversions"
* [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)]

* [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
* [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
* [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
* [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]
::: moniker-end

::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
* [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
* [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
* [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
* [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]

Сведения о переносе установки в режиме интеграции с [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint см. в статье [Перенос установки служб Reporting Services (режим интеграции с SharePoint)](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md).  

::: moniker-end
  
 Миграция определяется как перемещение файлов данных приложений на новый экземпляр SQL Server. Ниже приведены обычные причины, по которым может потребоваться перенести установку.  
  
* Имеется крупномасштабное развертывание, или требуется увеличение временных показателей работоспособности.  
  
* Изменяется оборудование или топология установки.  
  
* Обнаруживается проблема, блокирующая обновление.

## <a name="native-mode-migration-overview"></a><a name="bkmk_nativemode_migration_overview"></a> Обзор миграции в собственном режиме

 Процесс миграции для [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] состоит из шагов, выполняемых вручную и автоматически. При выполнении миграции сервера отчетов выполняются следующие задачи.  
  
* Создайте резервные копии базы данных, приложения и файлов конфигурации.  
  
* Выполните резервное копирование ключа шифрования.  
  
* Установите новый экземпляр SQL Server. Если используется такое же оборудование, можно параллельно установить SQL Server с существующей установкой, если она была одной из поддерживаемых версий.  
  
    > [!TIP]  
    >  Для параллельной установки может потребоваться установить SQL Server как именованный экземпляр.
  
* Переместите базу данных сервера отчетов и другие файлы приложения с существующего экземпляра на новый экземпляр SQL Server.  
  
* Переместите любые пользовательские файлы приложения в новый экземпляр.  
  
* Настройка сервера отчетов.  
  
* Измените файл **RSReportServer.config** , чтобы включить пользовательские параметры с предыдущего экземпляра.  
  
* Можно также настроить списки управления доступом (ACL) для новой группы служб Windows [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
* После проверки работоспособности нового экземпляра удалите неиспользуемые приложения и средства.  
  
 Установлены ограничения на выпуски [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , на которых размещается база данных сервера отчетов. Ознакомьтесь со следующим разделом, если повторно используется база данных сервера отчетов, которая была создана в предыдущей установке.  
  
* [Создание базы данных сервера отчетов](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
  
## <a name="fixed-database-name"></a><a name="bkmk_fixed_database_name"></a> Фиксированное имя базы данных

 Нельзя переименовать базу данных сервера отчетов. Идентификатор базы данных записывается в хранимых процедурах сервера отчетов при создании базы данных. Переименование первичной или временной баз данных сервера отчетов приводит к возникновению ошибок при выполнении этих процедур, поэтому установка сервера отчетов становится недействительной.  
  
 Если имя базы данных из существующей установки не подходит для новой установки, рассмотрите возможность создания новой базы данных с тем же именем, затем загрузки данных существующего приложения с использованием методов из следующего списка:  
  
* Запишите [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] скрипт, который вызывает методы SOAP веб-службы сервера отчетов, чтобы копировать данные между базами данных. Можно использовать служебную программу RS.exe для выполнения скрипта. Дополнительные сведения об этом подходе см. в статье [Сценарии и PowerShell со службами Reporting Services](../../reporting-services/tools/scripting-and-powershell-with-reporting-services.md).  
  
* Запишите код, который вызывает поставщика инструментария WMI, чтобы копировать данные между базами данных. Дополнительные сведения об этом подходе см. в статье [Доступ к поставщику WMI для служб Reporting Services](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  
  
* Если количество элементов невелико, можно переиздать отчеты и общие источники данных из конструктора отчетов, конструктора моделей и построителя отчетов на новом сервере отчетов. Повторно создайте назначения ролей, подписки, общие расписания, расписания моментальных снимков отчета, пользовательские свойства, установленные для отчетов или других элементов, безопасность элементов модели и свойства, назначенные на сервере отчетов. Будьте готовы к тому, что будут потеряны журнал отчетов и данные журнала выполнения отчета.
  
## <a name="before-you-start"></a><a name="bkmk_before_you_start"></a> Перед началом работы

 Несмотря на то что выполняется миграция, а не обновление экземпляра, попробуйте запустить помощник по обновлению на существующем экземпляре, который поможет обнаружить любые неполадки, влияющие на миграцию. Этот шаг особенно полезен, если выполняется миграция сервера отчетов, установленного и настроенного другим лицом. Запустив помощник по обновлению, можно обнаружить пользовательские настройки, которые могут не поддерживаться в новом экземпляре SQL Server.  
  
 Кроме того, помните о нескольких важных изменениях в SQL Server Reporting Services, которые влияют на метод миграции экземпляра.

* Новый [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] заменил собой диспетчер отчетов.
  
* Начиная с [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], службы IIS больше не нужны. При переносе экземпляра сервера отчетов на новый компьютер не обязательно добавлять роль веб-сервера. Кроме того, шаги для настройки URL-адресов и проверки подлинности отличаются от предыдущей версии, как и методы и средства диагностики и устранения проблем.  
  
* Веб-служба сервера отчетов, [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]и служба сервера отчетов Windows выполняются под одной учетной записью. Все три приложения считывают параметры конфигурации из файла RSReportServer.config.
  
* [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] и среда SQL Server Management Studio разработаны таким образом, чтобы устранить перекрытие функций. Каждое средство поддерживает отдельный набор задач.
  
* Фильтры ISAPI не поддерживаются в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и более поздних версий. Если используются фильтры ISAPI, необходимо до осуществления миграции перепроектировать решения по созданию отчетов.  
  
* Ограничения на IP-адреса не поддерживаются в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и более поздних версий. В случае применения ограничений на IP-адреса необходимо до осуществления миграции перепроектировать решения по созданию отчетов либо воспользоваться такой технологией, как брандмауэр, маршрутизатор или преобразование сетевых адресов (NAT) с целью настройки адресов, на которые наложены ограничения по доступу к серверу отчетов.  
  
* Клиентские сертификаты TLS (ранее — SSL) не поддерживаются в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и более поздних версий. Если используются клиентские TLS-сертификаты, необходимо до осуществления миграции перепроектировать решения по созданию отчетов.  
  
* Если используется тип проверки подлинности, отличный от встроенной проверки подлинности Windows, необходимо обновить элемент `<AuthenticationTypes>` в файле **RSReportServer.config** с учетом поддерживаемого типа проверки подлинности. К поддерживаемым типам проверки подлинности относятся NTLM, Kerberos, Negotiate и Basic. Такие методы проверки подлинности, как анонимный доступ, дайджест-проверка подлинности и .NET Passport, не поддерживаются в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и более поздних версий.  
  
* Если в среде подготовки отчетов применяются пользовательские каскадные таблицы стилей, они не подлежат переносу. Переместите их вручную по завершении миграции.
  
Дополнительные сведения об изменениях в службах SQL Server Reporting Services см. в документации советника по переходу и в разделе [Новые возможности служб Reporting Services](../../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md).  

## <a name="backup-files-and-data"></a><a name="bkmk_backup"></a> Резервное копирование файлов и данных

 Перед установкой нового экземпляра служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]не забудьте создать резервные копии всех файлов из текущего экземпляра.  
  
1. Создайте резервную копию ключа шифрования для базы данных сервера отчетов. Этот шаг важен для успешной миграции. На дальнейших этапах процесса миграции необходимо восстановить его для сервера отчетов, чтобы снова получить доступ к зашифрованным данным. Чтобы создать резервную копию ключа, используйте диспетчер конфигурации сервера отчетов.  
  
2. создайте резервную копию базы данных сервера отчетов с помощью любого из поддерживаемых методов резервного копирования базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Дополнительные сведения см. в инструкциях по созданию архивных копий сервера отчетов базы данных в статье [Перемещение баз данных сервера отчетов на другой компьютер &#40;собственный режим служб SSRS&#41;](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md).  
  
3. Создайте резервную копию файлов конфигурации сервера отчетов. Необходимо создать резервные копии следующих файлов:  
  
    1. RSReportServer.config  
  
    2. Rswebapplication.config;  
  
    3. Rssvrpolicy.config  
  
    4. Rsmgrpolicy.config;  
  
    5. Reportingservicesservice.exe.config;  
  
    6. Web.config для приложения [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] сервера отчетов.  
  
    7. Machine.config — для [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] , если он изменен для операций сервера отчетов.  

## <a name="install-sql-server-reporting-services"></a><a name="bkmk_install_ssrs"></a> Установите службы SQL Server Reporting Services

 Установите новый экземпляр сервера отчетов в режиме «Только файлы», чтобы настроить его на использование значений, отличных от выбираемых по умолчанию. Для установки из командной строки используйте аргумент **FilesOnly**. В мастере установки выберите параметр **Установить, но не настраивать сервер**.  
  
 Перейдите по одной из следующих ссылок для просмотра инструкций по установке нового экземпляра служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
* [Установка SQL Server Reporting Services 2016 и более ранних версий с помощью мастера установки (программы установки)](install-reporting-services-native-mode-report-server.md) 
  
* [Установка SQL Server Reporting Services 2016 и более ранних версий из командной строки](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)  

* [Установка SQL Server Reporting Services 2017 и более поздних версий](install-reporting-services.md)

## <a name="move-the-report-server-database"></a><a name="bkmk_move_database"></a> Перемещение базы данных сервера отчетов

 База данных сервера отчетов содержит опубликованные отчеты, модели отчетов, общие источники данных, расписания, ресурсы, подписки и папки. Она также содержит свойства системы и элемента и разрешения для доступа к содержимому сервера отчетов.  
  
 Если в процессе миграции используется другой экземпляр компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] , необходимо переместить базу данных сервера отчетов в новый экземпляр компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Если используется тот же экземпляр компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] , перейдите к разделу [Перемещение пользовательских сборок и расширений](#bkmk_move_custom).  
  
 Чтобы переместить базу данных сервера отчетов, выполните указанные ниже действия.
  
1. Выберите экземпляр компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] для использования. SQL Server Reporting Services требуют использования одной из следующих версий для размещения базы данных сервера отчетов.  
  
    ::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
    * [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)]

    * [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
    * [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
    * [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
    * [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]
    ::: moniker-end

    ::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
    * [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  

    * [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  

    * [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  

    * [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]
    ::: moniker-end
  
2. Запустите среду [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] и подключитесь к компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
3. Создайте роль **RSExecRole** в системных базах данных, если на компоненте [!INCLUDE[ssDE](../../includes/ssde-md.md)] никогда не размещалась база данных сервера отчетов. Дополнительные сведения см. в разделе [Создание RSExecRole](../../reporting-services/security/create-the-rsexecrole.md).  
  
4. Инструкции см. в статье [Перемещение баз данных сервера отчетов на другой компьютер &#40;собственный режим служб SSRS&#41;](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md).  
  
 Помните, что база данных сервера отчетов и временная база данных взаимозависимы, поэтому перемещать их необходимо вместе. Не копируйте базы данных. В процессе копирования в новый экземпляр переносятся не все параметры настройки безопасности. Не перемещайте задания агента SQL Server для запланированных операций сервера отчетов. Сервер отчетов автоматически создаст эти задания повторно.  

## <a name="move-custom-assemblies-or-extensions"></a><a name="bkmk_move_custom"></a> Перемещение пользовательских сборок и расширений

 Если в установку входят пользовательские элементы отчета, сборки или расширения, необходимо заново разместить пользовательские компоненты. Если пользовательские компоненты не используются, пропустите подраздел [Настройка сервера отчетов](#bkmk_configure_reportserver).  
  
 Для повторного развертывания пользовательских компонентов выполните указанные ниже действия.  
  
1. Определите, поддерживаются ли сборки или необходима повторная компиляция.

    * Пользовательские модули безопасности должны быть повторно написаны с использованием интерфейса [IAuthenticationExtension2](/dotnet/api/microsoft.reportingservices.interfaces.iauthenticationextension2).
  
    * Пользовательские модули подготовки отчетов для [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] должны быть переписаны с использованием объектной модели для подготовки отчетов (ROM).  
  
    * Модули подготовки отчетов, использующие HTML 3.2 и HTML OWC, не поддерживаются в [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и более поздних версий.  
  
    * Повторная компиляция других пользовательских сборок необязательна.  
  
2. Переместите сборки на новый сервер отчетов и в папку \bin. В SQL Server двоичные файлы сервера отчетов размещаются в следующем каталоге для экземпляра сервера отчетов по умолчанию.  
  
     `\Program files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer\bin`  
  
3. Измените файлы конфигурации, чтобы добавить элементы для пользовательского компонента. Элементы зависят от вида используемой сборки. Инструкции по выбору места размещения файлов и добавлению элементов конфигурации см. в следующих ресурсах.  
  
    1. [Развертывание пользовательской сборки](../../reporting-services/custom-assemblies/deploying-a-custom-assembly.md)  
  
    2. [Руководство. развернуть пользовательский элемент отчета](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)  
  
    3. [Развертывание модуля обработки данных](../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)  
  
    4. [Развертывание модуля доставки](../../reporting-services/extensions/delivery-extension/deploying-a-delivery-extension.md)  
  
    5. [Развертывание модуля подготовки отчетов](../../reporting-services/extensions/rendering-extension/deploying-a-rendering-extension.md)  
  
    6. [Реализация модуля безопасности](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)  

## <a name="configure-the-report-server"></a><a name="bkmk_configure_reportserver"></a> Настройка сервера отчетов

 Настройте URL-адреса для веб-службы сервера отчетов и портала, а также подключение к базе данных сервера отчетов.  
  
 При осуществлении миграции масштабного развертывания нужно перевести в режим «вне сети» все узлы сервера отчетов и перемещать все серверы по одному. После завершения переноса первого сервера отчетов и его подключения к базе данных сервера отчетов версия этой базы данных сервера отчетов автоматически обновляется до уровня версии базы данных SQL Server.  
  
> [!IMPORTANT]
>  Если какие-либо серверы отчетов масштабного развертывания функционируют в режиме "в сети" и не были перенесены, может возникнуть исключение *rsInvalidReportServerDatabase*, так как при подключении к обновленным базам данных эти серверы используют старую схему.  

Если перенесенный сервер отчетов был настроен в качестве общей базы данных для масштабного развертывания, то перед настройкой службы сервера отчетов необходимо удалить все старые ключи шифрования из таблицы **Keys** в базе данных **ReportServer**. Если ключи не удалить, то сервер отчетов после переноса попытается инициализироваться в режиме масштабного развертывания. Дополнительные сведения см. в статьях [Добавление и удаление ключей шифрования для развертывания с горизонтальным увеличением масштаба (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md) и [Настройка ключей шифрования и управление ими (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md).  

Ключи масштабного развертывания нельзя удалить с помощью диспетчера конфигурации служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Старые ключи из таблицы **Keys** в базе данных **ReportServer** необходимо удалить с помощью среды SQL Server Management Studio. Удалите все строки из таблицы Keys. При этом таблица очищается и подготавливается для восстановления симметричного ключа, как описано далее.  

До удаления ключей рекомендуется сначала создать резервную копию симметричных ключей шифрования. Это можно сделать с помощью диспетчера конфигурации служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Откройте диспетчер конфигурации, перейдите на вкладку "Ключи шифрования" и нажмите кнопку **Резервное копирование**. Можно также создать скрипт с командами WMI для резервного копирования ключа шифрования. Дополнительные сведения о WMI см. в статье [Метод BackupEncryptionKey &#40;WMI MSReportServer_ConfigurationSetting&#41;](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-backupencryptionkey.md).  
  
1. Запустите диспетчер конфигурации сервера отчетов и подключитесь к только что установленному экземпляру [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Дополнительные сведения см. в разделе [Диспетчер конфигурации сервера отчетов (собственный режим)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md).  
  
2. Настройте URL-адреса сервера отчетов и портала. Дополнительные сведения см. в статье [Настройка URL-адреса (диспетчер конфигурации сервера отчетов)](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md).  
  
3. Настройте базу данных сервера отчетов, выбрав существующую базу данных сервера отчетов из предыдущего экземпляра. После успешной настройки служба сервера отчетов перезапустится, и как только будет установлено подключение к базе данных сервера отчетов, эта база будет автоматически обновлена до SQL Server Reporting Services. Дополнительные сведения о методах запуска мастера изменения баз данных, который используется для создания и выбора базы данных сервера отчетов, см. в статье [Создание базы данных сервера отчетов, работающего в собственном режиме](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).  
  
4. Восстановите ключи шифрования. Этот шаг необходим, чтобы задействовать обратимое шифрование на предыдущих строках соединения и учетных данных, которые уже находятся в базе данных сервера отчетов. Дополнительные сведения см. в разделе [Резервное копирование и восстановление ключей шифрования служб Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
5. Если сервер отчетов установлен на новом компьютере и используется брандмауэр Windows, убедитесь, что порт, который прослушивает сервер отчетов, открыт. По умолчанию для этой цели используется порт 80. Дополнительные сведения см. в статье [Настройка брандмауэра для доступа к серверу отчетов](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md).  
  
6. Если есть необходимость локального администрирования сервера отчетов, работающего в собственном режиме, следует настроить операционную систему, чтобы разрешить локальное администрирование с веб-портала. Дополнительные сведения см. в разделе [Настройка сервера отчетов, работающего в основном режиме, для локального администрирования](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  

## <a name="copy-custom-configuration-settings-to-rsreportserverconfig-file"></a><a name="bkmk_copy_custom_config"></a> Копирование настроек пользовательской конфигурации в файл RSReportServer.config

Если изменен файл RSReportServer.config или RSWebApplication.config в предыдущей установке, следует внести те же изменения в новый файл RSReportServer.config. В следующем списке приведена сводка причин изменения предыдущего файла конфигурации и даны ссылки на дополнительную информацию о способах настройки этих же параметров в SQL Server 2016.  
  
|Настройка|Сведения|  
|-------------------|-----------------|  
|Доставка электронной почты сервера отчетов с пользовательскими параметрами|[Параметры электронной почты — собственный режим Reporting Services](../../reporting-services/install-windows/e-mail-settings-reporting-services-native-mode-configuration-manager.md).|  
|Настройки сведений об устройстве|[Настройка параметров модулей подготовки отчетов в RSReportServer.Config](../../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md)|

## <a name="windows-service-group-and-security-acls"></a><a name="bkmk_windowsservice_group"></a> Группа служб Windows и списки управления доступом

 В [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] для создания списков управления доступом для всех разделов реестра, файлов и папок, устанавливаемых со службами SQL Server Reporting Services, используется одна группа служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Windows. Имя этой группы Windows отображается в формате SQLServerReportServerUser$\<*computer_name*>$\<*instance_name*>.  

## <a name="verify-your-deployment"></a><a name="bkmk_verify"></a> Проверка развертывания

1. Проверьте виртуальные каталоги сервера отчетов и [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] , открыв браузер и введя URL-адрес. Дополнительные сведения см. в статье [Проверка установки служб Reporting Services](../../reporting-services/install-windows/verify-a-reporting-services-installation.md).  
  
2. Проверьте отчеты и убедитесь в том, что они содержат ожидаемые данные. Просмотрите сведения об источнике данных на предмет того, содержатся ли в них данные о подключении к источнику данных. При обработке и подготовке отчетов к просмотру на сервере отчетов используется объектная модель отчетов, но конструкции [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] или [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] новыми элементами языка определения отчетов не заменяются. Дополнительные сведения о выполнении существующих отчетов на сервере отчетов новой версии см. в статье [Обновление отчетов](../../reporting-services/install-windows/upgrade-reports.md).  

## <a name="remove-unused-programs-and-files"></a><a name="bkmk_remove_unused"></a> Удаление неиспользуемых программ и файлов

После переноса сервера отчетов на новый экземпляр, возможно, потребуется сделать следующее для удаления ненужных более программ и файлов.  
  
1. Удалите прежнюю версию служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , если она больше не нужна. Этот шаг не удаляет следующие элементы, но их можно удалить вручную, если они больше не нужны:  
  
    * старую базу данных сервера отчетов;  
  
    * роль RsExec;  
  
    * учетную запись службы сервера отчетов;  
  
    * пул приложений для веб-службы сервера отчетов;  
  
    * виртуальные каталоги для диспетчера отчетов и сервера отчетов;  
  
    * файлы журналов сервера отчетов.  
  
2. Удалите службы IIS, если они более не нужны на этом компьютере.

## <a name="next-steps"></a>Дальнейшие действия

* [Перенос установки служб Reporting Services](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md)  
* [База данных сервера отчетов](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
* [Обновление и перенос служб Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
* [Обратная совместимость служб Reporting Services](../../reporting-services/reporting-services-backward-compatibility.md)   
* [Диспетчер конфигурации сервера отчетов](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  

Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
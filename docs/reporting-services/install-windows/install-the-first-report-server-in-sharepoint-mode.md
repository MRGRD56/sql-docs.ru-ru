---
description: Установка первого сервера отчетов в режиме интеграции с SharePoint
title: Установка первого сервера отчетов в режиме интеграции с SharePoint | Документы Майкрософт
ms.date: 10/05/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: bf701e1ccdff887f7861c934d59bff9c09be66b6
ms.sourcegitcommit: 295b9dfc758471ef7d238a2b0f92f93e34acbb1b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/31/2021
ms.locfileid: "106054648"
---
# <a name="install-the-first-report-server-in-sharepoint-mode"></a>Установка первого сервера отчетов в режиме интеграции с SharePoint

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  В этом разделе описываются процедуры установки одиночного сервера служб Reporting Services в режиме интеграции с SharePoint. Эти шаги охватывают запуск мастера установки SQL Server, а также выполнение дополнительных задач по настройке с использованием центра администрирования SharePoint. В разделе также можно ознакомиться с отдельными процедурами обновления существующей установки, например с созданием приложения служб Reporting Services.  
  
> [!NOTE]
> Интеграция служб Reporting Services с SharePoint больше не доступна после выхода SQL Server 2016.
  
 Сведения о добавлении дополнительных серверов Reporting Services в существующую ферму см. в следующих разделах:  
  
-   [Добавление дополнительного сервера отчетов в ферму (горизонтально масштабируемые службы SSRS)](../../reporting-services/install-windows/add-an-additional-report-server-to-a-farm-ssrs-scale-out.md)  
  
-   [Добавление дополнительного клиентского веб-интерфейса служб Reporting Services в ферме](../../reporting-services/install-windows/add-an-additional-reporting-services-web-front-end-to-a-farm.md)  
  
 Установка на одиночный сервер полезна в сценариях разработки и тестирования, но не рекомендуется для рабочей среды.  
  
##  <a name="example-single-server-deployment"></a><a name="bkmk_singleserver"></a> Пример развертывания в конфигурации с одиночным сервером

 Установка на одиночный сервер полезна в сценариях разработки и тестирования, но не рекомендуется для рабочей среды. Под средой с одиночным сервером подразумевается один компьютер, на котором установлены компоненты SharePoint и Reporting Services. В разделе не рассматриваются масштабные конфигурации с несколькими серверами служб Reporting Services.  
  
 На приведенной ниже схеме показаны компоненты, которые входят в состав развертывания одиночного сервера Reporting Services.  
 
 > [!NOTE]
 > В SharePoint 2016 службы Excel перемещены в Office Online Server и не могут использоваться при развертывании в конфигурации с одиночным сервером. Office Online Server должен быть развернут на другом сервере. Дополнительные сведения см. в статьях [Общие сведения об Office Online Server](https://technet.microsoft.com/library/jj219437\(v=office.16\).aspx) и [Настройка административных параметров Excel Online](https://technet.microsoft.com/library/jj219698\(v=office.16\).aspx).
  
|Компонент|Описание|  
|-|-|  
|**(1)**|Служба SharePoint, установленная из экземпляра SQL Server. Можно создать одно или несколько приложений служб Reporting Services.|  
|**(2)**|Надстройка Reporting Services для продуктов SharePoint предоставляет компоненты пользовательского интерфейса на серверах SharePoint.|  
|**(3)**|Приложение службы Excel используется в Power View и [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]. Оно недоступно при развертывании в конфигурации с одиночным сервером для SharePoint 2016. Наличие [Office Online Server](https://technet.microsoft.com/library/jj219437\(v=office.16\).aspx) является обязательным.|  
|**(4)**|[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .|  
  
 ![Развертывание одиночного сервера в режиме интеграции служб SSRS с SharePoint](../../reporting-services/install-windows/media/rs-sharepoint-1server-deployment.gif "Развертывание одиночного сервера в режиме интеграции служб SSRS с SharePoint")  
  
> [!TIP]  
>  Более сложные примеры развертывания см. в разделе [Топологии развертывания для компонентов бизнес-аналитики SQL Server в SharePoint](/previous-versions/sql/sql-server-2016/hh231674(v=sql.130)).  
  
##  <a name="setup-accounts"></a><a name="bkmk_setupaccounts"></a> Учетная запись для установки

 В этом разделе описываются учетные записи и разрешения, используемые для основных шагов развертывания служб Reporting Services в режиме интеграции с SharePoint.  
  
 **Установка и регистрация служб Reporting Services:**  
  
-   Учетной записи, которая является текущей в процессе установки служб Reporting Services в режиме интеграции с SharePoint (она называется "учетной записью для установки"), необходимы права администратора на локальном компьютере. Если установка служб Reporting Services выполняется после установки SharePoint и учетная запись для установки также принадлежит к группе администраторов фермы SharePoint, программа установки Reporting Services зарегистрирует службы Reporting Services автоматически. Если установка служб Reporting Services осуществляется до установки SharePoint или учетная запись для установки не принадлежит к группе администраторов фермы, службы придется зарегистрировать вручную. Обратитесь к разделу [Этап 2. Регистрация и запуск службы Reporting Services в SharePoint](#bkmk_install_SSRS_sharedservice).  
  
 **Создание приложений служб Reporting Services**  
  
-   После установки и регистрации служб Reporting Services создайте одно или несколько приложений служб Reporting Services. Для создания приложения службы Reporting Services учетную запись службы фермы SharePoint необходимо временно включить в группу локальных администраторов. Дополнительные сведения о разрешениях учетной записи SharePoint 2013 см. в статье [Разрешения и параметры безопасности учетной записи в SharePoint 2013](/SharePoint/install/account-permissions-and-security-settings-in-sharepoint-server-2016) (https://technet.microsoft.com/library/cc678863.aspx), а о разрешениях учетной записи SharePoint 2016 — в статье [Разрешения и параметры безопасности учетной записи в SharePoint 2016](https://technet.microsoft.com/library/cc678863\(v=office.16\).aspx).  
  
     В целях безопасности не рекомендуется, чтобы учетные записи администраторов фермы SharePoint также являлись учетными записями администраторов локальной операционной системы. Если вы добавили учетную запись администратора фермы в группу локальных администраторов в ходе процесса настройки, рекомендуется удалить учетную запись из группы локальных администраторов после завершения установки.  
  
##  <a name="step-1-install-reporting-services-report-server-in-sharepoint-mode"></a><a name="bkmk_install_SSRS"></a> Этап 1. Установка сервера отчетов служб Reporting Services в режиме интеграции с SharePoint

 На этом шаге устанавливается сервер отчетов Reporting Services в режиме интеграции с SharePoint и надстройка служб Reporting Services для продуктов SharePoint. В зависимости от продуктов, уже установленных на компьютере, могут не отображаться некоторые страницы установки, описанные в следующих разделах.  
 
 > [!IMPORTANT]
 > В случае с SharePoint 2016 сервер SharePoint, на который будут устанавливаться службы Reporting Services, должен иметь **пользовательскую** роль сервера. Развертывание служб Reporting Services может быть успешно выполнено на сервере SharePoint, который не относится к **пользовательской** роли, однако во время следующего периода обслуживания SharePoint MinRole остановит службы Reporting Services, так как обнаружит, что службы Reporting Services в режиме интеграции с SharePoint не поддерживают ни одну из остальных ролей сервера SharePoint. Приложение служб Reporting Services поддерживает только **пользовательскую** роль.
 
 > [!NOTE]
 > Если одновременно в SharePoint 2016 планируется установка службы Power Pivot, установите ее до начала установки служб Reporting Services. Службу Power Pivot можно установить на сервер SharePoint только в **пользовательской** роли.
 
 ### <a name="apply-the-custom-server-role-to-a-sharepoint-2016-server"></a>Применение пользовательской роли к серверу SharePoint 2016
 
 > [!NOTE]
 > Данный раздел не относится к SharePoint 2013.
 
 1. Войдите на сервер SharePoint, на который будет устанавливаться [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)].
 
 2. Запустите **консоль управления SharePoint 2016** от имени администратора. 
  
    Щелкните **консоль управления SharePoint 2016** правой кнопкой мыши и выберите команду **Запуск от имени администратора**.

3. В командной строке PowerShell введите следующую команду:

    > [!NOTE]
    > Убедитесь, что имя сервера SharePoint указано правильно.
    
    ```powershell
    Set-SPServer SERVERNAME -Role Custom
    ```

4. Должно появиться сообщение о том, что задание таймера запланировано. Дождитесь, пока задание будет выполнено.

5. Проверьте назначенную роль сервера с помощью указанной ниже команды.

    ```powershell
    Get-SPServer SERVERNAME 
    ```
 
 6. В поле **Роли** должно отобразиться значение **Пользовательская**.
 
 ### <a name="install-reporting-services"></a>Установка служб Reporting Services
  
1.  Запустите мастер установки SQL Server (Setup.exe).  
  
2.  Выберите пункт **Установка** в левой части мастера, затем пункт **Новая изолированная установка SQL Server или добавление функций в существующую установку**.  

3.  Если отображается страница **Ключ продукта**, введите ключ или примите выбранный по умолчанию вариант "Enterprise (ознакомительная версия)".  
  
     Выберите **Далее**.  
  
4.  Если отображаются условия лицензионного соглашения, ознакомьтесь с условиями лицензии и примите их. Корпорация Майкрософт будет признательна вам за согласие отправлять данные об использовании компонентов, которые помогут улучшить функции и поддержку продукта.  
  
     Выберите **Далее**.  

5.  Рекомендуется выбрать параметр **Использовать Центр обновления Майкрософт для проверки наличия обновлений (рекомендуется)** . Водить описание не обязательно.
  
     Выберите **Далее**.   
  
6.  В зависимости от продуктов, уже установленных на компьютере, на странице **Установка файлов установки** может появиться следующее сообщение:  
  
    -   "Для одного или нескольких необходимых файлов существуют незавершенные операции. После завершения процесса установки следует перезагрузить компьютер".  
  
    -   Выберите **Далее**.  
  
7.  Если открылась страница **Правила установки** . Просмотрите все предупреждения и определите критические препятствия. Выберите **Далее**.
 
8. Выберите следующие компоненты на странице **Выбор компонентов** :  
  
    -   **Службы Reporting Services — SharePoint**  
  
    -   **Надстройка служб Reporting Services для продуктов SharePoint**.  
  
    -   При необходимости для полной среды можно также выбрать **службы компонента Database Engine** , однако для этого требуется экземпляр компонента SQL Server Database Engine, в котором размещается база данных SharePoint.  
  
     Выберите **Далее**.  
  
     ![Снимок экрана: страница "Выбор компонентов" с выбранными параметрами "Службы Reporting Services — SharePoint" и "Надстройка служб Reporting Services для продуктов SharePoint".](../../reporting-services/install-windows/media/rs-setupfeatureselection-sharepoint-with-circles.png)
  
9. Если выбрана установка служб Database Engine, примите экземпляр **MSSQLSERVER** по умолчанию на странице **Настройка экземпляра** и нажмите кнопку **Далее**.  
  
     ![примечание](/analysis-services/analysis-services/instances/install-windows/media/ssrs-fyi-note.png "Примечание")Архитектура службы Reporting Services SharePoint не основывается на "экземпляре" SQL Server, как в прежней версии архитектуры Reporting Services.  
  
10. Если отображается страница **Настройка сервера** , введите соответствующие учетные данные. Если необходимо использовать функции предупреждения об изменении данных или подписки на данные служб Reporting Services, необходимо изменить **тип запуска** для агента SQL Server на **автоматический**. Страница **Конфигурация сервера** может не отображаться в зависимости от продуктов, уже установленных на компьютере.  
  
     Выберите **Далее**.  
  
11. Если выбрана установка служб ядра СУБД, откроется страница **Настройка компонента Database Engine** , на которой следует добавить соответствующие учетные записи в список администраторов SQL и нажать кнопку **Далее**.  
  
12. На странице **Настройка служб Reporting Services** будет выбран вариант **Только установка** . Этот параметр устанавливает файлы сервера отчетов и не настраивает среду SharePoint для служб Reporting Services.  
  
    > [!NOTE]
    > По завершении установки SQL Server используйте сведения в других подразделах этого раздела для настройки среды SharePoint. В том числе нужно будет установить общие службы Reporting Services и создать приложения служб Reporting Services.  
  
     ![Снимок экрана: раздел "Режим интеграции служб Reporting Services с SharePoint" с выбранным и выделенным параметром "Выборочная установка".](../../reporting-services/install-windows/media/ssrs-2016-setup-configuration.png)
  
13. Просмотрите все предупреждения, а затем нажмите **Далее** на странице **Правила конфигурации компонентов** , если она откроется.  
  
14. На странице **Все готово для установки** просмотрите сводку по установке. Сводка должна охватывать дочерний сайт **Службы Reporting Services в режиме интеграции с SharePoint** , который покажет значение **SharePointFilesOnlyMode**. Выберите пункт **Установить**.  
  
15. Для выполнения программы установки требуется несколько минут. Появится страница **Готово** с перечислением функций и состояния каждого компонента. Возможно, откроется диалоговое окно с уведомлением о необходимости перезагрузки компьютера.  
  
##  <a name="step-2-register-and-start-the-reporting-services-sharepoint-service"></a><a name="bkmk_install_SSRS_sharedservice"></a> Этап 2. Регистрация и запуск служб Reporting Services в SharePoint  
 ![Содержимое, связанное с PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Содержимое, связанное с PowerShell")  
  
> [!NOTE]
> Если производится установка в существующую ферму SharePoint, шаги, описанные в этом подразделе, выполнять не нужно. Службы Reporting Services в SharePoint были установлены и запущены в результате выполнения мастера установки SQL Server при изучении предыдущего раздела этого документа.  
  
 Ниже перечислены основные причины, по которым необходимо вручную зарегистрировать службы Reporting Services.  
  
-   Установка служб Reporting Services выполнена до установки SharePoint.  
  
-   Учетная запись, которая использовалась для установки Reporting Services в режиме интеграции с SharePoint, не принадлежит к группе администраторов фермы SharePoint. За дополнительной информацией обратитесь к разделу [Setup accounts](#bkmk_setupaccounts).  
  
 Необходимые файлы были установлены в процессе работы мастера установки SQL Server, но службы необходимо зарегистрировать в ферме SharePoint.  
  
 Следующие шаги позволяют открыть консоль управления SharePoint и запустить командлеты PowerShell.  
  
1.  Нажмите кнопку **Пуск** .  
  
2.  Выберите группу **Продукты Microsoft SharePoint 2016** или **Продукты Microsoft SharePoint 2013** .  
  
3.  Щелкните **консоль управления SharePoint 2016** или **консоль управления SharePoint 2013** правой кнопкой мыши и выберите пункт **Запуск от имени администратора**. 

    > [!NOTE]
    > Команды SharePoint не распознаются в основном окне Windows PowerShell. Используйте **консоль управления SharePoint**.  
  
4.  Выполните следующую команду PowerShell, чтобы установить службу SharePoint Reporting Services. При успешном завершении команды отобразится новая строка в консоли управления. При успешном завершении работы команды в консоль управления **не возвращаются какие-либо сообщения** :  
  
    ```  
    Install-SPRSService  
    ```  
  
5.  Выполните следующую команду PowerShell, чтобы установить прокси-сервер служб SQL Server Reporting Services. При успешном завершении команды отобразится новая строка в консоли управления. При успешном завершении работы команды в консоль управления **не возвращаются какие-либо сообщения** :  
  
    ```  
    Install-SPRSServiceProxy  
    ```  
  
6.  Выполните следующую команду PowerShell, чтобы запустить службу, или просмотрите нижеперечисленные замечания для получения указаний по запуску службы из центра администрирования SharePoint.  
  
    ```  
    get-spserviceinstance -all |where {$_.TypeName -like "SQL Server Reporting*"} | Start-SPServiceInstance  
    ```  
  
    > [!IMPORTANT]
    > Если вы появится примерно следующее сообщение об ошибке:  
    >   
    ```powershell
    >     Install-SPRSService : The term 'Install-SPRSService' **is not recognized** as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.  
    ```
    >
    > Либо вы находитесь в Windows PowerShell, а не в командной консоли SharePoint, либо не установлены службы Reporting Services в режиме интеграции с SharePoint. Дополнительные сведения о службах Reporting Services и PowerShell см. в статье [Командлеты PowerShell для режима служб SharePoint Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md).  
  
 Можно также запустить службу из центра администрирования SharePoint, а не с помощью третьей команды PowerShell. С помощью следующих шагов можно также проверить, выполняется ли служба.  
  
1.  В центре администрирования SharePoint выберите пункт **Управление службами на сервере** в группе **Параметры системы** .  
  
2.  Найдите **Службу SQL Server Reporting Services** и нажмите кнопку **Запуск** в столбце «Действие».  
  
3.  Состояние службы Reporting Services изменится с **Остановлена** на **Запущена**. Если служба Reporting Services отсутствует в списке, используйте PowerShell для установки службы.  
  
    > [!NOTE]  
    >  Если служба Reporting Services остается в состоянии **Запускается** и не переходит в состояние **Запущена**, в диспетчере Windows Server убедитесь в том, что запущена служба "Администрирование SharePoint 2013".  
  
##  <a name="step-3-create-a-reporting-services-service-application"></a><a name="bkmk_create_serrviceapplication"></a> Шаг 3. Создание приложения службы Reporting Services  
 В этом разделе описаны шаги по созданию приложения службы и описания его свойств (в случае просмотра существующего приложения службы).  
  
1.  В центре администрирования SharePoint в разделе **Управление приложениями** выберите **Управление приложениями служб**.  
  
2.  На ленте SharePoint нажмите кнопку **Создать** .  
  
3.  В меню кнопки "Создать" выберите пункт **Приложение службы SQL Server Reporting Services**.  
  
    > [!IMPORTANT]  
    >  Если параметр Reporting Services отсутствует в списке, значит **общая служба Reporting Services не установлена**. Просмотрите предыдущий раздел, в котором описана установка служб Reporting Services с помощью командлетов PowerShell.  
  
4.  На странице **Создание приложения службы SQL Server Reporting Services** введите имя приложения. Если создается несколько приложений службы Reporting Services, описательные имена или контекст именования помогут упорядочить операции по администрированию и управлению.  
  
5.  В разделе **Пул приложений** создайте для данного приложения новый пул приложений (рекомендуется). Если использовать одно имя как для нового пула приложений, так и для приложения службы, дальнейшее администрирование может упроститься. Кроме того, многое зависит от количества создаваемых приложений служб и от того, требуется ли использовать несколько из них в одном пуле приложений. Рекомендации и сведения по управлению пулом приложений см. в документации по SharePoint Server.  
  
     Выберите или создайте учетную запись безопасности для пула приложений. Обязательно укажите учетную запись домена. Учетная запись пользователя домена позволяет использовать функцию управляемой учетной записи SharePoint, с помощью которой пароли и данные учетной записи можно изменять в одном месте. Учетные записи домена также требуются, если планируется расширение развернутой системы и включение дополнительных экземпляров службы, которые работают от лица одного идентификатора.  
  
6.  В поле **Сервер баз данных** можно указать текущий сервер или выбрать другой SQL Server.  
  
7.  В поле **Имя базы данных** будет содержаться значение по умолчанию `ReportingService_<guid>`, представляющее собой уникальное имя базы данных. При вводе нового значения оно должно быть уникальным. Это новая база данных, создаваемая специально для приложений служб.  
  
8.  В списке **Проверка подлинности базы данных** по умолчанию выбрано значение «Проверка подлинности Windows». Если выбран параметр **Проверка подлинности SQL**, то рекомендации по использованию этого типа проверки подлинности в развернутой системе SharePoint см. в документации SharePoint.  
  
9. В подразделе **Связь с веб-приложением** выберите веб-приложение, которое должно быть подготовлено для доступа со стороны текущего приложения службы Reporting Services. Одно приложение службы Reporting Services можно связать с одним веб-приложением. Если все имеющиеся веб-приложения уже связаны с приложением службы Reporting Services, отображается предупреждение.  
  
10. Щелкните **ОК**.  
  
11. Создание приложения службы может занять несколько минут. По завершении этого процесса отобразятся подтверждение и ссылка на страницу **Подготовка подписок и предупреждений** . Выполните шаг подготовки, если нужно использовать функции подписки или предупреждений об изменении данных служб Reporting Services. Дополнительные сведения см. в разделе [Подготовка подписок и предупреждений для приложений служб SSRS](../../reporting-services/install-windows/provision-subscriptions-and-alerts-for-ssrs-service-applications.md).  
  
 ![Содержимое, связанное с PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Содержимое, связанное с PowerShell") Сведения о создании приложения службы Reporting Services с помощью PowerShell см. в следующих статьях:  
  
-   Обратитесь к следующему разделу [Скрипт Windows PowerShell для шагов 1–4](#bkmk_full_script).  
  
-   Статья [Создание приложения службы Reporting Services с помощью PowerShell](../../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md).  

##  <a name="step-4-activate-the-power-view-site-collection-feature"></a><a name="bkmk_powerview"></a> Шаг 4. Активация функции Power View семейства веб-сайтов.

 [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)], компонент надстройки служб SQL Server 2016 Reporting Services для продуктов [!INCLUDE[msCoName](../../includes/msconame-md.md)] SharePoint, является компонентом семейства веб-сайтов. Этот компонент активируется автоматически для корневых семейств веб-сайтов, созданных после установки надстройки служб Reporting Services. Если вы планируете использовать [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)], необходимо удостовериться в том, что этот компонент активирован.  
  
 При установке надстройки служб Reporting Services для продуктов SharePoint Server после установки SharePoint Server функции интеграции с сервером отчетов и с Power View будут активированы только для корневых семейств веб-сайтов. Для других семейств веб-сайтов необходимо активировать эти компоненты вручную.  
  
#### <a name="to-activate-or-verify-the-power-view-site-collection-feature"></a>Активация и проверка компонента Power View семейства веб-сайтов  
  
1.  При выполнении следующих шагов предполагается, что сайт SharePoint настроен для **версии взаимодействия** 2013 в SharePoint 2013.  
  
     Откройте в браузере требуемый сайт SharePoint. Например, https://\<servername>/sites/bi  
  
2.  Щелкните **Параметры**![Параметры SharePoint](/analysis-services/analysis-services/media/as-sharepoint2013-settings-gear.gif "Параметры SharePoint").  
  
3.  Выберите **Настройки сайта**.  
  
4.  Щелкните **Компоненты семейства веб-сайтов** в группе **Администрирование семейства веб-сайтов**.  
  
5.  Найдите в списке **Компонент интеграции Power View** .  
  
6.  Выберите **Активировать**. Состояние функции меняется на **Активно**.  
  
 Эта процедура выполняется для каждого семейства веб-сайтов. Дополнительные сведения см. в статье [Активация функций интеграции семейства веб-сайтов с сервером отчетов и Power View в SharePoint](../../reporting-services/report-server-sharepoint/site-collection-features-report-server-and-power-view.md).  
  
##  <a name="windows-powershell-script-for-steps-1-4"></a><a name="bkmk_full_script"></a> Скрипт Windows PowerShell для шагов 1–4  
 Сценарии PowerShells в данном разделе эквивалентны выполнению шагов с 1 по 4 предыдущего раздела. Скрипт выполняет следующие действия.  
  
-   Устанавливает службы Reporting Services и прокси-сервер служб, а затем запускает службы.  
  
-   Создает прокси-сервер службы с именем Reporting Services.  
  
-   Создает приложение служб Reporting Services с именем Reporting Services Application.  
  
-   Включает компонент [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] для семейства веб-сайтов.  
  
 Параметры  
  
-   Обновите параметр **-Account** для прокси-сервера службы. В качестве учетной записи необходимо использовать управляемую учетную запись службы на ферме SharePoint. Дополнительные сведения см. в разделе, посвященном SharePoint, [Планирование учетных записей администрирования и служб в SharePoint 2013](/SharePoint/security-for-sharepoint-server/plan-for-administrative-and-service-accounts).  
  
-   Изменяет параметр **-DatabaseServer** для приложения службы. Этот параметр является экземпляром компонента Database Engine.  
  
-   Изменяет параметр **-url** сайта, на котором необходимо включить функцию [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)].  
  
 **Использование скрипта.**  
  
1.  Откройте Windows PowerShell с правами администратора.  
  
2.  Скопируйте следующий код в окно скрипта.  
  
3.  Измените три параметра, описанные в предыдущем разделе, а затем выполните скрипт.  
  
```  
#This script Configures SQL Server Reporting Services SharePoint mode  
  
$starttime=Get-Date  
write-host -foregroundcolor DarkGray StartTime>> $starttime   
  
Write-Host -ForegroundColor Green "Import the SharePoint PowerShell snappin"  
Add-PSSnapin Microsoft.Sharepoint.Powershell -EA 0  
  
Write-Host -ForegroundColor Green "Install SSRS Service and Service Proxy, and start the service"  
Write-Host -ForegroundColor Green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"  
  
    Write-Host -ForegroundColor Green "Install the Reporting Services Shared Service"  
    Install-SPRSService  
  
    Write-Host -ForegroundColor Green " Install the Reporting Services Service Proxy"  
    Install-SPRSServiceProxy  
  
    # Get the ID of the RS Service Instance and start the service   
    Write-Host -ForegroundColor Green "Start the Reporting Services Service"  
    $RS = Get-SPServiceInstance | Where {$_.TypeName -eq "SQL Server Reporting Services Service"}  
    Start-SPServiceInstance -Identity $RS.Id.ToString()  
  
    # Wait for the Reporting Services Service to start...  
    $Status = Get-SPServiceInstance $RS.Id.ToString()  
    While ($Status.Status -ne "Online")  
    {  
        Write-Host -ForegroundColor Green "SSRS Service Not Online...Current Status = " $Status.Status  
        Start-Sleep -Seconds 2  
        $Status = Get-SPServiceInstance $RS.Id.ToString()  
    }  
  
$time=Get-Date  
write-host -foregroundcolor DarkGray StartTime>> $starttime   
write-host -foregroundcolor DarkGray $time  
  
Write-Host -ForegroundColor Green "Create a new application pool and Reporting Services service application"  
Write-Host -ForegroundColor Green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"  
Write-Host -ForegroundColor Green "Create a new application pool"  
#!!!! update "-Account" with an existing Managed Service Account  
New-SPServiceApplicationPool -Name "Reporting Services" -Account "<domain>\User name>"  
$appPool = Get-SPServiceApplicationPool "Reporting Services"  
  
Write-Host -ForegroundColor Green " Create the Reporting Services Service Application"  
#!!!! Update "-DatabaseServer", an instance of the SQL Server database engine   
$rsService = New-SPRSServiceApplication -Name "Reporting Services Application" -ApplicationPool $appPool -DatabaseName "Reporting_Services_Application" -DatabaseServer "<server name>"  
  
Write-Host -ForegroundColor Green "Create the Reporting Services Service Application Proxy"  
$rsServiceProxy = New-SPRSServiceApplicationProxy -Name "Reporting Services Application Proxy" -ServiceApplication $rsService  
  
Write-Host -ForegroundColor Green "Associate service application proxy to default web site and grant web applications rights to SSRS application pool"  
Write-Host -ForegroundColor Green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"     
# Associate the Reporting Services Service Applicatoin Proxy to the default web site...  
Get-SPServiceApplicationProxyGroup -default | Add-SPServiceApplicationProxyGroupMember -Member $rsServiceProxy  
  
$time=Get-Date  
write-host -foregroundcolor DarkGray StartTime>> $starttime   
write-host -foregroundcolor DarkGray $time  
  
Write-Host -ForegroundColor Green "Enable the PowerView and reportserver site features"  
Write-Host -ForegroundColor Green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"  
#!!!! update "-url"  of the site where you want the features enabled  
Enable-SPfeature -identity "powerview" -Url https://server/sites/bi  
Enable-SPfeature -identity "reportserver" -Url https://server/sites/bi  
  
####To Verify, you can run the following:  
#Get-SPRSServiceApplication  
#Get-SPServiceApplicationPool | where {$_.name -like "reporting*"}  
#Get-SPRSServiceApplicationProxy  
  
```  
  
##  <a name="additional-configuration"></a><a name="bkmk_additional_config"></a> Дополнительная настройка  
 В этом разделе описываются дополнительные действия по настройке, которые важны для большинства развертываний SharePoint.  
  
###  <a name="configure-excel-services-and-power-pivot"></a><a name="bkmk_configure_ECS"></a> Настройка служб Excel и Power Pivot  
 Чтобы посмотреть отчеты [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] Power View в книге Excel 2016 или Excel 2013 в SharePoint, необходимо настроить службы Excel для использования сервера служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] в режиме интеграции с SharePoint. 
 
 Для SharePoint 2016 [Office Online Server](https://technet.microsoft.com/library/jj219456\(v=office.16\).aspx) должен быть настроен для использования служб Excel. Подробные сведения см. в следующих документах.
 
 - [Развертывание SQL Server 2016 PowerPivot и Power View в SharePoint 2016](/analysis-services/instances/install-windows/deploying-sql-server-2016-powerpivot-and-power-view-in-sharepoint-2016)
 
 - [Deploying SQL Server 2016 PowerPivot and Power View in a Multi-Tier SharePoint 2016 Farm (Развертывание SQL Server 2016 PowerPivot и Power View в многоуровневой ферме SharePoint 2016)](/analysis-services/instances/install-windows/deploy-powerpivot-and-power-view-multi-tier-sharepoint-2016-farm)
 
 Для SharePoint 2016 необходимо будет создать и настроить приложение служб Excel. Дополнительные сведения см. в следующих разделах:  
  
-   Раздел "Настройка служб Excel для интеграции служб Analysis Services" в статье [Установка служб Analysis Services в режиме Power Pivot](/analysis-services/instances/install-windows/install-analysis-services-in-power-pivot-mode).  
  
-   [Управление параметрами модели данных служб Excel (SharePoint Server 2013)](/SharePoint/administration/manage-excel-services-data-model-settings).  

Кроме того, учетная запись безопасности для пула приложений, используемая приложением служб Reporting Services, должна иметь права администратора на сервере служб Analysis Services.
  
###  <a name="provision-subscriptions-and-alerts"></a><a name="bkmk_provision_agent"></a> Подготовка подписок и оповещений  
 Для функций подписки и предупреждений об изменении данных в службах Reporting Services может потребоваться настройка разрешений для агента SQL Server. Если отображается сообщение об ошибке, указывающее, что необходим агент SQL Server, хотя агент SQL Server уже запущен, обновите разрешения. Щелкните ссылку **Подготовка подписок и предупреждений** на странице с сообщением об успешном создании приложения службы, чтобы перейти на страницу провизионирования агента SQL Server. Шаг подготовки необходим, если система развернута более чем на одном компьютере, например если экземпляр базы данных SQL Server находится на другом компьютере. Дополнительные сведения см. в разделе [Подготовка подписок и предупреждений для приложений служб SSRS](../../reporting-services/install-windows/provision-subscriptions-and-alerts-for-ssrs-service-applications.md).  
  
### <a name="configure-e-mail-for-ssrs-service-applications"></a>Настройка электронной почты для приложений служб SSRS  
 Функция предупреждения об изменении данных служб Reporting Services отправляет предупреждения в сообщениях электронной почты. Для отправки электронной почты могут потребоваться настройка приложения служб Reporting Services и изменение модуля доставки электронной почты приложением службы. Параметры электронной почты необходимо настроить и в случае, если вы планируете использовать модуль доставки электронной почты для функции подписки служб Reporting Services. Дополнительные сведения см. в статье [Настройка электронной почты для приложения служб Reporting Services (SharePoint 2013 и SharePoint 2016)](./configure-e-mail-for-a-reporting-services-service-application.md). 
  
### <a name="add-reporting-services-content-types-to-content-libraries"></a>Добавление типов содержимого служб Reporting Services в библиотеки документов  
 Службы Reporting Services предоставляют стандартные типы содержимого для управления файлами общих источников данных (RSDS) и определений отчетов (RDL) построителя отчетов. Добавив в библиотеку типы содержимого **Отчет построителя отчетов** и **Источник данных отчета**, вы можете использовать команду **Создать** для создания документов этих типов. Дополнительные сведения см. в разделе [Добавление типов содержимого служб Reporting Services в библиотеку SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  
  
### <a name="activate-the-report-server-file-sync-feature"></a>Активация функции синхронизации файлов сервера отчетов  
 Если пользователи часто загружают элементы опубликованных отчетов непосредственно в библиотеки документов SharePoint, может оказаться полезной функция **синхронизации файлов сервера отчетов** уровня сайта. Функция синхронизации файлов позволяет синхронизировать каталог сервера отчетов с элементами библиотек документов на более регулярной основе. Дополнительные сведения см. в статье [активировать функции синхронизации файлов сервера отчетов в центре администрирования SharePoint](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md).  
  
##  <a name="verify-the-installation"></a><a name="bkmk_verify_installation"></a> Проверка установки  
 Далее приведены действия и процедуры, которые предлагается выполнить для проверки развертывания служб Reporting Services в режиме интеграции с SharePoint.  
  
-   См. подраздел, посвященный SharePoint, в разделе [Verify a Reporting Services Installation](../../reporting-services/install-windows/verify-a-reporting-services-installation.md).  
  
-   В библиотеке документов SharePoint создайте базовый отчет служб Reporting Services, который содержит только одно текстовое поле, например заголовок. Отчет не содержит никаких источников данных и наборов данных. Цель состоит в том, чтобы проверить возможность открытия построителя отчетов, создания простого отчета и просмотра этого отчета.  
  
     Сохраните отчет в библиотеке документов и запустите отчет из библиотеки. Дополнительные сведения о создании отчетов с помощью построителя отчетов см. в разделе [Запуск построителя отчетов (построитель отчетов)](../report-builder/start-report-builder.md).  
  
## <a name="next-steps"></a>Дальнейшие действия

[Командлеты PowerShell для режима служб SharePoint Reporting Services](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md)   
[Обновление и перенос служб Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[Возможности, поддерживаемые различными выпусками SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md)   
[Служба SharePoint и Служебные приложения службы Reporting Services](../../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md)  

Остались вопросы? [Посетите форум служб Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
---
description: Занятие 3–2. Запуск мастера установки пакета
title: Шаг 2. Запуск мастера установки пакета | Документы Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: f91fbb89-4626-4c47-b96d-56052dc45861
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f30bc07edc2d6d513eb9078e1758caf99a2822cf
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "88472066"
---
# <a name="lesson-3-2---running-the-package-installation-wizard"></a>Занятие 3–2. Запуск мастера установки пакета

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


В этой задаче мастер установки пакета используется для развертывания пакетов из проекта Deployment Tutorial в экземпляре [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. В таблицу sysssispackages базы данных [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] msdb можно установить только пакеты; файлы поддержки, включенные в пакет развертывания, будут развернуты в файловой системе.  
  
Мастер установки пакета проводит пользователя через последовательность шагов по установке и настройке пакетов. Пакеты будут устанавливаться в экземпляр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на компьютере назначения (куда скопирован пакет развертывания). Будет также создана папка «C:\DeploymentTutorialInstall», в которую мастер установит файлы, не относящиеся к пакету.  
  
На предыдущем занятии были изменены пакеты в учебнике, чтобы сконфигурировать их. С помощью мастера установки пакета будут изменены эти настройки, чтобы пакеты могли успешно выполняться в той среде, куда они установлены.  
  
### <a name="to-install-the-packages"></a>Установка пакетов  
  
1.  Найдите на компьютере назначения пакет развертывания.  
  
    Если в качестве размещения программы развертывания использовалось значение по умолчанию (bin\Deployment), пакет развертывания будет находиться в папке Deployment в проекте Deployment Tutorial.  
  
2.  В папке Deployment дважды щелкните файл манифеста, Deployment Tutorial.SSISDeploymentManifest.  
  
3.  На странице приветствия мастера установки пакетов нажмите кнопку **Далее**.  
  
4.  На странице "Развертывание пакетов служб SSIS" выберите параметр **Установить на SQL Server** , установите флажок **Проверить пакеты после установки** и нажмите кнопку **Далее**.  
  
5.  На странице "Выбор целевого сервера SQL Server" укажите **(локальный)** в поле **Имя сервера** .  
  
6.  Если экземпляр SQL Server поддерживает проверку подлинности Windows, выберите **Использовать проверку подлинности Windows**; в противном случае выберите **Использовать проверку подлинности SQL Server** и задайте имя пользователя и пароль.  
  
7.  Убедитесь в том, что снят флажок **Шифрование обеспечивается хранением на сервере** .  
  
8.  Нажмите кнопку **Далее**.  
  
9. На странице "Выбор папки для установки" нажмите кнопку **Обзор**.  
  
10. В диалоговом окне **Выбор папки** разверните **Мой компьютер** и щелкните **Локальный диск (C:)**.  
  
11. Щелкните **Создать папку** и замените имя по умолчанию для новой папки ( **Новая папка**) на **DeploymentTutorialInstall**.  
  
    > [!IMPORTANT]  
    > На это имя ссылается одна из переменных среды конфигурации. Имя папки и ссылка должны соответствовать друг другу, иначе пакет не сможет выполняться.  
  
12. Нажмите кнопку **ОК**.  
  
13. Убедитесь в том, что на странице "Выбор папки установки" в поле "Папка" содержится **C:\DeploymentTutorialInstall** и нажмите кнопку **Далее**.  
  
14. На странице "Подтверждение установки" нажмите кнопку **Далее**.  
  
    Мастер устанавливает пакеты. После завершения установки открывается страница «Настройка пакетов».  
  
15. Убедитесь в том, что на странице "Настройка пакетов" в списке **Файл конфигурации** содержатся файлы datatransferconfig.dtsconfig и loadxmldataconfig.dtsconfig.  
  
16. В списке **Файл конфигурации** щелкните **datatransferconfig.dtsconfig**, разверните "Свойство" в столбце **Путь** поля **Конфигурации** и обновите столбец **Значение** указанными ниже значениями.  
  
    |Property (Свойство)|Значение|Обновленное значение|  
    |------------|---------|-----------------|  
    |\Package.Connections[Deployment Tutorial Log].Properties[ConnectionString]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Completed Packages\Deployment Tutorial Log|C:\DeploymentTutorialInstall\Deployment Tutorial Log|  
    |\Package.Connections[NewCustomers].Properties[ConnectionString]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\NewCustomers.txt|C:\DeploymentTutorialInstall\NewCustomers.txt|  
  
17. В списке **Файл конфигурации** щелкните loadxmldataconfig.dtsconfig, разверните "Свойство" в столбце **Путь** поля **Конфигурации** и обновите столбец **Значение** указанными ниже значениями.  
  
    |Property (Свойство)|Значение|Обновленное значение|  
    |------------|---------|-----------------|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLData]]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xml|C:\DeploymentTutorialInstall\orders.xml|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLSchemaDefinition]]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xsd|C:\DeploymentTutorialInstall\orders.xsd|  
  
18. На странице "Проверка пакета" просмотрите результаты проверки каждого установленного пакета и нажмите кнопку **Далее**.  
  
    Поскольку значения переменных среды на целевом компьютере отличаются от значений переменных среды на компьютере разработчика, на странице проверки пакетов отображается несколько предупреждений. Следует ожидать следующих предупреждений.  
  
    -   Файл конфигурации C:\DeploymentTutorial\DataTransferConfig.dtsConfig недопустим. Проверьте имя файла конфигурации.  
  
    -   Не удалось загрузить по крайней мере одну запись конфигурации в пакете. Проверьте записи конфигурации и предыдущие предупреждения, чтобы увидеть, какая из них ошибочна.  
  
    -   Файл конфигурации C:\DeploymentTutorial\LoadXMLDataConfig.dtsConfig недопустим. Проверьте имя файла конфигурации.  
  
    -   Не удалось загрузить по крайней мере одну запись конфигурации в пакете. Проверьте записи конфигурации и предыдущие предупреждения, чтобы увидеть, какая из них ошибочна.  
  
    Эти предупреждения не влияют на установку пакета.  
  
    Если не установлен флажок **Проверить пакеты после установки** на странице "Развертывание пакетов служб SSIS", то страницы "Проверка пакетов" не открываются и мастер не выводит сведения о проверке.  
  
19. На странице "Завершение работы мастера установки пакета" прочитайте сводку результатов установки и нажмите кнопку **Готово**.  
  
    > [!NOTE]  
    > Для проверки пакетов создается временный файл журнала. Этот файл не используется при выполнении пакета.  
  
## <a name="next-task-in-lesson"></a>Следующая задача занятия  
[Шаг 3. Тестирование развернутых пакетов](../integration-services/lesson-3-3-testing-the-deployed-packages.md)  
  
## <a name="see-also"></a>См. также:  
[Службы Integration Services (службы SSIS)](../integration-services/service/integration-services-service-ssis-service.md)  

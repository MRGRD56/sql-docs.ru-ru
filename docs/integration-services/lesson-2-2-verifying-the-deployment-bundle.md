---
description: Занятие 2–2. Проверка пакета развертывания
title: Шаг 2. Проверка пакета развертывания | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 6c13f5c9-c75e-4e52-94dc-2d2db2c578fe
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 4704aea54f73a0fa25db60ab9145a0ad3bc9359d
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/26/2020
ms.locfileid: "88449700"
---
# <a name="lesson-2-2---verifying-the-deployment-bundle"></a>Занятие 2–2. Проверка пакета развертывания

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


На занятии 1 был создан проект учебного развертывания и в него добавлены пакеты и вспомогательные файлы; в предыдущей задаче была построена программа развертывания для проекта.  
  
В этой задаче будет проверено содержимое пакета развертывания. Пакет развертывания представляет собой папку, которая будет скопирована на целевой компьютер и использована для установки пакетов. Если в качестве местонахождения для программы развертывания было использовано значение по умолчанию bin\Deployment, пакетом развертывания будет папка Bin\Deployment в папке Deployment Tutorial в проекте [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)].  
  
### <a name="to-verify-the-content-of-deployment-bundle"></a>Проверка содержимого пакета развертывания  
  
1.  Выберите папку bin\Deployment.  
  
2.  Проверьте наличие в папке следующих файлов:  
  
    -   DataTransfer.dtsx  
  
    -   datatransferconfig.dtsconfig  
  
    -   Deployment Tutorial.SSISDeploymentManifest  
  
    -   LoadXMLData.dtsx  
  
    -   loadxmldataconfig.dtsconfig  
  
    -   NewCustomers.txt  
  
    -   orders.xml  
  
    -   orders.xsd  
  
    -   Readme.txt  
  
3.  Щелкните правой кнопкой файл Deployment Tutorial.SSISDeploymentManifest, наведите указатель на пункт **Открыть с помощью** и выберите пункт **Internet Explorer**. Файл также можно открыть в текстовом редакторе, например в приложении «Блокнот». Код XML в файле должен быть таким:  
  
    `<?xml version="1.0"?><DTSDeploymentManifest GeneratedBy="Domain\UserName" GeneratedFromProjectName="Deployment Tutorial" GeneratedDate="2006-02-24T13:29:02.6537669-08:00" AllowConfigurationChanges="true"><Package>DataTransfer.dtsx</Package><Package>LoadXMLData.dtsx</Package><ConfigurationFile>datatransferconfig.dtsconfig</ConfigurationFile><ConfigurationFile>loadxmldataconfig.dtsconfig</ConfigurationFile><MiscellaneousFile>Readme.txt</MiscellaneousFile><MiscellaneousFile>orders.xml</MiscellaneousFile><MiscellaneousFile>NewCustomers.txt</MiscellaneousFile><MiscellaneousFile>orders.xsd</MiscellaneousFile></DTSDeploymentManifest>`  
  
4.  Убедитесь в том, что значение атрибута **AllowConfigurationChanges** равно **true** , а код XML включает элемент **Package** для каждого из двух пакетов, элемент **MiscellaneousFile** для каждого из четырех непакетных файлов и элемент **ConfigurationFile** для каждого из двух XML-файлов конфигурации.  
  
5.  Закройте обозреватель Internet Explorer или текстовый редактор.  
  
## <a name="next-lesson"></a>Следующее занятие  
[Занятие 3. Установка пакетов SSIS](../integration-services/lesson-3-install-ssis-packages.md)  
  
  
  

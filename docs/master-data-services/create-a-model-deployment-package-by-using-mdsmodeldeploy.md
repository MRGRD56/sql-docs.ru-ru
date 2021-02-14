---
title: Создание пакета развертывания модели (MDSModelDeploy)
description: Используйте MDSModelDeploy для создания пакета развертывания в Master Data Services. Пакет может содержать либо только объекты модели, либо объекты модели и данные.
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: c2687e39-dc20-494f-a707-2aa29f4c329e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ef35b92932cd4e55b47e2f75a16825f14202a815
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100272645"
---
# <a name="create-a-model-deployment-package-by-using-mdsmodeldeploy"></a>Создание пакета развертывания модели при помощи MDSModelDeploy

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  В [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]для создания пакетов используется средство MDSModelDeploy. В зависимости от указанных команд, пакет может содержать:  
  
-   только объекты модели;  
  
-   объекты модели и данные.  
  
 Если необходимо развернуть пакет, содержащий только объекты модели, можно воспользоваться мастером развертывания моделей в веб-приложении [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . Дополнительные сведения см. в разделе [Создание пакета развертывания модели с помощью мастера](../master-data-services/create-a-model-deployment-package-by-using-the-wizard.md).  
  
## <a name="prerequisites"></a>Предварительные требования  
 Для выполнения этой процедуры:  
  
1.  Для запуска средства MDSModelDeploy требуются следующие основные разрешения.  
  
    -   Такие же разрешения Windows, как у диспетчера конфигурации MDS (администратор Windows)  
  
    -   Разрешение DBA на базу данных MDS.  
  
2.  Для создания пакета с помощью средства MDSModelDeploy требуются следующие основные разрешения.  
  
    -   Разрешение администратора модели MDS на модель данных.  
  
    -   Разрешение для функциональной области MDS ImportExport.  
  
3.  Для развертывания модели с помощью средства MDSModelDeploy требуются следующие основные разрешения.  
  
    -   Разрешение для функции обозревателя MDS  
  
    -   Разрешение для функции администрирования системы MDS.  
  
4.  Для перечисления моделей с помощью средства MDSModelDeploy требуются следующие основные разрешения.  
  
    -   Разрешение для функции обозревателя MDS  
  
    -   Разрешение администратора модели MDS на модель данных с тем, чтобы видеть модель в списке.  
  
 Для создания пакета модели модель должна существовать. Дополнительные сведения см. в разделе [Создание модели (службы Master Data Services)](../master-data-services/create-a-model-master-data-services.md).  
  
 Дополнительные сведения см. в разделе [administrators &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-create-a-model-deployment-package-by-using-mdsmodeldeploy"></a>Создание пакета развертывания модели с помощью MDSModelDeploy  
  
1.  Откройте командную строку администратора.  
  
2.  Перейдите к расположению файла MDSModelDeploy.exe.  
  
    -   Если службы MDS установлены в папку по умолчанию, то этот файл находится в каталоге *диск*:\Program Files\Microsoft SQL Server\130\Master Data Services\Configuration.  
  
    -   Если службы MDS установлены не в папку по умолчанию, найдите файл MDSModelDeploy.exe на локальном компьютере.  
  
3.  Необязательный элемент. Просмотрите параметры и справку.  
  
    -   Чтобы показать все доступные параметры, введите `MDSModelDeploy` и нажмите клавишу ВВОД.  
  
    -   Чтобы вывести справку для параметра, введите следующую строку, где *OptionName* — имя параметра: `MDSModelDeploy help OptionName`.  
  
4.  Необязательный элемент. При наличии нескольких веб-приложений определите имя развертываемой службы, введя следующую команду, и нажмите клавишу ВВОД.  
  
    ```  
    MDSModelDeploy listservices  
    ```  
  
     Возвращается список значений, например `MDS1, Default Web Site, MDS`. Первое значение в этом списке (в данном случае `MDS1`) необходимо для развертывания модели.  
  
5.  Чтобы создать пакет, содержащий объекты модели и данные, введите следующую строку, в которой *ModelName*, *VersionName*, *ServiceName* и *PackageName* — это имена модели, версии, службы и выходного PKG-файла соответственно:  
  
    ```  
    MDSModelDeploy createpackage -model ModelName -version VersionName -service ServiceName -package PackageName -includedata  
    ```  
  
     Если включать данные не требуется, не указывайте параметры `-version` и `-includedata` .  
  
6.  Нажмите клавишу ВВОД. После успешного создания пакета отображается сообщение "Операция MDSModelDeploy успешно завершена".  
  
## <a name="next-steps"></a>Next Steps  
  
-   [Развертывание пакета развертывания модели при помощи MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)  
  
## <a name="see-also"></a>См. также:  
 [Параметры развертывания модели &#40;Master Data Services&#41;](../master-data-services/model-deployment-options-master-data-services.md)   
 [Развертывание моделей (службы Master Data Services)](../master-data-services/deploying-models-master-data-services.md)  
  
  

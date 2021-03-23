---
title: Развертывание моделей
description: Разверните пакеты модели, чтобы переместить копии моделей из одной Master Data Services среды в другую или для создания новых моделей в среде.
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- deployment packages [Master Data Services], about deployment packages
- deployment packages [Master Data Services]
ms.assetid: 30085c08-034f-4efe-80fe-408f9091ff5c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ec1216cdc50f13279be057558dfd6e10800e9b47
ms.sourcegitcommit: efce0ed7d1c0ab36a4a9b88585111636134c0fbb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/23/2021
ms.locfileid: "104833807"
---
# <a name="deploying-models-master-data-services"></a>Развертывание моделей (службы Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  В службах [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]пакет представляет собой XML-файл, содержащий развертываемую структуру модели и (необязательно) данные этой модели. Пакеты модели используется для перемещения копий моделей из одной среды служб MDS в другую, либо для создания новых моделей в существующей среде MDS.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] и более поздние версии **средства MDSModelDeploy** обратно совместимы с пакетами, созданными в [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] или более поздней версии.  
  
## <a name="tools-for-deploying-models"></a>Инструменты для развертывания моделей  
 Для работы с пакетами модели можно использовать одно из трех средств, в зависимости от задач, которые требуется выполнить.  
  
-   **Средство MDSModelDeploy**. Для создания и развертывания объектов и данных модели используйте средство MDSModelDeploy.exe. Если при установке служб MDS вы выбрали путь по умолчанию, это средство находится в расположении *диск*:\Program Files\Microsoft SQL Server\130\Master Data Services\Configuration.  
  
-   **Мастер развертывания модели**. Чтобы создать и развернуть только пакеты структуры модели, воспользуйтесь мастером в веб-приложении [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] . Этот мастер нельзя использовать для развертывания данных.  
  
-   **Редактор модели пакета**. Чтобы изменить пакет модели, используйте файл ModelPackageEditor.exe, который запускает редактор пакетов моделей. С помощью этого мастера выполняется изменения пакета, созданного средством MDSModelDeploy или мастером развертывания модели. Если при установке служб MDS вы выбрали путь по умолчанию, это средство находится в расположении *диск*:\Program Files\Microsoft SQL Server\130\Master Data Services\Configuration.  
  
> [!IMPORTANT]  
>  С помощью средства MDSDeployModel можно создавать и клонировать модели. Кроме того, модели можно изменять и обновлять их данные. Если средство MDSModelDeploy используется для обновления существующей модели и ее данных и пакет не содержит сущность, атрибут или элемент, имеющиеся в целевой модели, MDSModelDeploy не удаляет из модели эту сущность, атрибут или элемент.  
  
## <a name="what-packages-contain"></a>Содержимое пакетов  
 Пакет модели представляет собой XML-файл, сохраняемый с расширением PKG. При создании развертываемого пакета в него по желанию можно включить данные. Если данные решено включить, то нужно выбрать для них версию.  
  
 В пакет включаются все объекты модели. Эти объекты включают в себя:  
  
-   Сущности  
  
-   Атрибуты  
  
-   Группы атрибутов  
  
-   Иерархии  
  
-   Коллекции  
  
-   Бизнес-правила  
  
-   Флаги версии  
  
-   Представления подписки  
  
 Атрибуты файлов, а также разрешения пользователей и групп не входят в пакет. При развертывании модели их нужно обновить вручную.  
  
## <a name="sample-packages"></a>Образцы пакетов  
 При установке служб [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]также копируются файлы образцов пакетов. Эти файлы пакетов расположены в каталоге Master Data Services\Samples\Packages, где установлена среда [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. При развертывании этих образцов пакетов с помощью средства MDSModelDeploy создаются и заполняются данными образцы моделей.  
  
## <a name="related-tasks"></a>Связанные задачи  
  
|Описание задачи|Раздел|  
|----------------------|-----------|  
|Средство MDSModelDeploy используется для создания новых пакетов объектов модели и данных.|[Создание пакета развертывания модели при помощи MDSModelDeploy](../master-data-services/create-a-model-deployment-package-by-using-mdsmodeldeploy.md)|  
|Создать новый пакет развертывания объектов модели можно только с помощью мастера.|[Создание пакета развертывания модели с помощью мастера](../master-data-services/create-a-model-deployment-package-by-using-the-wizard.md)|  
|Средство MDSModelDeploy используется для развертывания пакета объектов модели и данных.|[Развертывание пакета развертывания модели при помощи MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)|  
|Развернуть пакет объектов модели можно только с помощью мастера.|[Развертывание пакета развертывания модели с помощью мастера](../master-data-services/deploy-a-model-deployment-package-by-using-the-wizard.md)|  
|Изменять пакет развертывания модели необходимо в тех случаях, когда требуется развернуть только определенные части модели, а не всю модель.|[Изменение пакета развертывания модели](../master-data-services/edit-a-model-deployment-package.md)|  
  
## <a name="related-content"></a>См. также  
  
-   [Варианты развертывания модели (службы Master Data Services)](../master-data-services/model-deployment-options-master-data-services.md)  
  
  

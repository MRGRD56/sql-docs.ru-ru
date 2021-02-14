---
description: Изменение пакета развертывания модели
title: Изменение пакета развертывания модели
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: 6b0fdb7d-83dd-4392-9011-4ae642c471f1
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: e1ad7283c99b87b56d2fe509dbbcc8fe3e4e9497
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341781"
---
# <a name="edit-a-model-deployment-package"></a>Изменение пакета развертывания модели

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  В этом разделе описывается развертывание в MDS выбранных частей модели вместо всей модели. Для этого пакет модели MDS необходимо изменить в редакторе пакетов моделей.  
  
 Мастер редактора пакетов моделей позволяет выбирать в модели отдельные сущности, производные иерархии, представления подписок и бизнес-правила, которые будут включены в пакет MDS и впоследствии развернуты. Части модели, развертывать которые не требуется, вы можете оставить невыбранными. При выборе сущности также автоматически выбираются все зависимые объекты в этой сущности.  
  
 С помощью редактора пакетов моделей можно выбирать части модели в файле пакета, созданном либо средством MDSModelDeploy (оно создает файл пакета, включающий объекты и данные), либо мастером развертывания моделей (он создает файл, включающий только структуру модели). После редактирования модели в пакете используйте средство MDSModelDeploy для развертывания объектов и данных или мастер развертывания моделей для развертывания лишь структуры модели.  
  
## <a name="prerequisites"></a>Предварительные требования  
 Для выполнения этой процедуры:  
  
-   необходимо быть администратором модели. Дополнительные сведения см. в разделе [administrators &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
-   Должен существовать пакет модели для изменения. Дополнительные сведения см. в разделах [Развертывание моделей (службы Master Data Services)](../master-data-services/deploying-models-master-data-services.md) и [Создание пакета развертывания модели с помощью мастера](../master-data-services/create-a-model-deployment-package-by-using-the-wizard.md) или [Создание пакета развертывания модели при помощи MDSModelDeploy](../master-data-services/create-a-model-deployment-package-by-using-mdsmodeldeploy.md).  
  
### <a name="to-edit-a-model-deployment-package"></a>Изменения пакета развертывания модели  
  
1.  В проводнике Windows на сервере MDS перейдите в папку *диск*:\Program Files\Microsoft SQL Server\130\Master Data Services\Configuration.  
  
2.  Запустите ModelPackageEditor.exe.  
  
3.  В мастере редактора пакетов моделей нажмите кнопку **Обзор**, перейдите к папке, в которой содержатся пакеты, выберите пакет и нажмите кнопку **Открыть**. Нажмите кнопку **Далее**.  
  
4.  Выберите сущности, производные иерархии, представления подписок или бизнес-правила, которые необходимо развернуть. Снимите выбор тех из них, которые не требуется развертывать. Нажмите кнопку **Далее**.  
  
5.  Проверьте список выбранных объектов для развертывания. Чтобы изменить его, нажмите кнопку **Назад** и повторите шаг 4.  
  
6.  Нажмите кнопку **Обзор**, перейдите к папке, в которой следует сохранить частичный пакет, и введите имя файла частичного пакета (с расширением .pkg). Нажмите кнопку **Сохранить**.  
  
7.  Нажмите кнопку **Готово**.  
  
## <a name="next-steps"></a>Next Steps  
  
-   [Развертывание пакета развертывания модели с помощью мастера](../master-data-services/deploy-a-model-deployment-package-by-using-the-wizard.md)  
  
-   [Развертывание пакета развертывания модели при помощи MDSModelDeploy](../master-data-services/deploy-a-model-deployment-package-by-using-mdsmodeldeploy.md)  
  
  

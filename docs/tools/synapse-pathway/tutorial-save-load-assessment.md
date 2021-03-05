---
title: Сохранение и загрузка оценок с помощью предварительной версии Azure Synapse Pathway
description: Сохранение и загрузка оценок хранилищ данных с помощью предварительной версии Azure Synapse Pathway
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: Azure Synapse Pathway
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: c2893272c3bc2cebf21b85378565af8ba0383bc8
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873144"
---
# <a name="save-and-load-assessments-with-azure-synapse-pathway-preview"></a>Сохранение и загрузка оценок с помощью предварительной версии Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

В следующих пошаговых инструкциях показано, как сохранить и отправить оценку хранилища данных из файла с помощью Azure Synapse Pathway.

В этом руководстве описано следующее:

> [!div class="checklist"]
> * сохранение оценки в файл;
> * загрузка оценки из файла.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством убедитесь, что вы установили [Azure Synapse Pathway](synapse-pathway-download.md). См. [обзор Azure Synapse Pathway](azure-synapse-pathway-overview.md), чтобы узнать больше об этом средстве.

## <a name="saving-an-assessment-to-a-file"></a>Сохранение оценки в файл
 
1. После выполнения преобразования отобразится отчет с обобщающими сведениями о преобразовании кода ![для оценки Azure Synapse Pathway.](./media/save-load-assessment/report-overview.png)
3. Нажмите кнопку **Сохранить оценку**, укажите имя файла, а затем нажмите **Сохранить**.
![Оценка Azure Synapse Pathway.](./media/save-load-assessment/save-assessment.png)

4. ASMPRJ-файл создается в указанном месте назначения

## <a name="loading-an-assessment-from-a-file"></a>Загрузка оценки из файла

1. Чтобы открыть ту же оценку, выберите **Загрузить оценку** и укажите имя ASMPRJ-файла ![оценки Azure Synapse Pathway.](./media/save-load-assessment/browse-location.png)

1. Исходные папки, папки входных и выходных данных будут заполнены на основе выбранной оценки.
![Оценка Azure Synapse Pathway.](./media/save-load-assessment/load-assessment.png)
1. Нажмите **Преобразовать**, чтобы повторно выполнить преобразование кода

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Создание отчета](report-generation.md)

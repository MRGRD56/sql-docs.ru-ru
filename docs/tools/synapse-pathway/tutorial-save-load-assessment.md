---
title: Сохранение и загрузка оценок с помощью предварительной версии Azure Synapse Pathway
description: Сохранение и загрузка оценок хранилищ данных с помощью предварительной версии Azure Synapse Pathway
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: tools-other
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.openlocfilehash: fbaa518e2f2a5485f85fc7812291beb88896ac6e
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186265"
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

1. После выполнения преобразования отобразится отчет со сводкой по преобразованию кода. ![Обзор отчета с оценкой Azure Synapse Pathway.](./media/tutorial-save-load-assessment/report-overview.png)
1. Нажмите кнопку **Сохранить оценку**, укажите имя файла, а затем нажмите **Сохранить**.
![Оценка Azure Synapse Pathway.](./media/tutorial-save-load-assessment/save-assessment.png)

1. Файл .asmprj создается в указанном расположении.

## <a name="loading-an-assessment-from-a-file"></a>Загрузка оценки из файла

1. Чтобы открыть ту же оценку, выберите **Загрузить оценку** и укажите имя файла .asmprj. ![Расположение оценки Azure Synapse Pathway.](./media/tutorial-save-load-assessment/browse-location.png)

1. Исходные папки, папки входных и выходных данных будут заполнены на основе выбранной оценки.
![Конфигурация оценки пути Azure Synapse Pathway: тип преобразования, входной каталог и выходной каталог.](./media/tutorial-save-load-assessment/load-assessment.png)
1. Нажмите **Преобразовать**, чтобы повторно выполнить преобразование кода

## <a name="next-steps"></a>Дальнейшие действия

> [!div class="nextstepaction"]
> [Создание отчета](report-generation.md)

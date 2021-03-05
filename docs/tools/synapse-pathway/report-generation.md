---
title: Создание отчетов для предварительной версии Azure Synapse Pathway
description: Azure Synapse Pathway предоставляет комплексные отчеты о преобразуемых сценариях.
author: anshul82-ms
ms.author: anrampal
ms.topic: tutorial
ms.prod: sql
ms.technology: tools-other
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-tutorial
ms.openlocfilehash: 26701c33f713a1b82e3bab604a03e7dca054a36b
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186380"
---
# <a name="report-generation-for-azure-synapse-pathway-preview"></a>Создание отчетов для предварительной версии Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Azure Synapse Pathway предоставляет комплексный отчет о количестве успешно преобразованных сценариев. В отчете также показано количество ошибок и предупреждений в инструкциях, которые не были преобразованы.

## <a name="generate-report"></a>Создание отчета

При нажатии **Преобразовать** отображается следующий отчет:

![отчет Azure Synapse Pathway.](./media/report-generaration/report-overview.png)

## <a name="report-summary"></a>Сводка по отчету

При успешном преобразовании будет показан процент успешно преобразованных сценариев.

![Azure Synapse Pathway.](./media/report-generaration/conversion-success.png)

В разделе "Распределение инструкций" содержатся сведения о количестве преобразованных инструкций DDL и DML, включая количество созданных схем, таблиц и баз данных.

![Azure Synapse, отчет_1.](./media/report-generaration/statement-distribution.png)

Экономия от миграции вычисляется на основе количества и уровня сложности (простой, высокий или средний) преобразуемых объектов. Предполагается, что на преобразование каждого объекта вручную с помощью Synapse Pathway уходит 30 минут.

![Azure Synapse, отчет_2.](./media/report-generaration/migration-savings.png)

## <a name="next-steps"></a>Дальнейшие действия

[Загрузить Azure Synapse Pathway](synapse-pathway-download.md)
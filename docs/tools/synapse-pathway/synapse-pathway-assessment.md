---
title: Оценка предварительной версии Azure Synapse Pathway.
description: Преобразование кода хранилища данных с помощью Azure Synapse Pathway
author: anshul82-ms
ms.author: anrampal
ms.prod: sql
ms.technology: Azure Synapse Pathway
ms.topic: tutorial
ms.date: 03/02/2021
monikerRange: =azure-sqldw-latest
ms.custom: template-tutorial
ms.openlocfilehash: b76fecf9a8a7eafc84a1b9eebd746287dddf3af9
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101873180"
---
# <a name="tutorial-to-perform-your-first-code-translation-with-azure-synapse-pathway-preview"></a>Руководство по выполнению первого преобразования кода с помощью предварительной версии Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Предварительная версия Azure Synapse Pathway предоставляет поддержку для преобразования схем, таблиц, представлений, функций и т. д. из **Netezza**, **Snowflake** и **Microsoft SQL Server** в совместимый с T-SQL код, который автоматизирует миграцию на Azure Synapse Analytics.

Дополнительные сведения см. в статье [Обзор предварительной версии Azure Synapse Pathway](azure-synapse-pathway-overview).

В этом руководстве описано следующее:

> [!div class="checklist"]
> * выполнение первого преобразования сценариев SQL из существующего хранилища данных в сценарии T-SQL для Azure Synapse SQL; 
> * выбор одного из доступных источников;
> * просмотр ошибок и предупреждений об объектах, которые не были преобразованы.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством вам потребуется установить [Azure Synapse Pathway](synapse-pathway-download.md). Если вам нужны вводные сведения, см. [обзор Azure Synapse Pathway](azure-synapse-pathway-overview.md).

## <a name="run-the-translation"></a>Запуск преобразования

1. Запустите MSI-файл Azure Synapse Pathway. 

1. Выберите один из доступных источников. Источники, которые будут добавлены в ближайшее время, выделены серым цветом.
1. В папке входного каталога нажмите "Обзор" и укажите расположение папок сценариев **DDL** и **DML**, которые необходимо преобразовать.

    > [!Note]
    > В качестве источника входных данных можно указать только файлы с расширением SQL. Если пользователь указывает сценарии DDL и DML в TXT-файле, средство не будет выполнять преобразование.

1. При преобразовании кода Netezza в Azure Synapse Analytics в раскрывающемся списке "Тип преобразования" выберите IBM Netezza.
  ![Входные данные оценки Azure Synapse.](./media/perform-assessment/assessment-input.png)

1. Чтобы выбрать выходной каталог, нажмите кнопку "Обзор", чтобы указать расположение, в котором будут создаваться выходные данные.
 ![Выходной каталог Azure Synapse.](./media/perform-assessment/output-directory.png)

1. Нажмите **Преобразовать**, чтобы начать преобразование

## <a name="view-results"></a>Просмотр результатов

1. Длительность оценки зависит от количества добавленных баз данных и размера схемы каждой базы данных. Результаты по каждой базе данных будут отображены, как только они станут доступны.
 ![Отчет об оценке Azure Synapse.](./media/perform-assessment/assessment-report.png)

1. Нажав "Просмотреть результаты", вы перейдете к выходному каталогу, указанному на предыдущем шаге, и увидите файлы преобразованного сценария на основе структуры входного каталога.

1. Он включает структуру проекта, которую можно легко зафиксировать в репозитории GitHub.
  
1. Файл результатов, в котором будет содержаться список ошибок и предупреждений, будет отправлен в тот же выходной каталог.

## <a name="next-steps"></a>Дальнейшие действия

[Узнайте, как сохранить и загрузить оценку](tutorial-save-load-assessment.md)

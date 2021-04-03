---
title: Часто задаваемые вопросы о предварительной версии Azure Synapse Pathway
description: Часто задаваемые вопросы об Azure Synapse Pathway
author: anshul82-ms
ms.author: anrampal
ms.topic: overview
ms.date: 03/02/2021
ms.prod: sql
ms.technology: tools-other
monikerRange: =azure-sqldw-latest
ms.custom: template-overview
ms.openlocfilehash: a23ca1052e2a82afd6277a2fc308d72fa600aaf1
ms.sourcegitcommit: e4b71e5d432a29b6c76ea457b00aa0abd4b6c77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2021
ms.locfileid: "106273489"
---
# <a name="azure-synapse-pathway-preview-faq"></a>Часто задаваемые вопросы о предварительной версии Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

В этом руководстве вы найдете наиболее часто задаваемые вопросы о предварительной версии Azure Synapse Pathway.

## <a name="general"></a>Общие сведения

### <a name="q-what-is-azure-synapse-pathway"></a>У. Что представляет собой Azure Synapse Pathway?

A. Azure Synapse Pathway — это средство преобразования кода, которое помогает выполнить обновление до современной платформы хранения данных за счет автоматизации преобразования кода существующего хранилища данных, чтобы сделать его совместимым с Azure Synapse SQL на основе T-SQL.

### <a name="q-how-can-i-download-azure-synapse-pathway"></a>У. Как загрузить Azure Synapse Pathway?

A. Synapse Pathway можно загрузить из [Центра загрузки Майкрософт](https://aka.ms/synapse-pathway-download).

### <a name="q-how-much-does-azure-synapse-pathway-cost"></a>У. Какова стоимость Azure Synapse Pathway?

A. Затраты, связанные с загрузкой и выполнением преобразований кода с помощью Synapse Pathway, отсутствуют.

### <a name="q-what-sourcetarget-pairs-does-azure-synapse-pathway-currently-support"></a>У. Какие пары источников и целевых платформ в настоящее время поддерживает Azure Synapse Pathway?

A. В эту предварительную версию Synapse Pathway в качестве источников включены следующие хранилища данных:
- Microsoft SQL Server
- Snowflake
- Netezza

### <a name="q-what-is-included-as-part-of-the-code-conversion"></a>У. Что входит в преобразование кода?

A. Предварительная версия Synapse Pathway поддерживает преобразование кода таблиц, схем и представлений.

| Исходная платформа| Поддерживаемые типы инструкций | 
|:-------------------:|:------------------|
| IBM Netezza  | Create/Alter/Drop Database<br /> Create/Alter/Drop  Schema <br /> Create/Alter/Drop Table |
|Microsoft SQL Server  | Create/Alter/Drop Database<br /> Create/Alter/Drop  Schema <br /> Create/Alter/Drop Table | 
| Snowflake |  Create/Alter/Drop Database<br /> Create/Alter/Drop  Schema <br /> Create/Alter/Drop Table |                       

  
### <a name="q-can-it-also-scan-my-environment-and-provide-an-assessment-report-of-all-the-objects-that-need-to-be-convertedtranslated"></a>У. Может ли это средство также проверить мою среду и предоставить отчет об оценке всех объектов, которые необходимо преобразовать?

A. В этой предварительной версии Synapse Pathway необходимо предоставить ссылку на сценарии DDL/DML, которые необходимо преобразовать. Synapse Pathway не будет проверять текущую среду для обнаружения объектов, которые необходимо преобразовать.

### <a name="q-what-information-does-azure-synapse-pathway-capture-about-my-current-data-warehouse-instance"></a>У. Какие сведения о текущем экземпляре хранилища данных захватывает Azure Synapse Pathway?

A. Так как вы можете запускать Synapse Pathway в локальной среде, данные не захватываются, за исключением вашего имени и названия организации, которые необходимы при загрузке MSI-файла из Центра загрузки Майкрософт.

### <a name="q-where-can-i-raise-issues-encountered-in-azure-synapse-pathway"></a>У. Где можно задать вопрос о проблемах, возникающих в Azure Synapse Pathway?

A. Сейчас Azure Synapse Pathway предоставляется в **предварительной версии**.   Поддержка Synapse Pathway доступна через канал поддержки Майкрософт. Запрос можно создать либо на портале Azure, либо на стандартных порталах (обычно на портале локальной службы поддержки).

> [!NOTE] 
> Как и любые другие службы Azure, все предварительные версии служб дают право на поддержку, но без соглашения об уровне обслуживания.

## <a name="next-steps"></a>Дальнейшие действия

[Загрузить Azure Synapse Pathway](synapse-pathway-download.md)
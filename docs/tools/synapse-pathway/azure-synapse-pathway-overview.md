---
title: Обзор предварительной версии Azure Synapse Pathway
description: Azure Synapse Pathway — это средство для миграции хранилища данных на Azure Synapse Analytics.
author: anshul82-ms
ms.author: anrampal
ms.topic: overview
ms.date: 03/02/2021
ms.prod: sql
ms.technology: tools-other
monikerRange: =azure-sqldw-latest
ms.custom: template-overview
ms.openlocfilehash: 5e3844f6e63fafca5137a646ff4c02edbc7105b8
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2021
ms.locfileid: "102185929"
---
# <a name="azure-synapse-pathway-preview-overview"></a>Обзор предварительной версии Azure Synapse Pathway
[!INCLUDE [Azure Synapse Analytics](../../includes/applies-to-version/asa.md)]

Когда клиенты рассматривают возможность модернизации своих систем хранения данных, одним из важнейших препятствий, с которым они сталкиваются, является преобразование кода SQL. Существующий код написан и оптимизирован для текущей системы, но его нужно оптимизировать для новой системы, на которую осуществляется миграция.

Организации по всему миру хотят модернизировать свою платформу аналитики, чтобы снизить совокупную стоимость владения и внедрить инновации. Однако клиенты вложили в хранилище данных тысячи часов работы, миллионы долларов и десятки тысяч написанных строк кода.
 
Чтобы преобразовать этот критически важный код SQL, клиенты должны либо вручную переписать существующий код SQL, либо потратить крупную сумму из своего бюджета, поручив переписать или преобразовать код сторонней организации.

> [!IMPORTANT]
> Сейчас Azure Synapse Pathway предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены.
 
> Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

**Azure Synapse Pathway** помогает выполнить обновление до современной платформы хранения данных, автоматизируя преобразование кода существующего хранилища данных. Это бесплатное, интуитивно понятное и простое в использовании средство, которое автоматизирует преобразование кода, обеспечивая более быструю миграцию на Azure Synapse Analytics.

 ![Обзор Azure Synapse Pathway.](./media/azure-synapse-pathway-overview/pathway-overview.png) 

Synapse Pathway преобразует инструкции языка описания данных (DDL) и языка обработки данных (DML) в совместимый с T-SQL язык, который также совместим с Azure Synapse SQL.

## <a name="behind-the-scenes"></a>Сопутствующие ресурсы

Дополнительные сведения об Azure Synapse Pathway см. в разделе [Сопутствующие ресурсы](synapse-pathway-behind-the-scenes.md); в нем шаг за шагом рассматривается работа Azure Synapse Pathway.

## <a name="get-azure-synapse-pathway"></a>Получение Azure Synapse Pathway

Чтобы установить Synapse Pathway, найдите в [загруженном файле Azure Synapse Pathway](synapse-pathway-download.md) сведения о необходимых компонентах и ссылку для загрузки последней версии.

## <a name="supported-sources"></a>Поддерживаемые источники

Azure Synapse Pathway поддерживает преобразование кода базы данных, схем и таблиц для следующих источников:
- **IBM Netezza**
- **Microsoft SQL Server**
- **Snowflake**

## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

Дополнительные сведения об Azure Synapse Pathway см. на [странице часто задаваемых вопросов](pathway-faq.md)

## <a name="next-steps"></a>Дальнейшие действия

- [Выполните первое преобразование с помощью Azure Synapse Pathway](synapse-pathway-assessment.md)
- Блог с анонсом. [Анонс Azure Synapse Pathway: ускорение миграции хранилища данных — сообщество Microsoft Tech](https://techcommunity.microsoft.com/t5/azure-synapse-analytics/announcing-azure-synapse-pathway-turbocharge-your-data-warehouse/ba-p/2176630)
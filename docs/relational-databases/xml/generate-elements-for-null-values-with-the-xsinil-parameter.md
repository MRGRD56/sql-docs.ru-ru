---
title: Создание элементов для значений NULL с помощью XSINIL | Документация Майкрософт
description: Сведения о том, как создавать XML-элементы для значений NULL с помощью параметра XSINIL в директиве ELEMENTS.
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- FOR XML clause, null values
- null values [SQL Server], XML
- ELEMENTS directive
- XSINIL parameter
ms.assetid: 2dbc4e48-1cae-4d83-b371-3265da9687cc
author: rothja
ms.author: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 812fc808942562f0852c00948951581abc44e250
ms.sourcegitcommit: 9142bb6b80ce22eeda516b543b163eb9918bc72e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2021
ms.locfileid: "107490847"
---
# <a name="generate-elements-for-null-values-with-the-xsinil-parameter"></a>Создание элементов для значений NULL с помощью параметра XSINIL

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Директива **ELEMENTS** формирует XML, в котором каждое значение в столбце соответствует элементу XML. По умолчанию, если это значение в столбце равно NULL, элемент не добавляется. Однако, указав в директиве ELEMENTS необязательный параметр **XSINIL**, можно запросить, чтобы для значений NULL тоже создавались элементы. В этом случае атрибут **xsi:nil** этого элемента имеет значение TRUE для каждого значения NULL в столбце.  
  
## <a name="see-also"></a>См. также:

[Использование RAW Mode с FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md)

[SELECT — предложение FOR](../../t-sql/queries/select-for-clause-transact-sql.md)

---
title: Образец простого входного файла XML (DTA)
description: В этой статье содержится пример входного файла XML для настройки рабочих нагрузок с использованием помощника по настройке ядра СУБД.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
dev_langs:
- XML
helpviewer_keywords:
- sample applications [DTA]
ms.assetid: 5b00e4eb-1742-43ec-98d8-d84216b6b840
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 1b475ce68b38283903ecd7295a89b18fce6b9636
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354416"
---
# <a name="simple-xml-input-file-sample-dta"></a>Образец простого входного файла XML (DTA)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Скопируйте и вставьте этот образец простого входного файла XML, используемого для настройки рабочих нагрузок, в любой текстовый или XML-редактор. Затем замените значения, заданные для элементов **Server**, **Database**, **Schema**, **Table**, **Workload** и **TuningOptions** , значениями для конкретного сеанса настройки. Дополнительные сведения об атрибутах и дочерних элементах, которые могут использоваться с этими элементами, см. в разделе [Справочник по входным XML-файлам (помощник по настройке ядра СУБД)](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md). В следующем образце используется только подмножество доступных атрибутов и параметров дочерних элементов.  
  
## <a name="code"></a>Код  
 [!code-xml[InputFileSamples#SimpleXMLInputFile](../../tools/dta/codesnippet/xml/simple-xml-input-file-sa_1.xml)]  
  
## <a name="see-also"></a>См. также:  
 [Запуск и использование помощника по настройке ядра СУБД](../../relational-databases/performance/start-and-use-the-database-engine-tuning-advisor.md)   
 [Справочник по входным XML-файлам (помощник по настройке ядра СУБД)](../../tools/dta/xml-input-file-reference-database-engine-tuning-advisor.md)  
  
  

---
description: Диалоговое окно «Обнаруженные синтаксические ошибки SQL» (визуальные инструменты для баз данных)
title: Диалоговое окно "Обнаружены синтаксические ошибки SQL"
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.sqlsyntaxerrorsencountered
- vdtsql.chm:69641
ms.assetid: bc9e5784-227e-4c5d-8084-24274fa6c14a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: e143bbb2f094ee403b1cdab0defac86b999410cf
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88313250"
---
# <a name="sql-syntax-errors-encountered-dialog-box-visual-database-tools"></a>Диалоговое окно «Обнаруженные синтаксические ошибки SQL» (визуальные инструменты для баз данных)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Это диалоговое окно уведомляет о том, что конструктор не может выполнить синтаксический анализ инструкции SQL на панели SQL.  
  
Это диалоговое окно появляется при вводе или редактировании инструкции SQL на панели SQL, после чего следует или переключиться на другую панель, или проверить запрос, или попытаться выполнить запрос. При этом действительно одно из следующих условий.  
  
-   Инструкция SQL неполная или содержит одну или несколько синтаксических ошибок.  
  
-   Инструкция SQL допустима, но не поддерживается в графических панелях (например запрос объединения).  
  
-   Инструкция SQL допустима, но содержит синтаксические выражения, применимые только к используемому подключению к данным.  
  
> [!TIP]  
> Можно проверить, допустима ли инструкция, с помощью клавиши **Проверить синтаксис SQL** на панели инструментов **Запрос** .  
  
В диалоговом окне отображается сообщение, указывающее на причину, по которой нельзя выполнить анализ этой инструкции SQL. Чтобы продолжить, нажмите кнопку **ОК**.  
  
## <a name="see-also"></a>См. также:  
[Разделы по конструированию запросов и представлений (визуальные инструменты для баз данных)](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  

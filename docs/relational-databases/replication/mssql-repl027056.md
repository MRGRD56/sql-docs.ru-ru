---
description: MSSQL_REPL027056
title: MSSQL_REPL027056 | Документация Майкрософт
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_REPL027056 error
ms.assetid: 92d62f3c-b8ae-482e-a348-2e9a8ee9786e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 82c2afc30c295b390ba76ceed4c24c95b6b0d666
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206153"
---
# <a name="mssql_repl027056"></a>MSSQL_REPL027056
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Сведения о сообщении  
  
|attribute|Значение|  
|-|-|  
|Название продукта|SQL Server|  
|Идентификатор события|27056|  
|Источник события|MSSQLSERVER|  
|Компонент|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Символическое имя||  
|Текст сообщения|Процессу слияния не удалось изменить журнал поколений в '%1'. В целях диагностики запустите синхронизацию повторно, включив подробное протоколирование и укажите выходной файл для записи.|  
  
## <a name="explanation"></a>Объяснение  
 Эта ошибка обычно возникает в результате состязания между чрезмерно увеличившимися системными таблицами репликации слиянием. Большой размер системных таблицы обычно обусловлен длительным сроком хранения публикации, поскольку метаданные должны сохраняться в этих таблицах до тех пор, пока не закончится срок хранения.  
  
## <a name="user-action"></a>Действие пользователя  
 **Способы устранения проблемы.**  
  
1.  Уменьшите значение параметров **DownloadGenerationsPerBatch** и **-UploadGenerationsPerBatch** агента слияния, чтобы разрешить продолжение обработки, пока устраняются причины ошибки. Параметры агента могут задаваться в профилях агента или в командной строке. Дополнительные сведения см. в разделе:  
  
    -   [Работа с профилями агента репликации](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
    -   [Просмотр и изменение параметров командной строки агента репликации (среда SQL Server Management Studio)](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Replication Agent Executables Concepts](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
2.  Укажите наименьшее возможное значение срока хранения публикации. Дополнительные сведения см. в разделе [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
3.  Обслуживая репликацию слиянием, иногда проверяйте увеличение размера системных таблиц, связанных с репликацией слиянием: **MSmerge_contents**, **MSmerge_genhistory**, **MSmerge_tombstone**, **MSmerge_current_partition_mappings** и **MSmerge_past_partition_mappings**. Время от времени проводите повторную индексацию этих таблиц. Дополнительные сведения см. в статье [Реорганизация и перестроение индексов](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
## <a name="see-also"></a>См. также:  
 [Справочник по ошибкам и событиям (репликация)](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  

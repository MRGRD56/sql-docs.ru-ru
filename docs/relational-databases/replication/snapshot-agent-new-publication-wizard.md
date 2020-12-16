---
description: Агент моментальных снимков (мастер создания публикаций)
title: Агент моментальных снимков (мастер создания публикаций) | Документация Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.configuresnapshotagent.f1
ms.assetid: 0257d4ee-1f7b-49fd-b4ef-65bfc1ef6951
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 6dab75938bc65b741bd556447d140c57d9557184
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479705"
---
# <a name="snapshot-agent-new-publication-wizard"></a>Агент моментальных снимков (мастер создания публикаций)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  Агент моментальных снимков создает файлы, содержащие схему публикации и данные для инициализации новых подписок. По умолчанию агент моментальных снимков запускается сразу же после создания публикации в мастере создания публикаций. Впоследствии этот агент запускается по указанному расписанию. Будет ли этот агент создавать новые файлы моментальных снимков при каждом запуске, зависит от типа репликации и выбранных параметров. Дополнительные сведения см. в статье [Создание и применение моментального снимка](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
 Для публикаций слиянием, использующих параметризованные фильтры, необходимо создать моментальный снимок для каждой секции данных после завершения снимка публикации. Дополнительные сведения см. в статье [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
## <a name="options"></a>Варианты  
 **Создать моментальный снимок немедленно** (репликация слиянием) или **Создать моментальный снимок немедленно и обеспечить доступ к нему для инициализации подписок** (репликация транзакций)  
 Установите этот флажок для немедленного создания моментального снимка по завершении работы мастера создания публикаций. Снимите этот флажок, если планируете изменить свойства моментального снимка в диалоговом окне **Свойства публикации** перед созданием снимка или если будете инициализировать подписчик без моментального снимка. Дополнительные сведения см. в статье [Инициализация подписки на публикацию транзакций без моментального снимка](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
> [!NOTE]  
>  Мастер может запросить соединение с распространителем, чтобы запустить соответствующее задание для агента распространителя или агента слияния.  
  
 **Запланировать запуск агента моментальных снимков в следующее время**  
 Примите расписание по умолчанию для запусков агента моментальных снимков или нажмите кнопку **Изменить** , чтобы задать собственное расписание.  
  
## <a name="see-also"></a>См. также:  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Создание и применение исходного моментального снимка](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)   
 [Просмотр и изменение свойств публикации](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Инициализация подписки с помощью моментального снимка](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)   
 [Публикация данных и объектов базы данных](../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Replication Agents Overview](../../relational-databases/replication/agents/replication-agents-overview.md)  
  
  

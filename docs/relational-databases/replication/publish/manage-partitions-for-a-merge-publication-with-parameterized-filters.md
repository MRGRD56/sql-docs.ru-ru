---
title: Управление секцией с параметризованными фильтрами (слияние)
description: Управление секциями с параметризованными фильтрами, используемыми для репликации слиянием на SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- partitions [SQL Server replication]
- merge replication partitions [SQL Server replication], SQL Server Management Studio
- parameterized filters [SQL Server replication], partition management
ms.assetid: fb5566fe-58c5-48f7-8464-814ea78e6221
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0b24e2e404dbea75d77cdfc8acdfa50189db2286
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765724"
---
# <a name="manage-partitions-for-a-merge-publication-with-parameterized-filters"></a>Управление секциями для публикации слиянием с параметризованными фильтрами
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  В данном разделе описывается управление секциями для публикации слиянием с параметризованными фильтрами в [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]или объектов RMO. Параметризованные фильтры строк могут быть использованы для формирования неперекрывающихся секций. Такие секции могут быть ограничены таким образом, чтобы каждая данная секция предоставлялась только одной подписке. В таком случае, чем больше число подписчиков, тем большее число секций потребуется, а это, в свою очередь, приведет к необходимости создания такого же числа секционированных моментальных снимков. Дополнительные сведения см. в разделе [Параметризованные фильтры строк](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Рекомендации](#Recommendations)  
  
-   **Для управления секциями для публикации слиянием с параметризованными фильтрами используется:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [объекты RMO;](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
  
-   Если в соответствии с рекомендациями создается скрипт топологии репликации, скрипты публикации содержат вызовы хранимых процедур для создания секций данных. Скрипт содержит справочную информацию для созданных секций и способ воссоздания одной или нескольких секций в случае необходимости. Дополнительные сведения см. в разделе [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
-   Если публикация имеет параметризованные фильтры, позволяющие получать подписки с неперекрывающимися секциями, то при необходимости повторного создания подписки в случае ее утраты необходимо удалить секцию, которая была на нее подписана, создать заново подписку, а затем повторно создать секцию. Дополнительные сведения см. в разделе [Параметризованные фильтры строк](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md). При формировании скрипта публикации репликация формирует скрипты создания для существующих секций подписчика. Дополнительные сведения см. в разделе [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 Управление секциями осуществляется на странице **Секции данных** диалогового окна **Свойства публикации — \<Publication>** . Дополнительные сведения о доступе к этому диалоговому окну см. в разделе [Просмотр и изменение свойств публикации](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md). На этой странице доступны следующие возможности: создание и удаление секций, разрешение подписчикам выполнять создание и доставку моментального снимка, создание моментальных снимков для одной или нескольких секций, очистка моментальных снимков.  
  
#### <a name="to-create-a-partition"></a>Создание секции  
  
1.  На странице **Секции данных** диалогового окна **Свойства публикации — \<Publication>** нажмите кнопку **Добавить**.  
  
2.  В диалоговом окне **Добавить секцию данных** введите значение **HOST_NAME()** и/или значение **SUSER_SNAME()** , связанное с секцией, которую требуется создать.  
  
3.  При необходимости задайте расписание обновления моментальных снимков.  
  
    1.  Установите флажок **Запланировать запуск агента моментальных снимков для этой секции в следующее время**.  
  
    2.  Примите используемое по умолчанию расписание обновления моментальных снимков или щелкните **Изменить** , чтобы указать другое расписание.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

#### <a name="to-delete-a-partition"></a>Удаление секции  
  
1.  На странице **Секции данных** выберите секцию в сетке.  
  
2.  Щелкните **Удалить**.  
  
#### <a name="to-allow-subscribers-to-initiate-snapshot-generation-and-delivery"></a>Разрешение подписчикам запускать создание и доставку моментальных снимков  
  
1.  На странице **Секции данных** выберите **Автоматически определять секцию и, при необходимости, создавать моментальный снимок при попытке синхронизации от нового подписчика**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-generate-a-snapshot-for-a-partition"></a>Создание моментального снимка секции  
  
1.  На странице **Секции данных** выберите секцию в сетке.  
  
2.  Щелкните **Создать выбранные моментальные снимки**.  
  
#### <a name="to-clean-up-a-snapshot-for-a-partition"></a>Очистка моментального снимка секции  
  
1.  На странице **Секции данных** выберите секцию в сетке.  
  
2.  Щелкните **Очистить существующие моментальные снимки**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 Программный перебор существующих секций с помощью хранимых процедур репликации позволяет повысить управляемость публикаций с параметризованными фильтрами. Кроме того, секции можно создавать и удалять. Для существующей секции доступны следующие сведения.  
  
-   Метод фильтрации секции (при помощи функций [SUSER_SNAME (Transact-SQL)](../../../t-sql/functions/suser-sname-transact-sql.md) или [HOST_NAME (Transact-SQL)](../../../t-sql/functions/host-name-transact-sql.md)).  
  
-   Имя задания, которым формируется секционированный моментальный снимок.  
  
-   Время последнего запуска задания секционированного снимка.  
  
 Если вторая часть снимка, состоящего из двух частей, может быть сформирована по запросу в момент инициализации новой подписки, то приведенные ниже процедуры позволят выполнить предварительное формирование снимка в подходящее время и управлять его формированием. Дополнительные сведения см. в статье [Snapshots for Merge Publications with Parameterized Filters](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
#### <a name="to-view-information-on-existing-partitions"></a>Просмотр информации о существующих секциях  
  
1.  На издателе в базе данных публикации выполните хранимую процедуру [sp_helpmergepartition (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-helpmergepartition-transact-sql.md). Укажите имя публикации в параметре `@publication`. (Необязательно) Укажите с помощью параметра `@suser_sname` или `@host_name`, что нужно вернуть данные на основе одного критерия фильтрации.  
  
#### <a name="to-define-a-new-partition-and-generate-a-new-partitioned-snapshot"></a>Определение новой секции и формирование нового секционированного снимка  
  
1.  На издателе в базе данных публикации выполните хранимую процедуру [sp_addmergepartition (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md). Укажите имя публикации в параметре `@publication`, а также параметризованное значение, определяющее секцию, в одном из следующих параметров:  
  
    -   `@suser_sname`, если параметризованный фильтр определен значением, возвращенным [SUSER_SNAME &#40;Transact-SQL&#41;](../../../t-sql/functions/suser-sname-transact-sql.md);  
  
    -   `@host_name`, если параметризованный фильтр определен значением, возвращенным [HOST_NAME &#40;Transact-SQL&#41;](../../../t-sql/functions/host-name-transact-sql.md).  
  
2.  Создайте и инициализируйте параметризованный снимок для новой секции. Дополнительные сведения см. в статье [Создание моментального снимка для публикации слиянием с параметризованными фильтрами](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md).  
  
#### <a name="to-delete-a-partition"></a>Удаление секции  
  
1.  На издателе в базе данных публикации выполните хранимую процедуру [sp_dropmergepartition (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-dropmergepartition-transact-sql.md). Укажите имя публикации в параметре `@publication`, а также параметризованное значение, определяющее секцию, в одном из следующих параметров:  
  
    -   `@suser_sname`, если параметризованный фильтр определен значением, возвращенным [SUSER_SNAME &#40;Transact-SQL&#41;](../../../t-sql/functions/suser-sname-transact-sql.md);  
  
    -   `@host_name`, если параметризованный фильтр определен значением, возвращенным [HOST_NAME &#40;Transact-SQL&#41;](../../../t-sql/functions/host-name-transact-sql.md).  
  
     При этом также будет удалено задание моментального снимка и все файлы снимка для этой секции.  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> При помощи объектов RMO  
 Программное формирование новых, перебор существующих и удаление секций подписчика с помощью объектов RMO обеспечивает большую управляемость публикации с параметризованными фильтрами. Сведения о создании секций подписчика см. в разделе [Создание моментального снимка для публикации слиянием с параметризованными фильтрами](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md). Для существующей секции доступны следующие сведения.  
  
-   Значение и функция фильтрации, на которых базируется секция.  
  
-   Имя задания, формирующего параметризованный снимок для подписчика.  
  
-   Время последнего выполнения задания параметризованного моментального снимка.  
  
#### <a name="to-view-information-on-existing-partitions"></a>Просмотр информации о существующих секциях  
  
1.  Создайте соединение с издателем с помощью класса <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.MergePublication>. Задайте для публикации свойства <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> и <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> , а также установите созданное на шаге 1 соединение <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> в качестве значения для свойства <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
3.  Чтобы получить свойства объекта, вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Если этот метод возвращает **false**, то либо на шаге 2 были неверно определены свойства публикации, либо публикация не существует.  
  
4.  Вызовите метод <xref:Microsoft.SqlServer.Replication.MergePublication.EnumMergePartitions%2A> и передайте его результат в массив объектов <xref:Microsoft.SqlServer.Replication.MergePartition> .  
  
5.  Для каждого объекта <xref:Microsoft.SqlServer.Replication.MergePartition> в массиве доступны любые интересующие свойства.  
  
#### <a name="to-delete-existing-partitions"></a>Удаление существующих секций  
  
1.  Создайте соединение с издателем с помощью класса <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Создайте экземпляр класса <xref:Microsoft.SqlServer.Replication.MergePublication>. Задайте для публикации свойства <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> и <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> , а также установите созданное на шаге 1 соединение <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> в качестве значения для свойства <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
3.  Чтобы получить свойства объекта, вызовите метод <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Если этот метод возвращает **false**, то либо на шаге 2 были неверно определены свойства публикации, либо публикация не существует.  
  
4.  Вызовите метод <xref:Microsoft.SqlServer.Replication.MergePublication.EnumMergePartitions%2A> и передайте его результат в массив объектов <xref:Microsoft.SqlServer.Replication.MergePartition> .  
  
5.  Определите для каждого объекта <xref:Microsoft.SqlServer.Replication.MergePartition> в массиве, необходимо ли удаление соответствующей секции. Решение обычно принимается исходя из значения свойств <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterLogin%2A> и <xref:Microsoft.SqlServer.Replication.MergePartition.DynamicFilterHostName%2A> .  
  
6.  Вызовите метод <xref:Microsoft.SqlServer.Replication.MergePublication.RemoveMergePartition%2A> объекта <xref:Microsoft.SqlServer.Replication.MergePublication> , созданного на шаге 2. Передайте объект <xref:Microsoft.SqlServer.Replication.MergePartition> , созданный на шаге 5.  
  
7.  Повторите шаг 6 для каждой удаляемой секции.  
  
## <a name="see-also"></a>См. также:  
 [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [Snapshots for Merge Publications with Parameterized Filters](../../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md)  
  
  
  

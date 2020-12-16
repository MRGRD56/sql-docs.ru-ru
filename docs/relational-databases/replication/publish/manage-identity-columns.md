---
description: Управление столбцами идентификаторов
title: Управление столбцами идентификаторов | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- identity values [SQL Server replication]
- merge replication [SQL Server replication], identity range management
- publishing [SQL Server replication], identity columns
- transactional replication, identity range management
- identity columns [SQL Server], replication
ms.assetid: 98892836-cf63-494a-bd5d-6577d9810ddf
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 6c7f660c4550a3bd792b2132ad0699d944b90a2e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468995"
---
# <a name="manage-identity-columns"></a>Управление столбцами идентификаторов
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  В этом разделе описывается управление столбцами идентификаторов в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Если операции вставки с подписчика реплицируются обратно на издатель, необходимо управлять столбцами идентификаторов, чтобы избежать присваивания одинаковых значений идентификаторов на подписчике и издателе. Репликация может управлять диапазонами идентификаторов автоматически, или можно управлять диапазонами идентификаторов вручную.  Сведения о параметрах управления диапазонами идентификаторов, предоставляемых репликацией, см. в статье [Replicate Identity Columns](../../../relational-databases/replication/publish/replicate-identity-columns.md) (Репликация столбцов идентификаторов).  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Рекомендации](#Recommendations)  
  
-   **Для управления столбцами идентификаторов используются:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
  
-   При публикации таблицы в нескольких публикациях необходимо указать одинаковые параметры управления диапазонами идентификаторов для всех публикаций. Дополнительные сведения см. в разделе "Публикация таблиц в нескольких публикациях" статьи [Публикация данных и объектов базы данных](../../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
-   Сведения о том, как создать автоматически увеличивающееся числовое значение, которое может использоваться в нескольких таблицах или вызываться из приложений без ссылки на какие-либо таблицы, см. в разделе [Порядковые номера](../../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 Укажите параметр управления столбцами идентификаторов на вкладке **Свойства** в диалоговом окне **Свойства статьи —\<Article>** мастера создания публикации. Дополнительные сведения об использовании мастера см. в статье [Создание публикации](../../../relational-databases/replication/publish/create-a-publication.md). В мастере создания публикации выполните следующие действия.  
  
-   Если выбрана настройка **Публикация слиянием** или **Публикация транзакций с обновляемыми подписками** на странице **Тип публикации** , выберите автоматическое или ручное управление диапазонами идентификаторов (рекомендуется автоматический режим, используемый по умолчанию). После публикации таблицы данное свойство невозможно изменить, но другие связанные свойства изменить можно.  
  
-   При выборе других типов публикаций должен быть установлен ручной режим управления диапазонами идентификаторов.  
  
 Диапазоны идентификаторов и пороговые значения можно изменить на вкладке **Свойства** диалогового окна **Свойства статьи —\<Article>** , которая доступна в диалоговом окне **Свойства публикации — \<Publication>** . Дополнительные сведения о доступе к этому диалоговому окну см. в разделе [Просмотр и изменение свойств публикации](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-an-identity-column-management-option"></a>Указание параметра управления столбцом идентификаторов  
  
1.  Если на издателе запущена версия [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , более ранняя, чем [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)], то на странице **Тип публикации** мастера создания публикаций выберите **Публикация слиянием** или **Публикация транзакций с обновляемыми подписками**.  
  
2.  На странице **Статьи** выберите таблицу со столбцом идентификаторов.  
  
3.  Щелкните **Свойства статьи**, затем щелкните **Указать свойства выделенной статьи таблицы**.  
  
4.  На вкладке **Свойства** диалогового окна **Свойства статьи — \<Article>** , в разделе **Управление диапазоном идентификаторов** установите для свойства **Автоматически управлять диапазонами идентификаторов** значение **Автоматически** или **Вручную** (для издателей, использующих [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] или более позднюю версию) либо **True** или **False** (для издателей, использующих версию [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], предшествующую [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]).  
  
5.  Если на шаге 4 выбрано **Автоматически** или **True** , введите значения для параметров в следующей таблице. Дополнительные сведения об использовании данных настроек см. в разделе "Назначение диапазонов идентификаторов" статьи [Replicate Identity Columns](../../../relational-databases/replication/publish/replicate-identity-columns.md) (Репликация столбцов идентификаторов).  
  
    |Параметр|Значение|Описание|  
    |------------|-----------|-----------------|  
    |**Размер диапазона издателя**|Целое значение для размера диапазона (например, 20 000).|Дополнительные сведения см. в разделе "Назначение диапазонов идентификаторов" статьи [Replicate Identity Columns](../../../relational-databases/replication/publish/replicate-identity-columns.md) (Репликация столбцов идентификаторов).|  
    |**Размер диапазона на стороне подписчика**|Целое значение для размера диапазона (например, 10000).|Дополнительные сведения см. в разделе "Назначение диапазонов идентификаторов" статьи [Replicate Identity Columns](../../../relational-databases/replication/publish/replicate-identity-columns.md) (Репликация столбцов идентификаторов).|  
    |**Процентный порог выделения диапазона**|Целое значение для процентного порога (например, 90 эквивалентно 90 процентам).|Процент общего числа значений идентификаторов, используемый в узле до задания нового диапазона идентификаторов.<br /><br /> <br /><br /> Примечание. Данное значение должно быть указано, но оно используется только подписчиками, использующими подписки, обновляемые посредством очередей, а также подписчиками публикаций слиянием, на которых выполняется [!INCLUDE[ssEW](../../../includes/ssew-md.md)] или предыдущие версии других выпусков SQL Server. Дополнительные сведения см. в разделе "Назначение диапазонов идентификаторов" статьи [Replicate Identity Columns](../../../relational-databases/replication/publish/replicate-identity-columns.md) (Репликация столбцов идентификаторов).|  
    |**Начальное значение следующего диапазона**|Целое значение. Только для чтения.|Значение, с которого начнется следующий диапазон. Например, если текущий диапазон 5001-6000, то это значение будет равно 6001.|  
    |**Максимальное значение идентификатора**|Целое значение. Только для чтения.|Наибольшее значение для столбца идентификаторов. Определяется базовым типом данных столбца.|  
    |**Приращение**|Целое значение. Только для чтения.|Значение, на которое должно увеличиться или уменьшиться для каждой вставки значение в столбце идентификаторов. Обычно используется значение 1.|  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-modify-identity-ranges-and-thresholds-after-a-table-is-published"></a>Изменение диапазонов идентификаторов и пороговых значений после публикации таблицы  
  
1.  На странице **Статьи** диалогового окна **Свойства публикации — \<Publication>** выберите таблицу в столбце идентификаторов.  
  
2.  Щелкните **Свойства статьи**, затем щелкните **Указать свойства выделенной статьи таблицы**.  
  
3.  На вкладке **Свойства** диалогового окна **Article Свойства статьи — \<Article>** в разделе **Управление диапазонами идентификаторов** введите значения для одного или нескольких из следующих параметров: **Размер диапазона издателя**, **Размер диапазона на стороне подписчика** и **Процентный порог выделения диапазона**.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Нажмите кнопку **ОК**, чтобы вернуться в диалоговое окно **Свойства публикации — \<Publication>** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 Чтобы указать параметры управления диапазонами идентификаторов, при создании статьи можно использовать хранимые процедуры репликации.  
  
#### <a name="to-enable-automatic-identity-range-management-when-defining-articles-for-a-transactional-publication"></a>Включение автоматического управления диапазонами идентификаторов при определении статей для публикации транзакций  
  
1.  Выполните процедуру [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)на издателе в базе данных публикации. Если в публикуемой исходной таблице имеется столбец идентификаторов, укажите значение **auto** в параметре **\@identityrangemanagementoption**, диапазон значений идентификаторов, назначенный издателю, в параметре **\@pub_identity_range**, диапазон значений идентификаторов, назначенный каждому подписчику, в параметре **\@identity_range** и процент использованных значений идентификаторов, по достижении которого назначается новый диапазон идентификаторов, в параметре **\@threshold**. Дополнительные сведения об определении статей см. в [этой статье](../../../relational-databases/replication/publish/define-an-article.md).  
  
    > [!NOTE]  
    >  Убедитесь, что тип данных столбца идентификаторов достаточно большой, чтобы поддерживать полный диапазон идентификаторов, назначаемый всем подписчикам.  
  
#### <a name="to-disable-automatic-identity-range-management-when-defining-articles-for-a-transactional-publication"></a>Отключение автоматического управления диапазонами идентификаторов при определении статей для публикации транзакций  
  
1.  Выполните процедуру [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)на издателе в базе данных публикации. Укажите значение **manual** в параметре **\@identityrangemanagementoption**. Дополнительные сведения об определении статей см. в [этой статье](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Назначьте диапазоны для столбцов идентификаторов в статье на подписчике, чтобы избежать возникновения конфликтов при обновлении подписчиков. Дополнительные сведения см. в разделе Assigning ranges for manual identity range management (Назначение диапазонов для управления диапазонами идентификаторов вручную) статьи [Replicate Identity Columns](../../../relational-databases/replication/publish/replicate-identity-columns.md) (Репликация столбцов идентификаторов).  
  
#### <a name="to-enable-automatic-identity-range-management-when-defining-articles-for-a-merge-publication"></a>Включение автоматического управления диапазонами идентификаторов при определении статей для публикации слиянием  
  
1.  В базе данных публикации на издателе выполните процедуру [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Если в публикуемой исходной таблице имеется столбец идентификаторов, укажите значение **auto** в параметре **\@identityrangemanagementoption**, диапазон значений идентификаторов, назначенный серверной подписке, в параметре **\@pub_identity_range**, диапазон значений идентификаторов, назначенный издателю и каждой клиентской подписке, в параметре **\@identity_range**, и процент значений идентификаторов, который должен быть использован перед назначением нового диапазона идентификаторов, в параметре **\@threshold**. Дополнительные сведения о времени назначения диапазонов идентификаторов см. в разделе Assigning Identity Ranges (Назначение диапазонов идентификаторов) статьи [Replicate Identity Columns](../../../relational-databases/replication/publish/replicate-identity-columns.md) (Репликация столбцов идентификаторов). Дополнительные сведения об определении статей см. в [этой статье](../../../relational-databases/replication/publish/define-an-article.md).  
  
    > [!NOTE]  
    >  Убедитесь, что тип данных столбца идентификаторов достаточно большой, чтобы поддерживать полный диапазон идентификаторов, назначаемый всем подписчикам, особенно для подписчиков с серверными подписками.  
  
#### <a name="to-disable-automatic-identity-range-management-when-defining-articles-for-a-merge-publication"></a>Отключение автоматического управления диапазонами идентификаторов при определении статей для публикации слиянием  
  
1.  В базе данных публикации на издателе выполните процедуру [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Укажите одно из следующих значений в параметре **\@identityrangemanagementoption**:  
  
    -   **manual** — диапазоны идентификаторов должны назначаться вручную для обновляемых подписчиков;  
  
    -   **none** — столбцы идентификаторов на издателе не будут определяться как столбцы идентификаторов на подписчике.  
  
     Дополнительные сведения об определении статей см. в [этой статье](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Назначьте диапазоны для столбцов идентификаторов в статье на подписчике, чтобы избежать возникновения конфликтов при обновлении подписчиков.  
  
#### <a name="to-change-automatic-identity-range-management-settings-for-an-existing-article-in-a-snapshot-or-transactional-publication"></a>Изменение настроек автоматического управления диапазонами идентификаторов для существующей статьи в публикации моментального снимка или публикации транзакций  
  
1.  На издателе в базе данных публикации выполните хранимую процедуру [sp_helparticle](../../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md) и проверьте значение параметра **identityrangemanagementoption** в результирующем наборе. Если это значение равно **0**, автоматическое управление диапазонами идентификаторов отключено.  
  
2.  Если значение **identityrangemanagementoption** в результирующем наборе равно **1**, измените настройки следующим образом.  
  
    -   Чтобы изменить назначенные диапазоны идентификаторов, выполните на издателе в базе данных публикации хранимую процедуру [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) . Укажите значение **identity_range** или **pub_identity_range** в параметре **\@property** и задайте новое значение диапазона в параметре **\@value**.  
  
    -   Чтобы изменить пороговое значение, с которого назначаются диапазоны идентификаторов, выполните на издателе в базе данных публикации хранимую процедуру [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) . Укажите значение **threshold** в параметре **\@property** и задайте новое пороговое значение в параметре **\@value**.  
  
#### <a name="to-change-automatic-identity-range-management-settings-for-an-existing-article-in-a-merge-publication"></a>Изменение настроек автоматического управления диапазонами идентификаторов для существующей статьи в публикации слиянием  
  
1.  На издателе в базе данных публикации выполните хранимую процедуру [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) и проверьте значение параметра **identity_support** в результирующем наборе. Если это значение равно **0**, автоматическое управление диапазонами идентификаторов отключено.  
  
2.  Если значение **identity_support** в результирующем наборе равно **1**, измените настройки следующим образом.  
  
    -   Чтобы изменить назначенный диапазон идентификаторов, выполните на издателе в базе данных публикации хранимую процедуру [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) . Укажите значение **identity_range** или **pub_identity_range** в параметре **\@property** и задайте новое значение диапазона в параметре **\@value**.  
  
    -   Чтобы изменить пороговое значение, с которого назначаются диапазоны идентификаторов, выполните на издателе в базе данных публикации хранимую процедуру [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) . Укажите значение **threshold** в параметре **\@property** и задайте новое пороговое значение в параметре **\@value**. Дополнительные сведения о времени назначения диапазонов идентификаторов см. в разделе Assigning Identity Ranges (Назначение диапазонов идентификаторов) статьи [Replicate Identity Columns](../../../relational-databases/replication/publish/replicate-identity-columns.md) (Репликация столбцов идентификаторов).  
  
    -   Чтобы отключить автоматическое управление диапазонами идентификаторов, выполните на издателе в базе данных публикации хранимую процедуру [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) . Укажите значение **identityrangemanagementoption** в параметре **\@property** и значение **manual** или **none** в параметре **\@value**.  
  
## <a name="see-also"></a>См. также:  
 [Одноранговая репликация транзакций](../../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Репликация столбцов идентификаторов](../../../relational-databases/replication/publish/replicate-identity-columns.md)  
  
  

---
description: Определение и изменение статического строкового фильтра
title: Определение и изменение статического строкового фильтра | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- modifying filters, static row
- static row filters
- filters [SQL Server replication], static row
ms.assetid: a6ebb026-026f-4c39-b6a9-b9998c3babab
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 626665bff61cd6f0c1799ef52134c50688d962b6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477735"
---
# <a name="define-and-modify-a-static-row-filter"></a>Определение и изменение статического строкового фильтра
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  В этом разделе описывается определение и изменение статического строкового фильтра [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] при помощи среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Ограничения](#Restrictions)  
  
     [Рекомендации](#Recommendations)  
  
-   **Для определения и изменения статической строковой фильтрации используется:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Ограничения  
  
-   Если добавление, изменение или удаление статической строковой фильтрацией выполняется после инициализации подписок на публикацию, следует создать новый моментальный снимок и повторно инициализировать все подписки после внесения изменений. Дополнительные сведения о требованиях к изменениям свойств см. в статье [Изменение свойств публикации и статьи](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
-   Если публикация включена для одноранговой репликации транзакций, таблицы не могут быть отфильтрованы.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Рекомендации  
  
-   Поскольку эти фильтры являются статическими, все подписчики получат один и тот же поднабор данных. Если необходимо динамически фильтровать строки в статье таблицы, принадлежащей к публикации слиянием, чтобы каждый подписчик получал собственную секцию данных, см. раздел [Определение и изменение параметризованного фильтра строк для статьи публикации слиянием](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md). Репликация слиянием также позволяет фильтровать связанные строки на основе существующего фильтра строк. Дополнительные сведения см. в статье [Определение и изменение фильтра соединения между статьями публикации слиянием](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
 Операции определения, изменения и удаления статических строковых фильтров выполняются на странице **Фильтрация строк таблицы** мастера создания публикаций или на странице **Фильтрация строк** диалогового окна **Свойства публикации — \<Publication>** . Дополнительные сведения об использовании мастера и доступе к этому диалоговому окну см. в статьях [Создание публикации](../../../relational-databases/replication/publish/create-a-publication.md) и [Просмотр и изменение свойств публикации](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-define-a-static-row-filter"></a>Определение статического фильтра строк  
  
1.  Действия, выполняемые на странице **Фильтрация строк таблицы** мастера создания публикаций или на странице **Фильтрация строк** диалогового окна **Свойства публикации — \<Publication>** , зависят от типа публикации:  
  
    -   Для публикации моментальных снимков или публикации транзакций щелкните **Добавить**.  
  
    -   Для публикации слиянием щелкните **Добавить**, а затем щелкните **Добавить фильтр**.  
  
2.  В окне **Добавление фильтра** выберите в раскрывающемся списке таблицу для фильтрации.  
  
3.  Создайте инструкцию фильтра в текстовом поле **Инструкция фильтра** . Можно ввести текст в тестовом поле или перетащить столбцы из списка **Столбцы** .  
  
    > [!NOTE]  
    >  В предложении WHERE необходимо использовать имена, состоящие из двух частей; имена, состоящие из трех или четырех частей, не поддерживаются. Если публикация является публикацией от издателя Oracle, предложение WHERE должно соответствовать синтаксису Oracle.  
  
    -   Текстовая область **Инструкция фильтра** содержит текст по умолчанию, в виде:  
  
        ```sql  
        SELECT <published_columns> FROM [schema].[tablename] WHERE  
        ```  
  
    -   Текст по умолчанию изменять нельзя. Введите предложение фильтра после ключевого слова WHERE, используя стандартный синтаксис SQL. Законченное предложение фильтра должно выглядеть следующим образом:  
  
        ```sql  
        SELECT <published_columns> FROM [HumanResources].[Employee] WHERE [LoginID] = 'adventure-works\ranjit0'  
        ```  
  
    -   В статическом фильтре строк может содержаться определяемая пользователем функция. Полное предложение фильтра для статического фильтра строк с определяемой пользователем функцией должно выглядеть следующим образом:  
  
        ```sql  
        SELECT <published_columns> FROM [Sales].[SalesOrderHeader] WHERE MyFunction([Freight]) > 100  
        ```  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Если вы находитесь в диалоговом окне **Свойства публикации — \<Publication>** , нажмите кнопку **OK**, чтобы сохранить результаты и закрыть диалоговое окно.  

#### <a name="to-modify-a-static-row-filter"></a>Изменение статического фильтра строк  
  
1.  На странице **Фильтрация строк таблицы** мастера создания публикаций или на странице **Фильтрация строк** диалогового окна **Свойства публикации — \<Publication>** выберите фильтр в области **Отфильтрованные таблицы**, а затем нажмите кнопку **Изменить**.  
  
2.  В окне **Изменение фильтра** измените фильтр.  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-delete-a-static-row-filter"></a>Удаление статического фильтра строк  
  
1.  На странице **Фильтрация строк таблицы** мастера создания публикаций или на странице **Фильтрация строк** диалогового окна **Свойства публикации — \<Publication>** выберите фильтр в области **Отфильтрованные таблицы**, а затем нажмите кнопку **Удалить**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
 При создании статей таблиц можно определить предложение WHERE для фильтрации строк из статьи. Также можно изменить фильтр строк уже после того, как он был определен. Статические фильтры строк можно создавать и изменять программно, с помощью хранимых процедур репликации.  
  
#### <a name="to-define-a-static-row-filter-for-a-snapshot-or-transactional-publication"></a>Определение статического фильтра строк для публикации транзакций или публикации моментальных снимков  
  
1.  Определите статью для фильтрации. Дополнительные сведения см. в статье [определить статью](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  В издателе в базе данных публикации выполните процедуру [sp_articlefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md). Укажите имя статьи в параметре **\@article**, имя публикации в параметре **\@publication**, имя фильтра в параметре **\@filter_name** и предложение фильтрации в параметре **\@filter_clause** (не включая `WHERE`).  
  
3.  При необходимости определения фильтра столбцов см. раздел [Определение или изменение фильтра столбцов](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md). В противном случае выполнение процедуру [sp_articleview (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). В параметре **\@publication** укажите имя публикации, в параметре **\@article** — имя фильтруемой статьи, а в параметре **\@filter_clause** — условие фильтра, указанное в шаге 2. Будут созданы объекты синхронизации для отфильтрованной статьи.  
  
#### <a name="to-modify-a-static-row-filter-for-a-snapshot-or-transactional-publication"></a>Изменение статического фильтра строк для моментального снимка публикации транзакций  
  
1.  В издателе в базе данных публикации выполните процедуру [sp_articlefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md). Укажите имя статьи в параметре **\@article**, имя публикации в параметре **\@publication**, имя фильтра в параметре **\@filter_name** и новое предложение фильтрации в параметре **\@filter_clause** (не включая `WHERE`). Так как в результате этого изменения данные в существующей подписке станут недопустимыми, укажите значение **1** в параметре **\@force_reinit_subscription**.  
  
2.  В издателе в базе данных публикации выполните процедуру [sp_articleview &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). В параметре **\@publication** укажите имя публикации, в параметре **\@article** — имя фильтруемой статьи, а в параметре **\@filter_clause** — условие фильтра, указанное в шаге 1. В результате будет повторно создано представление, определяющее опубликованную статью.  
  
3.  Чтобы сформировать обновленный моментальный снимок, перезапустите задание агента моментальных снимков для публикации. Дополнительные сведения см. в разделе [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
4.  Повторная инициализация подписок. Дополнительные сведения см. в статье [Повторная инициализация подписок](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-delete-a-static-row-filter-for-a-snapshot-or-transactional-publication"></a>Удаление статического фильтра строк для моментального снимка публикации транзакций  
  
1.  В издателе в базе данных публикации выполните процедуру [sp_articlefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md). Укажите имя статьи в параметре **\@article**, имя публикации в параметре **\@publication**, значение NULL в параметре **\@filter_name** и NULL в параметре **\@filter_clause**. Так как в результате этого изменения данные в существующей подписке станут недопустимыми, укажите значение **1** в параметре **\@force_reinit_subscription**.  
  
2.  Чтобы сформировать обновленный моментальный снимок, перезапустите задание агента моментальных снимков для публикации. Дополнительные сведения см. в разделе [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
3.  Повторная инициализация подписок. Дополнительные сведения см. в статье [Повторная инициализация подписок](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-define-a-static-row-filter-for-a-merge-publication"></a>Определение статического фильтра строк для публикации слиянием  
  
1.  В базе данных публикации на издателе выполните процедуру [sp_addmergearticle (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Укажите предложение фильтрации в параметре **\@subset_filterclause** (не включая `WHERE`). Дополнительные сведения см. в статье [определить статью](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  При необходимости определения фильтра столбцов см. раздел [Определение или изменение фильтра столбцов](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md).  
  
#### <a name="to-modify-a-static-row-filter-for-a-merge-publication"></a>Изменение статического фильтра строк для публикации слиянием  
  
1.  В базе данных публикации на издателе выполните процедуру [sp_changemergearticle (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Укажите имя публикации в параметре **\@publication**, имя фильтруемой статьи в параметре **\@article**, значение свойства **subset_filterclause** в параметре **\@property** и новое предложение фильтра в параметре **\@value** (не включая `WHERE`). Так как в результате этого изменения данные в существующих подписках станут недопустимыми, укажите значение 1 в параметре **\@force_reinit_subscription**.  
  
2.  Чтобы сформировать обновленный моментальный снимок, перезапустите задание агента моментальных снимков для публикации. Дополнительные сведения см. в разделе [Create and Apply the Initial Snapshot](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
3.  Повторная инициализация подписок. Дополнительные сведения см. в статье [Повторная инициализация подписок](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Примеры (Transact-SQL)  
 В этом примере репликации транзакций статья фильтруется горизонтально, чтобы удалить все неподдерживаемые продукты.  
  
 [!code-sql[HowTo#sp_AddTranArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-stat_1.sql)]  
  
 В этом примере репликации слиянием статьи фильтруются горизонтально, чтобы возвратить только строки, связанные с указанным менеджером по продажам. Также используется фильтр соединения. Дополнительные сведения см. в статье [Определение и изменение фильтра соединения между статьями публикации слиянием](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-stat_2.sql)]  
  
## <a name="see-also"></a>См. также:  
 [Определение и изменение параметризованного фильтра строк для статьи публикации слиянием](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Изменение свойств публикации и статьи](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Фильтрация опубликованных данных](../../../relational-databases/replication/publish/filter-published-data.md)   
 [Фильтрация опубликованных данных для репликации слиянием](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)  
  
  

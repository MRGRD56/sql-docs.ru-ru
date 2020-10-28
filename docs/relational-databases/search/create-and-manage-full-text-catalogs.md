---
title: Создание и управление полнотекстовыми каталогами
description: Создание и управление полнотекстовыми каталогами
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
f1_keywords:
- sql13.swb.fulltextsearch.ftcatalog.general.f1
helpviewer_keywords:
- full-text catalogs [SQL Server], creating
- full-text search [SQL Server], using SQL Server Management Studio
ms.assetid: 824b7131-44a6-4815-89e6-62b7bab060e3
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ff19e0cd9ef6b88dae2410edbc4cae74e911c1a0
ms.sourcegitcommit: 5f3e0eca9840db20038f0362e5d88a84ff3424af
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344006"
---
# <a name="create-and-manage-full-text-catalogs"></a>Создание и управление полнотекстовыми каталогами

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
Полнотекстовый каталог — это логический контейнер для группы полнотекстовых индексов. Необходимо создать полнотекстовый каталог, прежде чем создавать полнотекстовый индекс.

Полнотекстовый каталог является виртуальным объектом и не входит в какую-либо файловую группу.
  
##  <a name="create-a-full-text-catalog"></a><a name="creating"></a> Создание полнотекстового каталога  

### <a name="create-a-full-text-catalog-with-transact-sql"></a>Создание полнотекстового каталога с помощью Transact-SQL
Используйте инструкцию [CREATE FULLTEXT CATALOG](../../t-sql/statements/create-fulltext-catalog-transact-sql.md). Пример:

```sql 
USE AdventureWorks;  
GO  
CREATE FULLTEXT CATALOG ftCatalog AS DEFAULT;  
GO  
``` 

### <a name="create-a-full-text-catalog-with-management-studio"></a>Создание полнотекстового каталога в SQL Server Management Studio
1.  В обозревателе объектов разверните сервер, затем узел **Базы данных** , а затем — базу данных, в которой необходимо создать полнотекстовый каталог.  
  
2.  Разверните узел **Хранилище** , затем щелкните правой кнопкой мыши **Полнотекстовые каталоги** .  
  
3.  Выберите **Создание полнотекстового каталога** .  
  
4.  В диалоговом окне **Создание полнотекстового каталога** укажите сведения о вновь создаваемом каталоге. Дополнительные сведения см. в разделе [Создание полнотекстового каталога (страница "Общие")](../../t-sql/statements/create-fulltext-catalog-transact-sql.md).  
  
    > [!NOTE]  
    >  Идентификаторы полнотекстовых каталогов начинаются с 00005 и увеличиваются на единицу для каждого вновь создаваемого каталога.  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

##  <a name="get-the-properties-of-a-full-text-catalog"></a><a name="props"></a> Получение свойств полнотекстового каталога  
Используйте функцию [!INCLUDE[tsql](../../includes/tsql-md.md)]**FULLTEXTCATALOGPROPERTY** , чтобы получить значения различных свойств, связанных с полнотекстовыми каталогами. Дополнительные сведения см. в разделе [FULLTEXTCATALOGPROPERTY](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md).

Например, выполните следующий запрос, чтобы получить количество индексов в полнотекстовом каталоге `Catalog1`.

```sql 
USE <database>;  
GO  
SELECT fulltextcatalogproperty('Catalog1', 'ItemCount');  
GO  
```  
  
В следующей таблице перечислены свойства, о которых сообщается в полнотекстовых каталогах. Эти сведения полезны для администрирования и устранения нарушений в работе средств полнотекстового поиска. 
  
|Свойство|Описание|  
|--------------|-----------------|  
|**AccentSensitivity**|Настройка учета диакритических знаков:|
|**ImportStatus**|Выполняется ли в настоящее время импорт полнотекстового каталога.|  
|**IndexSize**|Размер полнотекстового каталога в мегабайтах (МБ).| 
|**ItemCount**|Количество полнотекстовых индексированных элементов в полнотекстовом каталоге.|  
|**MergeStatus**|Выполняется ли слияние в единый файл.| 
|**PopulateCompletionAge**|Разница в секундах между завершением последнего заполнения полнотекстового индекса и 01/01/1990 00:00:00.| 
|**PopulateStatus**|Состояние заполнения.<br /><br /> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]|  
|**UniqueKeyCount**|Количество уникальных ключей в полнотекстовом каталоге.| 

##  <a name="rebuild-a-full-text-catalog"></a><a name="rebuildone"></a> Перестроение полнотекстового каталога  

Выполните инструкцию Transact-SQL [ALTER FULLTEXT CATALOG... REBUILD](
../../t-sql/statements/alter-fulltext-catalog-transact-sql.md) или следующие действия в SQL Server Management Studio (SSMS).

1.  В обозревателе объектов SSMS последовательно разверните узел сервера, а затем **Базы данных** и базу данных, содержащую полнотекстовый каталог, который необходимо перестроить.  
  
2.  Разверните узел **Хранилище** , а затем **Полнотекстовые каталоги** .  
  
3.  Щелкните правой кнопкой мыши имя полнотекстового каталога, который необходимо перестроить, и выберите **Перестроить** .  
  
4.  На вопрос **Удалить полнотекстовый каталог и перестроить его?** нажмите кнопку **ОК** .  
  
5.  В диалоговом окне **Перестроить полнотекстовый каталог** нажмите кнопку **Закрыть** .  
   
##  <a name="rebuild-all-full-text-catalogs-for-a-database"></a><a name="rebuildall"></a> Перестроение полнотекстовых каталогов базы данных  

1.  В обозревателе объектов последовательно разверните узел сервера, а затем **Базы данных** и базу данных, содержащую полнотекстовый каталог, который необходимо перестроить.  
  
2.  Разверните узел **Хранилище** , затем щелкните правой кнопкой мыши **Полнотекстовые каталоги** .  
  
3.  Выберите **Перестроить все** .  
  
4.  В ответ на запрос **Удалить все полнотекстовые каталоги и перестроить их?** нажмите кнопку **ОК** .  
  
5.  В диалоговом окне **Перестроить все полнотекстовые каталоги** нажмите **Закрыть** .  
  
  
  
##  <a name="remove-a-full-text-catalog-from-a-database"></a><a name="removing"></a> Удаление полнотекстового каталога из базы данных  

Выполните инструкцию Transact-SQL [DROP FULLTEXT CATALOG](
../../t-sql/statements/drop-fulltext-catalog-transact-sql.md) или следующие действия в SQL Server Management Studio (SSMS).

1.  В обозревателе объектов SSMS разверните узел сервера, а затем **Базы данных** и базу данных, содержащую полнотекстовый каталог, который необходимо удалить.  
  
2.  Разверните узел **Хранилище** , а затем **Полнотекстовые каталоги** .  
  
3.  Щелкните правой кнопкой мыши полнотекстовый каталог, который необходимо удалить, и выберите в меню пункт **Удалить** .  
  
4.  В диалоговом окне **Удаление объектов** нажмите кнопку **ОК** .  

## <a name="next-step"></a>Следующий шаг
[Создание и управление полнотекстовыми индексами](../../relational-databases/search/create-and-manage-full-text-indexes.md)
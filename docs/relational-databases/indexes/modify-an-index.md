---
description: Изменение индекса
title: Изменение индекса | Документация Майкрософт
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes [SQL Server], modifying
- modifying indexes
- index changes [SQL Server]
ms.assetid: 97e3110d-fde7-4f5d-9309-dc1697960aeb
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: 0b653936dd08b5c124fd1249697dec35e9ae7ba9
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766318"
---
# <a name="modify-an-index"></a>Изменение индекса
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  В этом разделе описывается изменение индекса в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!IMPORTANT]  
>  Индексы, созданные в результате применения ограничения PRIMARY KEY или UNIQUE, изменить этим способом нельзя. Вместо этого необходимо изменить ограничение.  
  
 **В этом разделе**  
  
-   **Для изменения индекса используется:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-modify-an-index"></a>Изменение индекса  
  
1.  В обозревателе объектов подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] и разверните его.  
  
2.  Разверните **Базы данных**, затем базу данных, которой принадлежит таблица, и **Таблицы**.  
  
3.  Раскройте таблицу, которой принадлежит индекс, а затем — узел **Индексы**.  
  
4.  Щелкните правой кнопкой мыши индекс, который нужно изменить, и выберите пункт **Свойства**.  
  
5.  В диалоговом окне **Свойства индекса** внесите необходимые изменения. Например, можно добавить или удалить столбец из ключа индекса или изменить значение параметра индекса.  
  
#### <a name="to-modify-index-columns"></a>Изменение столбцов индекса  
  
1.  Чтобы добавить столбец индекса, удалить его или изменить его позицию, выберите в диалоговом окне **Свойства индекса** страницу **Общие** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-modify-an-index"></a>Изменение индекса  
  
В следующем примере удаляется и создается повторно существующий индекс для столбца `ProductID` таблицы `Production.WorkOrder` в базе данных AdventureWorks с использованием параметра `DROP_EXISTING`. Указываются также параметры `FILLFACTOR` и `PAD_INDEX`.  
  
[!code-sql[IndexDDL#CreateIndex4](../../relational-databases/indexes/codesnippet/tsql/modify-an-index_1.sql)]  
  
В следующем примере с помощью инструкции ALTER INDEX задаются несколько параметров для индекса `AK_SalesOrderHeader_SalesOrderNumber`.  
  
[!code-sql[IndexDDL#AlterIndex4](../../relational-databases/indexes/codesnippet/tsql/modify-an-index_2.sql)]  
  
#### <a name="to-modify-index-columns"></a>Изменение столбцов индекса  
  
1.  Чтобы добавить, удалить или изменить позицию столбца индекса, необходимо удалить и повторно создать индекс.  
  
## <a name="see-also"></a>См. также  
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)   
 [INDEXPROPERTY (Transact-SQL)](../../t-sql/functions/indexproperty-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [Установка параметров индекса](../../relational-databases/indexes/set-index-options.md)   
 [Переименование индексов](../../relational-databases/indexes/rename-indexes.md)  
  
  

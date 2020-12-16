---
description: Учебник по T-SQL. Удаление объектов базы данных
title: Учебник T-SQL. Удаление объектов базы данных | Документация Майкрософт
ms.custom: ''
ms.date: 07/30/2018
ms.prod: sql
ms.technology: t-sql
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- deleting database objects
ms.assetid: ecf26dd5-4535-4ed6-86fc-c73f9d9dedec
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 517042032a8b7b146f02b620ca6d47a90ae074a0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462275"
---
# <a name="lesson-3-delete-database-objects"></a>Урок 3. Удаление объектов базы данных
[!INCLUDE[tsql-appliesto-ss2008-all-md](../includes/tsql-appliesto-ss2008-all-md.md)]
На этом коротком занятии рассматривается удаление объектов, созданных на занятиях 1 и 2, а затем удаление базы данных.  
  
Перед удалением объектов необходимо убедиться, что используется нужная база данных:
  
  ```sql  
  USE TestData;  
  GO  
  ```  

## <a name="revoke-stored-procedure-permissions"></a>Отзыв разрешений на хранимые процедуры
  
С помощью инструкции `REVOKE` удаляется разрешение на выполнение, предоставленное `Mary` на хранимую процедуру:
  
  ```sql  
  REVOKE EXECUTE ON pr_Names FROM Mary;  
  GO  
  ```  
  
## <a name="drop-permissions"></a>Удаление разрешений

1. С помощью инструкции `DROP` удаляется разрешение, предоставленное `Mary` для доступа к базе данных `TestData` :
  
   ```sql  
   DROP USER Mary;  
   GO  
   ```  


2. С помощью инструкции `DROP` удаляется разрешение, предоставленное `Mary` для доступа к экземпляру [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]:
  
   ```sql  
   DROP LOGIN [<computer_name>\Mary];  
   GO   
   ```  
  
3. С помощью инструкции `DROP` удаляется хранимая процедура `pr_Names`:  
  
   ```sql  
   DROP PROC pr_Names;  
   GO   
   ```  
  
4. С помощью инструкции `DROP` удаляется представление `vw_Names`:  
  
   ```sql  
   DROP VIEW vw_Names;  
   GO  
   ```  

## <a name="delete-table"></a>Удалить таблицу
  
1. С помощью инструкции `DELETE` удаляются все строки таблицы `Products` :  
  
    ```sql  
    DELETE FROM Products;  
    GO  
    ```  
  
2.  С помощью инструкции `DROP` удаляется таблица `Products` :  
  
    ```sql  
    DROP TABLE Products;  
    GO    
    ```  

## <a name="remove-database"></a>Удаление базы данных
  
Базу данных `TestData` невозможно удалить во время нахождения в ней; поэтому сначала требуется переключить контекст на другую базу данных и только после этого с помощью инструкции `DROP` удалить базу данных `TestData` :  
  
  ```sql  
  USE MASTER;  
  GO  
  DROP DATABASE TestData;  
  GO   
  ```  
  
Это заключительный шаг учебника «Составление инструкций [!INCLUDE[tsql](../includes/tsql-md.md)] ». Помните, что этот учебник содержит только краткий обзор и не включает описания всех параметров используемых инструкций. Для проектирования и создания эффективной структуры базы данных и настройки безопасного доступа к данным требуется более сложная база данных, чем показанная в примерах данного учебника.  

  
  

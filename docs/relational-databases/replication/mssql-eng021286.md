---
description: MSSQL_ENG021286
title: MSSQL_ENG021286 | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG021286 error
ms.assetid: b63620b7-1c6d-46f7-90ea-3a8e99af8de4
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 59066e248bd059376ec26e32d8541a3427366562
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460217"
---
# <a name="mssql_eng021286"></a>MSSQL_ENG021286
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Сведения о сообщении  
  
|attribute|Значение|  
|-|-|  
|Название продукта|SQL Server|  
|Идентификатор события|21286|  
|Источник события|MSSQLSERVER|  
|Компонент|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Символическое имя||  
|Текст сообщения|Таблица конфликтов "%s" не существует.|  
  
## <a name="explanation"></a>Объяснение  
 Эта ошибка возникает, если для статьи, указанной в [sysmergearticles (Transact-SQL)](../../relational-databases/system-tables/sysmergearticles-transact-sql.md), не существует таблица конфликтов. Эта ошибка может возникнуть при попытке добавить или удалить столбец в таблице, опубликованной для репликации слиянием.  
  
## <a name="user-action"></a>Действие пользователя  
 Выполните инструкцию [DBCC CHECKDB (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) в базе данных с отсутствующей таблицей конфликтов, чтобы убедиться в отсутствии проблем согласованности данных.  
  
 Если таблица конфликтов отсутствует на подписчике, удалите подписку и создайте ее заново. Если таблица конфликтов отсутствует на издателе, удалите все подписки, удалите публикацию, а затем заново создайте публикацию и все подписки. Дополнительные сведения см. в статьях [Публикация данных и объектов базы данных](../../relational-databases/replication/publish/publish-data-and-database-objects.md) и [Подписка на публикации](../../relational-databases/replication/subscribe-to-publications.md).  
  
## <a name="see-also"></a>См. также:  
 [Справочник по ошибкам и событиям (репликация)](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  

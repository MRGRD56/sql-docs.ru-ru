---
description: MSSQLSERVER_2527
title: MSSQLSERVER_2527 | | Документация Майкрософт
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 2527 (Database Engine error)
ms.assetid: 1cef90ef-9c39-44e6-bc7f-316c8f53c10c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 1727d825bd3dac9df07824e5e26f58d6cc4ec625
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188939"
---
# <a name="mssqlserver_2527"></a>MSSQLSERVER_2527
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Сведения  
  
| attribute | Значение |  
| :-------- | :---- |  
|Название продукта|SQL Server|  
|Идентификатор события|2527|  
|Источник события|MSSQLSERVER|  
|Компонент|SQLEngine|  
|Символическое имя|DBCC_INDEX_FILEGROUP_IS_OFFLINE|  
|Текст сообщения|Невозможно обработать индекс I_NAME таблицы O_NAME, так как файловая группа F_NAME находится в режиме «вне сети».|  
  
## <a name="explanation"></a>Объяснение  
Это информационное сообщение указывает, что индекс невозможно проверить, поскольку одна из файловых групп, в которой хранятся данные индекса, находится в режиме «вне сети». Состояние файлов в файловой группе определяет доступность всей файловой группы. Чтобы файловая группа была доступна, необходимо, чтобы все файлы в файловой группе находились в режиме в сети. Если других проблем нет, будут проверяться все остальные индексы того же объекта.  
  
## <a name="user-action"></a>Действие пользователя  
Чтобы просмотреть текущее состояние файлов указанной файловой группы, выполните запрос к представлению каталога **sys.database_files** или **sys.master_files**.  
  
Восстановите автономный файл из резервной копии.  
  
## <a name="see-also"></a>См. также:  
[sys.database_files (Transact-SQL)](~/relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[sys.master_files (Transact-SQL)](~/relational-databases/system-catalog-views/sys-master-files-transact-sql.md)  
[RESTORE (Transact-SQL)](~/t-sql/statements/restore-statements-transact-sql.md)  
  

---
description: MSSQLSERVER_41301
title: MSSQLSERVER_41301 | Документация Майкрософт
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 41301 (Database Engine error)
ms.assetid: c6127e1e-2846-4ee9-bc42-2d896ea9730e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 5d7fef050bc8287fb2665343e0fda148d38bf523
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185122"
---
# <a name="mssqlserver_41301"></a>MSSQLSERVER_41301
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Сведения  
  
| attribute | Значение |  
| :-------- | :---- |  
|Название продукта|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Идентификатор события|41301|  
|Источник события|MSSQLSERVER|  
|Компонент|SQLEngine|  
|Символическое имя|COMMIT_DEPENDENCY_FAILURE|  
|Текст сообщения|Предыдущая транзакция, от которой зависит текущая транзакция, прервана, текущая транзакция не может быть зафиксирована.|  
  
## <a name="explanation"></a>Объяснение  
В ходе транзакции произошла ошибка зависимости, поэтому транзакция завершается.  
  
Эта ошибка также может быть вызвана слишком большим числом зависимых транзакций. Любые транзакции записи могут иметь только ограниченное число зависимых транзакций. Например, эта ошибка может возникнуть в случае, если слишком большое число транзакций чтения пытаются установить зависимость от транзакции обновления.  
  
## <a name="user-action"></a>Действие пользователя  
Не выполняйте никаких действий в ходе транзакции. Вызовите ROLLBACK TRAN для отката транзакции. Дополнительные сведения см. в разделе [In-Memory OLTP (оптимизация в памяти)](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
## <a name="see-also"></a>См. также:  
[Включение и отключение групп доступности AlwaysOn (SQL Server)](~/database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  

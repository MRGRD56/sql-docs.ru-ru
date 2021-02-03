---
description: MSSQLSERVER_1204
title: MSSQLSERVER_1204 | Документация Майкрософт
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1204 (Database Engine error)
ms.assetid: de6ece78-79de-484d-9224-ca0f7645815f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0b909cfcf53c2e805a334f52b501c28673f6774d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197176"
---
# <a name="mssqlserver_1204"></a>MSSQLSERVER_1204
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Сведения  
  
| attribute | Значение |  
| :-------- | :---- |  
|Название продукта|SQL Server|  
|Идентификатор события|1204|  
|Источник события|MSSQLSERVER|  
|Компонент|SQLEngine|  
|Символическое имя|LK_OUTOF|  
|Текст сообщения|Экземпляру компонента SQL Server Database Engine не удается получить ресурс LOCK в данный момент времени. Запустите инструкцию повторно, когда число активных пользователей уменьшится. Попросите администратора баз данных проверить конфигурацию блокировки и памяти для данного экземпляра либо выполнить проверку давно выполняющихся транзакций.|  
  
## <a name="explanation"></a>Объяснение  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не удается получить ресурс блокировки. Возможны следующие причины этой ошибки.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не удается получить дополнительную память операционной системы из-за того, что другие процессы используют ее, либо из-за того, что для сервера установлено значение параметра **max server memory**.  
  
-   Диспетчер блокировок не может использовать более 60 процентов доступной памяти для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="user-action"></a>Действие пользователя  
Если возникли подозрения, что SQL Server не может выделить необходимое количество памяти, попробуйте следующее.  
  
-   Если другие приложения, кроме SQL Server, используют ресурсы, попытайтесь закрыть эти приложения или запустите их на отдельном сервере. В результате память от других процессов освободится для SQL Server.  
  
-   Если задан параметр max server memory, увеличьте его значение.  
  
Если есть подозрения, что диспетчер блокировок использовал максимальное количество свободной памяти, определите, какая транзакция удерживает больше всего блокировок, и завершите ее работу. С помощью следующего скрипта можно определить, какая из транзакций удерживает больше всего блокировок:  
  
```  
SELECT request_session_id, COUNT (*) num_locks  
FROM sys.dm_tran_locks  
GROUP BY request_session_id   
ORDER BY count (*) DESC  
```  
  
Завершите работу сеанса с первым идентификатором с помощью команды KILL.  
  

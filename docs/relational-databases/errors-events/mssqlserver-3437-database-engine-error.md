---
description: MSSQLSERVER_3437
title: MSSQLSERVER_3437 | Документация Майкрософт
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3437 (Database Engine error)
ms.assetid: b38216e2-b650-43bd-97af-061d54f60031
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a61f6b419a04df737ea30e3c6a68a491b9368f2e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201602"
---
# <a name="mssqlserver_3437"></a>MSSQLSERVER_3437
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Сведения  
  
| attribute | Значение |  
| :-------- | :---- |  
|Название продукта|SQL Server|  
|Идентификатор события|3437|  
|Источник события|MSSQLSERVER|  
|Компонент|SQLEngine|  
|Символическое имя|NODTC|  
|Текст сообщения|При восстановлении базы данных «%.*ls» произошла ошибка. Невозможно подключиться к координатору распределенных транзакций Майкрософт (MS DTC) для проверки состояния завершения транзакции %S_XID. Исправьте MS DTC и запустите восстановление еще раз.|  
  
## <a name="explanation"></a>Объяснение  
На момент закрытия базы данных одна или несколько распределенных транзакций, использующих координатор распределенных транзакций [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC), не были завершены. Восстановление этой базы данных завершилось неуспешно, так как [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не удалось подключиться к MS DTC для завершения или отката транзакций.  
  
## <a name="user-action"></a>Действие пользователя  
Для восстановления этой базы данных сначала необходимо устранить неполадку с MS DTC. Для поиска проблемы, связанной с MS DTC, просмотрите журналы событий Windows. Если решить проблему с MS DTC и исправить базу данных невозможно, восстановите базу данных из резервной копии.  
  

---
description: MSSQLSERVER_3937
title: MSSQLSERVER_3937 | Документация Майкрософт
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3937 (Database Engine error)
ms.assetid: 312d5bbe-c8de-42db-af4b-4ccb448ce6ef
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d9c592a077e38da67073691e6d8eabb916fe791b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185192"
---
# <a name="mssqlserver_3937"></a>MSSQLSERVER_3937
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Сведения  
  
| attribute | Значение |  
| :-------- | :---- |  
|Название продукта|MSSQLSERVER|  
|Идентификатор события|3937|  
|Источник события|MSSQLSERVER|  
|Компонент|SQLEngine|  
|Символическое имя|XACT_FILESTREAM_ROLLBACK_ERROR|  
|Текст сообщения|При уведомлении драйвера фильтра FILESTREAM об откате транзакции произошла ошибка. Код ошибки: 0x%0x.|  
  
## <a name="explanation"></a>Объяснение  
Драйвер RsFx возвратил ошибку при выдаче уведомления об откате транзакции. Возможно, это вызвано недостатком ресурсов. Это может привести к небольшой утечке памяти в драйвере фильтра RsFx, но память будет освобождена, когда завершится процесс sqlservr.exe, создавший транзакцию.  
  
## <a name="user-action"></a>Действие пользователя  

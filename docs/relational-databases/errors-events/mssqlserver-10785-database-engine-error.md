---
description: MSSQLSERVER_10785
title: MSSQLSERVER_ 10785 | Документация Майкрософт
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10785 (Database Engine error)
ms.assetid: 32f96c1e-9e94-4603-9bcd-b0c2e4af9fda
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0c5d6e842cb6fd8c8d761855559e07d0edb61433
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197261"
---
# <a name="mssqlserver_10785"></a>MSSQLSERVER_10785
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Сведения  
  
| attribute | Значение |  
| :-------- | :---- |  
|Название продукта|MSSQLSERVER|  
|Идентификатор события|10785|  
|Источник события|MSSQLSERVER|  
|Компонент|SQLEngine|  
|Символическое имя|P3_HEKATON_XACT|  
|Текст сообщения|*Построение* "*функция*" не поддерживается для активных транзакций, которые обращаются к оптимизированным для памяти таблицам или скомпилированным в собственном коде хранимым процедурам.|  
  
## <a name="user-action"></a>Действие пользователя  
Список неподдерживаемых компонентов и рекомендации по альтернативным решениям см. в статье [Конструкции языка Transact-SQL, не поддерживаемые в In-Memory OLTP](~/relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md).  
  
Дополнительные сведения см. в разделе [In-Memory OLTP (оптимизация в памяти)](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  

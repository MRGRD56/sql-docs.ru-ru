---
description: MSSQLSERVER_9955
title: MSSQLSERVER_9955 | Документация Майкрософт
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9955 (Database Engine error)
ms.assetid: 77f30570-7790-4747-b372-eac71c036e19
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: cad93e9358d7bbe315dd83b52229b9b5a6d92c6a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187545"
---
# <a name="mssqlserver_9955"></a>MSSQLSERVER_9955
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Сведения  
  
| attribute | Значение |  
| :-------- | :---- |  
|Название продукта|SQL Server|  
|Идентификатор события|9955|  
|Источник события|MSSQLSERVER|  
|Компонент|SQLEngine|  
|Символическое имя|FTXT2_MSSEARCHACCESSDENY|  
|Текст сообщения|SQL Server не удалось создать именованный канал "%ls" для обмена данными с управляющей программой полнотекстовой фильтрации (ошибка Windows: %d). Именованный канал для процесса узла управляющей программы фильтрации уже существует, системе не хватает ресурсов или не удалось найти идентификатор безопасности для ее группы учетной записи. чтобы решить эту проблему, завершите все запущенные процессы управляющей программы полнотекстовой фильтрации и при необходимости измените конфигурацию учетной записи службы запуска этой программы.|  
  
## <a name="explanation"></a>Объяснение  
Это сообщение возникло потому, что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не удалось создать именованный канал для обмена данными с управляющей программой полнотекстовой фильтрации. Именованный канал для процесса узла управляющей программы фильтрации уже существует, системе не хватает ресурсов или не удалось найти идентификатор безопасности для ее группы учетной записи.  
  
## <a name="user-action"></a>Действие пользователя  
Чтобы устранить эту ошибку, завершите все запущенные процессы управляющей программы полнотекстовой фильтрации и при необходимости измените конфигурацию учетной записи этой программы с помощью диспетчера конфигурации SQL Server.  
  
## <a name="see-also"></a>См. также:  
[Диспетчер конфигурации SQL Server](~/relational-databases/sql-server-configuration-manager.md)  
[Настройка учетной записи службы средства запуска управляющей программы полнотекстовой фильтрации](~/relational-databases/search/set-the-service-account-for-the-full-text-filter-daemon-launcher.md)  
[Полнотекстовый поиск](~/relational-databases/search/full-text-search.md)  
  

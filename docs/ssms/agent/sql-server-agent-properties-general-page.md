---
description: Свойства агента SQL Server (страница «Общие»)
title: Свойства агента SQL Server (страница «Общие»)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.agent.general.f1
ms.assetid: b51601e9-5454-43c6-bb5e-24eb2ff043c8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f08f8fe21876f5a04e817c367ece49d44eef93c5
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88370920"
---
# <a name="sql-server-agent-properties-general-page"></a>Свойства агента SQL Server (страница «Общие»)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> В [Управляемом экземпляре Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) в настоящее время поддерживается большинство функций агента SQL Server (но не все). Подробные сведения см. в статье [Различия в T-SQL между Управляемым экземпляром SQL Azure и SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Эта страница используется для просмотра и изменения общих свойств службы агента [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="options"></a>Параметры  
**Состояние службы**  
Отображается текущее состояние службы агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**Автоматический перезапуск SQL Server в случае его неожиданной остановки**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Агент перезапускает [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при непредвиденной остановке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**Автоматический перезапуск агента SQL Server в случае его неожиданной остановки**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] перезапускает агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при непредвиденной остановке агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**Имя файла**  
Укажите имя файла для журнала ошибок.  
  
**...**  
Перейдите к файлу журнала ошибок.  
  
**Включая трассировочные сообщения**  
Сообщения трассировки выполнения включаются в журнал ошибок. Сообщения трассировки предоставляют подробные сведения о работе агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Поэтому при выборе этого параметра для файла журнала необходимо дополнительное место на диске. Этот параметр нужно выбирать только для устранения неполадок, связанных с агентом [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
**Записать файл OEM**  
Запись файла журнала ошибок производится не в Юникоде. Это позволяет уменьшить место на диске, необходимое для файла журнала. Однако при включении этого параметра могут возникнуть трудности с чтением сообщений, содержащих данные в Юникоде.  
  
**Получатель для команды net send**  
Введите имя оператора, получающего уведомления, отправленные командой net send, с сообщениями, которые агент [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] записывает в файл журнала.  
  
## <a name="see-also"></a>См. также:  
[Операторы](../../ssms/agent/operators.md)  
[Журнал ошибок агента SQL Server](../../ssms/agent/sql-server-agent-error-log.md)  
  

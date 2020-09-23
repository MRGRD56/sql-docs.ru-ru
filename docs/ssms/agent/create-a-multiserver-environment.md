---
description: Создание многосерверной среды
title: Создание многосерверной среды
ms.custom: seo-lt-2019
ms.date: 01/30/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, multiserver environments
- master servers [SQL Server], about master servers
- target servers [SQL Server], about target servers
- multiserver environments [SQL Server]
ms.assetid: edc2b60d-15da-40a1-8ba3-f1d473366ee6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 0ea258ee3fc53012286f171e78379f9459343c0e
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/17/2020
ms.locfileid: "88418220"
---
# <a name="create-a-multiserver-environment"></a>Создание многосерверной среды
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> В [Управляемом экземпляре Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) в настоящее время поддерживается большинство функций агента SQL Server (но не все). Подробные сведения см. в статье [Различия в T-SQL между Управляемым экземпляром SQL Azure и SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Администрирование нескольких серверов требует настройки главного сервера (MSX) и одного или более целевых серверов (TSX). Задания, которые будут обрабатываться на всех целевых серверах, сначала определяются на главном сервере, а затем загружаются на целевые.  
  
Для соединений между главными и целевыми серверами по умолчанию включено полное шифрование протокола защиты транспортного уровня (TLS), заменившего SSL, и проверка сертификата. Дополнительные сведения см. в статье [Установка параметров шифрования на целевых серверах](../../ssms/agent/set-encryption-options-on-target-servers.md).  
  
Если используется большое количество целевых серверов, по возможности не размещайте главный сервер на рабочем сервере, на котором [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] значительно загружен другими задачами, так как трафик от целевых серверов может снизить производительность рабочего сервера. При переадресации событий на выделенный главный сервер можно централизовать администрирование на одном сервере. Дополнительные сведения см. в статье [Управление событиями](../../ssms/agent/manage-events.md).  
  
> [!NOTE]  
> Чтобы использовать обработку заданий в многосерверном окружении, учетная запись агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] должна быть членом роли базы данных **msdb** **TargetServersRole** на главном сервере. Мастер настройки главного сервера автоматически добавляет к этой роли учетную запись службы в ходе процесса прикрепления  
  
## <a name="considerations-for-multiserver-environments"></a>Замечания по многосерверным средам  
  
При создании многосерверной среды возможны следующие проблемы:  
  
-   Используйте последнюю версию в качестве главного сервера. Поддерживается текущая и две предыдущие версии.

-   Каждый целевой сервер отправляет отчеты только одному главному серверу. Нужно открепить целевой сервер от одного главного сервера перед прикреплением его к другому.  
  
-   Если меняется имя целевого сервера, нужно сначала открепить целевой сервер от главного, а после изменения имени прикрепить снова.  
  
-   Если нужно удалить многосерверную конфигурацию, необходимо отключить все целевые серверы от главного сервера.  
  
-   Службы SQL Server Integration Services поддерживают только серверы, которые имеют такую же или более высокую версию, что и версия главного сервера.  
  
## <a name="related-tasks"></a>Связанные задачи  
В следующих разделах описываются типовые задачи при создании многосерверной среды.  
  
|Описание|Раздел|  
|---------------|---------|  
|Описывает, как создать главный сервер.|[Создание главного сервера](../../ssms/agent/make-a-master-server.md)|  
|Описывает, как создать целевой сервер.|[Создание целевого сервера](../../ssms/agent/make-a-target-server.md)|  
|Описывает, как прикрепить целевой сервер к главному.|[Прикрепление целевого сервера к главному](../../ssms/agent/enlist-a-target-server-to-a-master-server.md)|  
|Описывает, как исключить целевой сервер из главного.|[Отключение целевого сервера от главного сервера](../../ssms/agent/defect-a-target-server-from-a-master-server.md)|  
|Описывает, как исключить несколько целевых серверов из главного.|[Отключение нескольких целевых серверов от главного](../../ssms/agent/defect-multiple-target-servers-from-a-master-server.md)|  
|Описывает, как проверить состояние целевого сервера.|[sp_help_targetserver (Transact-SQL)](https://msdn.microsoft.com/f841d3bd-901a-4980-ad0b-1c6eeba3f717)<br /><br />[sp_help_targetservergroup (Transact-SQL)](https://msdn.microsoft.com/ec3a4a68-b591-431c-9518-053ede522d0c)|  
  
## <a name="see-also"></a>См. также:  
[Устранение неполадок, связанных с многосерверными заданиями, использующими учетные записи-посредники](../../ssms/agent/troubleshoot-multiserver-jobs-that-use-proxies.md)  
  

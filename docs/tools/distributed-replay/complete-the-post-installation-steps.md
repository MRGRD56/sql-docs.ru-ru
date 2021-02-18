---
title: Выполнение действий после установки
titleSuffix: SQL Server Distributed Replay
description: После установки компонента распределенного воспроизведения следует изменить учетные записи служб клиента и контроллера распределенного воспроизведения.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 0a788a2a-9b4f-4bfc-b1b5-83eeb1ea9ab2
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 94cf69eb0a4159c4bfda5a0ef0b79a2b35bbfcb7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349547"
---
# <a name="complete-the-post-installation-steps"></a>Выполнение действий после установки

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

После установки компонента распределенного воспроизведения следует изменить учетные записи служб клиента и контроллера распределенного воспроизведения.  
  
## <a name="to-complete-the-post-installation-steps"></a>Выполнение действий после установки  
  
1. **Создание правил брандмауэра**. На компьютерах контроллера и клиента в брандмауэре необходимо разрешить входящий трафик для соответствующей службы. Укажите правила брандмауэра для исполняемых файлов службы, расположенных в установочных папках.  
  
    1. Для службы контроллера создайте правило для файла **DReplayController.exe**, расположенного в папке установки. Например, следующая команда включает это правило, где `%InstallPath%` — папка установки службы:  
  
         `netsh advfirewall firewall add rule name="allow dreplay controller" dir=in program="%InstallPath%\DReplayController\DReplayController.exe" action=allow`  
  
    2. Для службы клиента на каждом клиентском компьютере создайте правило для файла **DReplayClient.exe**, расположенного в папке установки. Например, следующая команда включает это правило, где `%InstallPath%` — папка установки службы:  
  
         `netsh advfirewall firewall add rule name="allow dreplay client" dir=in program="%InstallPath%\DReplayClient\DReplayClient.exe" action=allow`  
  
2. **Предоставьте каждому клиенту разрешения на целевом сервере**. После завершения установки службы клиента на клиентских компьютерах следует вручную добавить учетные записи службы клиента в роль системного администратора на целевом экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="net-framework-security"></a>Безопасность .NET Framework

Для установки компонентов распределенного воспроизведения необходимо обладать разрешениями администратора. Только имя входа [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с разрешениями sysadmin может добавлять учетные записи службы клиента в роль sysadmin тестового сервера. Дополнительные сведения о вопросах безопасности распределенного воспроизведения см. в разделе [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).
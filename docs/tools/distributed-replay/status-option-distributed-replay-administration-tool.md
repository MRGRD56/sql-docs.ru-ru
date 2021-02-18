---
title: Параметр состояния в средстве администрирования
titleSuffix: SQL Server Distributed Replay
description: В этой статье описывается параметр status командной строки и синтаксис средства администрирования распределенного воспроизведения SQL Server для отображения текущего состояния.
ms.prod: sql
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: ea89386e-1598-4412-8b37-680d14b2a5b6
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: a8b9ddc9b67fc1838aa1de7cc2aa523a99ecea25
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338750"
---
# <a name="status-option-distributed-replay-administration-tool"></a>Параметр состояния (средство администрирования распределенного воспроизведения)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Средство администрирования распределенного воспроизведения [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (**DReplay.exe**) представляет собой программу командной строки, которая служит для взаимодействия с контроллером распределенного воспроизведения. В этой статье описан параметр командной строки **status** и соответствующий синтаксис.  
  
 Параметр **status** опрашивает контроллер и отображает его текущее состояние.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") Дополнительные сведения о синтаксических обозначениях, используемых в синтаксисе средства администрирования, см. в разделе [Синтаксические обозначения в Transact-SQL (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
dreplay status [-m controller] [-f status_interval]  
```  
  
#### <a name="parameters"></a>Параметры  
 **-m** _контроллер_  
 Задает имя компьютера для контроллера. Локальный компьютер можно указать как «`localhost`» или «`.`».  
  
 Если параметр **-m** не задан, то используется локальный компьютер.  
  
 **-f** _status_interval_  
 Указывает частоту (в секундах) отображения состояния.  
  
 Если параметр **-f** не задан, то интервал по умолчанию составляет 30 секунд.  
  
## <a name="examples"></a>Примеры  
 В следующем примере текущий статус отображается каждые 60 секунд. Значение `localhost` указывает, что служба контроллера запущена на том же компьютере, что и средство администрирования.  
  
```  
dreplay status -m localhost -f 60  
```  
  
## <a name="permissions"></a>Разрешения  
 Средство администрирования должно запускаться как интерактивный пользователь, с учетной записью локального пользователя или пользователя домена. Для использования учетной записи локального пользователя средство администрирования и контроллер должны быть запущены на одном компьютере.  
  
 Дополнительные сведения см. в статье [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
## <a name="see-also"></a>См. также:  
 [Распределенное воспроизведение SQL Server](../../tools/distributed-replay/sql-server-distributed-replay.md)   
 [Отладчик Transact-SQL](../../ssms/scripting/transact-sql-debugger.md)  
  

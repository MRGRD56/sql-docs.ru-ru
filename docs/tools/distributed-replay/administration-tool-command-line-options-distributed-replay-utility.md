---
title: Параметры командной строки средства администрирования
description: Средство администрирования распределенного воспроизведения Microsoft SQL Server (DReplay.exe) представляет собой программу командной строки, которая служит для взаимодействия с контроллером распределенного воспроизведения.
titleSuffix: SQL Server Distributed Replay
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 08/12/2016
ms.openlocfilehash: 80cb7abb5f1bf3511b30719563908f0b59cecc93
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836951"
---
# <a name="administration-tool-command-line-options-distributed-replay-utility"></a>Параметры командной строки средства администрирования (программа распределенного воспроизведения)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Средство администрирования программы распределенного воспроизведения [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (**DReplay.exe**) представляет собой программу командной строки, которая служит для взаимодействия с контроллером распределенного воспроизведения. При помощи средства администрирования можно инициировать операции на контроллере, наблюдать за ними и отменять.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") Дополнительные сведения о синтаксических обозначениях, используемых в синтаксисе средства администрирования, см. в разделе [Синтаксические обозначения в Transact-SQL (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
dreplay {preprocess|replay|status|cancel} [options] [-?]}  
  
Usage:  
  
  dreplay preprocess [-m controller] -i input_trace_file  
    -d controller_working_dir [-c config_file] [-f status_interval]  
  
  dreplay replay [-m controller] -d controller_working_dir [-o]  
    [-s target_server] -w clients [-c config_file]  
    [-f status_interval]  
  
  dreplay status [-m controller] [-f status_interval]  
  
  dreplay cancel [-m controller] [-q]   
```  
  
## <a name="remarks"></a>Remarks  
 В программе **DReplay.exe** можно применять следующие параметры командной строки:  
  
 **preprocess**  
 Инициирует стадию предварительной обработки. Подготавливаются для воспроизведения на целевом сервере входные данные трассировки, отслеженные в рабочей среде.  
  
 **replay**  
 Инициирует стадию воспроизведения события. Данные воспроизведения отправляются указанным клиентам, запускается распределенное воспроизведение, синхронизируются клиенты. При необходимости каждый выбранный клиент может записывать последовательность воспроизведения и сохранять получившиеся файлы трассировки в локальном кэше.  
  
 **status**  
 Опрашивает контроллер и отображает его текущее состояние.  
  
 **cancel**  
 Отменяет текущую операцию, выполняемую на контроллере.  
  
 Подробные сведения о синтаксисе, включая командные аргументы и примеры, см. в следующих разделах:  
  
-   [Параметр предварительной обработки (средство администрирования распределенного воспроизведения)](../../tools/distributed-replay/preprocess-option-distributed-replay-administration-tool.md)  
  
-   [Параметр воспроизведения (средство администрирования распределенного воспроизведения)](../../tools/distributed-replay/replay-option-distributed-replay-administration-tool.md)  
  
-   [Параметр состояния (средство администрирования распределенного воспроизведения)](../../tools/distributed-replay/status-option-distributed-replay-administration-tool.md)  
  
-   [Параметр отмены (средство администрирования распределенного воспроизведения)](../../tools/distributed-replay/cancel-option-distributed-replay-administration-tool.md)  
  
 Удаленные вызовы процедур (RPC) воспроизводятся как RPC, а не события языка.  
  
## <a name="permissions"></a>Разрешения  
 Средство администрирования должно запускаться как интерактивный пользователь, с учетной записью локального пользователя или пользователя домена. Для использования учетной записи локального пользователя средство администрирования и контроллер должны быть запущены на одном компьютере.  
  
 Дополнительные сведения см. в статье [Distributed Replay Security](../../tools/distributed-replay/distributed-replay-security.md).  
  
## <a name="see-also"></a>См. также раздел  
 [Распределенное воспроизведение SQL Server](../../tools/distributed-replay/sql-server-distributed-replay.md)  
  
  

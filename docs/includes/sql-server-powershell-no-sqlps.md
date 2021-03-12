---
title: Использование NOSQLPS для агента SQL Server с помощью PowerShell
description: Сообщение, которое необходимо объяснить, прежде чем использовать командлет sqlserver PowerShell вместо командлета sqlps с агентом SQL Server
ms.topic: include
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier
ms.openlocfilehash: bf161bd583f3d48dc389cea6b69f55c32e2cce59
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839412"
---
Начиная с версии SQL Server 2019, вы можете отключить SQLPS. Для этого в первой строке шага задания типа PowerShell можно добавить `#NOSQLPS`, чтобы Агент SQL не запускал автоматическую загрузку модуля SQLPS. После этого задание Агента SQL запустит установленную на компьютере версию PowerShell, и вы можете использовать любой другой модуль PowerShell.

Если вы хотите использовать модуль [**SqlServer**](https://www.powershellgallery.com/packages/Sqlserver/21.1.18235) в шаге задания Агента SQL, можно поместить этот код в первые две строки скрипта.

```powershell
#NOSQLPS
Import-Module -Name SqlServer
```
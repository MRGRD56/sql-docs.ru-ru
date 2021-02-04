---
description: Изменение имени зарегистрированного сервера или зарегистрированной группы серверов
title: Изменение имени зарегистрированного сервера или группы серверов
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 10e1546b-9edb-400c-8676-2ea1192d6134
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 08/02/2016
ms.openlocfilehash: c5c8566a40d3993c6e273de3e3b6137d35e969c1
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250993"
---
# <a name="change-the-name-of-registered-server-or-registered-server-group"></a>Изменение имени зарегистрированного сервера или зарегистрированной группы серверов

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

В этом разделе описывается изменение имени зарегистрированного сервера или группы серверов в [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Это имя может быть изменено в любое время. Изменение имени сервера для зарегистрированных серверов изменяет только отображение имени. Чтобы подключиться к другому серверу, необходимо изменить свойства соединения зарегистрированного сервера.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio

В меню перейдите к **Вид\\Зарегистрированные серверы** открыть панель **Зарегистрированные серверы**.

### <a name="to-change-the-name-of-a-server"></a>Изменение имени сервера

1. В списке **Зарегистрированные серверы** разверните узел **Ядро СУБД** и затем узел **Группы локальных серверов**.  

2. Щелкните правой кнопкой мыши сервер и выберите **Свойства** , чтобы открыть диалоговое окно **Изменение данных регистрации серверов** .

3. В поле **Имя зарегистрированного сервера** введите новое имя для регистрации сервера и нажмите кнопку **Сохранить**.  

### <a name="to-change-the-name-of-a-server-group"></a>Изменение имени группы серверов  

1. В списке **Зарегистрированные серверы** разверните узел **Ядро СУБД** и затем узел **Группы локальных серверов**.  

2. Щелкните правой кнопкой мыши группу серверов и выберите **Свойства** , чтобы открыть диалоговое окно **Изменение свойств группы серверов** . 

3. В поле **Имя группы** введите новое имя для группы серверов и нажмите кнопку **Сохранить**.  

## <a name="see-also"></a>См. также:

[Изменение регистрационных данных сервера (среда SQL Server Management Studio)](./change-a-server-s-registration-sql-server-management-studio.md)
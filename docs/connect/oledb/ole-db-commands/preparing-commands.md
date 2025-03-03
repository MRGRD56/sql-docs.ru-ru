---
title: Подготовка команд (драйвер OLE DB)
description: Для одной команды, выполняемой несколько раз, OLE DB Driver for SQL Server поддерживает подготовку команд для повышения производительности.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, commands
- prepared statements [OLE DB Driver for SQL Server]
- commands [OLE DB]
- command preparation [OLE DB Driver for SQL Server]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6b682c54374bed8645ff45f7f4fadc59b9d0f828
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735514"
---
# <a name="preparing-commands"></a>Подготовка команд
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Драйвер OLE DB для SQL Server поддерживает подготовку команды для оптимизированного многократного выполнения, однако подготовка команды создает дополнительную нагрузку, поэтому потребителю нет необходимости подготавливать команду для ее выполнения более одного раза. Как правило, подготовку команды следует выполнять в том случае, если планируется ее не менее чем четырехкратное выполнение.  
  
 По соображениям производительности подготовка команды откладывается до ее выполнения. Это поведение по умолчанию. Об ошибках, содержащихся в подготавливаемой команде, не будет ничего известно до ее выполнения или до выполнения операции с метасвойством. При установке свойства [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] SSPROP_DEFERPREPARE в значение FALSE это поведение по умолчанию может быть отключено.  
  
 Если команда выполняется напрямую (без предварительной подготовки), то [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] создает и кэширует план выполнения. При повторном выполнении инструкции SQL [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] применяет эффективный алгоритм сопоставления новой инструкции с существующим планом выполнения и повторно использует для этой инструкции тот же план выполнения.  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] предусматривает встроенную поддержку подготавливаемых и выполняемых инструкций. При подготовке инструкции [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] создает план выполнения, кэширует его и возвращает поставщику дескриптор этого плана. Поставщик пользуется этим дескриптором для повторного выполнения инструкции. Хранимые процедуры не создаются. Поскольку дескриптор прямо указывает на план выполнения инструкции SQL, а не сопоставляет инструкцию с планом выполнения, содержащимся в кэше (как при прямом выполнении), значительно эффективнее выполнить подготовку инструкции, а не выполнять ее напрямую, если известно, что инструкция будет использоваться многократно.  
  
 В [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] подготовленные инструкции не могут применяться для создания временных объектов (например, временных таблиц) и не могут ссылаться на создающие их системные хранимые процедуры. Эти процедуры следует выполнять напрямую.  
  
 Некоторые команды не могут быть подготовлены. Например, команды, указывающие выполнение хранимой процедуры или содержащие недопустимый текст для создания хранимой процедуры [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], подготавливаться не должны.  
  
 При создании временной хранимой процедуры драйвер OLE DB для SQL Server выполняет временную хранимую процедуру и возвращает результаты, как если бы выполнялась сама инструкция.  
  
 Создание временных хранимых процедур управляется свойством инициализации SSPROP_INIT_USEPROCFORPREP, определяемым драйвером OLE DB для SQL Server. Если свойство имеет значение SSPROPVAL_USEPROCFORPREP_ON или SSPROPVAL_USEPROCFORPREP_ON_DROP, то драйвер OLE DB для SQL Server при подготовке команды выполняет попытку создать хранимую процедуру. Хранимая процедура будет создана успешно, если пользователь приложения имеет все необходимые разрешения [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Для потребителей, изредка теряющих соединение, создание временных хранимых процедур может потребовать значительных ресурсов базы **tempdb**, системной базы данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], в которой создаются временные объекты. Если свойство SSPROP_INIT_USEPROCFORPREP имеет значение SSPROPVAL_USEPROCFORPREP_ON, то временные хранимые процедуры, созданные драйвером OLE DB для SQL Server, удаляются только после потери соединения сеанса, создавшего команду, с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Если это соединение по умолчанию, созданное при инициализации источника данных, то временная хранимая процедура удаляется только тогда, когда источник данных становится неинициализированным.  
  
 Если свойство SSPROP_INIT_USEPROCFORPREP имеет значение SSPROPVAL_USEPROCFORPREP_ON_DROP, то временные хранимые процедуры драйвера OLE DB для SQL Server будут удаляться только в указанных ниже случаях.  
  
-   Потребитель вызывает метод **ICommandText::SetCommandText** для задания новой команды.  
  
-   Потребитель вызывает метод **ICommandPrepare::Unprepare** для указания на то, что текст команды больше не нужен.  
  
-   Потребитель освобождает все ссылки на объект команды, связанной с временной хранимой процедурой.  
  
 Объект команды имеет в базе данных **tempdb** не более одной хранимой процедуры. Любая существующая временная хранимая процедура представляет текущий текст команды для этого объекта.  
  
## <a name="see-also"></a>См. также:  
 [Команды](../../oledb/ole-db-commands/commands.md)  
  
  

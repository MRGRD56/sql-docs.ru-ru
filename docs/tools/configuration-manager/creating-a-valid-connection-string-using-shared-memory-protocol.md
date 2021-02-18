---
title: Создание допустимой строки соединения с использованием протокола общей памяти
description: Сведения о том, когда подключения к SQL Server используют протокол общей памяти и как создать допустимую строку подключения для этого протокола.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- connection strings [Database Engine], shared memory
- aliases [SQL Server], shared memory
ms.assetid: 5fff42e8-377f-4b40-b0c8-b02393f8a1af
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 9b67ee3355916447015da000dea1286060467ee3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349805"
---
# <a name="creating-a-valid-connection-string-using-shared-memory-protocol"></a>Создание допустимой строки соединения с использованием протокола общей памяти
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  при подключении к [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с клиента, запущенного на том же компьютере, используется протокол общей памяти. У общей памяти нет настраиваемых свойств. Протокол общей памяти всегда используется первым и его нельзя переместить с верхней строчки списка **Включенные протоколы** окна **Свойства клиентских протоколов** . Протокол общей памяти может быть отключен, что бывает полезным при устранении неполадок в одном из других протоколов.  
  
 При помощи протокола общей памяти нельзя создать псевдоним, но если протокол общей памяти включен, то во время подключения к компоненту [!INCLUDE[ssDE](../../includes/ssde-md.md)] по имени создается соединение общей памяти. В строке подключения общей памяти используется формат `lpc:<servername>[\instancename]`.  
  
## <a name="connecting-to-the-local-server"></a>Подключение к локальному серверу  
 При подключении к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , запущенному на том же компьютере, что и клиент, в качестве имени сервера можно использовать **(local)** . Это действие не рекомендуется, поскольку может вызвать неоднозначность, но может быть полезным, если известно, что клиент запущен на нужном компьютере. Например, при создании приложения для мобильных отключенных пользователей, таких как торговый персонал, когда [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] будет запускаться на переносных компьютерах и использоваться для хранения данных проекта, клиент, подключающийся к **(local)** , будет всегда подключаться к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , выполняющемуся на переносном компьютере. Вместо **(local)** можно использовать слово **localhost** или точку ( **.** ).  
  
## <a name="verifying-your-connection-protocol"></a>Проверка протокола соединения  
 Следующий запрос возвратит протокол, используемый в текущем соединении.  
  
```  
SELECT net_transport   
FROM sys.dm_exec_connections   
WHERE session_id = @@SPID;  
  
```  
  
## <a name="examples"></a>Примеры:  
 Следующие имена будут подключаться к локальному компьютеру при помощи протокола общей памяти, если он включен:  
  
 `<servername>`  
  
 `<servername>\<instancename>`  
  
 `(local)`  
  
 `localhost`  
  
 Невозможно создать псевдоним для соединения по протоколу общей памяти.  
  
> [!NOTE]  
>  При указании IP-адреса в поле **Сервер** будет установлено соединение TCP/IP.  
  
## <a name="see-also"></a>См. также:  
 [Создание допустимой строки подключения с использованием протокола TCP/IP](../../tools/configuration-manager/creating-a-valid-connection-string-using-tcp-ip.md)   
 [Создание допустимой строки подключения, использующей протокол именованных каналов](/previous-versions/sql/sql-server-2016/ms189307(v=sql.130))   
 [Выбор сетевого протокола](/previous-versions/sql/sql-server-2016/ms187892(v=sql.130))  
  

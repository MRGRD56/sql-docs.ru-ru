---
description: Создание приложения поставщика OLE DB для собственного клиента SQL Server
title: Создание приложения OLE DB
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, application creation
- applications [SQL Server Native Client]
- OLE DB, creating applications
ms.assetid: f3ae6815-f32d-4913-a1a2-2ba2f20cfd88
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0a15d78c5e1a3f53a812eb13d94eded070f6b3e1
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753694"
---
# <a name="creating-a-sql-server-native-client-ole-db-provider-application"></a>Создание приложения поставщика OLE DB для собственного клиента SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Создание [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] приложения поставщика собственного клиента OLE DB включает следующие шаги:  
  
1.  установление соединения с источником данных;  
  
2.  выполнение команды;  
  
3.  обработку результатов.  

> [!NOTE]  
>  По возможности используйте аутентификацию Windows. Если проверка подлинности Windows недоступна, запросите у пользователя ввод учетных данных во время выполнения. Избегайте хранения учетных данных в файле. Если необходимо сохранение учетных данных, зашифруйте их с помощью [API-интерфейса шифрования Win32](/windows/win32/seccng/cng-portal).  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Установка подключения к источнику данных](../../relational-databases/native-client-ole-db-provider/establishing-a-connection-to-a-data-source.md)  
  
-   [Выполнение команды](../../relational-databases/native-client-ole-db-provider/executing-a-command.md)  
  
-   [Обработка результатов](../../relational-databases/native-client-ole-db-provider/processing-results.md)  
  
-   [О свойствах OLE DB](../../relational-databases/native-client-ole-db-provider/about-ole-db-properties.md)  
  
-   [Использование предложения OUTPUT с OLE DB в SQL Server Native Client](../../relational-databases/native-client-ole-db-provider/using-the-output-clause-with-ole-db-in-sql-server-native-client.md)  
  
## <a name="see-also"></a>См. также:  
 [SQL Server Native Client (OLE DB)](../../relational-databases/native-client/ole-db/sql-server-native-client-ole-db.md)  
  

---
title: Создание приложений с помощью драйвера OLE DB для SQL Server
description: Сведения об общих проблемах при создании приложений с помощью OLE DB Driver for SQL Server и о том, на что следует рассчитывать при обновлении с более старого драйвера OLE DB.
ms.custom: ''
ms.date: 02/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], building applications
- MSOLEDBSQL, building applications
- applications [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, building applications
author: David-Engel
ms.author: v-daenge
ms.reviewer: mikeray
ms.openlocfilehash: cea8fc9132b7c138865e2ad3a0ba40181ec82b4d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746604"
---
# <a name="building-applications-with-ole-db-driver-for-sql-server"></a>Создание приложений с помощью драйвера OLE DB для SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  При разработке приложения, использующего библиотеку OLE DB Driver for SQL Server, следует учитывать ряд проблем. В этом разделе обсуждаются многие из этих проблем, в том числе переход от компонентов MDAC к OLE DB Driver for SQL Server и использование файлов заголовков и библиотек OLE DB Driver for SQL Server, а также приведены общие сведения о различных строках соединения, которые можно использовать с OLE DB Driver for SQL Server.  

## <a name="in-this-section"></a>В этом разделе  
 [Установка драйвера OLE DB для SQL Server](../../oledb/applications/installing-oledb-driver-for-sql-server.md)  
 Обсуждение способа установки OLE DB Driver for SQL Server, расположения установки различных компонентов, а также способ удаления OLE DB Driver for SQL Server.  

 [Компоненты драйвера OLE DB для SQL Server](../../oledb/applications/components-of-oledb-driver-for-sql-server.md)  
 Обсуждение компонентов, входящих в состав OLE DB Driver for SQL Server, в том числе файлов библиотек, ресурсов, справки и заголовков.  

 [Использование ключевых слов строки подключения с драйвером OLE DB для SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)  
 Обсуждаются различные типы строк подключения, которые можно использовать для соединения с базой данных через драйвер OLE DB для SQL Server.  

 [Использование файлов библиотеки и заголовков драйвера OLE DB для SQL Server](../../oledb/applications/using-the-oledb-driver-for-sql-server-header-and-library-files.md)  
 Обсуждение использования файлов заголовков и библиотек OLE DB Driver for SQL Server в приложении.  

 [Обновление приложения с переходом от MDAC на драйвер OLE DB для SQL Server](../../oledb/applications/updating-an-application-to-oledb-driver-for-sql-server-from-mdac.md)  
 Обсуждение отличий OLE DB Driver for SQL Server от компонентов MDAC и проблемы, которые следует принимать во внимание при переходе с компонентов MDAC на OLE DB Driver for SQL Server.  

 [Updating an application from SQL Server 2005 Native Client](../../oledb/applications/updating-an-application-from-sql-server-2005-native-client.md) (Обновление приложения с переходом от SQL Server 2005 Native Client)  
 Обсуждение вопросов, которые необходимо учитывать при обновлении [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] Native Client до OLE DB Driver for SQL Server.  

 [Использование объектов ADO с драйвером OLE DB для SQL Server](../../oledb/applications/using-ado-with-oledb-driver-for-sql-server.md)  
 Обсуждение использования OLE DB Driver for SQL Server технологией ADO для доступа к службе [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и ее функциям.  

 [Политики поддержки для драйвера OLE DB для SQL Server](../../oledb/applications/support-policies-for-oledb-driver-for-sql-server.md)  
 Объяснение использования различных компонентов доступа к данным с разными версиями OLE DB Driver for SQL Server.  

## <a name="see-also"></a>См. также:  
 [Драйвер OLE DB для SQL Server](../../oledb/oledb-driver-for-sql-server.md)     
 [Инструкции по OLE DB](../../oledb/ole-db-how-to/ole-db-how-to-topics.md)  

---
title: Возможности драйвера OLE DB для SQL Server
description: Сведения о функциях, поддерживаемых OLE DB Driver for SQL Server, таких как зеркальное отображение базы данных, асинхронная операция, Azure Active Directory и т. п.
ms.custom: ''
ms.date: 02/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- MDAC [SQL Server]
- MSOLEDBSQL, about OLE DB Driver for SQL Server
- data access [OLE DB Driver for SQL Server], features
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 07d1c6f77cd9df46269a6d36261369ec057fd778
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751184"
---
# <a name="ole-db-driver-for-sql-server-features"></a>Возможности драйвера OLE DB для SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Помимо возможностей компонентов доступа к данным Windows DAC (ранее MDAC), в драйвере OLE DB для SQL Server реализовано множество других функций, позволяющих пользоваться функциональностью [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="in-this-section"></a>в этом разделе    
 [Использование зеркального отображения базы данных](../../oledb/features/using-database-mirroring.md)  
 Обсуждение поддержки зеркальных баз данных драйвером OLE DB для SQL Server, то есть возможности поддерживать копию (зеркало) базы данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на резервном сервере.  
  
 [Выполнение асинхронных операций](../../oledb/features/performing-asynchronous-operations.md)  
 Обсуждение поддержки драйвером OLE DB для SQL Server асинхронных операций, то есть способности немедленно возвращать управление, не блокируя вызывающий поток.  

[Использование Azure Active Directory](using-azure-active-directory.md)  
Обсуждение новых методов проверки подлинности, появившихся в драйвере OLE DB версии 18.2.1, которые имеют более безопасные стандартные параметры и позволяют подключаться к экземпляру Базы данных SQL Azure с помощью федеративного удостоверения.

 [Использование множественных активных результирующих наборов (MARS)](../../oledb/features/using-multiple-active-result-sets-mars.md)  
 Обсуждение поддержки OLE DB Driver for SQL Server режима MARS. Режим MARS позволяет выполнять и получать несколько результирующих наборов через одно подключение к базе данных.  
  
 [Использование типов данных XML](../../oledb/features/using-xml-data-types.md)  
 Обсуждение поддержки драйвером OLE DB для SQL Server типа данных XML (представляющего собой тип данных на основе XML), который можно использовать как тип столбца, переменной, параметра или значения, возвращаемого функцией.  
  
 [Использование определяемых пользователем типов](../../oledb/features/using-user-defined-types.md)  
 Обсуждение поддержки драйвером OLE DB для SQL Server пользовательских типов (UDT), которые расширяют систему типов SQL, позволяя хранить в базе данных [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] объекты и пользовательские структуры данных.  
  
 [Использование типов больших значений](../../oledb/features/using-large-value-types.md)  
 Обсуждение поддержки OLE DB Driver for SQL Server типов данных больших значений, то есть типов данных больших объектов (LOB).  
  
 [Смена пароля программным способом](../../oledb/features/changing-passwords-programmatically.md)  
 Обсуждение поддержки драйвером OLE DB для SQL Server управления истекшими паролями с возможностью сменить пароль в клиенте без вмешательства администратора.  
  
 [Работа с изоляцией моментального снимка](../../oledb/features/working-with-snapshot-isolation.md)  
 Обсуждение поддержки драйвером OLE DB для SQL Server расширений управления версиями строк, предназначенных для улучшения производительности путем исключения сценариев блокировки модулей чтения или записи.  
  
 [Работа с уведомлениями запросов](../../oledb/features/working-with-query-notifications.md)  
 Обсуждение поддержки OLE DB Driver for SQL Server уведомления потребителя об изменении набора строк.  
  
 [Выполнение операций массового копирования](../../oledb/features/performing-bulk-copy-operations.md)  
 Обсуждение поддержки драйвером OLE DB для SQL Server операций массового копирования, позволяющих передавать большие объемы данных в таблицу или представление [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], а также из таблицы или представления.  
  
 [Использование шифрования без проверки](../../oledb/features/using-encryption-without-validation.md)  
 Обсуждение использования OLE DB Driver for SQL Server для шифрования данных, передаваемых на сервер, без проверки сертификата.  
  
 [Параметры с табличным значением &#40;драйвер OLE DB для SQL Server&#41;](../../oledb/features/table-valued-parameters-oledb-driver-for-sql-server.md)  
 Обсуждение поддержки OLE DB Driver for SQL Server табличных значений параметров.  
  
 [Большие определяемые пользователем типы данных CLR](../../oledb/features/large-clr-user-defined-types.md)  
 Обсуждение поддержки определяемых пользователем типов данных среды CLR.  
  
 [Поддержка FILESTREAM](../../oledb/features/filestream-support.md)  
 Обсуждение поддержки OLE DB Driver for SQL Server улучшенной функции FILESTREAM.  
  
 [Поддержка имени субъекта-службы &#40;SPN&#41; в клиентских соединениях](../../oledb/features/service-principal-name-spn-support-in-client-connections.md)  
 Обсуждение расширенной поддержки имен участника-службы (SPN) для проведения взаимной проверки подлинности по всем протоколам.  
  
 [Поддержка разреженных столбцов в драйвере OLE DB для SQL Server](../../oledb/features/sparse-columns-support-in-oledb-driver-for-sql-server.md)  
 Обсуждение поддержки OLE DB Driver for SQL Server разреженных столбцов.  
  
 [Улучшения функций даты и времени](../../oledb/features/date-and-time-improvements.md)  
 Обсуждение поддержки новых типов данных даты и времени, добавленной в OLE DB Driver for SQL Server.  
  
 [Обнаружение метаданных](../../oledb/features/metadata-discovery.md)  
 Обсуждение улучшений в механизме обнаружения метаданных, внесенных в [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
 [Поддержка UTF-16 в драйвере OLE DB для SQL Server](../../oledb/features/utf-16-support-in-oledb-driver-for-sql-server.md)  
 Рассматривает изменение поведения, появившееся в [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. Если при привязке результата столбца или выходного параметра указывается буфер фиксированной длины, символ **wchar**, записываемый в буфер перед завершающим символом, является старшей кодовой точкой суррогатной пары, а следующий символ **wchar** является младшей кодовой точкой суррогатной пары, то драйвер OLE DB для SQL Server не добавит в буфер старшую кодовую точку суррогатной пары.  
 
 [Поддержка UTF-8 в драйвере OLE DB для SQL Server](../../oledb/features/utf-8-support-in-oledb-driver-for-sql-server.md)  
 Обсуждение поддержки кодировки сервера UTF-8 и мер предосторожности при работе с данными в кодировке UTF-8.
  
 [Поддержка высокого уровня доступности и аварийного восстановления в драйвере OLE DB для SQL Server](../../oledb/features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)  
 Описывается настройка приложения для использования функций высокого уровня доступности и аварийного восстановления, появившихся в [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
 [Доступ к диагностическим сведениям в журнале расширенных событий](../../oledb/features/accessing-diagnostic-information-in-the-extended-events-log.md)  
 Описываются улучшения, реализованные в драйвере OLE DB для SQL Server, и функции отслеживания данных, которые дают доступ к диагностическим данным в кольцевом буфере и журналах XEvents.  
  
 [Поддержка драйвера OLE DB для SQL Server в LocalDB](../../oledb/features/oledb-driver-for-sql-server-support-for-localdb.md)  
 Обсуждение поддержки OLE DB Driver for SQL Server для функции LocalDB.  
  
 [Использование разрешения IP-адресов прозрачной сети](../../oledb/features/using-transparent-network-ip-resolution.md)  
 Обсуждение поддержки OLE DB Driver for SQL Server прозрачного разрешения сетевых IP-адресов в отказоустойчивом кластере.  
  
## <a name="see-also"></a>См. также:  
 [Драйвер OLE DB для SQL Server](../../oledb/oledb-driver-for-sql-server.md)      
 [Инструкции по OLE DB](../../oledb/ole-db-how-to/ole-db-how-to-topics.md)   
 [Установка драйвера OLE DB для SQL Server](../../oledb/applications/installing-oledb-driver-for-sql-server.md)  
  
  

---
title: Использование файлов библиотеки и заголовков драйвера OLE DB для SQL Server | Документация Майкрософт
description: Узнайте, как использовать файлы заголовков и библиотек OLE DB Driver for SQL Server в среде разработки.
ms.custom: ''
ms.date: 02/12/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- header files [OLE DB Driver for SQL Server]
- MSOLEDBSQL, header files
- OLE DB, header files
- library files [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, header files
- data access [OLE DB Driver for SQL Server], header files
- data access [OLE DB Driver for SQL Server], library files
- OLE DB Driver for SQL Server, library files
- MSOLEDBSQL, library files
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9126ef09a59a04fe54c29050839de35afca84ca5
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742954"
---
# <a name="using-the-ole-db-driver-for-sql-server-header-and-library-files"></a>Использование файлов библиотеки и заголовков драйвера OLE DB для SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Заголовок OLE DB Driver for SQL Server и файлы библиотек устанавливаются, если в процессе установки выбран параметр OLE DB Driver for SQL Server. При разработке приложения важно скопировать и установить все требуемые файлы для работы среды разработки. Дополнительные сведения об установке и распространении драйвера OLE DB для SQL Server см. в статье [Installing OLE DB Driver for SQL Server](../../oledb/applications/installing-oledb-driver-for-sql-server.md) (Установка драйвера OLE DB for SQL Server).  
  
 Файлы заголовка и библиотеки OLE DB Driver for SQL Server устанавливаются в следующий каталог.  
  
 *%PROGRAM FILES%* \Microsoft SQL Server\Client SDK\OLEDB\182\SDK  
  
 Файл заголовков OLE DB Driver for SQL Server (msoledbsql.h) можно использовать для добавления функций доступа к данным OLE DB Driver for SQL Server в пользовательские приложения. Файл заголовка драйвера OLE DB для SQL Server содержит все определения, атрибуты, свойства и интерфейсы, необходимые для использования новых функций, представленных в [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].  
  
 Помимо файла заголовков OLE DB Driver for SQL Server существует также файл библиотеки msoledbsql.lib, который является библиотекой экспорта для функциональности [OpenSqlFilestream](../../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md).  
  
 Файл заголовка драйвера OLE DB для SQL Server имеет обратную совместимость с файлом заголовка sqloledb.h, используемым компонентами доступа к данным MDAC, но не содержит идентификаторов CLSID для SQLOLEDB (поставщик OLE DB для [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], включенный в компоненты MDAC) или символов для функциональных возможностей XML (которые не поддерживаются драйвером OLE DB для SQL Server).    
  
 Приложениям OLE DB, использующим OLE DB Driver for SQL Server, требуется только ссылка на msoledbsql.h. Если приложение использует как компоненты MDAC (SQLOLEDB), так и драйвер OLE DB для SQL Server, оно может ссылаться как на sqloledb.h, так и на msoledbsql.h при условии, что ссылка на файл sqloledb.h будет идти первой.  
  
## <a name="using-the-ole-db-driver-for-sql-server-header-file"></a>Использование файла заголовков OLE DB Driver for SQL Server  
 Для использования файла заголовков OLE DB Driver for SQL Server в программном коде на C/C++ необходимо использовать инструкцию **include**. В следующих разделах описывается как это сделать в приложениях OLE DB.  
  
> [!NOTE]  
>  Файлы заголовка и библиотеки OLE DB Driver for SQL Server можно скомпилировать только с помощью компилятора C++ среды Visual Studio 2012 или более поздней версии.  
  
### <a name="ole-db"></a>OLE DB  
 Чтобы использовать файл заголовка OLE DB Driver for SQL Server в приложении OLE DB, вставьте в программный код следующие строки:  
  
```    
include "msoledbsql.h";  
```  
  
> [!NOTE]  
>  Кроме того, если приложение содержит инструкцию **include** для файла sqloledb.h, то инструкция **include** для файла msoledbsql.h должна идти после нее.  
  
 При создании соединения с источником данных с помощью OLE DB Driver for SQL Server используйте в качестве строки имени поставщика "MSOLEDBSQL".  

  
## <a name="component-names-and-properties-by-version"></a>Имена и свойства компонентов в зависимости от версии  

|Свойство|Драйвер OLE DB для SQL Server|MDAC|  
|--------|----------------------------|----|   
|Идентификатор PROGID OLE DB|MSOLEDBSQL|SQLOLEDB|  
|Имя файла заголовка OLE DB|msoledbsql.h|Sqloledb.h|  
|DLL-библиотека поставщика OLE DB|msoledbsql.dll|Sqloledb.dll| 
  
  
## <a name="static-linking-and-bcp-functions"></a>Статическая компоновка и функции BCP  
 Если в приложении используются функции BCP, важно указывать в строке подключения драйвер из той же версии, которая поставлялась с файлом заголовка и библиотекой, использованными при компиляции приложения.  
  
 Дополнительные сведения см. в разделе [Performing Bulk Copy Operations](../../oledb/features/performing-bulk-copy-operations.md) (Выполнение операций массового копирования).  
  
## <a name="see-also"></a>См. также:  
 [Создание приложений с помощью драйвера OLE DB для SQL Server](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  
  
  

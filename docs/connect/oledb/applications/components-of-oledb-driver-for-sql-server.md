---
title: Компоненты драйвера OLE DB для SQL Server | Документация Майкрософт
description: Сведения о компонентах OLE DB Driver for SQL Server, в том числе библиотеке, содержащей функции драйвера, других библиотеках и файле заголовка.
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], components
- components [OLE DB Driver for SQL Server]
- MSOLEDBSQL, about OLE DB Driver for SQL Server
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b4d4ea136dbc216f758bed78caff5d84382caa45
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751284"
---
# <a name="components-of-ole-db-driver-for-sql-server"></a>Компоненты драйвера OLE DB для SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Компоненты Microsoft OLE DB Driver for SQL Server.  

|Компонент|Описание|  
|---------------|-----------------|  
|msoledbsql.dll|Файл библиотеки динамической компоновки (DLL), содержащий все функциональные возможности Microsoft OLE DB Driver for SQL Server.|  
|msoledbsqlr.rll|Сопутствующий файл ресурса для библиотеки Microsoft OLE DB Driver for SQL Server.|   
|msoledbsql.h|Файл заголовков для Microsoft OLE DB Driver for SQL Server, который содержит все новые определения, необходимые для использования Microsoft OLE DB Driver for SQL Server. Этот файл заголовка заменяет собой файл sqloledb.h.<br /><br /> Примечание. Вы можете использовать в одной программе ссылки на msoledbsql.h и sqloledb.h, но sqloledb.h в этом случае нужно определить первым.|  
|msoledbsql.lib|Файл библиотеки, необходимой для прямого вызова функции [OpenSqlFilestream](../../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md), которая включена в Microsoft OLE DB Driver for SQL Server.<br /><br /> Примечание. Если в программном коде есть ссылка на файл msoledbsql.lib, необходимо убедиться, что файл msoledbsql.dll находится в системном пути разработчика и в системных путях пользователей, использующих данное приложение.|  

## <a name="see-also"></a>См. также:  
 [Создание приложений с помощью драйвера OLE DB для SQL Server](../../oledb/applications/building-applications-with-oledb-driver-for-sql-server.md)  

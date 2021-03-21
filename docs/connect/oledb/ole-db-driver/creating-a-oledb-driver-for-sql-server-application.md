---
title: Создание приложения драйвера OLE DB для SQL Server | Документы Майкрософт
description: Сведения о действиях, необходимых для создания приложения OLE DB Driver for SQL Server и поиска дополнительных ресурсов.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, application creation
- applications [OLE DB Driver for SQL Server]
- OLE DB, creating applications
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ec04a609b67f2e1297b2415f6768dba1b6b6db12
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2021
ms.locfileid: "104735116"
---
# <a name="creating-an-ole-db-driver-for-sql-server-application"></a>Создание приложения драйвера OLE DB для SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Чтобы создать приложение OLE DB Driver for SQL Server, необходимо выполнить такие шаги:  
  
1.  установление соединения с источником данных;  
  
2.  выполнение команды;  
  
3.  обработку результатов.  
  
> [!NOTE]  
>  По возможности используйте аутентификацию Windows. Если проверка подлинности Windows недоступна, запросите у пользователя ввод учетных данных во время выполнения. Избегайте хранения учетных данных в файле. Если необходимо сохранение учетных данных, зашифруйте их с помощью [API-интерфейса шифрования Win32](/windows/win32/seccng/cng-portal).  
  
## <a name="in-this-section"></a>в этом разделе  
  
-   [Установка подключения к источнику данных](../../oledb/ole-db-driver/establishing-a-connection-to-a-data-source.md)  
  
-   [Выполнение команды](../../oledb/ole-db-driver/executing-a-command.md)  
  
-   [Обработка результатов](../../oledb/ole-db-driver/processing-results.md)  
  
-   [О свойствах OLE DB](../../oledb/ole-db-driver/about-ole-db-properties.md)  
  
-   [Использование предложения OUTPUT с OLE DB в драйвере OLE DB для SQL Server](../../oledb/ole-db-driver/using-the-output-clause-with-ole-db-in-oledb-driver-for-sql-server.md)  
  
## <a name="see-also"></a>См. также:  
 [Программирование драйвера OLE DB для SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
